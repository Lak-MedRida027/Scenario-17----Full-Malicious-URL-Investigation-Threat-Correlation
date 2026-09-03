---
tags: [workflow]
---
# 🔬 Analysis Workflow
Hub: [[00 - MOC Scenario 17]].

**Five phases** applied to every indicator:
1. **Initial analysis** — [[VirusTotal]] reputation, expand shorteners, record serving IP / status / hash.
2. **Deep analysis** — detonate in [[ANY.RUN]], watch DNS / HTTP / process / file activity.
3. **Technique ID** — obfuscation, typosquatting, cloud & TLS abuse, non-standard ports.
4. **Risk assessment** — phishing / malware / credential theft? user exposure?
5. **Reporting** — consolidate defanged IOCs, severity, recommendations.

## Per-URL triage (rendered in Obsidian)
```mermaid
flowchart TD
    A[Suspect URL / IP] --> B[VirusTotal:\nreputation + details]
    B --> C{Raw IP +\nnon-standard port?}
    C -- yes --> CC[Cluster C\nIoT Botnet C2]
    C -- no --> D[ANY.RUN:\ndetonate & observe]
    D --> E{Seed-phrase /\nwallet brand?}
    E -- yes --> CB[Cluster B\nWallet Drainer]
    E -- no --> CA[Cluster A\nPhishing / SE]
    CA --> F[Extract & defang IOCs\ndomain / IP / URL / hash]
    CB --> F
    CC --> F
    F --> G[MITRE ATT&CK + risk rating]
```

Tools used: [[VirusTotal]] · [[VirusTotal Graph]] · [[ANY.RUN]].
