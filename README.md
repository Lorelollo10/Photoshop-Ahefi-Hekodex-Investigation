# Fake Photoshop Installer — Ahefi Artifact Correlation & Hekodex Account Abuse

**Independent malware incident investigation | October 2026 | Public, redacted**

A Windows host was compromised after running a counterfeit Photoshop installer. Malwarebytes recorded **23 findings** including active malicious processes, scheduled-task/service persistence, and a Windows Defender process exclusion. Subsequent unauthorized access affected **Instagram and Discord**; the compromised accounts distributed a cryptocurrency-themed link to `hekodex[.]com`.

A separate public case involving the **Ahefi VS Code extension** documents **seven identical process, service and scheduled-task identifiers**. However, the compared executable SHA-256 hashes differ. This establishes a significant artifact correlation, **not proof** of a shared threat actor, identical payload builds, or a common infection vector.

## Reports

| Language | Full technical report | PDF |
|---|---|---|
| English (primary) | [Read the investigation](INCIDENT_REPORT_Photoshop_Ahefi_Hekodex_EN.md) | [Download PDF](INCIDENT_REPORT_Photoshop_Ahefi_Hekodex.pdf) |
| Italiano | [Leggi il rapporto](Rapporto_incidente_Photoshop_Ahefi_Hekodex_IT.md) | [Scarica PDF](Rapporto_incidente_Photoshop_Ahefi_Hekodex.pdf) |

- [Indicators of compromise — 19 entries (CSV)](indicators.csv)
- [Publication and community post drafts (IT/EN)](Testi_pronti_pubblicazione_IT_EN.md)

## Key observations

- The original counterfeit installer (`Photoshop27Full.exe`) was detected as `Spyware.InfoStealer.Electron.Generic`; additional resident binaries were flagged as Trojan/Backdoor components.
- **Seven exact names** for executables and persistence entries overlap with an Ahefi-related public victim report; **four corresponding executable hashes differ**.
- The victim reports that **both Instagram and Discord** sent `hekodex[.]com` scam links following account compromise.
- An isolated analysis of a **later, distinct MSI file** documented an obfuscated Node.js/N-API loader, two native addons, and a 388,112-byte PE input. Equivalence to the original EXE infection chain is **unproven**.
- **No original-host packet capture or proven exfiltration/C2 endpoint** is available. `hekodex[.]com` is documented as a *scam-propagation destination*, **not a demonstrated data-exfiltration endpoint**.

## Evidence and limitations

The findings combine original-host Malwarebytes logs, first-hand account observations, controlled VM analysis and public third-party reports. The full reports distinguish directly observed facts, independently reported similarities and open hypotheses. **Attribution to a specific actor or a claimed connection between the operators of Ahefi and Hekodex has not been established.**

The repository contains **no executable malware, passwords, tokens or victim-identifying details**. Domains are defanged in the analytical text.

For the technical methods, full SHA-256 values, timeline, confidence matrix and source links, see the [English report](INCIDENT_REPORT_Photoshop_Ahefi_Hekodex_EN.md).

> Defensive research only. Do not execute unknown samples on a production computer or treat shared filenames as proof of identical binaries.
