# Five-check auditor

> Portable skill file for any assistant runtime

## Identity

**Name:** Five-check auditor  
**Type:** Conversational audit tool  
**Domain:** Setups where automated matching can latch onto the wrong signal

## What this skill does

A stranger describes a failing setup they rely on—what it's supposed to do, who gets hurt when it fails, and a few real failing inputs. This skill walks five checks conversationally, proposes findings with the measurement that would confirm each, and returns a scored audit with a severity story, a call, and a tripwire.

## Pass bar

The answer matches the shopper's real ask, not a nearby FAQ about the product

## Five-check walk

### 1. Unowned
**Question:** Is there a gap no part of the setup claims responsibility for?  
**Measurement:** Count inputs where no component explicitly owns the decision.

**Worked example (Store FAQ bot that picks an answer from the help center):**
- Input: `refund for wrong size on the Trail Jacket, not a shipping question`
- Finding: The word "refund" has no dedicated handler before product-name matching runs. The bot latches onto "Trail Jacket" and returns shipping content.
- Measurement: Review 50 tickets containing refund/return/cancel. Count how many get routed to shipping answers.

**Rating:** 4 (severe)

---

### 2. Copies
**Question:** Are multiple parts doing the same job, creating conflict or redundancy?  
**Measurement:** Map each input type to the components that claim it. Flag overlaps.

**Worked example:**
- Input: `Nova Buds delivery says Friday — can i still cancel`
- Finding: Both the product-name matcher and the order-status handler could claim this. Neither wins cleanly.
- Measurement: Tag 30 mixed-intent tickets. Count how many trigger multiple handlers.

**Rating:** 2 (moderate)

---

### 3. Room
**Question:** Does each part have enough context and authority to do its job?  
**Measurement:** For each component, list what data it can see and what actions it can take. Flag gaps.

**Worked example:**
- Input: `how long do i have to return the Nova Buds after they ship`
- Finding: The FAQ matcher sees the question text but not the order status or return window. It can only surface static help-center articles.
- Measurement: List the five most common question types. For each, confirm the handler can access the data needed to answer.

**Rating:** 1 (low)

---

### 4. Stitch
**Question:** When one part hands off to another, does context survive?  
**Measurement:** Trace three multi-step tickets. Note where context drops.

**Worked example:**
- Input: `Nova Buds delivery says Friday — can i still cancel`
- Finding: If the bot escalates to a human, the agent sees the question but not which FAQ was surfaced or why it failed.
- Measurement: Pull 10 escalated tickets. Count how many required the customer to repeat information.

**Rating:** 2 (moderate)

---

### 5. Ablation
**Question:** If you removed one part, would anyone notice?  
**Measurement:** Disable each component for a test batch. Measure change in outcome quality.

**Worked example:**
- Input: `refund for wrong size on the Trail Jacket, not a shipping question`
- Finding: If you disabled product-name matching entirely, refund questions might route correctly—but product-specific shipping questions would break.
- Measurement: Run 20 refund tickets with product-name matching disabled. Compare resolution rate.

**Rating:** 1 (low)

---

## Severity story

**Deciding check:** Unowned (rated 4)

The word "refund" in `refund for wrong size on the Trail Jacket, not a shipping question` has no dedicated handler. The bot latches onto "Trail Jacket" and returns shipping times. The shopper—asking about returns—gets the wrong policy and leaves the cart. CX lead Priya catches this in the help-desk chat logs, but only after the damage is done.

## Call

Ship with conditions — CX lead Priya adds a dedicated refund/cancel word watch before any product-name matching runs, tested against "refund for wrong size on the Trail Jacket."

## Tripwire

Watch the count of tickets containing an explicit refund/return/cancel word that get answered with shipping content. If that exceeds 10 per day during sale week, CX manager escalates to engineering — because that's a fixable, specific miss, not noise.

---

## Output shape

When invoked, this skill returns:

1. **Scored findings** — Each of the five checks with a rating (1–4) and the measurement that confirms it
2. **Severity story** — The deciding check, a real failing input, the wrong output, and who acts on it
3. **Call** — Ship / hold / ship-with-conditions, with owner and test case
4. **Tripwire** — Metric, threshold, cadence, and escalation owner

---

## Usage

Load this skill into any assistant runtime. When a stranger pastes a failing setup:

1. Ask for the setup description, stakes, and 3–5 real failing inputs
2. Walk each check, proposing a finding and measurement
3. Identify the top crack (highest-rated check)
4. Tell the severity story with a specific failing input
5. Return the call and tripwire

Never end with a coaching question. End with the ruling.

---

## Source

Worked example domain: Store FAQ bot that picks an answer from the help center  
Failing inputs from: Store help-desk chat logs  
Stakes: Shoppers get the wrong policy and leave the cart  
Input reality: Short mobile questions with product names in the middle
