# Capsule Corp Phish Desk

Junior SOC phishing triage for a fictional Capsule Corporation helpdesk. One 30-minute playbook, three closed tickets, written the way a shift lead would want them.

The Dragon Ball names are the hook. The work is the job: headers, auth results, lookalike domains, BEC, verdict, containment.

**Author:** [cachuchablanco](https://github.com/cachuchablanco) · junior cybersecurity · SOC / analyst roles

## Why this exists

A large share of Tier-1 SOC work is email. Hiring managers do not need another scanner. They need to see that you can take a reported message, decide what it is, write it down, and tell a lead what happens next.

This repo is that queue.

## Skills this shows

- Email header reading (From vs Return-Path vs Reply-To)
- SPF / DKIM / DMARC results at a high level (pass vs fail vs alignment)
- Lookalike-domain spotting vs vendor/CEO business email compromise
- Ticket hygiene a lead can skim
- MITRE ATT&CK mapping (T1566 Phishing, T1566.002 Spearphishing Link)
- Containment recommendations (block, reset, notify finance) without running an attack

## Case board

| ID | Type | Verdict | ATT&CK | Writeup |
|----|------|---------|--------|---------|
| 001 | Lookalike login reset | True positive | T1566.002 | [tickets/001-lookalike-domain.md](tickets/001-lookalike-domain.md) |
| 002 | Vendor / CEO invoice (BEC) | True positive | T1566 | [tickets/002-invoice-bec.md](tickets/002-invoice-bec.md) |
| 003 | Fake HR / payroll portal | True positive | T1566.002 | [tickets/003-fake-hr-link.md](tickets/003-fake-hr-link.md) |

## How to read this (about two minutes)

1. This README (you are here).
2. Skim the three verdicts in the table.
3. If you want the process, open [playbook/phishing-triage.md](playbook/phishing-triage.md).
4. If you want evidence, open one ticket. 002 is the BEC case. It is a different problem than 001/003 on purpose.

Blank form: [templates/ticket.md](templates/ticket.md)

## What this is not

- Not a mail-filter product and not a live phishing kit
- Not red team and not instructions for sending phish
- All domains, headers, and staff names are fictional (`*.example` only)
- No malware, payloads, or exploit steps

Capsule Corp IT is a lab setting. The triage is the portfolio.
