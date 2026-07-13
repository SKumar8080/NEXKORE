<div align="center">

```
███╗   ██╗███████╗██╗  ██╗██╗  ██╗ ██████╗ ██████╗ ███████╗
████╗  ██║██╔════╝╚██╗██╔╝██║ ██╔╝██╔═══██╗██╔══██╗██╔════╝
██╔██╗ ██║█████╗   ╚███╔╝ █████╔╝ ██║   ██║██████╔╝█████╗  
██║╚██╗██║██╔══╝   ██╔██╗ ██╔═██╗ ██║   ██║██╔══██╗██╔══╝  
██║ ╚████║███████╗██╔╝ ██╗██║  ██╗╚██████╔╝██║  ██║███████╗
╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝

           N E X T - G E N   R E C O N   E N G I N E
```

**A modular, terminal-native orchestration framework for authorized security assessments**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey.svg)](#)
[![Status](https://img.shields.io/badge/Status-Active-success.svg)](#)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg)](#contributing)

</div>

---

> ⚠️ **Legal & Ethical Use Notice**
> NEXKORE is built strictly for **authorized security testing** — engagements you own, or for which you hold explicit written permission to test. It automates and organizes output from well-known, publicly available security tools (subfinder, nuclei, httpx, ffuf, sqlmap, dalfox, etc.); it does not contain exploit code of its own. Running it against systems you do not have permission to test is illegal in most jurisdictions. **You are solely responsible for how you use this tool.**

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Tool Arsenal](#-tool-arsenal)
- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [Main Menu](#-main-menu)
- [Workspace Layout](#-workspace-layout)
- [Sample Report](#-sample-report)
- [Configuration](#-configuration)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🧭 Overview

**NEXKORE** is a single-file, menu-driven Python framework that wraps industry-standard reconnaissance and vulnerability-scanning tools into one guided workflow. It creates a clean, timestamped workspace per target, chains tool output together automatically (subdomains → DNS → HTTP probing → crawling → scanning), and generates a Markdown findings report at the end — all inside a polished `rich`-powered terminal UI.

It's designed for bug bounty hunters, red teamers, and pentest engineers who want the repetitive parts of recon and triage automated without giving up visibility into what's actually running.

---

## ✨ Features

| Category | Capability |
|---|---|
| 🎛️ **Guided TUI** | Animated banner, spinners, progress bars, and structured tables via `rich` |
| 🌐 **Recon Pipeline** | Subdomain enumeration → DNS resolution → HTTP probing → URL harvesting → crawling, fully chained |
| 🕷️ **Parameter Mining** | Automated parameter discovery via ParamSpider for fuzzing & injection testing |
| 🛡️ **Vulnerability Scanning** | Nuclei template scans, XSS hunting (Dalfox), SQLi testing (SQLMap), directory fuzzing (FFUF) |
| 🔓 **Exposure Checks** | Exposed `.git` repository detection, CORS misconfiguration testing, Host Header / SSRF probing |
| 🔑 **Secret Hunting** | TruffleHog integration for leaked API keys, tokens, and credentials |
| 📁 **Workspace Management** | Auto-generated per-target, per-session directory tree with `metadata.json` |
| 🧰 **Environment Manager** | One-shot install/verify for Go, pip, and APT-based tool dependencies, plus isolated venv creation |
| 📝 **Reporting** | Auto-generated Markdown report aggregating every phase's findings |

---

## 🏗️ Architecture

```
                         ┌────────────────────┐
                         │   NEXKORE  Core     │
                         │  (menu / TUI layer) │
                         └──────────┬──────────┘
                                    │
        ┌───────────────┬──────────┼──────────┬───────────────┐
        ▼               ▼          ▼          ▼               ▼
 ┌─────────────┐ ┌─────────────┐ ┌────────┐ ┌───────────┐ ┌──────────┐
 │ EnvManager  │ │ Workspace   │ │ Recon  │ │   Vuln    │ │  Report  │
 │             │ │ Manager     │ │ Engine │ │  Scanner  │ │Generator │
 └──────┬──────┘ └──────┬──────┘ └───┬────┘ └─────┬─────┘ └────┬─────┘
        │               │            │            │            │
   installs Go/     creates per-  subfinder   nuclei/ffuf/  writes
   pip/apt tools    target session  dnsx/httpx  dalfox/sqlmap/ Markdown
   + venv           directory tree  gau/katana  trufflehog/   report
                                    paramspider  cors/hostHdr
```

---

## 🧰 Tool Arsenal

NEXKORE orchestrates these external, independently-maintained tools. Install instructions for each are handled automatically by the **Environment Manager** module (menu option `1`).

| Tool | Source | Role |
|---|---|---|
| [subfinder](https://github.com/projectdiscovery/subfinder) | Go | Subdomain enumeration |
| [dnsx](https://github.com/projectdiscovery/dnsx) | Go | DNS resolution / filtering |
| [httpx](https://github.com/projectdiscovery/httpx) | Go | HTTP/HTTPS probing & tech detection |
| [katana](https://github.com/projectdiscovery/katana) | Go | Active web crawling |
| [nuclei](https://github.com/projectdiscovery/nuclei) | Go | Template-based vulnerability scanning |
| [naabu](https://github.com/projectdiscovery/naabu) | Go | Fast port scanning |
| [ffuf](https://github.com/ffuf/ffuf) | Go | Directory / endpoint fuzzing |
| [gau](https://github.com/lc/gau) | Go | Wayback / CommonCrawl URL harvesting |
| [dalfox](https://github.com/hahwul/dalfox) | Go | XSS scanning |
| [paramspider](https://github.com/devanshbatham/ParamSpider) | pip | Parameter mining |
| [trufflehog3](https://github.com/feeltheajf/trufflehog3) | pip | Secret / credential scanning |
| [sqlmap](https://github.com/sqlmapproject/sqlmap) | apt | SQL injection testing |
| [nmap](https://nmap.org/) | apt | Network mapping |
| [wfuzz](https://github.com/xmendez/wfuzz) | apt | Web fuzzing |

---

## ⚙️ Installation

### Prerequisites

- **Python** 3.8+
- **Go** 1.21+ (required for the Go-based tools — [install guide](https://go.dev/dl/))
- **git**
- Linux is recommended for full APT-tool support; macOS/Windows can run the Python-native modules

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/nexkore.git
cd nexkore
```

### 2. Install Python dependencies

```bash
pip install -r requirements.txt
```

> NEXKORE also self-bootstraps its core Python dependencies (`rich`, `pyfiglet`, `requests`) on first run if they're missing — `requirements.txt` is provided for reproducible environments and CI.

### 3. (Optional) Install the external security tools

Launch NEXKORE and use the in-app **Environment Setup** menu, or install manually:

```bash
python3 nexkore.py
# → select option 1 → e (Install all)
```

---

## 🚀 Quick Start

```bash
python3 nexkore.py
```

1. Watch the boot sequence and banner load.
2. Select **`2`** to create a new target workspace (enter domain + output path).
3. Select **`3`** to run the full recon pipeline, or step through individual modules.
4. Select **`15`** at any point to generate a Markdown findings report.

---

## 📋 Main Menu

| # | Module | Description |
|---|---|---|
| 1 | Environment Setup | Check / install all tools + create a venv |
| 2 | New Target Workspace | Create session directory structure |
| 3 | Full Recon Pipeline | `subfinder → dnsx → httpx → gau → katana` |
| 4 | Subdomain Enum Only | `subfinder` + `dnsx` |
| 5 | HTTP Probe Only | `httpx` on a subdomain list |
| 6 | URL Harvest + Crawl | `gau` + `katana` + `paramspider` |
| 7 | Nuclei Vuln Scan | Automated CVE + misconfiguration scanning |
| 8 | XSS Scan (Dalfox) | Parameter-based XSS hunting |
| 9 | Dir Fuzzing (ffuf) | Directory & endpoint fuzzing |
| 10 | SQLi Scan (sqlmap) | SQL injection testing |
| 11 | Secret Hunting | TruffleHog — API key / token leak detection |
| 12 | CORS Check | Automated CORS misconfiguration testing |
| 13 | Host Header Injection | Host header + SSRF probing |
| 14 | `.git` Exposure Check | Detect exposed git repositories |
| 15 | Generate Report | Markdown report of all findings |
| 16 | Workspace Tree | Show current session file tree |
| 0 | Exit | Quit NEXKORE |

---

## 📂 Workspace Layout

Every target gets an isolated, timestamped session directory:

```
nexkore_targets/
└── example.com/
    └── 20260713_143210/
        ├── metadata.json
        ├── 01_recon/
        │   ├── subdomains/
        │   ├── dns/
        │   ├── http_probing/
        │   ├── urls/
        │   ├── js_analysis/
        │   ├── params/
        │   └── screenshots/
        ├── 02_vuln/
        │   ├── nuclei/
        │   ├── ffuf/
        │   ├── xss/
        │   ├── sqli/
        │   ├── ssrf/
        │   ├── idor/
        │   ├── lfi/
        │   ├── open_redirect/
        │   └── race_condition/
        ├── 03_secrets/
        ├── 04_reports/
        ├── 05_wordlists/
        ├── 06_notes/
        └── 07_payloads/
```

---

## 📄 Sample Report

```markdown
# NEXKORE Pentest Report
**Author:** Your Name
**Tool:** NEXKORE v1.0.0
**Target:** example.com
**Date:** 2026-07-13 14:32:10
**Session:** 20260713_143210

---

## Recon Summary
- **Subdomains (subfinder):** 128 entries
- **Live DNS hosts:** 94 entries
- **Live HTTP hosts:** 71 entries
- **Archived URLs (gau):** 3,402 entries
- **Crawled URLs (katana):** 1,215 entries
- **Parameters (paramspider):** 340 entries

## Vulnerability Findings
### nuclei_20260713_144501.txt
- Findings: 6
...
```

---

## 🔧 Configuration

Default paths, tool registries, and the workspace directory blueprint are defined at the top of `nexkore.py`:

```python
GO_TOOLS   = { ... }   # Go-installable tools
PIP_TOOLS  = { ... }   # pip-installable tools
APT_TOOLS  = { ... }   # apt-installable tools
DIR_TREE   = [ ... ]   # workspace folder structure
```

Edit these dictionaries/lists directly to add tools or change the workspace layout — no config file is required.

---

## 🗺️ Roadmap

- [ ] JSON/HTML report export alongside Markdown
- [ ] Config file support (`.nexkore.yml`) instead of interactive-only prompts
- [ ] Parallel execution for independent recon phases
- [ ] Docker image for a fully self-contained toolchain
- [ ] Slack / Discord webhook notifications on scan completion

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m "Add my feature"`)
4. Push to your branch (`git push origin feature/my-feature`)
5. Open a Pull Request

Please keep new modules consistent with the existing `rich`-based UI conventions and workspace layout.

---

## ⚠️ Disclaimer

This tool is provided for **educational and authorized security testing purposes only**. The author(s) and contributors accept no liability and are not responsible for any misuse or damage caused by this program. Always obtain **explicit written authorization** before scanning any system you do not own.

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

<div align="center">

Made with ☕ and `rich` panels

</div>
