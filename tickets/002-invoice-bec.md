# PHISH-002 · "Vegeta signed off" wire-change (BEC)

**Analyst:** Oscar Hernandez  
**Opened:** 2026-08-19 14:03 PT  
**Closed:** 2026-08-19 14:31 PT  
**Verdict:** True positive (BEC)  
**ATT&CK:** [T1566 Phishing](https://attack.mitre.org/techniques/T1566/) — business email compromise, no malware required  
**Scouter:** If it says the prince approved a bank change over email, the prince did not approve a bank change over email.

## Two sentences for whoever is on lead

AP got a "West City Parts changed banks, Vegeta signed off, wire PO 8842 today" email. That is BEC, not a virus. Nobody paid. I called the vendor on the number already in the file, not the number in the email.

## Reporter

- Name / role: A. Briefs, accounts payable
- When they reported it: 2026-08-19 14:03 PT (the amount felt off, and Vegeta does not "sign off" in AP's inbox)
- What they did: opened it, did not click, did not pay, did not reply

## Message

- Subject: `URGENT: West City Parts — updated remittance / PO 8842`
- From: `West City Parts Billing <billing@west-city-parts.example>`
- Return-Path: `noreply@quick-invoice-mail.example`
- Reply-To: `accounts@westcity-parts-billing.example`
- Date: 19 Aug 2026 20:41:08 +0000
- To: `ap@capsule-corp.example`
- Name-drop in the body: "Prince Vegeta signed off. Do not question it. Process today before close."

### Auth results

```
Authentication-Results: mx.capsule-corp.example;
  spf=pass smtp.mailfrom=quick-invoice-mail.example;
  dkim=pass (domain=quick-invoice-mail.example);
  dmarc=fail (p=none) header.from=west-city-parts.example
```

This one matters. SPF and DKIM **pass** for the bulk-mail host. That is why "it authenticated" is the wrong stop. DMARC **fails** because that pass is not aligned with the vendor domain they are pretending to be.

### Header lines I actually used

```
From: West City Parts Billing <billing@west-city-parts.example>
Reply-To: accounts@westcity-parts-billing.example
Return-Path: <noreply@quick-invoice-mail.example>
```

Ask in the body (paraphrased): bank changed this morning, do not use the account on file, Vegeta already approved it, wire the new details so the gravity-training parts ship. Optional PDF link. I did not open the link.

## What I pulled out

| Type | Value | Notes |
|------|-------|-------|
| sender | `billing@west-city-parts.example` | not the vendor domain we have on file |
| reply-to | `accounts@westcity-parts-billing.example` | extra hyphen. replies leave the claimed domain |
| envelope | `quick-invoice-mail.example` | bulk mail, not the vendor, not the prince |
| url | `https://files.quick-invoice-mail.example/po-8842` | text only |
| ask | new bank account, pay today, do not ask Vegeta | this is the whole attack |

## What I think it is

Different problem than 001.

I used to work in financial services. This is the email that actually empties an account. No malware. No fake login. Just urgency, a scary name, and "use the new bank." If AP trusts the body, the money is gone.

Two tells that are not even technical:

1. Real Vegeta would not politely email AP. He would be in the building.
2. Anyone who actually had Vegeta's approval does not need to write "do not question it" in the same paragraph as a new routing number. Also they would not CC Kakarot for "positive energy." There is no CC. I checked.

Our file says West City Parts is `ar@westcityparts.example` and has a phone number. I used that number. They did not send this. Their bank did not change. Vegeta did not sign off, which I know because I asked Briefs, not because I asked the email.

Reporter did not pay. Nothing to claw back. The job left is stop the next person in AP from paying a copy of this.

## What I did

- [x] Block `billing@west-city-parts.example`, `accounts@westcity-parts-billing.example`, `quick-invoice-mail.example`
- [x] Search AP for "updated remittance" / PO 8842 / "Vegeta signed off" (one unopened sibling, quarantined, nobody paid)
- [x] Called finance / vendor **out of band** on the known number
- [x] Confirmed PO 8842 was not paid today
- [x] Told AP: bank changes go through the vendor file, not through email, not through the prince's name
- [ ] Password reset (they did not type a password)
- [x] Flagged to the finance lead (awareness, no loss)

## If I had 20 seconds with a lead

True positive BEC on AP. Fake bank change, DMARC fail, Reply-To mismatch, Vegeta name-drop. No payment. Vendor confirmed by phone. Domains blocked. The prince's power level does not live in a Reply-To header.
