# The Five Checks: PRISM

When a setup claims to split work across multiple heads—retrieval, classification, generation—these five checks reveal whether the split is real or cosmetic.

---

## P — Partition the Space

Each head must own a distinct region of the input space. If two heads both activate on the same query type, the partition is broken.

**Test:** For a given input, can you name exactly one head that should handle it? If the answer is "both" or "it depends," the space isn't partitioned.

*Example from the Store FAQ bot:* When a shopper asks "refund for wrong size on the Trail Jacket, not a shipping question," does the refund-policy head own that query, or does the product-name matcher grab it first? If both claim it, the partition fails.

---

## R — Run in Parallel

Heads that don't depend on each other's output should run simultaneously. Serial chains where Head B waits for Head A create bottlenecks and hide which head actually decided.

**Test:** Draw the dependency graph. If Head B needs nothing from Head A but still waits, you've serialized what should be parallel.

*Example from the Store FAQ bot:* Product-name extraction and intent classification (refund vs. shipping) don't need each other's results. If intent classification waits for product extraction to finish, the bot may latch onto "Nova Buds" before it ever asks "what does this shopper actually want?"

---

## I — Individuate the Pattern

Each head must recognize its own pattern, not a blurred average of several patterns. A head that fires on "anything mentioning a product" isn't individuated—it's a catch-all.

**Test:** Can you write three inputs that should activate this head and three that shouldn't? If the boundary is fuzzy, the pattern isn't individuated.

*Example from the Store FAQ bot:* A head that triggers on "Nova Buds" regardless of whether the question is about delivery, returns, or cancellation hasn't individuated the pattern. It's matching a noun, not a need.

---

## S — Stitch the Spectra

When multiple heads produce partial answers, something must stitch them into a coherent response. If stitching is missing or naive (e.g., "just concatenate"), the output will contradict itself.

**Test:** Feed an input that activates two heads with conflicting implications. Does the output reconcile them or just mash them together?

*Example from the Store FAQ bot:* "Nova Buds delivery says Friday — can i still cancel" activates both delivery-status and cancellation-policy heads. If the bot answers with delivery info only because "delivery" appeared first, the stitch failed.

---

## M — Map What Each Head Sees

You must be able to inspect what each head "saw" in the input and why it fired (or didn't). If the heads are opaque, you can't debug misroutes.

**Test:** For a misclassified input, can you point to the exact feature that misled the head? If not, the map is missing.

*Example from the Store FAQ bot:* When "how long do i have to return the Nova Buds after they ship" gets answered with shipping times, can you see that the product-name head grabbed "Nova Buds" and the shipping head grabbed "ship" while the return-policy head never activated? Without that map, you're guessing.

---

## The Anti-Pattern: Collapse to Monochrome

When checks fail, teams often "fix" by collapsing everything into a single head—one giant prompt, one retrieval index, one classifier. This hides the split problem by eliminating the split.

**Why it's a trap:** The original split existed because the problem had real structure. Collapsing loses that structure. You trade visible misroutes for invisible ones buried in a monolithic black box.

**The tell:** After collapsing, accuracy on simple queries stays the same, but accuracy on edge cases (queries that need two kinds of knowledge) drops—and you can't see why.

---

## Using PRISM

Walk each check in order. Score each 1–5 based on how cleanly the setup passes. The lowest-scoring check is your deciding factor—fix that before touching anything else.

The audit doesn't tell you *how* to fix. It tells you *where* the split is broken and *what measurement* would confirm the fix worked.
