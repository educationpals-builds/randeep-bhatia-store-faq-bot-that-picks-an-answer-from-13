# Five-check auditor

Walk five checks on any setup where parts are supposed to split the work — and find out whether they actually do.

---

## What this is

A discipline for auditing multi-part setups before they fail in production. You paste a failing setup, walk five checks, and get back a scored audit with a severity story, a call, and a tripwire.

**Worked example:** Store FAQ bot that picks an answer from the help center

**Stakes:** Shoppers get the wrong policy and leave the cart

---

## Verdict

Ship with conditions — CX lead Priya adds a dedicated refund/cancel word watch before any product-name matching runs, tested against "refund for wrong size on the Trail Jacket."

---

## Tripwire

Watch the count of tickets containing an explicit refund/return/cancel word that get answered with shipping content. If that exceeds 10 per day during sale week, CX manager escalates to engineering — because that's a fixable, specific miss, not noise.

---

## One-paste rebuild

Copy the block below into a new audit when you need to run the same discipline on a different setup:

```
Setup: [name the tool and what it's supposed to do]
Stakes: [what breaks if the parts aren't really splitting the work]
Standard: [the pass bar — a concrete fail condition, not vibes]
Real inputs: [length, volume, mess of actual usage]
Sample lines: [five or more raw lines from the stream]
Source: [where those lines came from, and when]
```

Then walk the five checks. See [charter.md](charter.md) for the full audit on the store FAQ bot, including all five check scores, the deciding check, and the severity story.

---

## Sample asks

A stranger can paste any same-class failing setup:

- "Our support bot routes tickets to sales or tech, but it keeps sending billing complaints to tech because 'subscription' appears in the product name."
- "The triage classifier splits inbound emails into urgent/normal/spam, but anything mentioning 'CEO' goes to urgent even when it's newsletter spam."
- "Our document sorter files contracts vs. invoices, but purchase orders with contract language get misfiled half the time."

Each gets the same five-check walk, scored findings, severity story, call, and tripwire.

---

## Files

| File | Purpose |
|------|---------|
| [charter.md](charter.md) | Full audit for the store FAQ bot — standard, inputs, all five checks, severity story, call, tripwire |
| [METHOD.md](METHOD.md) | The five principles behind the checks |
| [VERIFY.md](VERIFY.md) | How a stranger confirms the audit works on their own setup |

<!-- educationpals-build-verified -->
