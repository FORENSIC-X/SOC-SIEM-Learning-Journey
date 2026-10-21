# Repository Plan: soc-siem-learning-journey

This is my plan for how this repository is organised, named, written and committed. The module content itself lives in `learning-plan.md`.

---

## 1. Repository Settings

| Setting | Choice |
|---|---|
| Name | `SOC-SIEM-Learning-Journey` |
| Visibility | **Public** (defensive work I want to show) |
| Default branch | `main` (all work lands here so it counts on the contribution graph) |
| Description | "My first-person documentation of learning SOC analysis and SIEM from scratch using only free tools. Wazuh as the core SIEM, plus Splunk Free, Sysmon, auditd, Suricata, Zeek and Sigma. Lab-only attacks from Kali are detected, investigated and documented: lessons, labs, detection rules, queries, playbooks, hunts and writeups." |
| Topics | `soc`, `siem`, `wazuh`, `blue-team`, `detection-engineering`, `sigma`, `splunk`, `threat-hunting`, `mitre-attack` |
| Licence | MIT for code, rules and queries; notes are mine to share with attribution |
| Not a fork | Forks do not count toward the contribution graph |

## 2. Directory Structure

```
soc-siem-learning-journey/
├── README.md                          # Overview, structure, progress tracker
├── learning-plan.md                   # Module-by-module plan (source of truth)
├── repository-plan.md                 # This file
├── progress.md                        # Detailed checklist, one line per sub-topic
├── LICENSE
├── SECURITY.md                        # Lab-only statement and what is never committed
├── .gitignore
├── .gitattributes
│
├── docs/                              # Lesson notes, first person, one file per sub-topic
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
├── labs/                              # Raw walkthroughs, command output, screenshots
│   └── <same module folder names>/
│       ├── <NN>-<lab-name>.md
│       └── assets/                    # Screenshots and sanitised output for that lab
│
├── lab-config/                        # Sanitised configs I actually used
│   ├── network/                       # Host-only design, diagram source
│   ├── wazuh/                         # ossec.conf snippets, agent group configs
│   ├── sysmon/                        # Annotated Sysmon config
│   ├── auditd/                        # Audit rules
│   ├── suricata-zeek/                 # Sensor configuration snippets
│   └── splunk/                        # Inputs and props snippets
│
├── detections/
│   ├── sigma/                         # One rule per file
│   ├── wazuh/                         # Custom rules and decoders, one per file
│   ├── suricata/                      # Custom Suricata rules
│   ├── yara/                          # Simple YARA rules
│   └── queries/
│       ├── splunk-spl/
│       ├── wazuh-opensearch/
│       └── kql/
│
├── attack-coverage/                   # ATT&CK Navigator layer JSON files, dated
│
├── investigations/                    # One writeup per scenario or platform challenge
│   ├── lab-scenarios/
│   └── platform-challenges/
│
├── playbooks/                         # Triage and incident-response playbooks
├── hunts/                             # Hunt plans and reports
├── scripts/                           # Python, Bash, PowerShell tools, each with README
├── datasets/                          # README with links only; no large or third-party data
├── diagrams/                          # Architecture and flow diagrams (source plus export)
├── templates/                         # Blank templates (see Section 4)
│
├── daily-log/                         # YYYY-MM-DD.md, short dated entries
├── reports/                           # Capstone report, monthly mock SOC reports
│
├── notes/
│   ├── glossary.md
│   ├── mistakes-log.md
│   └── cheatsheets/                   # One file per cheatsheet
│
└── resources/
    └── links.md
```

`context.md` is kept **locally only** and listed in `.gitignore`.

## 3. Naming Conventions

- Folders and files: lowercase, `kebab-case`.
- Lesson notes: `docs/<module-folder>/<NN>-<topic>.md` (for example `docs/05-log-fundamentals/03-windows-event-ids.md`).
- Labs: `labs/<module-folder>/<NN>-<lab-name>.md`.
- Detection rules: `<tool>-<behaviour>-<technique-id>.yml` (for example `sigma-ssh-brute-force-t1110.yml`).
- Queries: `<behaviour>.spl` / `.kql` / `.json` or `.txt` for OpenSearch.
- Investigations: `<YYYY-MM-DD>-<short-name>.md`.
- Daily log: `daily-log/YYYY-MM-DD.md`.
- Dates are ISO (`YYYY-MM-DD`), times in UTC unless stated.
- Screenshots: `<lab-number>-<what-it-shows>.png`.

## 4. Templates (stored in `templates/`)

**Lesson note**
```markdown
# <Topic>

## Objectives
## Theory
## Lab
## Result
## Troubleshooting
## Takeaways
```

**Detection rule header (works for any rule file)**
```text
Title:         
ID:            
Status:        draft | tested | tuned
Author:        me
Date:          YYYY-MM-DD
ATT&CK:        Tactic / Technique (Txxxx.xxx)
Data source:   (log source and event IDs)
Description:   
Test method:   (how I validated it in my lab)
False positives: 
Tuning notes:  
```

**Investigation writeup**
```markdown
# <Scenario or alert name>
- Date / data source / SIEM
## Summary
## Alert or question that started it
## Evidence
## Timeline
## Analysis
## Verdict (true positive | false positive | inconclusive)
## ATT&CK mapping
## Response or recommended action
## Detection improvements
## What I got wrong or missed
```

**Daily log entry**
```markdown
# YYYY-MM-DD
- Module / topic:
- What I did:
- What I learned:
- What broke and how I fixed it:
- Next:
```

**Mistakes-log entry**
```markdown
## YYYY-MM-DD: <short title>
- What happened:
- Cause:
- Fix:
- How I will avoid it:
```

**Glossary entry**
```markdown
**Term**: plain-English definition. First used in Module N.
```

## 5. `.gitignore` (starting point)

```gitignore
# Never committed
context.md
*.env
.env*
secrets/
*.key
*.pem
*.p12
api-keys*
credentials*

# Raw logs and captures from anything other than sanitised excerpts
raw-logs/
*.evtx
*.pcap
*.pcapng
*.log

# VM and disk artefacts
*.ova
*.ovf
*.vdi
*.vmdk
*.vbox
*.vbox-prev
*.iso
Snapshots/

# Samples and archives
*.exe
*.dll
*.zip
*.7z
*.rar
samples/

# OS and editor noise
.DS_Store
Thumbs.db
.vscode/
__pycache__/
*.pyc
```

If I do need a tiny sanitised capture or log excerpt, I rename it into an `assets/` folder with an explicit `git add -f` and note in the lab that I sanitised it.

## 6. Commit Conventions

- **Atomic:** one finished artifact per commit (one rule, one query, one lesson note, one cheatsheet entry).
- **Message style:** imperative, specific, no area prefix needed. Examples:
  - `Add Sigma rule: SSH brute force (T1110)`
  - `Add SPL query: failed logins by source IP`
  - `Add lesson note: TCP three-way handshake`
  - `Add lab: Wazuh agent enrolment on Ubuntu`
  - `Add investigation: DVWA SQL injection from Kali`
  - `Add glossary terms: IOC, TTP, triage`
  - `Fix typo in Wazuh install troubleshooting`
- **Genuine only:** no empty, placeholder or whitespace-only commits. A commit exists because a piece of work is complete.
- **Screenshots and lab output** are committed separately from the notes that reference them.
- **Identity:** `git config user.name` and `git config user.email` set to the address linked to the defensive GitHub profile (GitHub's no-reply address is fine). I check `git log -1 --format='%an <%ae>'` after the first commit.
- **Branch:** commit directly to `main`; no force-pushes.

### 6.1 A natural daily rhythm (10+ commits comes from the work, not from splitting hairs)

| Moment | Typical commits |
|---|---|
| Study session | 2 to 4 lesson notes, one per sub-topic |
| Lab session | Lab walkthrough, assets, config snippet, troubleshooting entry |
| Detection or query work | One rule or query per file, plus tuning notes |
| End of day | Glossary additions, cheatsheet entry, mistakes-log entry, daily-log entry, progress update |

## 7. Pre-Commit Sanitisation Checklist

Before every `git add`, I check:

1. No passwords, tokens, API keys, certificates or private keys (a free secret scanner such as `gitleaks` can run locally before pushing).
2. No real personal data, usernames from my real accounts, email addresses or phone numbers.
3. No home-network details: Wi-Fi name, public IP, router addresses, home hostnames.
4. No hardware identifiers: serial numbers, MAC addresses, product keys, device IDs.
5. Screenshots cropped so the taskbar, notification area, browser tabs and account names are not visible.
6. Windows paths like `C:\Users\<real-name>\` replaced with `C:\Users\<user>\`.
7. Logs come only from my own lab or from public, intentionally-released datasets.
8. No malware samples, and no raw captures that contain third-party data.

Host-only lab addresses (the `192.168.56.0/24` range VirtualBox uses by default) are generic and safe to publish.

## 8. README Plan

The root `README.md` contains: a short first-person introduction, the lab diagram, the module progress table (copied from `learning-plan.md`), a "how this repo is organised" section, a tool stack summary, the safety and scope statement, and a link to `notes/glossary.md`. I update the progress table whenever I finish a module.

## 9. Repo Setup Order (First Commits, Done in Module 0)

1. Add `README.md` (stub) and `LICENSE`.
2. Add `.gitignore` and `.gitattributes`.
3. Add `SECURITY.md` (lab-only statement).
4. Add `learning-plan.md` and `repository-plan.md`.
5. Create folders only when real content arrives; I do not commit empty placeholder folders.
6. Add `templates/` files, one per commit.
7. Add `notes/glossary.md` with the first terms, then `notes/mistakes-log.md`.
8. Add `progress.md`.
