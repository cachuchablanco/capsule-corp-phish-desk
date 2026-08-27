# Capsule Corp Phish Desk

I want a junior SOC job. A lot of that job is email. So I wrote a 30 minute playbook and closed three fake Capsule Corp tickets the way I would if they landed on my queue.

Capsule Corp is Dragon Ball. I am a Vegeta guy. The prince, Trunks, and Kakarot all work here (his name is Kakarot, I am not calling him the other thing). The tickets are still headers, lookalike domains, BEC, and a fake HR link. Fictional company. Real process. Occasional jokes. If the joke and the verdict ever fight, the verdict wins.

**Oscar Hernandez** · [GitHub](https://github.com/cachuchablanco) · [LinkedIn](https://www.linkedin.com/in/oscar-hernandez-34355210a)

## Why I built this

I came from financial services. I have watched people get rushed into sending money because an email sounded urgent. Ticket 002 is that problem, except the name they dropped is Vegeta. 001 is Kakarot vs a lookalike reset (he forwarded it instead of clicking, which is growth). 003 is Trunks, intern, phone, bad idea.

This is not a mail filter. It is me showing I can take a reported message, decide what it is, and write it down.

## Cases

| ID | What it is | Verdict | ATT&CK | Writeup |
|----|------------|---------|--------|---------|
| 001 | Kakarot vs lookalike password reset | True positive | T1566.002 | [tickets/001-lookalike-domain.md](tickets/001-lookalike-domain.md) |
| 002 | "Vegeta signed off" wire-change (BEC) | True positive | T1566 | [tickets/002-invoice-bec.md](tickets/002-invoice-bec.md) |
| 003 | Trunks vs fake HR benefits | True positive | T1566.002 | [tickets/003-fake-hr-link.md](tickets/003-fake-hr-link.md) |

Start with the [playbook](playbook/phishing-triage.md) if you want the process. Start with **002** if you only have two minutes. Blank form: [templates/ticket.md](templates/ticket.md)

## What this is not

No live phishing. No payloads. No "how to send this." Every domain is `*.example`. I did not visit the links. I wrote them down as text. Power level of this repo as a production SOC: 2. As an interview talk-track: higher.
