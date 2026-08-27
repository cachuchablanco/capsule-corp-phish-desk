# PHISH-002 · Vendor wire-change / invoice BEC

**Queue:** Capsule Corp IT  
**Analyst:** cachuchablanco  
**Opened:** 2026-08-19 14:03 PT  
**Closed:** 2026-08-19 14:31 PT  
**Verdict:** True positive (BEC)  
**ATT&CK:** [T1566 Phishing](https://attack.mitre.org/techniques/T1566/) (spearphishing with a business ask; no malware required)

## Lead summary (two sentences)

Finance got a "updated bank details for West City Parts" invoice from an address that only looked like the vendor. This is BEC, not a drive-by link. Payment was not sent. Finance was called on the known vendor number. Sender blocked.

## Reporter

- Name / role: A. Brief, accounts payable
- Reported at: 2026-08-19 14:03 PT after the amount felt off
- User action: opened the mail, did **not** click, did **not** pay, did not reply

## Message

- Subject: `URGENT: West City Parts — updated remittance / PO 8842`
- Visible From: `West City Parts Billing <billing@west-city-parts.example>`
- Return-Path: `noreply@quick-invoice-mail.example`
- Reply-To: `accounts@westcity-parts-billing.example`
- Date (header): 19 Aug 2026 20:41:08 +0000
- Recipients: `ap@capsule-corp.example`
- Mentioned authority: "Bulma signed off, process today before close"

### Auth results (as the gateway reported them)

```
Authentication-Results: mx.capsule-corp.example;
  spf=pass smtp.mailfrom=quick-invoice-mail.example;
  dkim=pass (domain=quick-invoice-mail.example);
  dmarc=fail (p=none) header.from=west-city-parts.example
```

### Header excerpt (relevant lines only)

```
From: West City Parts Billing <billing@west-city-parts.example>
Reply-To: accounts@westcity-parts-billing.example
Return-Path: <noreply@quick-invoice-mail.example>
```

Body ask (paraphrased): "Our bank changed this morning. Do not use the account on file. Wire to the details below for PO 8842 so we can ship the gravity-training parts." No attachment in this case. A short URL was listed as "optional invoice PDF" and was **not** visited.

## Indicators

| Type | Value | Notes |
|------|-------|-------|
| sender | `billing@west-city-parts.example` | claimed vendor; not our recorded vendor domain |
| reply-to | `accounts@westcity-parts-billing.example` | hyphenation change; replies would leave the claimed domain |
| envelope | `quick-invoice-mail.example` | bulk-mail host, not the vendor |
| url | `https://files.quick-invoice-mail.example/po-8842` | recorded as text, not visited |
| ask | change remittance / wire today | classic BEC payment diversion |

## Analysis

This is a different problem than PHISH-001.

Auth is mixed on purpose: SPF and DKIM **pass** for the bulk-mail host, which is why a "green padlock" or "it authenticated" instinct is the wrong stop. DMARC **fails** because those passes do not align with `west-city-parts.example`. The visible From is a vendor costume. Reply-To is a second costume.

There is no need for malware. The loss happens if AP trusts the body and changes a bank account. Urgency, a named executive, and "use the new account" are the tells.

Capsule Corp's recorded vendor contact for West City Parts is `ar@westcityparts.example` (lab value) and a phone number in the vendor master file. That number was used. The vendor said they did not send this and their bank did not change.

User did not pay. No funds to recover. The remaining job is to stop the next AP clerk from paying a sibling mail.

## Actions

- [x] Block `billing@west-city-parts.example`, `accounts@westcity-parts-billing.example`, and `quick-invoice-mail.example` on the mail gateway
- [x] Search AP inboxes for "updated remittance" / PO 8842 / same Reply-To (none paid; one sibling unopened, quarantined)
- [x] Finance notified **out-of-band** on the known vendor number (not a number in the email)
- [x] Confirmed no payment was released on PO 8842 today
- [x] Asked AP to ignore bank-change requests that do not go through the vendor-master process
- [ ] Password reset (not a credential case)
- [x] Escalated to: finance controller (awareness only, no loss)

## What I would tell the shift lead

```
True positive BEC against AP: fake West City Parts bank-change, DMARC fail, Reply-To mismatch.
No payment sent. Vendor confirmed out-of-band. Sender domains blocked. One unopened sibling quarantined.
```
