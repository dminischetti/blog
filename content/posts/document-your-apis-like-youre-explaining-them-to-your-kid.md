---
title: "Document Your APIs Like You're Explaining Them to Your Kid"
link: document-your-apis-like-youre-explaining-them-to-your-kid
date: 2025-10-14T00:00:00
description: "A case for clarity over cleverness in API documentation, because your future self deserves kindness."
tags: ["api design", "documentation", "clarity", "developer life"]
---

“Why does it do that?”

She was seven, sitting next to me, asking about a box on my screen labeled `/getUserSettings`.

I was halfway through writing documentation for it, and instead of brushing her off, I tried to answer, out loud, like a bedtime story.

“Well, this little box lets other parts of the app ask, ‘What does this user like?’ And it answers with things like language, theme, and notification preferences.”

She nodded like it made sense. Then she asked, “What if it says something wrong?”

And just like that, I saw it differently.

I’d written a hundred API docs before. Always with the same tone: formal, technical, full of types and edge cases. But rarely clear. Rarely kind.

That moment flipped something. I realized I wasn’t writing for other engineers. I was writing for *people*. People who are tired. People who are new. People who will one day inherit this thing, long after I’ve forgotten why I built it this way.

So I tried something: I rewrote the docs like I was explaining them to my kid.

Plain language. Purpose first. “Here’s what this does.” Then, “Here’s what to watch out for.” Then, “Here’s what it assumes.”

No metaphors, no baby talk, just clarity. Respect.

And it felt... honest. Like I was finally documenting not just what the code *does*, but what it *means* to someone who needs it.

The irony? The more human I made it, the more useful it became. Fewer questions from teammates. Fewer mistakes. Less guessing. It even made me see flaws in the API itself, naming inconsistencies, hidden assumptions, because if I couldn’t explain it simply, maybe it wasn’t so simple after all.

That’s the magic of writing for understanding, not just reference.

We think documentation is about recording truth. But really, it’s about *sharing orientation*, making sure the next person feels safe, grounded, able to move without fear.

I still include all the specs. But now I start with something softer: a sentence or two that says, “Here’s what this does, and why.”

Because someday, someone’s going to open that file and ask, “Why does it do that?”

And I want to be the kind of developer who answers with kindness.