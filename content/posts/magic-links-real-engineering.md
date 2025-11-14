---
title: "Magic Links, Real Engineering: What I Learned Building Passwordless Auth From Scratch"
link: magic-links-real-engineering
date: 2025-11-12T00:00:00
tags: ["authentication", "security", "backend", "email", "magic-links", "passless"]
description: "Behind the simplicity of magic links lies a surprisingly deep engineering story - from token lifecycle and delivery reliability to production-grade observability."
cover:
  image: "/blog/images/posts/magic-links-real-engineering.png"
---
There’s a kind of elegance that makes every engineer pause -
a small idea that feels *too clean* to be true.

For me, that idea was the **magic link**.

One email, one click, no password.
Frictionless authentication. No OAuth dependency. No password reset tickets.
Just pure, minimal flow.

And because I can’t resist a good experiment, I decided to build my own implementation from scratch - no frameworks, no auth libraries, just PHP, MySQL, and email delivery pipelines.

I called it **[Passless](https://github.com/dminischetti/passless)**.
You can try a live demo at [lab.minischetti.org/passless](https://lab.minischetti.org/passless).

---

## 1. The Promise of Simplicity

Magic links sound *deceptively simple*.

You type your email.
A link arrives.
You click, and you’re in.

Under the hood, it’s straightforward:
- Generate a cryptographically secure token
- Store it with metadata
- Email it to the user
- Verify it on click

That’s it, right?

Not quite.

As soon as I started testing Passless in real conditions - real users, real devices, real email clients - the simplicity began to unravel.

---

## 2. The Real Problems Start After “Send”

Email delivery isn’t an afterthought. It’s *the* backbone.

You can write the cleanest authentication flow in the world, but if your email provider silently delays or bounces a message, your login system just broke - invisibly.

Then there’s Outlook’s **Safe Links** protection, which “tests” links automatically.
That means it *clicks* your token before your user ever sees it.
Result: your magic link is consumed by a robot, not a human.

I built mitigation layers in Passless:
- Reject `HEAD` requests (used by scanners)
- Rate-limit verification attempts by IP
- Require a real button click before session creation
- Detect early token consumption and auto-regenerate

It worked, but the deeper lesson was clear:
passwordless doesn’t mean effortless.

---

## 3. Email Clients Are Not Browsers

Another unexpected issue: **context switching**.

On iOS, when you click a link in the Mail app, it doesn’t open Safari - it opens a mini-browser *inside* Mail.
You sign in, it works, but then Safari has no session cookie, and the user says,
> “I clicked the link, but I’m still not logged in.”

That tiny UX detail becomes an architectural issue.

The fix?
Add deep link support, optional QR fallback, and a user-facing “copy link to browser” hint.

A 5-second frustration avoided - but only after hours of debugging across multiple devices.

---

## 4. Token Lifecycle Is a Minefield

On paper, a magic link token is just a string -
but it lives, expires, and dies like any other credential.

In Passless, each token is hashed (SHA-256) before storage, with metadata:
- user ID
- IP address
- user agent
- creation time
- expiration time
- verification timestamp

All verification happens against the hash, not the raw token.
Tokens are single-use and atomically invalidated - if two requests hit at the same time, one wins, one fails.

Without this level of care, concurrent verification could let an attacker authenticate *alongside* the real user.
Subtle, rare, but real.

---

## 5. The Database Never Sleeps

At scale, tokens accumulate fast.
A typical flow generates thousands of tokens per day - many never clicked, some expired, all cluttering storage.

So I learned to think like an ops engineer:
- Index by `token_hash`, `expires_at`, and `used`
- Run cleanup in small batches (`LIMIT 10000`)
- Monitor growth rate and failure patterns
- Partition by date for easy archival

A small project suddenly had real operational weight -
connection pooling, query optimization, even metrics dashboards.

Because when authentication breaks, *everything* breaks.

---

## 6. The Observability Rabbit Hole

I didn’t want Passless to be a black box.

So I added full observability:
- structured JSON logs
- Prometheus metrics
- click-to-verification latency histograms
- email delivery tracking
- alerting on failed verification spikes

Those graphs told a story:
delivery delays, scanner interference, user confusion, browser quirks.
Problems that no static analysis could ever reveal.

The system didn’t just authenticate users - it taught me how real-world infrastructure behaves under pressure.

---

## 7. The Lesson Behind the Links

After months of refinement, Passless became stable, predictable, and production-grade.
It’s small, framework-free, and deliberately transparent - designed to run anywhere, from shared hosting to containers.

But the real takeaway wasn’t the code.
It was the reminder that **simplicity is an illusion earned through complexity**.

Magic links look effortless to the user because someone did the hard work to make them that way.

They’re not “magic.”
They’re craft.

---

## 8. Why It Matters

We talk a lot about elegant abstractions in engineering - clean APIs, frictionless UX, “serverless” everything.
But every layer of simplicity hides layers of invisible maintenance, retries, logs, metrics, and human care.

That’s not a flaw. That’s the design.

Magic links just make that tradeoff visible:
You remove passwords, and gain email delivery, verification race conditions, and operational risk instead.

And if you’re a backend developer, that’s not discouraging -
that’s where the real engineering begins.

---

## Closing Thought

I built [Passless](https://github.com/dminischetti/passless) because I wanted to understand the problem, not abstract it away.
And I learned that “passwordless” doesn’t mean “effortless.”
It means *you* become the password manager, the reliability engineer, the UX designer, and the security team - all at once.

And that’s the kind of complexity worth understanding.

PS: The “magic” part isn’t in the link.
It’s in the engineers who make it look that simple.
