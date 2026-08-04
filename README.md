# The Head-Map Interrogator

An audit tool for checking whether attention heads are really splitting the work — built from a real FAQ-bot failure where shoppers asked about refunds and got shipping answers instead.

## The Specimen

**Store FAQ bot that picks an answer for shopper questions**

Shoppers get the wrong policy and leave the cart.

## The Verdict

**Hold.** Run the ask-type classifier standalone on all three specimen sentences and log its raw output before any ship decision — owner: ML engineer. Reopen the ship call once that isolated result exists.

## The Tripwire

Watch whether the standalone ask-type test, once run, disagrees with the bot's final live answer on refund-tagged messages. Any disagreement rate above 10% means the bug is downstream of ask-type, redirecting engineering effort — ML engineer owns this check.

## Standard

If someone asks about refunds, the answer is about refunds — not shipping.

---

## One-Paste Rebuild

Copy the prompt from `/play/interrogator.prompt.md` into your conversation. Describe your own attention setup, paste your own failing sentences, and the interrogator will walk you through the five splits, propose per-head findings with measurements, and return a scored audit with severity story, call, and tripwire.

---

**Method:** See [METHOD.md](METHOD.md) for the five-principle framework.

**Full Audit:** See [charter.md](charter.md) for the complete builder's run.

**Verification:** See [VERIFY.md](VERIFY.md) for stranger testing instructions.

<!-- educationpals-build-verified -->
