# Capsule Corp — phishing triage playbook

**Queue:** Capsule Corp IT / junior SOC  
**Time box:** 30 minutes from report to verdict  
**Goal:** decide what the message is, contain what you can, write it so a lead does not have to re-investigate.

This is a defensive process for mail that staff already reported. It is not a guide for building or sending phishing.

## 0. Intake (2 min)

- Who reported it, when, and did they click, reply, or send money?
- Capture the original message as an `.eml` or full header copy. Do not forward as a new mail if you can avoid it (headers get rewritten).
- If they already wired funds or typed a password, skip ahead to containment. Analysis can wait five minutes. Harm cannot.

## 1. Identity of the message (5 min)

Read these before the body:

| Field | Why it matters |
|-------|----------------|
| `From` | Display name is cheap to fake. The address is the claim. |
| `Return-Path` / Envelope From | Who actually sent it. Often not the same as `From`. |
| `Reply-To` | Where a reply would go. BEC loves a mismatch here. |
| `Authentication-Results` | SPF, DKIM, DMARC as the mail gateway saw them. |
| `Message-ID` and `Received` chain | Rough path. Look for a first hop that does not match the claimed brand. |

High-level auth reading (no toolkit required):

- **SPF fail** — the sending server was not in the domain's allow list.
- **DKIM fail** — the signature does not check out (missing, broken, or wrong domain).
- **DMARC fail** — SPF/DKIM did not align with the visible `From` domain. Treat claimed brand mail with DMARC fail as hostile until proven otherwise.
- **DMARC pass** does not mean safe. A compromised real mailbox or a lookalike domain with its own valid auth can still pass.

Do not visit links from your own browser. If you need a URL check, use a sandbox or a URL scanner from the ticket, never the live site.

## 2. What kind of phish is this? (5 min)

Pick one primary bucket:

1. **Lookalike domain** — brand impersonation with a near-miss hostname (`capsu1e-corp.example` vs `capsule-corp.example`). Usually credential harvest or malware lure. Here we stop at the lure. We do not detonate files.
2. **BEC / invoice / wire change** — often no malware and sometimes no link. The ask is money, a W-2, or a vendor-bank change. The "payload" is the instruction.
3. **Fake internal portal** — HR, payroll, benefits, VPN, password reset. Link is the whole game.
4. **Benign** — newsletters, real vendor, user misread. Close it. Do not force a true positive.

## 3. Indicators (5 min)

Record, even if fictional in this lab:

- Sending address and any `Reply-To`
- Display name vs address
- Domain and whether it is a lookalike (homoglyph, dropped letter, extra TLD)
- URLs **as text** (do not click)
- Ask: reset password, pay invoice, open attachment, "verify payroll"
- User action: none / clicked / typed credentials / replied / paid

## 4. Verdict (3 min)

- **True positive** — hostile mail. Say which bucket.
- **Suspicious** — not enough to close, not enough to ignore. Escalate with what you still need.
- **False positive** — real sender, real ask, user got jumpy. Note why so the next analyst does not reopen it.

Write it in one sentence a lead can quote.

## 5. Containment (5–10 min)

Only what a junior analyst can actually request:

- Block the sending address and the sending domain on the mail gateway.
- If a lookalike or fake portal was involved, block the hostname at DNS/web proxy if you have that queue.
- If the user typed credentials: force password reset, kill sessions, tell them to report any MFA prompt they did not start.
- If money or bank details were in play: call finance on a **known** number, not a number in the email. Tell them to freeze the payment.
- Do not mass-email "we got phished" from a random mailbox. Use the comms path in the IR plan.

## 6. Escalate vs close

**Escalate now** if credentials were entered, money moved or is pending, an executive mailbox is involved, or you see the same lure in more than one inbox.

**Close** if verdict is clear, blocks are in, user is informed, and the ticket has evidence a lead can re-check without you.

## 7. Write the ticket (5 min)

Use [templates/ticket.md](../templates/ticket.md). Two sentences for the shift lead at the top. Evidence below. If they have to hunt through headers to find your verdict, the ticket is not done.
