# PHISH-001 · Kakarot vs lookalike password reset (it's a 1, not an L)

**Analyst:** Oscar Hernandez  
**Opened:** 2026-08-12 09:14 PT  
**Closed:** 2026-08-12 09:41 PT  
**Verdict:** True positive  
**ATT&CK:** [T1566.002 Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)  
**Scouter:** Power level of this domain: 2. It is a digit. Kakarot still forwarded it, which counts as a win.

## Two sentences for whoever is on lead

Kakarot got a "reset your Capsule mail or it locks in 30 minutes" email from `capsu1e-corp.example`. That is a fake Capsule domain (the letter L is a 1). He forwarded it instead of clicking. I blocked the sender and the host.

## Reporter

- Name / role: Kakarot, Capsule Corp field ops (hungry, trusting, named Kakarot)
- When they reported it: 2026-08-12 09:14 PT
- What they did: did not click, did not type a password, forwarded the original to IT. Do not let this go to his head.

## Message

- Subject: `Action required: Capsule Corp mailbox will lock in 30 minutes`
- From: `Capsule Corp IT <it-help@capsu1e-corp.example>`
- Return-Path: `bounce@mailer-prod.example`
- Reply-To: `it-help@capsu1e-corp.example`
- Date: 12 Aug 2026 16:02:11 +0000
- To: `kakarot@capsule-corp.example`

### Auth results

```
Authentication-Results: mx.capsule-corp.example;
  spf=fail smtp.mailfrom=mailer-prod.example;
  dkim=fail (no valid signature);
  dmarc=fail (p=quarantine) header.from=capsu1e-corp.example
```

### Header lines I actually used

```
From: Capsule Corp IT <it-help@capsu1e-corp.example>
Reply-To: it-help@capsu1e-corp.example
Return-Path: <bounce@mailer-prod.example>
Received: from unknown (helo=mailer-prod.example)
```

## What I pulled out

| Type | Value | Notes |
|------|-------|-------|
| sender | `it-help@capsu1e-corp.example` | digit 1 instead of L. Pride would never. Kakarot almost didn't notice either. |
| domain | `capsu1e-corp.example` | not `capsule-corp.example` |
| url | `https://mail.capsu1e-corp.example/reset` | wrote it down, did not open it |
| ask | reset your password now or you lock out | fake login page |

## What I think it is

It looks like our IT desk. It is not. One character in the domain is wrong, which is easy to miss on a phone. SPF, DKIM, and DMARC all fail. The envelope is `mailer-prod.example`, not Capsule.

Real Capsule IT does not misspell Capsule. Real Vegeta does not send "please reset or we lock you out in 30 minutes." Kakarot would send a 30 minute warning, but he would send it from `kakarot@capsule-corp.example` and also ask if you ate.

This is a fake reset page. Not BEC. Nobody asked for money. He forwarded instead of clicking, so I did not reset his password. I still blocked the domain in case the same mail hit Trunks next.

## What I did

- [x] Block `it-help@capsu1e-corp.example` and `capsu1e-corp.example` (Final Flash the domain)
- [x] Request block for `mail.capsu1e-corp.example`
- [x] Search last 24h for the same subject / domain (none in this lab)
- [ ] Password reset (not needed, do not tell him he is a prodigy)
- [ ] Finance (not a money email)
- [x] Told Kakarot: fake reset page, real IT will not send a 30 minute lockout from a lookalike domain, good job forwarding it, go eat

## If I had 20 seconds with a lead

True positive lookalike from capsu1e-corp.example. Kakarot forwarded, did not click. Sender and host blocked. No other hits today. It's a 1. It is not over 9000.
