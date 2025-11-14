---
title: "The Kindest Apps Are Local-First: CRDTs, Sync Anxiety, and Why SQLite Won 2025"
link: the-kindest-apps-are-local-first
date: 2024-12-14T00:00:00
description: "In 2025, building fast feels nice. Building local-first feels humane."
tags: ["local-first", "ux", "privacy", "crdts", "sqlite"]
cover:
  image: "/blog/images/posts/the-kindest-apps-are-local-first.png"
---
Latency isn't just a performance metric. It’s a posture.

A sub-100ms response says, *“I trust you.”* A spinning loader says, *“Hold on, we’re asking permission.”* And when you’re jotting a note, editing a task, or flipping a toggle on a flaky train ride, that trust, or lack of it, quietly shapes how you feel about the app, and about yourself.

That’s why I’ve started thinking of **local-first** not as a feature, but as a form of kindness.

It means you *can act immediately*. It means your data belongs to you *first*. And it means the server, when it shows up, is a collaborator, not a gatekeeper.

### CRDTs Without Panic

The heart of local-first is conflict resolution without drama. CRDTs (conflict-free replicated data types) sound intimidating, but the principle is simple: changes can arrive out of order and still resolve to the same truth.

Imagine two users editing a checklist offline. One deletes a task, the other marks it complete. When they sync, which one wins? If your app can't answer that, or worse, if *users* have to, you're asking them to manage state your system should handle.

CRDTs take that burden. They’re not magic, but they help turn sync anxiety into sync *confidence*.

### Privacy Isn’t a Checkbox

Local-first also reshapes the privacy model. What lives on the device stays there. You can compute summaries, analytics, or derived state on the server, but the core data, the raw story of the user’s activity, never needs to leave their hands.

It’s the difference between storing a diary and analyzing sentiment. The difference between trust and telemetry.

### The Operational Playbook

Of course, local-first isn’t easy. Schema changes require migration *everywhere*. You need **merge audits**, diffs that show how a sync resolved conflicts. You need **replay tooling** to test recovery paths. You need visibility into who wrote what, when, and why it resolved the way it did.

And when things go wrong (because they will), you need to *debug timelines*, not just logs.

SQLite has quietly become the backbone of this world. It’s everywhere, in browsers, in apps, in edge functions. Paired with CRDT-backed sync layers, it gives you durability *and* speed, without reaching for a server unless you really need one.

### Where It Breaks

It’s not a silver bullet.

Media blobs still need cloud storage. Encryption keys need secure coordination. And team-level analytics, anything that depends on a shared global state, gets complicated fast. You can’t just “merge” everyone’s metrics and expect the graph to be meaningful.

But if your app’s core loop is *personal*, local-first might be your sharpest, kindest tool.

### A Tiny Experiment

I built two versions of a notes app. One saved directly to Firestore (online-first). The other used local SQLite and CRDT sync on reconnect.

I measured:

- **Time to first action** (from app open to first keystroke)
- **Offline recovery success** (how many notes persisted through loss of network)

Local-first won both, by feel *and* by numbers. Users typed sooner. Changes synced without panic. I’ll share the repo and traces soon, but the takeaway is already clear:

---

We think latency is a UX issue.

But it’s also a dignity issue.

Every millisecond says something about who’s in charge, the user, or the infrastructure. And every design choice reflects that.

The kindest apps say, “You go first. I’ll catch up.”