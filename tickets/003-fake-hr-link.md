# PHISH-003 · Trunks vs fake HR benefits

**Analyst:** Oscar Hernandez  
**Opened:** 2026-08-24 08:22 PT  
**Closed:** 2026-08-24 08:50 PT  
**Verdict:** True positive  
**ATT&CK:** [T1566.002 Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)  
**Scouter:** Future Trunks would have restored from backup. This Trunks has a phone and a benefits email.

## Two sentences for whoever is on lead

HR-looking benefits email pointed at `hr-capsule.example`, not our real HR site. Reporter is Trunks Briefs, intern, not sure he tapped it on his phone. I treated that as a click: reset the password, kill sessions, block the host.

## Reporter

- Name / role: Trunks Briefs, engineering intern (Capsule Corp, purple hair, too much confidence)
- When they reported it: 2026-08-24 08:22 PT
- What they did: **not sure** if they tapped the button. Did not think they typed a password. I do not take "I think I didn't" as proof on a phone, even a time-machine phone.

## Message

- Subject: `Action needed: 2026 benefits enrollment closes today`
- From: `Capsule Corp HR <hr@capsule-corp.example>`
- Return-Path: `bounce@notify-center.example`
- Reply-To: `benefits@hr-capsule.example`
- Date: 24 Aug 2026 14:58:44 +0000
- To: `trunks.briefs@capsule-corp.example`

### Auth results

```
Authentication-Results: mx.capsule-corp.example;
  spf=fail smtp.mailfrom=notify-center.example;
  dkim=neutral;
  dmarc=fail (p=quarantine) header.from=capsule-corp.example
```

The From line copies our real HR address. Auth does not back it up. Matching From text is not the same as owning the mailbox. Sword does not help.

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
| domain | `hr-capsule.example` | not Capsule Corp. Not the future either. |
| url | `https://hr-capsule.example/enroll/2026` | text only |
| ask | enroll in benefits today | fake login |

## What I think it is

Two versions, because he was not sure:

If he did not click: block the host, tell HR this was not them, search for copies, close.

If he clicked or might have: treat it as a password problem even if he "doesn't think" he typed one. Phones make that memory bad. Interns make it worse. Reset, kill sessions, watch for MFA spam.

I took the second path. I would rather reset Trunks than explain to Vegeta why his kid's mailbox is in someone else's hands. Kakarot offered to "sense" whether the link was evil. I used DMARC instead.

Still not BEC. The ask is a login, not a wire. 001 and 003 are the intern-queue fakes. 002 is the finance queue with the prince's name on it. Same ticket shape, different call.

## What I did

- [x] Block `benefits@hr-capsule.example`, `notify-center.example`, `hr-capsule.example`
- [x] Forced password reset for `trunks.briefs@capsule-corp.example` and revoked sessions
- [x] Told him to report any MFA he did not start (including "but it said Super Saiyan login")
- [x] Search last 48h for that subject (two other people including Kakarot; one unopened, Kakarot did not click this time; both notified)
- [x] Told real HR this was not their mail
- [x] Told Trunks in plain language: fake enrollment page, password changed, real HR is `hr.capsule-corp.example` only, do not time-travel this

## If I had 20 seconds with a lead

True positive fake HR page at hr-capsule.example. From spoofed real HR, DMARC fail. Trunks unsure about the click so I reset anyway. Host blocked. Two other inboxes notified. Kid's going to be fine. The domain is not.
