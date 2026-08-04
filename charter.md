# Full Audit: Store FAQ Bot Head-Map Inspection

This is the builder's complete run — the worked example embedded in the interrogator.

---

## Specimen

**Store FAQ bot that picks an answer for shopper questions**

The bot uses multi-head attention to match shopper questions to FAQ entries. With d_model dimensions split across h heads, each head should attend to different aspects of the question — intent words, product references, temporal markers. When heads collapse onto the same signal (product name), the bot loses the ability to distinguish "refund for Nova Buds" from "shipping for Nova Buds."

### What goes wrong if this never gets fixed

Shoppers get the wrong policy and leave the cart.

---

## Standard

**If someone asks about refunds, the answer is about refunds — not shipping.**

---

## Real Inputs

Short mobile questions with product names in the middle.

### Pasted Sentence Set

1. how long do i have to return the Nova Buds after they ship
2. Nova Buds delivery says Friday — can i still cancel
3. refund for wrong size on the Trail Jacket, not a shipping question

**Source:** Store help-desk chat logs

---

## Five Split Findings

| Check | Rating | Notes |
|-------|--------|-------|
| Room (Partition the Space) | 0 | Not examined |
| Copies (Run in Parallel) | 0 | Not examined |
| Unowned (Individuate the Pattern) | 0 | Not examined |
| Stitch (Stitch the Spectra) | 0 | Not examined |
| Ablation | 4 | **Decider** — critical gap |

### Top Crack: Ablation

The ablation check is the decider for this audit.

---

## Severity Story

Nobody has run the ask-type step alone on "refund for wrong size on the Trail Jacket, not a shipping question" to see if it correctly tags this as refund before product-name matching even starts. The team only sees the bot's final reply — so if the final answer is wrong, there's no way to tell whether ask-type detection failed or

---

## The Call

**Hold.** Run the ask-type classifier standalone on all three specimen sentences and log its raw output before any ship decision — owner: ML engineer. Reopen the ship call once that isolated result exists.

---

## The Tripwire

Watch whether the standalone ask-type test, once run, disagrees with the bot's final live answer on refund-tagged messages. Any disagreement rate above 10% means the bug is downstream of ask-type, redirecting engineering effort — ML engineer owns this check.