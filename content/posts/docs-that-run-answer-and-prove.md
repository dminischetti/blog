---
title: "Docs That Run, Answer, and Prove: Turning Documentation into a Living Interface"
link: docs-that-run-answer-and-prove
date: 2025-04-27T00:00:00
description: "What happens when docs stop describing the product and start behaving like part of it."
tags: ["documentation", "dx", "testing", "ai"]
cover:
  image: "/blog/images/posts/docs-that-run-answer-and-prove.png"
---
I opened the integration guide to fix a minor typo and realized the steps didn’t work anymore.

Nothing dramatic, just drift. A renamed flag here, a new default there, a missing permission no one remembered to mention. The words were still pretty. The reality had moved on. That’s the quiet failure of static docs, they age in place while the system keeps walking.

So we tried something obvious we had somehow avoided: make the docs run.

Not just code blocks you can copy, but an actual surface where examples execute against a sandbox, return real responses, and fail loudly when the product changes. If the guide says “create a token, then call this endpoint,” the page itself creates the token, calls the endpoint, and shows the payload that came back today, not last quarter. If a deprecation lands, the doc breaks before a customer does. That pain is embarrassing exactly once, then it becomes a habit. You fix the product or the doc, you do it in the same PR, and move on.

The surprising part is what happens to trust. Running examples act like contract tests with manners. They turn documentation into a small CI surface. When the examples pass, you are not just reading a promise, you are watching it keep itself. When they fail, the doc raises its hand in public and says, I need attention. That honesty does more for developer experience than another paragraph of prose ever could.

We added notebooks next. Little guided scripts that you can fork and play with, half tutorial and half diagnostic. They carry state across cells, so the story holds together, and they double as support tools. “Run cell three, tell me what you get.” Suddenly the conversation is concrete. Less folklore, more evidence.

Then we layered in an answer engine. Plain language queries, yes, but with provenance. Every answer carries citations to the exact snippet and the product version it came from. If the engine cannot find a traceable source, it says “I don’t know,” and points you to the best place to test the claim instead. No confident guesses, no vibe-based help. Chunking turned out to matter more than the model did: write small, named blocks with one intent per block, and both the human and the AI stop getting lost.

Governance had to change to make this real. We made doc PRs fail if runnable examples fail. We wired the notebooks to a nightly job that exercises critical paths. We added light observability: which guides get stuck, which steps produce the most errors, which queries often end in “I don’t know.” It felt strange at first, like putting metrics on a library. Then it felt normal, because the docs were no longer a library, they were part of the product.

The payoff shows up in quiet ways. Fewer tickets that start with “I followed the guide and…” Faster time to first success for a brand new developer who is still figuring out where everything lives. Feature adoption that tracks reality instead of announcement energy. You do not need a dashboard to feel it. The team’s shoulders drop. Onboarding stops being a scavenger hunt.

We ran a small experiment to make sure we were not hallucinating. We took one integration that historically generated a steady trickle of support pings and rebuilt the guide as a living doc: embedded sandbox, notebook variant, and a citation-aware answer panel wired only to those blocks. For thirty days we tracked two numbers, support tickets about that flow and the time from page view to first successful call. Tickets dropped. Time shrank. The code did not change. The map finally matched the territory.

Static docs decay because they describe the past. Living docs survive because they participate in the present.

It turns out the best documentation does three simple things, it runs, it answers, it proves. And when it cannot, it admits it. That is all most of us wanted in the first place.
