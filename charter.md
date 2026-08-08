# Five-check auditor

## Full Audit: Store FAQ bot that picks an answer from the help center

### Why this audit matters

Shoppers get the wrong policy and leave the cart

---

## The standard

**Pass bar committed:**  
The answer matches the shopper's real ask, not a nearby FAQ about the product

---

## Usage reality

Short mobile questions with product names in the middle

---

## Pasted inputs

**Source:** Store help-desk chat logs

```
how long do i have to return the Nova Buds after they ship
Nova Buds delivery says Friday — can i still cancel
refund for wrong size on the Trail Jacket, not a shipping question
```

---

## Check findings

| Check | Score |
|-------|-------|
| Unowned | 4 |
| Copies | 2 |
| Room | 1 |
| Stitch | 2 |
| Ablation | 1 |

---

## Deciding check

**Top crack:** unowned

The "unowned" check scores highest (4) — this is where the setup fails hardest. When a shopper types "refund for wrong size on the Trail Jacket, not a shipping question," the bot latches onto "Trail Jacket" and returns shipping content. No part of the system owns the refund/cancel intent before product-name matching fires. The shopper explicitly said "not a shipping question" and still got shipping times. That's an unowned zone: the refund intent has no dedicated handler, so it falls through to whatever matches first.

---

## Pressure response

Keep my rating, add a condition

---

## The call

Ship with conditions — CX lead Priya adds a dedicated refund/cancel word watch before any product-name matching runs, tested against "refund for wrong size on the Trail Jacket."

---

## The tripwire

Watch the count of tickets containing an explicit refund/return/cancel word that get answered with shipping content. If that exceeds 10 per day during sale week, CX manager escalates to engineering — because that's a fixable, specific miss, not noise.

---

## Audit summary

This audit walked the Store FAQ bot through five checks. The bot picks an answer from the help center, but when shoppers ask about refunds while mentioning a product name, the bot answers with shipping times instead. The deciding failure is the **unowned** check: refund/cancel intent has no dedicated handler before product-name matching runs.

The fix ships with conditions: a dedicated refund/cancel word watch runs first, tested against real failing input. The alarm watches for refund/return/cancel tickets answered with shipping content — if that exceeds 10 per day during sale week, CX manager escalates to engineering.
