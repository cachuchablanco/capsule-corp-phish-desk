# PHISH-001 · Lookalike password reset for Capsule Corp mail

**Queue:** Capsule Corp IT  
**Analyst:** cachuchablanco  
**Opened:** 2026-08-12 09:14 PT  
**Closed:** 2026-08-12 09:41 PT  
**Verdict:** True positive  
**ATT&CK:** [T1566.002 Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)

## Lead summary (two sentences)

Staff got a "reset your Capsule mail password" message from `capsu1e-corp.example`, a one-character lookalike of `capsule-corp.example`. DMARC failed, Reply-To was off-domain, user did not click. Domain and sender are blocked.

## Reporter

- Name / role: Y. Satoshi, warehouse admin
- Reported at: 2026-08-12 09:14 PT (forwarded to IT, then pulled headers from the original)
- User action: did not click, did not type credentials

## Message

- Subject: `Action required: Capsule Corp mailbox will lock in 30 minutes`
- Visible From: `Capsule Corp IT <it-help@capsu1e-corp.example>`
- Return-Path: `bounce@mailer-prod.example`
- Reply-To: `it-help@capsu1e-corp.example`
- Date (header): 12 Aug 2026 16:02:11 +0000
- Recipients: `ysatoshi@capsule-corp.example`

### Auth results (as the gateway reported them)

```
Authentication-Results: mx.capsule-corp.example;
  spf=fail smtp.mailfrom=mailer-prod.example;
  dkim=fail (no valid signature);
  dmarc=fail (p=quarantine) header.from=capsu1e-corp.example
```

### Header excerpt (relevant lines only)

```
From: Capsule Corp IT <it-help@capsu1e-corp.example>
Reply-To: it-help@capsu1e-corp.example
Return-Path: <bounce@mailer-prod.example>
Received: from unknown (helo=mailer-prod.example)
Message-ID: <reset-88411@mailer-prod.example>
```

## Indicators

| Type | Value | Notes |
|------|-------|-------|
| sender | `it-help@capsu1e-corp.example` | digit `1` in place of `l` |
| domain | `capsu1e-corp.example` | lookalike of `capsule-corp.example` |
| url | `https://mail.capsu1e-corp.example/reset` | recorded as text, not visited |
| ask | password reset / "mailbox will lock" | urgency + credential harvest |

## Analysis

The display name is our IT desk. The domain is not. `capsu1e-corp.example` vs `capsule-corp.example` is a single-character swap that is easy to miss on a phone. Auth results do not support the claimed brand: SPF fail, DKIM fail, DMARC fail, and the envelope from is `mailer-prod.example`.

This is commodity credential phishing, not BEC. There is no payment ask and no executive sender. The lure is a fake password-reset page.

User reported they did not click. Residual risk is low for this mailbox. Residual risk for anyone who did click a sibling message is not known, so the domain still gets blocked org-wide.

## Actions

- [x] Block sender `it-help@capsu1e-corp.example` and domain `capsu1e-corp.example` on the mail gateway
- [x] Request DNS/proxy block for `mail.capsu1e-corp.example`
- [x] Search the last 24h for other recipients of the same subject / sending domain (none found in this lab)
- [ ] Password reset / session revoke (not needed: user did not click)
- [ ] Finance notified (not a payment case)
- [x] User told it was a fake reset page and that Capsule IT will not send lockout ultimatums from a lookalike domain
- [x] Escalated to: n/a (contained, no credential use)

## What I would tell the shift lead

```
True positive lookalike reset page from capsu1e-corp.example. User did not click.
Sender and hostname blocked. No sibling hits in the last day. No reset needed.
```
