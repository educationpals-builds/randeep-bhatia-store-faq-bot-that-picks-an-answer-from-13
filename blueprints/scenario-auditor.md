# Five-check auditor

One-paste spec for a conversational auditor that walks five checks on any failing setup, scores each, and returns a severity story, a call, and a tripwire.

---

## What this auditor does

A stranger describes a setup that's supposed to do one job but keeps failing. The auditor walks five checks, proposes findings with the measurement that would confirm each, and returns:

1. **Scored findings** — each check rated 1–5
2. **Severity story** — the deciding check and how it plays out on a real failing input
3. **Call** — ship, hold, or ship-with-conditions
4. **Tripwire** — an alarm with a trigger threshold and an owner who wakes when the dial drifts

---

## The five checks

| Check | What it asks |
|-------|--------------|
| **Unowned** | Is there a gap where no part of the setup takes responsibility? |
| **Copies** | Are multiple parts doing the same work, creating conflict? |
| **Room** | Does each part have enough context to do its job? |
| **Stitch** | Do the parts hand off cleanly, or do things fall between them? |
| **Ablation** | If you removed one part, would you notice? |

---

## Pass bar

The answer matches the shopper's real ask, not a nearby FAQ about the product

---

## Worked example

**Setup being audited:** Store FAQ bot that picks an answer from the help center

**Stakes:** Shoppers get the wrong policy and leave the cart

**Usage reality:** Short mobile questions with product names in the middle

**Source:** Store help-desk chat logs

### Failing inputs

```
how long do i have to return the Nova Buds after they ship
Nova Buds delivery says Friday — can i still cancel
refund for wrong size on the Trail Jacket, not a shipping question
```

### Scored findings

| Check | Rating | Finding |
|-------|--------|---------|
| Unowned | 4 | No part owns the distinction between "question about a product" and "question about a policy that mentions a product." The bot latches onto product names and ignores intent words like "return" or "cancel." |
| Copies | 2 | Minor overlap — product-name matching and FAQ retrieval both fire, but they don't directly conflict. |
| Room | 1 | The bot sees the full question; context isn't the issue. |
| Stitch | 2 | Handoff from intent detection to FAQ selection is loose — intent signals get overwritten by product-name matches. |
| Ablation | 1 | Every part is load-bearing; removing any breaks the flow entirely. |

### Top crack

**Unowned** — the highest-severity gap. No part owns the job of distinguishing policy questions from product questions when both appear in the same sentence.

### Severity story

When a shopper types "refund for wrong size on the Trail Jacket, not a shipping question," the bot sees "Trail Jacket" and returns shipping times. The shopper wanted the return policy. They leave the cart. CX gets a complaint ticket.

### Call

Ship with conditions — CX lead Priya adds a dedicated refund/cancel word watch before any product-name matching runs, tested against "refund for wrong size on the Trail Jacket."

### Tripwire

Watch the count of tickets containing an explicit refund/return/cancel word that get answered with shipping content. If that exceeds 10 per day during sale week, CX manager escalates to engineering — because that's a fixable, specific miss, not noise.

---

## Output shape

Every audit returns:

```
## Scored findings
[table: check | rating 1–5 | finding with measurement]

## Top crack
[the deciding check and why]

## Severity story
[a specific failure: real input → wrong output → who gets hurt]

## Call
[ship / hold / ship with conditions — conditions are checkable actions with owners]

## Tripwire
[cadence + threshold + owner who wakes when the dial drifts]
```

---

## How a stranger uses this

1. Paste a description of your failing setup: what it's supposed to do, who gets hurt when it fails, and a few real failing inputs.
2. The auditor walks all five checks, proposes a finding for each with the measurement that would confirm it.
3. You get back scored findings, the top crack, a severity story, a call, and a tripwire.

The auditor applies the same discipline the builder used on their own setup — now pointed at yours.
