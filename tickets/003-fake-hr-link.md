# PHISH-003 · Fake HR benefits / payroll portal

**Queue:** Capsule Corp IT  
**Analyst:** cachuchablanco  
**Opened:** 2026-08-24 08:22 PT  
**Closed:** 2026-08-24 08:50 PT  
**Verdict:** True positive  
**ATT&CK:** [T1566.002 Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)

## Lead summary (two sentences)

HR-looking benefits enrollment mail pointed at `hr-capsule.example`, not Capsule's real HR host. Reporter is not sure they clicked. We treated it as clicked: password reset, sessions killed, fake host blocked.

## Reporter

- Name / role: C. Green, engineering intern
- Reported at: 2026-08-24 08:22 PT
- User action: **unsure** whether they tapped the button on a phone. Did not think they typed a password. We do not take "I think I didn't" as evidence.

## Message

- Subject: `Action needed: 2026 benefits enrollment closes today`
- Visible From: `Capsule Corp HR <hr@capsule-corp.example>`
- Return-Path: `bounce@notify-center.example`
- Reply-To: `benefits@hr-capsule.example`
- Date (header): 24 Aug 2026 14:58:44 +0000
- Recipients: `cgreen@capsule-corp.example`

### Auth results (as the gateway reported them)

```
Authentication-Results: mx.capsule-corp.example;
  spf=fail smtp.mailfrom=notify-center.example;
  dkim=neutral;
  dmarc=fail (p=quarantine) header.from=capsule-corp.example
```

The visible From **copies our real HR address**. Auth does not support it. That is the point of this ticket: a matching From string is not ownership of the mailbox.

### Header excerpt (relevant lines only)

```
From: Capsule Corp HR <hr@capsule-corp.example>
Reply-To: benefits@hr-capsule.example
Return-Path: <bounce@notify-center.example>
Received: from unknown (helo=notify-center.example)
```

Link in body (as text only): `https://hr-capsule.example/enroll/2026`  
Real Capsule HR (lab): `https://hr.capsule-corp.example`

## Indicators

| Type | Value | Notes |
|------|-------|-------|
| visible from | `hr@capsule-corp.example` | spoofed display of a real address |
| reply-to | `benefits@hr-capsule.example` | off-brand HR host |
| domain | `hr-capsule.example` | not a Capsule Corp property in this lab |
| url | `https://hr-capsule.example/enroll/2026` | recorded as text, not visited |
| ask | benefits enrollment / "closes today" | credential harvest on a fake SSO page |

## Analysis

Two branches, because the reporter was unsure:

**If they did not click.** Block the host, warn HR that enrollment mail does not come from `hr-capsule.example`, close after the search for siblings.

**If they clicked (or might have).** Treat as credential exposure even when they "don't think" they typed a password. Mobile WebViews make that memory unreliable. Reset the mailbox password, revoke sessions / refresh tokens, watch for MFA fatigue, tell them Capsule HR will not ask for a password through a random enroll page.

We took the second branch.

This is still not BEC. The ask is a login, not a wire. Combined with PHISH-001 it shows the intern queue (reset pages, HR portals). PHISH-002 is the finance queue. Different playbooks, same ticket shape.

## Actions

- [x] Block `benefits@hr-capsule.example`, `notify-center.example`, and host `hr-capsule.example`
- [x] Forced password reset for `cgreen@capsule-corp.example` and revoked active sessions
- [x] Asked the intern to report any MFA prompt they did not start
- [x] Search last 48h for subject "benefits enrollment closes" (two other recipients; one unopened, one did not click; both notified)
- [x] Told real HR (lab) that this was not their mail
- [x] User given a plain-language outcome: fake enrollment page, password rotated, real HR is `hr.capsule-corp.example` only
- [x] Escalated to: identity on-call (reset already done; no further action)

## What I would tell the shift lead

```
True positive fake HR enrollment page at hr-capsule.example. Visible From spoofed real HR, DMARC fail.
Reporter unsure if they clicked; we reset and killed sessions anyway. Host blocked. Two sibling inboxes notified.
```
