# Split Walk Prompt Pack

Five standalone prompts, one per split, each ending in the per-head measurement it demands. Usable in any chat model.

---

## Prompt 1: Room

```
You are auditing an attention setup for capacity issues.

The system under audit:
- Specimen: Store FAQ bot that picks an answer for shopper questions
- Stakes: Shoppers get the wrong policy and leave the cart
- Standard: If someone asks about refunds, the answer is about refunds — not shipping
- Reality: Short mobile questions with product names in the middle

Specimen sentences where it fails:
- how long do i have to return the Nova Buds after they ship
- Nova Buds delivery says Friday — can i still cancel
- refund for wrong size on the Trail Jacket, not a shipping question

Room split: Is there enough capacity in the attention heads for this task? Are the heads overloaded, trying to do too many things at once?

Walk through the specimen sentences. For each one, consider whether the heads have enough room to track both the product name and the question type simultaneously.

Propose a candidate finding about room capacity.

End with the per-head measurement that would confirm your finding:
→ What specific test would you run on individual heads to verify whether room is the problem?
```

---

## Prompt 2: Copies

```
You are auditing an attention setup for redundancy issues.

The system under audit:
- Specimen: Store FAQ bot that picks an answer for shopper questions
- Stakes: Shoppers get the wrong policy and leave the cart
- Standard: If someone asks about refunds, the answer is about refunds — not shipping
- Reality: Short mobile questions with product names in the middle

Specimen sentences where it fails:
- how long do i have to return the Nova Buds after they ship
- Nova Buds delivery says Friday — can i still cancel
- refund for wrong size on the Trail Jacket, not a shipping question

Copies split: Are multiple heads doing redundant work? Are several heads all latching onto the product name instead of dividing labor?

Walk through the specimen sentences. For each one, consider whether multiple heads might be duplicating effort on the same feature (like the product name) while ignoring other features (like the question type).

Propose a candidate finding about redundant heads.

End with the per-head measurement that would confirm your finding:
→ What specific test would you run to see if multiple heads are attending to the same tokens?
```

---

## Prompt 3: Unowned

```
You are auditing an attention setup for coverage gaps.

The system under audit:
- Specimen: Store FAQ bot that picks an answer for shopper questions
- Stakes: Shoppers get the wrong policy and leave the cart
- Standard: If someone asks about refunds, the answer is about refunds — not shipping
- Reality: Short mobile questions with product names in the middle

Specimen sentences where it fails:
- how long do i have to return the Nova Buds after they ship
- Nova Buds delivery says Friday — can i still cancel
- refund for wrong size on the Trail Jacket, not a shipping question

Unowned split: Is there a subtask no head is responsible for? Is question-type detection (refund vs. shipping vs. cancellation) falling through the cracks because no head owns it?

Walk through the specimen sentences. For each one, identify the subtasks that need to happen (detect question type, identify product, match to FAQ). Consider whether any subtask lacks a dedicated head.

Propose a candidate finding about unowned subtasks.

End with the per-head measurement that would confirm your finding:
→ What specific test would you run to see which head (if any) is responsible for question-type detection?
```

---

## Prompt 4: Stitch

```
You are auditing an attention setup for handoff issues.

The system under audit:
- Specimen: Store FAQ bot that picks an answer for shopper questions
- Stakes: Shoppers get the wrong policy and leave the cart
- Standard: If someone asks about refunds, the answer is about refunds — not shipping
- Reality: Short mobile questions with product names in the middle

Specimen sentences where it fails:
- how long do i have to return the Nova Buds after they ship
- Nova Buds delivery says Friday — can i still cancel
- refund for wrong size on the Trail Jacket, not a shipping question

Stitch split: Are heads handing off to each other correctly? Even if one head detects "refund" and another detects "Nova Buds," is the information being combined properly downstream?

Walk through the specimen sentences. For each one, trace how information would need to flow between heads to produce the correct answer. Consider where the handoff might be breaking.

Propose a candidate finding about stitch failures.

End with the per-head measurement that would confirm your finding:
→ What specific test would you run to see if information from the question-type head reaches the answer-selection head?
```

---

## Prompt 5: Ablation

```
You are auditing an attention setup for isolation issues.

The system under audit:
- Specimen: Store FAQ bot that picks an answer for shopper questions
- Stakes: Shoppers get the wrong policy and leave the cart
- Standard: If someone asks about refunds, the answer is about refunds — not shipping
- Reality: Short mobile questions with product names in the middle

Specimen sentences where it fails:
- how long do i have to return the Nova Buds after they ship
- Nova Buds delivery says Friday — can i still cancel
- refund for wrong size on the Trail Jacket, not a shipping question

Ablation split: If you disable one step, does the system still work? Can you run the ask-type classifier alone to see if it correctly identifies "refund" before product-name matching even starts?

This is the top crack identified in the builder's audit. The severity story:

Nobody has run the ask-type step alone on "refund for wrong size on the Trail Jacket, not a shipping question" to see if it correctly tags this as refund before product-name matching even starts. The team only sees the bot's final reply — so if the final answer is wrong, there's no way to tell whether ask-type detection failed or

Walk through the specimen sentences. For each one, consider what you would learn by running the ask-type classifier in isolation.

Propose a candidate finding about ablation.

End with the per-head measurement that would confirm your finding:
→ Run the ask-type classifier standalone on all three specimen sentences and log its raw output. Compare to the bot's final live answer on refund-tagged messages. Any disagreement rate above 10% means the bug is downstream of ask-type.
```

---

## Using This Pack

1. Copy any prompt into your chat model
2. The model will walk the split and propose a finding
3. The final line gives you the per-head measurement to run
4. Collect all five measurements to complete your audit

The ablation prompt (Prompt 5) is pre-loaded with the builder's severity story and tripwire threshold (10% disagreement rate) as a worked example.