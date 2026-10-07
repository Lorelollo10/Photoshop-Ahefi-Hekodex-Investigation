# Ready-to-publish notices (redacted)

## English — GitHub repository description

Independent forensic case study of a fake Photoshop installer (Oct 2026): seven full process/persistence identifiers overlap with a reported Ahefi infection; hijacked Instagram and Discord accounts propagated hekodex[.]com crypto scam links. Includes Malwarebytes observations, SHA-256 IOCs, VM reverse-engineering notes and explicit attribution limitations. No executable samples or private data.

## English — r/malwares / r/cybersecurity post

**Title:** Fake Photoshop infection shares 7 exact artifacts with Ahefi case; hijacked Instagram + Discord spread Hekodex crypto links (IOCs and report)

I investigated a Windows compromise after a counterfeit Photoshop installer was executed on 6 October 2026. Malwarebytes quarantined 23 findings including an info-stealer detection, three suspicious active executables, a fourth dropped executable, two scheduled tasks, a service and a Defender exclusion. The full names of seven artifacts **exactly match** a publicly documented Ahefi-associated infection, but SHA-256 hashes of the four binaries **do not match**. This suggests related deployment artifacts or tooling, **not necessarily the same build or operator**.

The affected Instagram **and Discord** accounts both sent crypto promotion links to hekodex[.]com. Other users have publicly reported suspicious Instagram links for the same domain. I have **no evidence** that Hekodex is the stealer C2 or exfiltration endpoint, nor proof that the Ahefi and Hekodex operators are linked.

I also analyzed a later MSI variant in an isolated VM: it used obfuscated Node.js and two native N-API addons before passing a 388,112-byte PE to another routine, where execution failed. No attacker traffic was observed in the simulated Internet setup; this does not exclude malware functionality on real hosts. The initial EXE and later MSI are **not proven identical in payload delivery**.

Report and exact SHA-256 hashes: [insert your GitHub repository URL here]. Please avoid running these files or visiting the scam domain. I welcome corrections supported by hashes, packet captures, or reproducible evidence.

## Italiano — segnalazione breve

**Titolo:** Falso Photoshop, indicatori identici a un caso Ahefi e link crypto Hekodex diffuso su Instagram e Discord

Dopo l'esecuzione di un falso installer Photoshop, Malwarebytes ha identificato 23 elementi sospetti/malevoli, tra cui un rilevamento infostealer, eseguibili attivi e meccanismi di persistenza. Sette nomi completi di file, attività e servizio coincidono con un caso pubblico associato ad Ahefi, ma gli hash dei binari differiscono. Entrambi gli account Instagram e Discord compromessi hanno diffuso il link hekodex[.]com. Questo collega Hekodex alla propagazione della truffa, **non dimostra** che fosse il server del malware o che gli stessi operatori gestissero Ahefi e Hekodex. Nel report sono presenti analisi tecnica, hash e limiti delle evidenze. [Inserire qui il link al repository del report]

## Suggested outlets (moderated, not mass-spammed)

1. Your own **GitHub repository** (main English report and CSV; Italian PDF in releases or docs).
2. **Reddit** r/malwares (read rules, provide self-post summary + repository link; disclose sources and limitations).
3. **Reddit** r/antivirus (a brief victim-warning with no executable/link to scam site).
4. **Discord Trust & Safety** and **Instagram report flows** (report compromised accounts and scam messages; include defanged domain and evidence screenshots privately, not public account details).
5. **Google Safe Browsing** phishing report and registrar/hosting abuse reporting channels (only with actual evidence; Cloudflare is a proxy, not necessarily origin host).

*Do not post identical messages indiscriminately across communities or attach live malware. Preserve screenshots privately before sharing.*