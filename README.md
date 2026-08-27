# Capsule Corp Phish Desk

I want a junior SOC job. A lot of that job is email. So I wrote a 30 minute playbook and closed three fake Capsule Corp tickets the way I would if they landed on my queue.

Capsule Corp is Dragon Ball. I actually like DBZ. The work is still headers, lookalike domains, BEC, and a fake HR link. Fictional company. Real process.

**Oscar Hernandez** · [GitHub](https://github.com/cachuchablanco) · [LinkedIn](https://www.linkedin.com/in/oscar-hernandez-34355210a)

## Why I built this

I came from financial services. I have watched people get rushed into sending money because an email sounded urgent. Ticket 002 is that problem. 001 and 003 are the other half of the queue: fake login pages.

This is not a mail filter. It is me showing I can take a reported message, decide what it is, and write it down.

## Cases

| ID | What it is | Verdict | ATT&CK | Writeup |
|----|------------|---------|--------|---------|
| 001 | Lookalike password reset | True positive | T1566.002 | [tickets/001-lookalike-domain.md](tickets/001-lookalike-domain.md) |
| 002 | Vendor wire-change (BEC) | True positive | T1566 | [tickets/002-invoice-bec.md](tickets/002-invoice-bec.md) |
| 003 | Fake HR benefits link | True positive | T1566.002 | [tickets/003-fake-hr-link.md](tickets/003-fake-hr-link.md) |

Start with the [playbook](playbook/phishing-triage.md) if you want the process. Start with **002** if you only have two minutes. Blank form: [templates/ticket.md](templates/ticket.md)

## What this is not

No live phishing. No payloads. No "how to send this." Every domain is `*.example`. I did not visit the links. I wrote them down as text.
