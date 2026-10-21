# Authorisation and Scope

## Objectives

- Explain what *authorisation* means in security work and why "I can" is not the same as "I'm allowed to".
- Write a scope statement for this repo and commit to it.
- Understand the ethics of being a defender who can see other people's data.

## Theory

**Authorisation** is explicit permission, from the person or organisation who owns a system, to do a specific thing to it. Knowing how to do something never grants permission to do it. A port scan is routine on my own lab and a potential offence on someone else's network.

**Scope** is the written list of what I am allowed to touch and what I am allowed to do to it. Anything not on the list is out of scope by default.

**Rules of engagement (RoE)** are the agreed limits inside the scope: which hosts, which times, which techniques, and who to tell if something breaks.

**Why a SOC analyst needs this as much as an attacker does.** A SOC analyst has a privileged view of an organisation: who logged in where, which sites people visited, what files were touched. That power comes with duties:

- **Need to know.** I look at data because an alert or a task requires it, not out of curiosity.
- **Minimum necessary.** I pull only the data I need to reach a decision.
- **Document my actions.** Another analyst should be able to see what I searched and why.
- **Don't tamper with evidence.** I never change or delete the data I'm investigating.
- **Report honestly,** including my own mistakes.

**Why my lab design reflects this.** My lab is built so that nothing can reach the home network or the internet by accident: host-only networking for lab traffic, NAT only for updates, Kali's bridged adapter off during attack labs, and Metasploitable2 single-homed.

## Lab

### My scope statement

*Written on: 2026-10-21*

**In scope (I own and control these):**

| Asset | Notes |
|---|---|
| Wazuh server VM | My SIEM |
| Windows endpoint VM | Evaluation VM, lab only |
| Ubuntu VM (DVWA, WebGoat, Juice Shop) | Monitored web/Linux endpoint |
| Kali Linux VM | Attack generator |
| Metasploitable2 VM | Optional legacy target |
| The host-only network between these VMs | VirtualBox host-only range; I confirm the exact range in Module 0 Part B |

**Out of scope, always:**

- My home Wi-Fi, router and every other device on my home network
- The internet and any public IP address
- Any machine, account or service that I do not own
- My school, employer, family or friends' systems and accounts
- Any real person's data

**Rules I follow:**

1. Attack activity only runs between my own VMs on the host-only network.
2. Kali's bridged adapter is off during attack labs. Metasploitable2 never gets a second adapter.
3. I snapshot before installing agents, changing SIEM config or running attacks.
4. If I am unsure whether something is in scope, it is out of scope until I have decided otherwise in writing.
5. I stop and note it in `notes/mistakes-log.md` if I realise I have touched something outside this list.

## Result

I have a written scope statement committed to this repo. I will copy the final version into `SECURITY.md` in Module 0 Part B.

## Troubleshooting

- **A lab step seems to need the internet** (for example, installing a package). I enable the NAT adapter for the update only, then disable it again before any attack activity.
- **I can't tell which network a VM is on.** I stop and check the VM's adapter settings before running anything. Module 0 Part B covers this check.

## Takeaways

- Permission comes from the owner, in writing, before the work starts.
- Scope is a list of what is allowed; everything else is off-limits.
- As a defender I will see sensitive data, so need-to-know and minimum-necessary are habits, not options.
