# Incident report: Fake Photoshop installer, Ahefi-linked artifacts, and Hekodex propagation on Instagram and Discord

**Independent defensive case study · 8 October 2026 · Version 1.0 · Public/redacted edition**

> **Executive finding:** A Windows host was compromised after running a counterfeit Photoshop installer. Malwarebytes recorded active malicious processes, persistence, a Defender exclusion, and classified the initial installer as `Spyware.InfoStealer.Electron.Generic`. **Seven unusually specific process/task/service identifiers exactly match** a separately published Ahefi-associated victim report, although the four compared executable SHA-256 hashes are **different**. The victim reports that compromised **Instagram and Discord** accounts both sent a promotional crypto link to `hekodex[.]com`. A shared criminal operator, exfiltration server, or identical malware build **has not been established**.

## Scope and evidence handling

This case is based on privately supplied Malwarebytes scan logs dated 6 October 2026, owner-observed account incidents, file hashes, reverse-engineering artifacts and screenshots from isolated VirtualBox Windows/Kali VMs, and independent public reports. Personal names, handles, email addresses, phone numbers, tokens, and victim-specific directory names are intentionally omitted. No malware executable is distributed with this publication.

**Evidence grades:** **Observed** = local forensic artifacts or first-hand victim report; **Corroborated** = cross-checked against public source; **Hypothesis** = plausible but unproven. Self-reported external cases are not independently authenticated by this author. There is **no packet capture from the originally compromised host**.

## Timeline

| Date | Event | Evidence grade |
|---|---|---|
| 6 Oct 2026 | Victim executed a counterfeit `Photoshop27Full.exe` | Observed / Malwarebytes detection |
| 6 Oct, 14:31 | Malwarebytes scan flagged **23 findings**, all quarantined | Observed / original scan report |
| After infection | Unauthorized access and loss of control involving Instagram and Discord, with Google security alerts | Victim-observed |
| After compromise | **Both Instagram and Discord** were used to distribute a `hekodex[.]com` crypto-promotion link | Victim-observed |
| 6–7 Oct | Accounts secured, sessions revoked, Windows reinstalled from clean media | Victim-observed |
| 7 Oct | Later MSI sample analyzed in an isolated VM with Sysmon, Procmon, PE disassembly and Node/N-API hooks | Laboratory evidence |

## Original-host findings

The original Malwarebytes report documented the following suspicious executables and persistence entries:

| Indicator | Type | SHA-256 |
|---|---|---|
| `Photoshop27Full.exe` | Original fake installer; InfoStealer detection | `6a4ebb1eb8c91f9eb89cb4059269be601758bbea911efa24e223263bdd4ae137` |
| `ProManagerc7e51.exe` | Trojan.Crypt | `388967831f49e57486244a4614cb6e71870478418e8c74ef0551d87b5c882ebf` |
| `ProUpdate7dd338.exe` | Trojan.MalPack.PES | `567b50a6e1f2e123d0b96f7a9ff4891097b6cbf90a8676e9b96f99010ed2df76` |
| `NetHost1dcc90.exe` | Backdoor.Agent | `5e19d99333b1d5f48c17a56b76436221d97c59ee416a3a2bdc4255abdbb1cc90` |
| `AppManagerServicef7ad.exe` | Malware.AI detection | `bbe1c6043618cd389e158774747cb9d73b02eb9d541ff4b8a67808af18e3ac2c` |

**Persistence:** scheduled tasks `UpdateHost_1117` and `MaintainHost_058f`; Windows service `CheckManager_29ce`. The logs also show an exclusion for `SVCHOST.EXE` under Windows Defender's process exclusions. The detections and subsequent account incidents substantiate serious compromise but **do not by themselves establish which individual cookies/passwords/documents were stolen or their destination**.

## Ahefi correlation: matching artifacts, non-matching binaries

A public account from a user who installed the Ahefi VS Code extension [1] lists **exactly the same seven full identifiers**, including alphanumeric suffixes:

`NetHost1dcc90.exe` · `ProManagerc7e51.exe` · `ProUpdate7dd338.exe` · `AppManagerServicef7ad.exe` · `CheckManager_29ce` · `MaintainHost_058f` · `UpdateHost_1117`.

This is strong evidence of **shared artifacts or deployment conventions** between the cases. It does **not** prove the same infection vector or a single operator. Importantly, the other reporter's hashes do not match the locally recovered binary hashes:

| Binary | Local SHA-256 prefix | Ahefi reporter SHA-256 prefix [1] |
|---|---|---|
| NetHost | `5e19d993...` | `bb2e245a...` |
| ProManager | `38896783...` | `de13c9db...` |
| ProUpdate | `567b50a6...` | `7ad335cc...` |
| AppManagerService | `bbe1c604...` | `c211c902...` |

A separate static analysis of Ahefi v1.0.4 [2] describes a Windows-targeted extension that retrieves encrypted payload configurations from a remote endpoint and executes retrieved code. This establishes a plausible **delivery model**, not that this victim installed Ahefi, not that the Photoshop installer called `ahefi[.]com`, and not that a given executable is identical.

## Hekodex: cross-platform account abuse, not proven C2

The victim reports promotional messages containing `hekodex[.]com` were sent by **both compromised Instagram and Discord accounts**. A Reddit post on 5 October [3] reported a suspicious crypto link sent by a known Instagram contact; an independently published analysis [4] associates the domain with a comparable crypto-promotion/MrBeast-themed scam. Domain reputation snapshots [5] report registration on **3 October 2026**, Cloudflare delivery and differing detection counts across scanners and times.

The observed role of `hekodex[.]com` is **social engineering / scam promotion through hijacked accounts**. There is **no direct evidence** that it was used to collect stolen cookies or served as malware command-and-control. Cloudflare edge IPs must not be used to infer an underlying server owner.

## Isolated laboratory investigation of the later MSI

Two separately downloaded ZIP archives had different overall archive hashes, but each extracted to the **same** `Photoshop27Full.msi` with SHA-256 `86d0759c3fb2c1ec6e40c5c4518a6641e37d806a422880ee66feb62f256304e4` (19,820,544 bytes). This MSI differed at the file level from the original `.exe`; equivalence of their full infection chains was not established.

The MSI identified itself as `WoodPoint Excel Export`, launched Node 12, executed an obfuscated `y.js`, and produced a transformed `clik5o3.js`. Execution showed dynamically created native Node N-API addons loaded into `node.exe`, then a binary PE payload:

`MSI → node.exe / y.js → clik5o3.js → first N-API addon → second N-API addon → 388,112-byte PE input`

| Recovered lab artifact | SHA-256 |
|---|---|
| Native addon, 309,248 bytes | `a8f8016f2f91ee3c4df9e9fc67f861c4aeaa960f70e89d51704822fd6022a756` |
| Native addon, 317,440 bytes | `0fcfa35275a5afbac12d483230c4d1cd9fc4e8bc3ec0798323ece06a27a4603e` |
| Extracted PE32+ x64, 388,112 bytes | `0d75e404a160b180039257aa1d16f36d2732d60bc281cfc4e411517cb07c7001` |

The native addons export obfuscated JavaScript-callable functions (`kjtt61h2k`, `gxlqtfyj`, `wvvc0sf52`, and others). Static traces of the first addon reach BCrypt hashing primitives, consistent with validation/unpacking; this is **not proof of credential theft**. The second addon received the PE buffer and the observed execution ended with `Error: h`; the root cause was not established.

**Network-test limitation:** The VM was restricted to VirtualBox Internal Network `malnet` with no real outbound route; Kali simulated DNS and HTTP endpoints. No malware-attributed DNS/HTTP transaction or accesses to decoy Chromium `Login Data`, `Cookies` and `Local State` files were observed in those runs. Such negatives concern **only the code path exercised**, and do not exonerate the sample.

## Confidence matrix

| Claim | Assessment |
|---|---|
| Windows compromise, persistence and Defender modification | **Observed** |
| Information-stealer label on original installer | **Observed AV classification**; behavior not fully reconstructed |
| Seven exact identifiers shared with an Ahefi-associated report | **Cross-source corroborated** |
| Same binary builds as the Ahefi report | **No; SHA-256 values differ** |
| Hekodex links sent from compromised Instagram **and** Discord accounts | **First-hand victim report** |
| Ahefi and Hekodex run by the same actor | **Unproven** |
| Hekodex as exfiltration/C2 destination | **Unproven** |
| Exact stolen data or original exfiltration endpoint | **Unknown** |
| Original EXE and later MSI deploy identical payloads | **Unproven** |

## Response guidance

Isolate impacted hosts, preserve logs/hashes, rebuild the OS from trusted media when appropriate, and review/purge startup tasks, services and Defender exclusions. From a known-clean device, rotate passwords and API keys, revoke **all existing sessions/tokens**, reconfigure 2FA if required, verify account recovery channels and recent security actions, and alert recipients of messages sent from hijacked Instagram or Discord accounts. Do not execute recovered binaries on a live workstation or visit the defanged scam domain to “test” it.

## Indicators of compromise

Machine-readable entries are provided in `indicators.csv`. **IOC scope matters:** hash matches identify specific builds, while filenames/task names may be reused or differ in other infections. `hekodex[.]com` is designated **propagation/scam-domain IOC**, **not exfil/C2**. `ahefi[.]com` is **public Ahefi distribution context**, and was not observed on this victim's original host.

## Sources and limitations

**[Local-1]** Malwarebytes original-host scan, 6 October 2026 at 14:31, provided privately by victim; public edition redacted. **[Local-2]** Full Malwarebytes scan, 6 October 2026. **[Lab]** VM logs, Sysmon, Procmon, PE analysis and screenshot/transcript evidence, supplied by victim.

1. Public Ahefi victim report: https://www.reddit.com/r/malwares/comments/1wpbhu7/from_my_brothers_wake_to_getting_infected_by_ahefi/
2. Independent Ahefi v1.0.4 analysis (25 September 2026): https://linux.sb/topic/23801
3. Hekodex scam inquiry (5 October 2026): https://www.reddit.com/r/antivirus/comments/1wygj7r/mrbeast_hekodex_crypto_scam_question/
4. Hekodex social-engineering analysis (6 October 2026): https://howtoremove.guide/hekodex-com-scam-casino/
5. Domain reputation snapshots: https://scanner.pcrisk.com/scan-results/hekodex.com and https://tools.malwaretips.com/url-scan/hekodex.com
6. Related counterfeit Photoshop promotional repository (not evidence of its owner being the operator): `github[.]com/stormkazekagematch/adobe-ps-v27-core`.

**Publication ethics:** This report intentionally omits identifying victim information, executable samples, passwords, tokens, precise user directory paths, and unsupported personal attribution. This is a defensive incident report, not an accusation that specific individuals or the Ahefi and Hekodex domain owners are one actor.