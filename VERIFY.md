# Verification: Store FAQ bot that picks an answer from the help center

## Purpose

Confirm that the Five-check auditor surfaces the deciding-check finding ("unowned") and demands a numeric measurement for it when a stranger runs the seeded specimen.

---

## Stranger verification steps

### 1. Open /play with the specimen

Paste this setup description:

> Store FAQ bot that picks an answer from the help center
>
> **What breaks:** Shoppers get the wrong policy and leave the cart
>
> **Pass bar:** The answer matches the shopper's real ask, not a nearby FAQ about the product
>
> **Real inputs:** Short mobile questions with product names in the middle
>
> **Sample failing lines:**
> - how long do i have to return the Nova Buds after they ship
> - Nova Buds delivery says Friday — can i still cancel
> - refund for wrong size on the Trail Jacket, not a shipping question
>
> **Source:** Store help-desk chat logs

### 2. Confirm the tool walks all five checks

The auditor should score each check. Expected ratings from the builder's run:

| Check | Score |
|-------|-------|
| unowned | 4 |
| copies | 2 |
| room | 1 |
| stitch | 2 |
| ablation | 1 |

### 3. Verify the deciding-check finding surfaces

The tool must identify **unowned** as the top crack — the check that turns the call.

Look for the auditor to surface this finding explicitly and explain why "unowned" is the deciding factor (score of 4, highest severity).

### 4. Confirm a numeric measurement is demanded

The auditor must propose a specific, countable measurement to confirm the "unowned" finding — not a vague description.

Example of what passes: "Count tickets containing an explicit refund/return/cancel word that get answered with shipping content."

Example of what fails: "Check if the bot is working correctly."

### 5. Verify the call and tripwire appear

**Call:** Ship with conditions — CX lead Priya adds a dedicated refund/cancel word watch before any product-name matching runs, tested against "refund for wrong size on the Trail Jacket."

**Tripwire:** Watch the count of tickets containing an explicit refund/return/cancel word that get answered with shipping content. If that exceeds 10 per day during sale week, CX manager escalates to engineering — because that's a fixable, specific miss, not noise.

---

## Pass criteria

- [ ] All five checks are walked and scored
- [ ] "unowned" is surfaced as the deciding check
- [ ] A numeric measurement is demanded for the "unowned" finding
- [ ] The call includes conditions and an owner (Priya)
- [ ] The tripwire names a threshold (10 per day), a trigger condition, and an escalation owner (CX manager → engineering)

If any criterion fails, the auditor is not correctly applying the builder's discipline to the specimen.
