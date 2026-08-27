# PHISH-003 · Fake HR benefits page

**Analyst:** Oscar Hernandez  
**Opened:** 2026-08-24 08:22 PT  
**Closed:** 2026-08-24 08:50 PT  
**Verdict:** True positive  
**ATT&CK:** [T1566.002 Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)

## Two sentences for whoever is on lead

HR-looking benefits email pointed at `hr-capsule.example`, not our real HR site. Reporter is not sure they tapped it on their phone. I treated that as a click: reset the password, kill sessions, block the host.

## Reporter

- Name / role: C. Green, engineering intern
- When they reported it: 2026-08-24 08:22 PT
- What they did: **not sure** if they tapped the button. Did not think they typed a password. I do not take "I think I didn't" as proof on a phone.

## Message

- Subject: `Action needed: 2026 benefits enrollment closes today`
- From: `Capsule Corp HR <hr@capsule-corp.example>`
- Return-Path: `bounce@notify-center.example`
- Reply-To: `benefits@hr-capsule.example`
- Date: 24 Aug 2026 14:58:44 +0000
- To: `cgreen@capsule-corp.example`

### Auth results

```
Authentication-Results: mx.capsule-corp.example;
  spf=fail smtp.mailfrom=notify-center.example;
  dkim=neutral;
  dmarc=fail (p=quarantine) header.from=capsule-corp.example
```

The From line copies our real HR address. Auth does not back it up. Matching From text is not the same as owning the mailbox.

### Header lines I actually used

```
From: Capsule Corp HR <hr@capsule-corp.example>
Reply-To: benefits@hr-capsule.example
Return-Path: <bounce@notify-center.example>
```

Link as text: `https://hr-capsule.example/enroll/2026`  
Real HR in this lab: `https://hr.capsule-corp.example`

## What I pulled out

| Type | Value | Notes |
|------|-------|-------|
| visible from | `hr@capsule-corp.example` | our address, spoofed |
| reply-to | `benefits@hr-capsule.example` | not us |
| domain | `hr-capsule.example` | not Capsule Corp |
| url | `https://hr-capsule.example/enroll/2026` | text only |
| ask | enroll in benefits today | fake login |

## What I think it is

Two versions, because they were not sure:

If they did not click: block the host, tell HR this was not them, search for copies, close.

If they clicked or might have: treat it as a password problem even if they "don't think" they typed one. Phones make that memory bad. Reset, kill sessions, watch for MFA spam.

I took the second path.

Still not BEC. The ask is a login, not a wire. 001 and 003 are the intern-queue fakes. 002 is the finance queue. Same ticket shape, different call.

## What I did

- [x] Block `benefits@hr-capsule.example`, `notify-center.example`, `hr-capsule.example`
- [x] Forced password reset for `cgreen@capsule-corp.example` and revoked sessions
- [x] Told them to report any MFA they did not start
- [x] Search last 48h for that subject (two other people; one unopened, one did not click; both notified)
- [x] Told real HR this was not their mail
- [x] Told the intern in plain language: fake enrollment page, password changed, real HR is `hr.capsule-corp.example` only

## If I had 20 seconds with a lead

True positive fake HR page at hr-capsule.example. From spoofed real HR, DMARC fail. User unsure about the click so I reset anyway. Host blocked. Two other inboxes notified.
