# All It Took Was Null

---

Sometimes you spend hours going deep — mapping every endpoint, tracing logic, building payloads that feel like art. And then sometimes, the thing that cracks it open is literally just `null`.

I'm writing this because I think the thought process matters more than the bug itself. The bug got fixed. But the thinking behind finding it? That applies everywhere.

---

## What I was looking at

Web app. UAT environment. The app used a third-party fraud detection API — pretty standard setup. User does something, the app sends data to the fraud service, fraud service returns a risk score, app decides whether to allow or block.

Simple enough on paper. But here's the thing — between the app and that fraud API, there's custom code. Someone on the dev team wrote that integration. And that someone probably wasn't thinking about what happens when things go sideways.

That in-between layer? That's where I always look first.

## How I think about third-party integrations

I don't care much about the third-party tool itself at first. What I care about is the **handoff** — the point where the app's data gets packaged up and sent to the external service. That's custom code. That's where devs make assumptions.

And devs make a lot of assumptions:

- Data coming through will always be in the right format
- The fraud service will always respond
- If something goes wrong, it won't really matter
- Edge cases are someone else's problem

Every single one of those is something I want to poke at.

So my first move is always: what's the trust model? The app trusts the fraud service to make decisions. Cool. But does anyone check that the *data going to the fraud service* is actually valid? What happens if it's not?

What happens if it's just... nothing?

## Sending nothing

I watched the normal flow first. Observed what data the app was sending to the fraud API, what format, what fields. Standard recon.

Then I intercepted the request and swapped a key value with `null`.

No fancy payload. No chained exploit. Just null.

The integration code didn't validate it. Didn't check for it. Didn't handle it. The data went through, the fraud check basically didn't happen — or it returned something the app treated as "yeah, we're good."

Transaction went through. Fraud detection? Skipped.

That was it. One null. Done.

## Why the glue code is always the weak spot

Look — the fraud product itself is probably fine. These companies have entire teams dedicated to making their detection solid. ML models, global data, the works. I'm not questioning that.

The problem is never the product. The problem is the integration.

Think about how security attention gets distributed:

- The main app gets pentested, code reviewed, the whole treatment
- The third-party service has its own security certifications and all that
- The custom integration code that connects the two? Nobody touches it after it ships

It worked in dev. It worked in the happy path. Nobody ever asked "what if the input is null" because why would the input ever be null?

Because a pentester made it null. That's why.

## What to take from this

**If you build software:**

- Validate data before it leaves your app. Not just when it comes in — when it goes out to third parties too
- Handle null. Handle empty. Handle the types you didn't expect. If you're not explicitly checking, you're implicitly allowing
- If the fraud check fails or returns garbage, **deny by default**. Don't let it through just because something broke
- Test the integration layer specifically. Your app gets tested. The vendor's product gets tested. The glue between them? Test that too

**If you break software:**

- Always look at the seams between systems. That's where the interesting stuff lives
- Try the dumb stuff first. Null. Empty string. Zero. Wrong type. Before you reach for anything complex, just send garbage and see what happens
- When a system trusts another system, ask yourself why — and whether anyone actually verified that trust is deserved

## Last word

Companies spend serious money on fraud detection. Machine learning, behavioral analysis, threat intelligence — all legit. But it means nothing if the three lines of code that connect it to your app don't check for null.

The weakest link wasn't the fraud system. It was a missing validation in code nobody looked at twice.

All it took was null.

---

*No specific clients, products, or systems are referenced here. Everything is generalized. This is about the methodology, not the target.*
