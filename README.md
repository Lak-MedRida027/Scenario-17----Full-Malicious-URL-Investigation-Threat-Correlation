# 🔗 Full Malicious URL Investigation & Threat Correlation

> **113 suspect URLs from a single email campaign → triaged, clustered, and mapped to MITRE ATT&CK.**

![Purple Team](https://img.shields.io/badge/Purple%20Team-Bootcamp-5B2D8E?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-27AE60?style=for-the-badge)
![Risk](https://img.shields.io/badge/Risk-HIGH%20%2F%20CRITICAL-C0392B?style=for-the-badge)
![IOCs](https://img.shields.io/badge/IOCs-113-E67E22?style=for-the-badge)

---

## 📋 Scenario

An email campaign targets employees using a shortened URL that leads to a phishing page or exploit kit. The investigation follows a five-phase methodology: **Initial Analysis → Deep Analysis → Technique Identification → Risk Assessment → Reporting**.

## 🧩 Threat Clusters Identified

| Cluster | Description | IOCs | Verdict |
|---------|-------------|------|---------|
| **A — Phishing & Social Engineering** | Credential/financial lures on abused free hosting (Netlify, Cloudflare Pages, GitHub Pages) | 8 | PHISHING |
| **B — Crypto Wallet Drainers** | Trezor/Uphold typosquats stealing recovery seed phrases via `*.pages.dev` | 6 | DRAINER |
| **C — IoT Botnet C2 & Malware Delivery** | Mirai/Gafgyt/Mozi multi-arch ELF payloads & loader infrastructure | 99 | BOTNET |

## 🔬 Analysis Toolchain

| Tool | Role |
|------|------|
| **VirusTotal** | Multi-engine reputation, passive DNS, resolutions, communicating files |
| **VirusTotal Graph** | Visual pivoting across domains, IPs, URLs, and dropped files |
| **ANY.RUN** | Interactive sandbox — live DNS, HTTP, process, and file-execution telemetry |

## 🗺️ MITRE ATT&CK Mapping

| ID | Technique | Cluster |
|----|-----------|---------|
| T1566.002 | Phishing: Spearphishing Link | A, B |
| T1656 | Impersonation (brand/vendor spoofing) | A, B |
| T1583.001 | Acquire Infrastructure: Domains (typosquatting) | B |
| T1583.006 | Acquire Infrastructure: Web Services (Netlify, CF Pages) | A, B |
| T1608.001/.004 | Stage Capabilities: Upload Malware / Drive-by Target | A, C |
| T1204.001 | User Execution: Malicious Link | A, B |
| T1110.001 | Brute Force: Password Guessing (SSH/Telnet spread) | C |
| T1105 | Ingress Tool Transfer (wget/curl of bin.sh + binaries) | C |
| T1059.004 | Command & Scripting Interpreter: Unix Shell | C |
| T1571 | Non-Standard Port (high ephemeral C2 ports) | C |
| T1027 | Obfuscated/Packed Files (UPX, stripped ELF) | C |
| T1498/T1499 | Network / Endpoint Denial of Service (botnet goal) | C |

## 🔑 Key Findings

- **Trusted-host abuse** — Phishing pages deployed on Netlify, Cloudflare Pages, and GitHub Pages with auto-issued TLS certificates to fake legitimacy.
- **Low VT score ≠ safe** — A fake "Flint Browser Pro" site targeting security professionals sat at just **1/95** on VirusTotal. Fresh infrastructure must be treated as unproven, not clean.
- **Irreversible crypto loss** — Wallet drainers harvest seed phrases; once submitted, all assets are drained with no recovery path.
- **Shared payloads across botnet nodes** — Two different IPs served identical **300.74 KB** packed ELF files — pattern matching (file size, hash) reveals hidden infrastructure relationships.
- **Intermittent C2 availability** — Binary hosts time out between campaigns, so blocking must target IP/port/path patterns rather than relying on live reachability.

## 📊 IOC Summary

| Type | Count |
|------|-------|
| Malicious Domains (Cluster A + B) | 10 |
| Malicious URLs — Phishing & Crypto | 16 |
| Botnet C2 / Distribution Servers | 3 |
| Botnet Loader / Infected-Bot Hosts | 47 |
| **Total** | **113** |

> ⚠️ All IOCs are **defanged** in the report (`hxxp`, `[.]`) and ready for ingestion into SIEM/firewall/proxy blocklists.

## 📁 Repository Contents

```
├── SC17-KG/                           # Obsidian knowledge graph 
├── Screenshots/                       # Evidence captures
├── Mohammed_Rida_Task17_Report.pdf    # Full investigation report (56 pages)
└── README.md
```

## 🛡️ Recommendations

1. **Block & sinkhole** every IOC across email gateway, web proxy, DNS resolver, and perimeter firewall
2. **Hunt retroactively** in proxy/DNS/firewall logs for prior connections to these hosts
3. **User awareness** — targeted briefing on brand typosquats and free-hosting padlock abuse
4. **Harden IoT/Linux** — disable Telnet, enforce key-based SSH, segment IoT VLANs
5. **Detection content** — proxy/IDS rules for `/bin.sh`, `/i`, `/main_<arch>` fetches over HTTP to raw IPs on non-standard ports

## 📖 Methodology

```
Phase 1              Phase 2             Phase 3              Phase 4              Phase 5
Initial Analysis  →  Deep Analysis   →  Technique ID     →  Risk Assessment  →  Reporting
─────────────────    ───────────────    ──────────────────   ────────────────    ──────────────
• Submit to VT       • Detonate in      • Obfuscation /      • Phishing /        • Findings & IOCs
• Expand shortener     ANY.RUN            shortening           malware / kit?    • Risk level
• Record redirects   • Monitor DNS /    • Domain shadowing   • Credential-theft  • Recommendations
                       HTTP / process   • Cloud & HTTPS        risk?
                     • File execution     abuse              • User exposure?
```

## 🏷️ Context

- **Bootcamp:** Purple Team Bootcamp — Golden Level
- **Instructors:** Mohammed Baqer Hasan & Anmar Mohammed
- **Student:** Mohammed Rida Lakhdari
- **Scenario ID:** `15-lec-30&31(Malicious Attachments&urls)-13-golden`
- **Report:** N° 17

## 📬 Connect

- **Portfolio:** [rlak.vercel.app](https://rlak.vercel.app)
- **LinkedIn:** [Mohammed Rida Lakhdari](https://www.linkedin.com/in/mohammed-rida-lakhdari/)
