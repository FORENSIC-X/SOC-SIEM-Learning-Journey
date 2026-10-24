# Learning Plan: SOC Analysis and SIEM for Defensive Security

This is the source of truth for my whole journey. I am learning SOC (Security Operations Centre) analysis and SIEM (Security Information and Event Management) from zero, using **only free tools**, inside a lab I own. Every theory topic is paired with a hands-on lab, and every lab produces something small and real that I can commit.

---

## 1. Ground Rules

- **Free only.** No paid tools, subscriptions or licences as a core dependency. Free-tier limits change, so I re-verify them in Module 0 and again whenever a lab depends on one.
- **Lab only.** I generate attack activity only against my own VMs on the host-only network. Nothing is ever aimed at my home Wi-Fi or the internet.
- **Snapshot first.** I snapshot before installing agents, changing SIEM configuration, or running attack labs.
- **Nothing sensitive in Git.** No credentials, API keys, personal data, home-network details or real logs from systems I do not own.
- **Ethics woven in.** Each module has an ethics and safety hook, not a single chapter at the start.
- **Install location.** All installations, VMs, ISOs and downloads go on the **D: drive**. C: stays untouched wherever Windows allows it (a few small driver and per-user config items can still land on C: and I note them when they do).

## 2. Module Structure (Every Module Follows This)

1. **Objectives**: what I can do at the end.
2. **Theory notes**: one Markdown file per sub-topic, with every term defined on first use.
3. **Labs**: step-by-step, with expected output, common errors and fixes.
4. **Deliverables**: the small artifacts I commit (queries, rules, writeups, cheatsheets, glossary entries).
5. **Recap, practice exercises and a 10-question quiz**.
6. **Self-check**: I only move on when I can explain the module's ideas without notes.

Lesson files use the headings: *Objectives, Theory, Lab, Result, Troubleshooting, Takeaways*.

## 3. Phases and Modules at a Glance

| Phase | # | Module | Est. study days* | VM profile |
|---|---|---|---|---|
| **A. Foundations** | 0 | Ethics, scope, lab foundation and repo setup | 4 | P0 |
| | 1 | Security and SOC fundamentals | 5 | P0 |
| | 2 | Networking for defenders | 7 | P1 |
| | 3 | Linux for defenders | 6 | P1 |
| | 4 | Windows for defenders | 6 | P3 |
| | 5 | Log fundamentals | 6 | P2 |
| **B. Platform and telemetry** | 6 | SIEM concepts and Wazuh deployment | 7 | P1 / P3 |
| | 7 | Endpoint telemetry (Sysmon, auditd, Wazuh agents) | 7 | P1 / P3 |
| | 8 | Searching and querying (Wazuh, KQL, Splunk SPL) | 8 | P1 / P5 |
| **C. Detection and analysis** | 9 | Detection engineering I: Wazuh rules and decoders | 6 | P1 / P3 |
| | 10 | Detection engineering II: Sigma, tuning, detection-as-code | 6 | P1 / P5 |
| | 11 | Alerting, triage and investigation workflow | 6 | P2 / P4 |
| **D. Network and intelligence** | 12 | Network security monitoring (Suricata, Zeek) | 8 | P6 |
| | 13 | Threat intelligence and IOCs | 5 | P1 |
| **E. Attack, response and hunting** | 14 | Attack-to-detection labs | 10 | P2 / P4 |
| | 15 | Incident response, playbooks, phishing and malware triage | 8 | P2 / P4 |
| | 16 | Threat hunting | 6 | P2 / P4 |
| **F. Automation and breadth** | 17 | Scripting for defenders (Python, Bash, PowerShell) | 8 | P1 |
| | 18 | Identity, Active Directory and cloud log analysis | 6 | P5 + datasets |
| | 19 | Practice platforms and public datasets | 10 | varies |
| **G. Operations and capstone** | 20 | SOC operations, metrics and compliance | 4 | P0 |
| | 21 | Capstone project | 10 | P4 |

\*Rough estimates at 2 to 3 hours a day. The total is about 150 days (around five months). Pace is mine to set, and depth beats speed.

## 4. Lab Resource Profiles (16 GB Host)

My laptop has an Intel Core i5-1335U (10 cores, 12 threads), 16 GB RAM and a single SSD split into C: and D:. Windows itself and my browser need roughly 4 to 5 GB, so I budget about **10 to 11 GB for running VMs**. That means I run **profiles**, not the whole lab at once.

| Profile | VMs running | Approx. VM RAM | Used for |
|---|---|---|---|
| **P0** | None or one small VM | under 2 GB | Theory, repo work |
| **P1** | Wazuh server (4 to 6 GB) + Ubuntu endpoint (1.5 GB) | 6 to 8 GB | Linux telemetry, Wazuh basics |
| **P2** | P1 + Kali (1 to 2 GB, headless is fine) | 8 to 10 GB | Linux attack-to-detection |
| **P3** | Wazuh server + Windows 11 evaluation VM (4 GB) | 8 to 10 GB | Windows telemetry |
| **P4** | P3 + Kali (1 to 2 GB) | 10 to 12 GB | Windows attack-to-detection (tight, measure first) |
| **P5** | Splunk VM (4 GB) + endpoint (1.5 GB), **Wazuh powered off** | 6 GB | Splunk SPL practice |
| **P6** | Ubuntu endpoint running Suricata and Zeek (about 3 GB) + Kali (1 to 2 GB) | about 4 to 5 GB | Network monitoring |
| **P7 (optional)** | MISP VM (4 GB), nothing else heavy | 4 to 5 GB | Only if space and RAM remain after the capstone |

Rules I follow: one SIEM at a time, suspend (save state) instead of shutting down, give each VM 2 vCPUs unless a lab needs more, and use headless installs wherever possible. If P4 does not fit when I measure it in Module 7, I will lower the Windows VM's footprint or run the Kali attack step separately and record that decision.

---

## 5. Modules

### Module 0: Ethics, Scope, Lab Foundation and Repo Setup
**Objectives:** I understand what I am and am not allowed to do, my lab foundation exists on D:, and my repo is live.
**Theory notes:**
- Authorisation and scope; rules of engagement; why "my own lab only" matters
- Law awareness: India's IT Act 2000 (unauthorised access provisions), DPDP Act 2023 (personal data in logs), CERT-In incident reporting directions (awareness only; I verify current text)
- Privacy of log data, PII in logs, responsible handling of samples
- Open-source licences versus free-tier licences (GPL, Apache 2.0, Splunk Free terms, evaluation-software limits)
- How to read vendor documentation; Git and GitHub hygiene (no-reply email, `.gitignore`, atomic commits)

**Labs:**
1. Record host specs (done) and free space on D:.
2. Set VirtualBox's default machine folder to D:, inventory my existing VMs, and move any that live on C: if I choose to.
3. Design and document the host-only network; confirm Kali's bridged adapter is off for lab work.
4. Practise snapshots (take, name, restore, delete) on an existing VM.
5. Create the repo skeleton, configure Git identity, push the first commits.
6. Verify current free-tier limits for Splunk Free, TryHackMe, CyberDefenders, BlueTeamLabs Online, LetsDefend and the Windows evaluation VM.

**Deliverables:** `README.md`, `.gitignore`, `lab-inventory.md`, network diagram, `free-tier-verification.md`, safety rules page, glossary seed.
**Ethics hook:** I write my own scope statement and commit it.

### Module 1: Security and SOC Fundamentals
**Objectives:** I can explain what a SOC does, who works in it, and how attacks and responses are described.
**Theory notes:**
- CIA triad, AAA, risk, threat, vulnerability, exploit; defence in depth
- Threat actor types and motivations; common attack categories
- SOC purpose, people, process, technology; tiers L1 to L3; specialist roles (hunter, detection engineer, IR, threat intel); in-house, MSSP and hybrid models
- Event versus alert versus incident; true and false positives and negatives
- Incident response lifecycle (NIST SP 800-61), Cyber Kill Chain, Diamond Model, Pyramid of Pain
- MITRE ATT&CK: tactics, techniques, sub-techniques, data sources, Navigator
- Frameworks overview: NIST CSF, CIS Controls, ISO 27001; CVE, CVSS, KEV, EPSS

**Labs:**
1. Explore ATT&CK Navigator and build a layer.
2. Map a public breach report to ATT&CK techniques.
3. Map the activities from my offensive repos (Metasploit, web app pentesting) to technique IDs.
4. Look up CVEs from my lab targets in NVD and read the CVSS vectors.

**Deliverables:** lesson notes, 40+ glossary terms, ATT&CK mapping of my offensive work, SOC roles one-pager.
**Ethics hook:** why ATT&CK describes behaviour, and how the same knowledge serves defenders and attackers.

### Module 2: Networking for Defenders
**Objectives:** I can read network traffic and explain what normal looks like.
**Theory notes:**
- OSI and TCP/IP models; Ethernet, MAC, ARP; IP addressing, subnetting and CIDR; private ranges and NAT; routing basics
- TCP (flags, handshake, teardown), UDP, ICMP; ports and services
- DNS (records, resolution, tunnelling and DGA concepts), DHCP
- HTTP (methods, status codes, headers), TLS (certificates, SNI, JA3/JA4 idea)
- SSH, SMB, RDP, LDAP, Kerberos ports; SMTP, IMAP and email headers
- Firewalls (stateful, ACLs), proxies, VPNs, IDS/IPS, segmentation
- VirtualBox network modes: NAT, host-only, internal, bridged

**Labs:**
1. `ping`, `traceroute`, `nslookup`/`dig`, `ip a`, `ss`.
2. Capture on the host-only network with `tcpdump` and open in Wireshark (read-only viewing; deeper Wireshark work stays in my offensive repo).
3. Identify the TCP handshake, a DNS lookup and an HTTP request to DVWA in a capture.
4. Run an Nmap scan from Kali against my Ubuntu VM and study the defender's view in the capture.
5. Subnetting exercises.

**Deliverables:** protocol and port cheatsheets, annotated small sanitised captures, subnetting notes.
**Ethics hook:** packet captures can hold credentials and personal data; I only capture my own lab.

### Module 3: Linux for Defenders
**Objectives:** I can investigate an Ubuntu host from the command line.
**Theory notes:**
- Filesystem layout, permissions, SUID/SGID, users, groups, sudo
- Processes, services and `systemd`, package management
- Bash essentials; text tools (`grep`, `awk`, `sed`, `cut`, `sort`, `uniq`, `jq`)
- `cron`, SSH configuration, `authorized_keys`, `journald`
- Where Linux persistence hides (cron, systemd units, shell profiles, SSH keys)

**Labs:**
1. Walk `/var/log` and read `auth.log` for my own logins.
2. Inspect process trees and listening ports.
3. Hunt persistence locations manually on a clean VM to learn the baseline.
4. Pipeline challenges: top 10 source IPs, failures per hour, `jq` on JSON logs.
5. Find SUID files and world-writable directories.

**Deliverables:** Linux triage cheatsheet, one-liner library, lesson notes.
**Ethics hook:** I only run investigation commands on machines I own.

### Module 4: Windows for Defenders
**Objectives:** I know what a normal Windows endpoint looks like so abnormal stands out.
**Theory notes:**
- User and kernel mode; processes, threads, services; the normal process tree (`smss`, `csrss`, `wininit`, `services`, `lsass`, `svchost`, `explorer`)
- Registry hives and autorun keys; NTFS and alternate data streams
- Users, groups, SIDs, privileges, UAC; logon types (2, 3, 10) and NTLM versus Kerberos basics; LSASS
- Active Directory fundamentals (domain, DC, OU, GPO), theory only for now
- PowerShell basics; scheduled tasks, WMI, services as persistence; LOLBins introduction
- Event log channels

**Labs:**
1. Download the Windows 11 evaluation ISO to D: and build the VM there; snapshot the clean state.
2. Install the Sysinternals Suite; explore Process Explorer, Autoruns, TCPView and Process Monitor.
3. Record a baseline: normal process tree, autoruns, listening ports.
4. Create a local user and a scheduled task, and observe what changes.
5. `Get-WinEvent` filtering by ID and time.

**Deliverables:** "what normal looks like" baseline notes, Windows triage cheatsheet, PowerShell snippets.
**Ethics hook:** the evaluation licence is for testing; I record its limits.

### Module 5: Log Fundamentals
**Objectives:** I can read, generate and parse the main log types.
**Theory notes:**
- Events, logs and telemetry; the log lifecycle (generate, collect, transport, parse, normalise, enrich, store, retain, dispose)
- Formats: syslog (RFC 3164/5424), JSON, CSV, CEF, LEEF, W3C, key=value, EVTX XML
- Time: UTC, NTP, timezone pitfalls
- Log source catalogue: OS, authentication, application, web, database, network devices, DNS, DHCP, proxy, EDR, cloud, email
- Linux logs: `auth.log`/`secure`, `syslog`, `journald`, `wtmp`/`btmp`, `auditd`
- Windows logs and key event IDs: 4624, 4625, 4634, 4648, 4672, 4688, 4720, 4732, 7045, 1102
- Web logs: Apache/Nginx combined format, status codes, user agents
- Integrity and tampering; privacy; volume and cost

**Labs:**
1. Generate failed SSH logins from Kali against Ubuntu and read the resulting `auth.log`.
2. Forward syslog with `rsyslog` between two VMs.
3. Generate 4625 failures on Windows and read them via Event Viewer and `Get-WinEvent`.
4. Analyse DVWA Apache logs with `grep` and `awk`.
5. Timestamp normalisation practice.

**Deliverables:** log source catalogue, Windows event ID cheatsheet, Linux log cheatsheet, regex basics notes.
**Ethics hook:** logs hold personal data; I keep them out of Git unless sanitised.

### Module 6: SIEM Concepts and Wazuh Deployment
**Objectives:** I can explain SIEM architecture and have a working Wazuh server with agents.
**Theory notes:**
- SIEM versus log management versus SOAR versus EDR versus XDR versus NDR
- SIEM pipeline: collect, normalise, correlate, alert, dashboard, retain; use cases
- Wazuh components: agent, manager, indexer, dashboard; ports and protocols; `ossec.conf`; rule levels
- Sizing, certificates and passwords, backup, hardening the SIEM itself

**Labs:**
1. Build an Ubuntu Server VM on D:, snapshot, then install Wazuh using the current official quickstart (I verify the version and requirements first).
2. Reach the dashboard from my Windows host over the host-only network; change default credentials.
3. Enrol the Ubuntu and Windows agents.
4. Tour the modules: Security events, Integrity monitoring, Vulnerability detection, MITRE ATT&CK, SCA.
5. Find alerts on disk (`alerts.json`), enable archives, create agent groups.

**Deliverables:** install walkthrough with screenshots, architecture diagram, dashboard tour, troubleshooting log.
**Ethics hook:** the SIEM now holds sensitive telemetry; I secure it as if it were production.

### Module 7: Endpoint Telemetry
**Objectives:** My endpoints send rich, useful telemetry to Wazuh.
**Theory notes:**
- Sysmon event IDs (1, 3, 7, 8, 10, 11, 12/13/14, 22, 23, 25) and configuration (SwiftOnSecurity, sysmon-modular)
- Windows advanced audit policy, command-line process auditing, PowerShell script block (4104) and module logging
- `auditd` rules, `ausearch`, `aureport`
- Wazuh collectors: `eventchannel`, `localfile`, syscheck (FIM, real-time, whodata), rootcheck, SCA, vulnerability detection, command monitoring, active response introduction

**Labs:**
1. Install Sysmon with a public community config; read and annotate it.
2. Turn on audit policy and PowerShell logging; ship Sysmon and Security channels to Wazuh.
3. Write `auditd` rules for sensitive files and commands on Ubuntu.
4. Configure FIM on a test directory and trigger changes.
5. Ingest DVWA web logs; forward Metasploitable2 syslog.
6. Measure RAM use of profiles P3 and P4 and record the result.

**Deliverables:** annotated Sysmon config, `auditd` rules, sanitised `ossec.conf` snippets, telemetry coverage table.
**Ethics hook:** endpoint telemetry can expose user activity; I define what I collect and why.

### Module 8: Searching and Querying
**Objectives:** I can answer security questions with queries in more than one language.
**Theory notes:**
- Wazuh dashboard Discover: DQL and Lucene; OpenSearch field names; visualisations; dashboards
- KQL concepts (practice on free learning resources)
- Splunk Free: installation, indexes, Universal Forwarder; SPL (`search`, `where`, `stats`, `eval`, `rex`, `table`, `dedup`, `timechart`, `top`/`rare`, lookups, subsearch, `transaction`)
- Query patterns: frequency, rarity, first and last seen, time bucketing, outliers, baselines
- Query performance and noise

**Labs:**
1. Build Wazuh saved searches for failed logins, new users, service creation and process creation.
2. Build a Wazuh dashboard for authentication activity.
3. Install Splunk Free (P5), forward Linux and Windows logs, and rebuild the same questions in SPL.
4. Practise on a public dataset.
5. Check current Splunk Free limits against my lab's volume.

**Deliverables:** one saved query per file in `detections/queries/`, a Wazuh dashboard export, SPL cheatsheet.
**Ethics hook:** query results can reveal individuals; I analyse only my lab or public datasets.

### Module 9: Detection Engineering I: Wazuh Rules and Decoders
**Objectives:** I can write, test and tune custom Wazuh detections.
**Theory notes:**
- Detection engineering lifecycle: idea, data check, rule, test, tune, document, review
- Wazuh decoders and rules (XML): `if_sid`, `frequency`, `timeframe`, `if_matched_sid`, `same_source_ip`, CDB lists, levels
- Regex and `OS_Match` patterns; `wazuh-logtest`
- ATT&CK tagging in rule metadata; false-positive thinking

**Labs:**
1. Write a custom decoder for a sample application log.
2. Rules for SSH brute force, new local admin, service installed, and suspicious PowerShell.
3. Test every rule in `wazuh-logtest` and with live events.
4. Tune one noisy rule with an exclusion and record the before and after.

**Deliverables:** one rule per file in `detections/wazuh/`, each with ATT&CK mapping in its header; a tuning writeup.
**Ethics hook:** detections that over-collect or over-alert have a human cost; I record that trade-off.

### Module 10: Detection Engineering II: Sigma, Tuning and Detection-as-Code
**Objectives:** I can write vendor-neutral rules and manage them like code.
**Theory notes:**
- Sigma structure: `logsource`, `detection`, selections, conditions, modifiers, levels, tags
- `sigma-cli` / pySigma backends; converting to Splunk and OpenSearch queries (I verify what the current Wazuh backend support looks like)
- Detection quality: precision, recall, coverage, ATT&CK coverage mapping
- Validation by running small, safe, well-understood lab simulations
- Version control for rules: reviews, changelogs, test cases

**Labs:**
1. Write Sigma rules for SSH brute force (T1110), scheduled task creation (T1053), encoded PowerShell (T1059.001), and new service (T1543.003).
2. Convert each to SPL and an OpenSearch query; compare results.
3. Validate each with a lab simulation and record true and false positives.
4. Build an ATT&CK Navigator coverage layer from my rules.

**Deliverables:** one rule per file in `detections/sigma/`, converted queries, coverage layer in `attack-coverage/`.
**Ethics hook:** simulations stay inside the lab and use only well-understood, benign-by-design techniques.

### Module 11: Alerting, Triage and Investigation Workflow
**Objectives:** I can take an alert from first look to documented decision.
**Theory notes:**
- Alert anatomy; severity versus priority; alert fatigue
- Triage questions (what, who, where, when, how, so what); the five Ws
- Enrichment and pivoting: host, user, process, network, file hash
- Timelines, true and false positive decisions, escalation criteria, case notes, handover
- Analyst mindset and cognitive bias; writing for a reader who was not there

**Labs:**
1. Trigger a set of alerts in the lab and triage each one using a fixed checklist.
2. Pivot from one alert to the related process, network and authentication events.
3. Build an incident timeline for one multi-alert scenario.
4. Write three investigation reports (one false positive, one true positive, one inconclusive).

**Deliverables:** triage checklist, investigation template, three writeups in `investigations/`.
**Ethics hook:** I record decisions honestly, including the ones I got wrong.

### Module 12: Network Security Monitoring (Suricata and Zeek)
**Objectives:** I can see and investigate network behaviour with free sensors.
**Theory notes:**
- NSM concepts: full packet, flow, metadata, alerts; sensor placement; VirtualBox promiscuous mode on host-only
- Suricata: rules syntax, Emerging Threats Open rules, `eve.json`, Wazuh integration
- Zeek: `conn`, `dns`, `http`, `ssl`, `files` logs; `zeek-cut`
- Behaviours to spot: scanning, beaconing, DNS tunnelling, unusual TLS, data exfiltration
- PCAP analysis workflow with `tshark` and Wireshark

**Labs:**
1. Install Suricata and Zeek on my existing Ubuntu VM (P6) and raise its RAM to about 3 GB for these labs; there is no separate sensor VM.
2. Generate scans and web attacks from Kali; examine Suricata alerts and Zeek logs for the same traffic.
3. Write two custom Suricata rules.
4. Forward `eve.json` into Wazuh.
5. Analyse a public training PCAP (this also covers traffic my Ubuntu-hosted sensor cannot see, such as attacks on the Windows VM).

**Deliverables:** sensor setup notes, custom Suricata rules, Zeek query cheatsheet, one network investigation.
**Ethics hook:** public PCAPs can contain live malware traffic; I only inspect them as data, never execute anything.

### Module 13: Threat Intelligence and IOCs
**Objectives:** I can enrich and use intelligence responsibly.
**Theory notes:**
- IOC types; Pyramid of Pain; TTPs; confidence and expiry; STIX/TAXII overview
- Free sources: VirusTotal, AbuseIPDB, AlienVault OTX, URLhaus (I verify API limits)
- MISP concepts (events, attributes, tagging, feeds) studied from its documentation; installing it is optional (P7)
- YARA basics; OSINT methods; ATT&CK-based intelligence

**Labs:**
1. Study MISP's data model from its documentation and sketch one event with attributes and tags on paper; I install it only if space remains at the end (P7, optional).
2. Enrich sample IPs and hashes with free APIs, storing keys outside the repo.
3. Import an IOC list from a free open feed (for example URLhaus) into a Wazuh CDB list and build a rule that uses it.
4. Write one simple YARA rule and test it on harmless sample files.

**Deliverables:** enrichment notes, IOC handling checklist, CDB-list rule, YARA rule.
**Ethics hook:** API keys never go into Git; sharing intelligence has handling rules (TLP).

### Module 14: Attack-to-Detection Labs
**Objectives:** For each attack, I can show the evidence, the detection and the gaps.
**Method per scenario:** snapshot, attack from Kali on the host-only network, collect evidence, find it in the SIEM, write or tune a rule, validate, document, restore snapshot.
**Scenarios:**
- Linux: SSH brute force (Hydra), port scan (Nmap), privilege escalation via a misconfiguration, cron persistence, `authorized_keys` backdoor
- Web: directory brute force, SQL injection and XSS against DVWA, with Apache log detections
- Legacy: Metasploit exploitation of Metasploitable2 observed through forwarded syslog and network sensors
- Windows: RDP/SMB brute force, scheduled task and Run-key persistence, encoded PowerShell, new local admin, log clearing (1102), LSASS access detection
- Lateral movement basics where my lab can safely support it

**Deliverables:** one writeup per scenario in `investigations/`, matching rules in `detections/`, a detection-gap table.
**Ethics hook:** every command shows its explicit host-only IP range; Kali's bridged adapter stays off.

### Module 15: Incident Response, Playbooks, Phishing and Malware Triage
**Objectives:** I can run a documented response and produce playbooks.
**Theory notes:**
- IR phases in practice; containment options; evidence handling and chain of custody; link to my Digital Forensics repo
- Playbooks versus runbooks; severity and escalation matrices
- Phishing analysis: headers, SPF, DKIM, DMARC, URL and attachment handling
- Malware triage basics: hashing, strings, static properties, free sandbox options (I verify limits)
- Wazuh active response; simple SOAR with Shuffle

**Labs:**
1. Write playbooks for brute force, phishing and suspicious PowerShell.
2. Analyse a sample phishing email's headers and links in a safe, isolated way.
3. Triage a harmless test file and a public sample hash without executing anything.
4. Build one Wazuh active-response action (block an IP in the lab) and test it.
5. Optionally automate one enrichment step in Shuffle.

**Deliverables:** three playbooks in `playbooks/`, a phishing analysis writeup, an active-response config.
**Ethics hook:** no live malware outside an isolated snapshot, and only if a module explicitly requires it.

### Module 16: Threat Hunting
**Objectives:** I can form a hypothesis and test it against my data.
**Theory notes:**
- Hunting versus alert-driven work; hypothesis-driven hunts; frameworks such as TaHiTI and PEAK
- Baselining, stacking and frequency analysis, long-tail analysis
- LOLBins, beaconing, rare parent-child processes, unusual persistence
- Turning a hunt into a detection; hunt reports

**Labs:**
1. Baseline my lab for a week, then hunt for deviations.
2. Hunt in Wazuh and Splunk for rare processes, odd logon times and periodic network connections.
3. Plant a benign "anomaly" in the lab and see whether I find it.

**Deliverables:** three hunt plans and reports, new detections born from hunts.
**Ethics hook:** hunting touches user behaviour; I stay inside my lab data.

### Module 17: Scripting for Defenders
**Objectives:** I can automate repetitive analysis safely.
**Theory notes:**
- Python for analysts: files, regex, JSON, `argparse`, `requests`, `pandas` basics
- Bash one-liners and small scripts; PowerShell for triage
- Wazuh API usage; rate limits and secret handling; testing small tools

**Labs:**
1. Parse `auth.log` and report brute-force sources.
2. Write an IOC enrichment script using free APIs (keys from environment variables).
3. Pull alerts via the Wazuh API and summarise them.
4. Build a small timeline-builder from mixed log files.

**Deliverables:** scripts in `scripts/` with a README each; no keys in code.
**Ethics hook:** my tools are for my lab data and respect API terms.

### Module 18: Identity, Active Directory and Cloud Log Analysis
**Objectives:** I can analyse identity-based attacks and cloud logs without paying for infrastructure.
**Theory notes:**
- Kerberos and NTLM attack patterns at a detection level; AD event IDs (4768, 4769, 4771, 4776, 4662)
- Cloud audit logs (AWS CloudTrail, Azure/Microsoft 365 audit concepts); common cloud detections
- How to practise without spending money: public datasets and sample logs only

**Labs:**
1. Analyse public EVTX attack samples and map findings to ATT&CK.
2. Analyse a public CloudTrail sample dataset in Splunk.
3. Write Sigma rules for two identity-based behaviours.

**Deliverables:** two investigation writeups, two Sigma rules.
**Ethics hook:** I use only public, intentionally-released datasets.

### Module 19: Practice Platforms and Public Datasets
**Objectives:** I test my skills against scenarios I did not build.
**Theory notes:** how to approach a challenge: scope, questions, evidence, answer, writeup, with spoiler discipline.
**Labs and work:** CyberDefenders, BlueTeamLabs Online, TryHackMe free rooms, LetsDefend free tier, Splunk BOTS datasets, and public PCAP and EVTX collections. I verify each platform's current free limits first.
**Deliverables:** one writeup per challenge, no flags or paid-content spoilers, and a running list in `resources/links.md`.
**Ethics hook:** I respect each platform's rules about sharing solutions.

### Module 20: SOC Operations, Metrics and Compliance
**Objectives:** I understand how a real SOC is run and measured.
**Theory notes:**
- Shift operations, handover, escalation, SLAs; metrics (MTTD, MTTR, false-positive rate, coverage)
- Alert tuning governance; detection coverage reviews; purple teaming
- Compliance awareness: PCI DSS, ISO 27001 logging controls, GDPR and India's DPDP Act, CERT-In reporting expectations (I verify current text)
- Reporting to technical and non-technical audiences

**Labs:** compute metrics from my own lab alerts; write a mock monthly SOC report.
**Deliverables:** metrics notes, a sample report, a detection coverage review.

### Module 21: Capstone Project
**Objectives:** I run an end-to-end defensive exercise and publish it.
**Project:** design a multi-stage attack chain in my lab; build detections beforehand; run it; investigate it from the first alert; contain it; write the incident report; update the playbooks; publish an ATT&CK coverage map and a dashboard export; reflect on gaps.
**Deliverables:** incident report, final detections, playbooks, dashboard export, coverage map, a polished README for the whole repo, and a lessons-learned entry.
**Career layer:** a short, honest note on which skills I can demonstrate and which free resources I will use next.

---

## 6. Progress Tracker

- [ ] 0 Ethics, scope, lab foundation and repo setup
- [ ] 1 Security and SOC fundamentals
- [ ] 2 Networking for defenders
- [ ] 3 Linux for defenders
- [ ] 4 Windows for defenders
- [ ] 5 Log fundamentals
- [ ] 6 SIEM concepts and Wazuh deployment
- [ ] 7 Endpoint telemetry
- [ ] 8 Searching and querying
- [ ] 9 Detection engineering I: Wazuh rules and decoders
- [ ] 10 Detection engineering II: Sigma, tuning, detection-as-code
- [ ] 11 Alerting, triage and investigation workflow
- [ ] 12 Network security monitoring
- [ ] 13 Threat intelligence and IOCs
- [ ] 14 Attack-to-detection labs
- [ ] 15 Incident response, playbooks, phishing and malware triage
- [ ] 16 Threat hunting
- [ ] 17 Scripting for defenders
- [ ] 18 Identity, Active Directory and cloud log analysis
- [ ] 19 Practice platforms and public datasets
- [ ] 20 SOC operations, metrics and compliance
- [ ] 21 Capstone project
