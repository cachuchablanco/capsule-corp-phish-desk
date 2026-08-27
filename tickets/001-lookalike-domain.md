# PHISH-001 · Lookalike password reset (it's a 1, not an L)

**Analyst:** Oscar Hernandez  
**Opened:** 2026-08-12 09:14 PT  
**Closed:** 2026-08-12 09:41 PT  
**Verdict:** True positive  
**ATT&CK:** [T1566.002 Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)  
**Scouter:** Power level of this domain: 2. It is a digit.

## Two sentences for whoever is on lead

Staff got a "reset your Capsule mail or it locks in 30 minutes" email from `capsu1e-corp.example`. That is a fake Capsule domain (the letter L is a 1). User did not click. I blocked the sender and the host.

## Reporter

- Name / role: Y. Satoshi, warehouse admin (not Vegeta, Vegeta does not file tickets, he yells)
- When they reported it: 2026-08-12 09:14 PT
- What they did: did not click, did not type a password

## Message

- Subject: `Action required: Capsule Corp mailbox will lock in 30 minutes`
- From: `Capsule Corp IT <it-help@capsu1e-corp.example>`
- Return-Path: `bounce@mailer-prod.example`
- Reply-To: `it-help@capsu1e-corp.example`
- Date: 12 Aug 2026 16:02:11 +0000
- To: `ysatoshi@capsule-corp.example`

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
| sender | `it-help@capsu1e-corp.example` | digit 1 instead of L. Pride would never. |
| domain | `capsu1e-corp.example` | not `capsule-corp.example` |
| url | `https://mail.capsu1e-corp.example/reset` | wrote it down, did not open it |
| ask | reset your password now or you lock out | fake login page |

## What I think it is

It looks like our IT desk. It is not. One character in the domain is wrong, which is easy to miss on a phone. SPF, DKIM, and DMARC all fail. The envelope is `mailer-prod.example`, not Capsule.

Real Capsule IT does not misspell Capsule. Real Vegeta does not send "please reset or we lock you out in 30 minutes." That is the whole personality test and it failed.

This is a fake reset page. Not BEC. Nobody asked for money. User said they did not click, so I did not reset their password. I still blocked the domain in case the same mail hit someone else.

## What I did

- [x] Block `it-help@capsu1e-corp.example` and `capsu1e-corp.example` (Final Flash the domain)
- [x] Request block for `mail.capsu1e-corp.example`
- [x] Search last 24h for the same subject / domain (none in this lab)
- [ ] Password reset (not needed)
- [ ] Finance (not a money email)
- [x] Told the user: fake reset page, real IT will not send a 30 minute lockout from a lookalike domain

## If I had 20 seconds with a lead

True positive lookalike from capsu1e-corp.example. User did not click. Sender and host blocked. No other hits today. It's a 1. It is not over 9000.
