# Phishing triage — 30 minutes

Not three days in the Hyperbolic Time Chamber. Thirty minutes, then a verdict.

This is for mail someone already reported. It is not a guide for sending phish.

## First: did they already get hurt?

If they typed a password, replied with data, or sent money, contain that first. Analysis can wait five minutes. The damage cannot.

- Password typed → reset it, kill sessions, tell them to ignore MFA they did not start
- Money or bank change → call finance on a number you already have. Not a number in the email. "Vegeta approved it" is not approval.

## Then read the headers, not the story

The body is a story. The headers are who actually sent it. A Saiyan prince does not need a lookalike domain. A scammer does.

I always look at:

- **From** — the name is easy to fake. Check the address.
- **Return-Path** — who sent it. Often not the same as From.
- **Reply-To** — where a reply goes. BEC loves this.
- **Authentication-Results** — SPF / DKIM / DMARC as the mail server saw them

How I read auth without making it a research paper:

- SPF fail = this server was not allowed to send for that domain
- DKIM fail = the signature is missing or wrong
- DMARC fail = the visible From does not line up with SPF/DKIM. If it claims to be us and DMARC fails, I treat it as fake until I have a reason not to
- DMARC pass does not mean safe. A lookalike domain can pass for itself. A real mailbox can get stolen.

Do not click the link on your own computer. Write it down. If you need to check a URL, use a sandbox. I am not training my scouter by visiting the fake planet.

## What kind of email is this?

I pick one bucket so I do not mix up the response:

1. **Lookalike domain** — `capsu1e-corp` instead of `capsule-corp`. Usually a fake login. Pride would never misspell the company.
2. **BEC / invoice / wire change** — sometimes no malware and no link. The ask is money or a new bank account. Bonus points if they name-drop Vegeta so nobody questions it.
3. **Fake internal page** — HR, payroll, VPN, password reset. Interns (Trunks) tap these on a phone.
4. **Not phish** — real vendor, user got nervous. Close it and say why.

## Verdict

- **True positive** — bad mail. Say which bucket.
- **Suspicious** — I need a lead or more mail to compare.
- **False positive** — real sender. Write why so the next person does not reopen it.

One sentence a lead can quote. Then the evidence. Joke after that if you want. Joke is not the verdict.

## What I actually request

I am junior. I do not "own the gateway." I request:

- Block the sender and the sending domain (Final Flash the domain, in the ticket)
- Block the fake hostname if there is one
- Password reset if they typed anything
- Finance freeze if money is in play
- Do not send a blast "we got phished" from a mailbox that might be dirty

## Escalate vs close

Escalate if credentials went in, money moved or is pending, an executive mailbox is in it, or I see the same lure in more than one inbox.

Close when the verdict is clear, the blocks are in, the user got a plain answer, and someone else can read the ticket without me.

Write it on [templates/ticket.md](../templates/ticket.md). Two sentences at the top. Evidence under that. Scouter line is optional and one line.
