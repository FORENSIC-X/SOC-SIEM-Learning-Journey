# SOC Analysis & SIEM Learning Journey

This repository is my public documentation of learning **SOC (Security Operations Centre) analysis and SIEM (Security Information and Event Management)** for defensive security, starting from absolute zero. I'm documenting every module, every lab, every detection rule, every mistake, and every fix as I go. It's partly for accountability, and partly so the next beginner who finds this repo has something honest to follow.

I'm not a security professional. I'm starting with no prior background in SOC work, log analysis, or detection engineering, and building up from there. Everything I use is **free**.

## Why this repo exists

- To force myself to understand things well enough to explain them, not just copy and paste commands.
- To keep a dated record of my progress that I can look back on.
- To pair my offensive-security labs with the other half of the story: I generate attacks from Kali in my own lab, then learn to detect and investigate them here.
- To keep everything reproducible: my lab setup, my queries, my rules, my results, my errors.

## My lab environment

> **Status:** this is my planned design. I'm building it in Module 0 and Module 6, so some machines don't exist yet.

| Machine | Platform | Role | Status |
|---|---|---|---|
| Host | Windows 11 laptop | Hypervisor host; browser access to SIEM dashboards | Ready |
| Wazuh server VM | Ubuntu Server, VirtualBox | SIEM: Wazuh manager, indexer, dashboard | Planned |
| Windows endpoint VM | Windows 11 evaluation, VirtualBox | Monitored endpoint with Sysmon and the Wazuh agent | Planned |
| Ubuntu VM | VirtualBox | Monitored Linux/web endpoint (DVWA, WebGoat, Juice Shop) with the Wazuh agent | Existing |
| Kali Linux VM | VirtualBox | Attack generator: produces the activity I then detect | Existing |
| Metasploitable2 VM | VirtualBox | Optional legacy target; forwards syslog | Existing |

**Networking:** all lab machines share the host-only network, so SIEM traffic and attack traffic never leave it. A second NAT adapter on the SIEM and endpoint VMs is for updates and downloads only, and I disable it during attack labs where practical. Kali's bridged adapter stays off during attack-generation labs. Metasploitable2 stays host-only and single-homed, exactly as in my offensive lab.

**Resources:** my laptop has 16 GB of RAM, so I run the lab in **profiles** (for example Wazuh plus one endpoint, or Wazuh plus Windows plus Kali) rather than every VM at once. One SIEM runs at a time. All VMs and installs live on a separate drive from my Windows system drive. The profiles are defined in [`learning-plan.md`](./learning-plan.md).

## Tool stack (free only)

| Role | Tool |
|---|---|
| Primary SIEM / XDR | Wazuh |
| Query-language practice | Splunk Free |
| Windows telemetry | Sysmon and Windows Event Logs |
| Linux telemetry | `auditd`, `syslog`/`journald`, web-server logs |
| Network monitoring | Suricata and Zeek |
| Detection rules | Sigma, Wazuh rules and decoders |
| Threat intelligence | MISP (self-hosted), plus free tiers of VirusTotal and AbuseIPDB |
| Automation | Python, Bash, PowerShell, optionally Shuffle |
| Practice platforms | CyberDefenders, BlueTeamLabs Online, TryHackMe free rooms, Splunk BOTS, LetsDefend free tier |

Free-tier limits change, so I verify them in Module 0 and again before relying on any of them.

## Repository structure

```
soc-siem-learning-journey/
├── README.md                          # This file: overview, structure, progress tracker
├── learning-plan.md                   # My full module-by-module learning plan
├── repository-plan.md                 # How this repo is organised, named and committed
├── progress.md                        # Detailed checklist, one line per sub-topic
├── SECURITY.md                        # Lab-only statement and what is never committed
│
├── docs/                              # Lesson notes, one file per sub-topic
│   ├── 00-ethics-and-lab-setup/
│   ├── 01-security-and-soc-fundamentals/
│   ├── 02-networking-for-defenders/
│   ├── 03-linux-for-defenders/
│   ├── 04-windows-for-defenders/
│   ├── 05-log-fundamentals/
│   ├── 06-siem-concepts-and-wazuh-deployment/
│   ├── 07-endpoint-telemetry/
│   ├── 08-searching-and-querying/
│   ├── 09-detection-engineering-wazuh-rules/
│   ├── 10-detection-engineering-sigma-and-tuning/
│   ├── 11-alerting-triage-and-investigation/
│   ├── 12-network-security-monitoring/
│   ├── 13-threat-intelligence-and-iocs/
│   ├── 14-attack-to-detection-labs/
│   ├── 15-incident-response-and-playbooks/
│   ├── 16-threat-hunting/
│   ├── 17-scripting-for-defenders/
│   ├── 18-identity-and-cloud-log-analysis/
│   ├── 19-practice-platforms-and-public-datasets/
│   ├── 20-soc-operations-metrics-and-compliance/
│   └── 21-capstone-project/
│
├── labs/                              # Mirrors docs/ numbering: raw walkthroughs, output, screenshots
│   └── <module>/assets/
│
├── lab-config/                        # Sanitised configs I actually used (Wazuh, Sysmon, auditd, ...)
│
├── detections/
│   ├── sigma/                         # One Sigma rule per file
│   ├── wazuh/                         # Custom Wazuh rules and decoders
│   ├── suricata/                      # Custom Suricata rules
│   ├── yara/                          # Simple YARA rules
│   └── queries/                       # Saved SPL, KQL and OpenSearch queries
│
├── attack-coverage/                   # MITRE ATT&CK Navigator layers
├── investigations/                    # One writeup per scenario or platform challenge
├── playbooks/                         # Triage and incident-response playbooks
├── hunts/                             # Threat hunt plans and reports
├── scripts/                           # Python, Bash and PowerShell tools
├── datasets/                          # Links only; no third-party data
├── diagrams/                          # Architecture and flow diagrams
├── templates/                         # Blank templates for lessons, rules, writeups
├── daily-log/                         # Short dated entries
├── reports/                           # Capstone and mock SOC reports
│
├── notes/
│   ├── glossary.md                    # Every term, defined when I first met it
│   ├── cheatsheets/                   # Windows event IDs, SPL, Wazuh rules, ...
│   └── mistakes-log.md                # Errors I hit and how I fixed them
│
├── resources/
│   └── links.md                       # Reading lists, references, tool docs
│
└── .gitignore                         # Excludes credentials, API keys, raw logs, VM exports
```

Each lesson file under `docs/` follows the same shape: **Objectives → Theory → Lab → Result → Troubleshooting → Takeaways**, so the whole repo reads consistently module to module. Every detection rule carries its MITRE ATT&CK mapping in its header.

## Progress tracker

| # | Module | Status |
|---|---|---|
| 0 | Ethics, Scope, Lab Foundation & Repo Setup | ⬜ Not started |
| 1 | Security & SOC Fundamentals | ⬜ Not started |
| 2 | Networking for Defenders | ⬜ Not started |
| 3 | Linux for Defenders | ⬜ Not started |
| 4 | Windows for Defenders | ⬜ Not started |
| 5 | Log Fundamentals | ⬜ Not started |
| 6 | SIEM Concepts & Wazuh Deployment | ⬜ Not started |
| 7 | Endpoint Telemetry | ⬜ Not started |
| 8 | Searching & Querying | ⬜ Not started |
| 9 | Detection Engineering I: Wazuh Rules & Decoders | ⬜ Not started |
| 10 | Detection Engineering II: Sigma, Tuning & Detection-as-Code | ⬜ Not started |
| 11 | Alerting, Triage & Investigation Workflow | ⬜ Not started |
| 12 | Network Security Monitoring | ⬜ Not started |
| 13 | Threat Intelligence & IOCs | ⬜ Not started |
| 14 | Attack-to-Detection Labs | ⬜ Not started |
| 15 | Incident Response, Playbooks, Phishing & Malware Triage | ⬜ Not started |
| 16 | Threat Hunting | ⬜ Not started |
| 17 | Scripting for Defenders | ⬜ Not started |
| 18 | Identity, Active Directory & Cloud Log Analysis | ⬜ Not started |
| 19 | Practice Platforms & Public Datasets | ⬜ Not started |
| 20 | SOC Operations, Metrics & Compliance | ⬜ Not started |
| 21 | Capstone Project | ⬜ Not started |

Legend: ⬜ Not started · 🟨 In progress · ✅ Complete

See [`learning-plan.md`](./learning-plan.md) for what each module actually covers, and [`repository-plan.md`](./repository-plan.md) for how the repo is organised.

## Rules I'm holding myself to

1. Attack activity is generated only against machines I own and control, on the host-only network. Nothing is ever aimed at my home Wi-Fi or the internet.
2. Metasploitable2 stays single-homed (host-only only), with no second adapter, ever.
3. I snapshot VMs before installing agents, changing SIEM configuration, or running attack labs.
4. No real credentials, API keys, personal data, or home-network details get committed. IPs and hostnames get sanitised.
5. I only analyse public, intentionally-released datasets or logs from my own lab, and I treat any malware sample as lab-only, in an isolated snapshot.
6. The detection rules and queries I write are for my own lab, and I stay within the law and authorised scope.
7. I don't move to the next module until the current one's labs actually work and I understand why.

---
*This README gets updated as my environment or plan changes.*
