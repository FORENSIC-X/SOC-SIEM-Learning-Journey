# Law and Privacy Awareness

> I am not a lawyer. These are awareness notes for my own learning, not legal advice. I verify current text from official sources before relying on any detail.

## Objectives

- Know the main Indian laws that touch security testing and log data.
- Explain why log data counts as sensitive.
- Describe how I handle logs, captures and samples in this repo.

## Theory

### Unauthorised access

India's Information Technology Act, 2000 contains offences around accessing or damaging a computer system without permission (sections 43 and 66 are the ones I will read first). The practical lesson is the same as in lesson 01: attacking only my own lab is how I stay on the right side of this, and the scope statement is my evidence of intent.

### Personal data in logs

Logs contain personal data far more often than I would expect:

| Log field | Why it can identify someone |
|---|---|
| IP address | Can be linked to a person or household |
| Username, email | Directly identifies an account holder |
| Hostname | Often contains a person's name |
| URLs and search terms | Reveal behaviour and interests |
| File paths | Often contain a real name (`C:\Users\<name>\`) |
| Command lines | Can contain passwords typed as arguments |

### India's data protection law

The Digital Personal Data Protection Act, 2023 sets rules for processing digital personal data. The Rules under it were notified in November 2025, and the implementation is phased: some provisions started immediately, and most of the operational duties start about 18 months after publication (around May 2027). I check the official source for the current status before writing about it anywhere else.

Why a SOC cares: the SIEM collects the very data these laws protect, so a SOC has to secure it, limit who sees it, and plan retention and breach handling.

### Incident reporting and log retention directions

CERT-In (India's national cyber incident response team) has issued directions about reporting cyber incidents and keeping logs. I have not yet verified the current text, so I will read it from CERT-In's own site in Module 20 and record what it says.

### Responsible handling of data and samples

My rules for this repo:

1. I analyse only logs from my own lab and public, intentionally-released datasets.
2. I sanitise anything I commit (checklist in `repository-plan.md`).
3. I never execute malware samples. Anything that needs handling happens in an isolated snapshot and only when a module requires it.
4. I don't publish raw captures that contain third-party data.
5. API keys and credentials stay out of Git.

## Lab

1. Open a recent log from my Ubuntu VM (`/var/log/auth.log`) and list every field that could identify a person.
2. Write down which of those fields I would need to sanitise before committing an excerpt.

## Result

I can name at least five kinds of personal data that hide in ordinary logs, and I know which ones I redact before committing.

## Troubleshooting

- **I'm not sure whether a log excerpt is safe to commit.** I redact more than I think I need and re-read it once more before `git add`.

## Takeaways

- Attack only what I own; the scope statement is my safeguard.
- Logs are personal data, so a SIEM must be secured like a sensitive system.
- Legal details change, so I verify from official sources instead of trusting my notes.
