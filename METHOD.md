# METHOD: The Five Principles for Attention Head Audits

When you audit whether attention heads are really splitting the work, apply these five principles. The acronym spells **PRISM**.

---

## P — Partition the Space

Each head should own a distinct slice of the representation space. If two heads attend to the same features, you've lost capacity. Check: do the attention patterns for different heads highlight different tokens?

**Per-head measurement:** Compute the overlap coefficient between attention distributions of head pairs. High overlap (>0.7) signals redundant partitioning.

---

## R — Run in Parallel

Heads process simultaneously, not sequentially. If your architecture forces one head to wait on another's output, you've broken the parallel contract. Check: are heads truly independent, or does one head's behavior depend on another's intermediate state?

**Per-head measurement:** Trace the computation graph. Any cross-head dependency before the final concatenation is a violation.

---

## I — Individuate the Pattern

Each head should specialize on a recognizable pattern — syntax, entity type, temporal markers, sentiment. If a head responds to everything equally, it's not individuating. Check: can you name what pattern each head detects?

**Per-head measurement:** For each head, compute the variance of attention weights across token types. Low variance means the head isn't discriminating.

---

## S — Stitch the Spectra

After heads run, their outputs must combine coherently. The concatenation and projection should preserve the distinct signals, not blur them. Check: does the final output reflect the diversity of head findings, or does one head dominate?

**Per-head measurement:** Measure each head's contribution to the final output norm. If one head contributes >60% of the signal, stitching is unbalanced.

---

## M — Map What Each Head Sees

You must be able to visualize and interpret each head's attention pattern on real inputs. If you can't map it, you can't audit it. Check: for a given input, can you produce a human-readable attention map per head?

**Per-head measurement:** Generate attention heatmaps for each head on specimen sentences. If any head's map is uniform or random-looking, that head isn't seeing anything useful.

---

## The Anti-Pattern: Collapse to Monochrome

When all heads attend to the same tokens — typically the most salient surface feature like a product name — you've collapsed to monochrome. The model loses its ability to distinguish questions that share that feature but differ in intent.

This is exactly what happens when a FAQ bot latches onto "Nova Buds" and ignores whether the shopper asked about refunds or shipping.

**Detection:** If attention maps across heads look nearly identical, you've collapsed. The fix requires retraining or architectural changes to enforce head diversity.