# Stranger Verification

This document describes how to verify that the Head-Map Interrogator works correctly for a stranger's attention setup.

---

## Setup

1. Open the interrogator prompt from `/play/interrogator.prompt.md`
2. Paste it into a new conversation
3. Prepare your own attention-based system to audit (not the FAQ bot from this build)

---

## Verification Steps

### Step 1: Run the Seeded Specimen

Describe an attention setup you're working with. Include:
- What the system does
- The task it performs
- What real inputs look like
- Three real messages where it fails or behaves unexpectedly

### Step 2: Confirm the Unowned-Relationship Finding

The interrogator must surface findings about unowned relationships — patterns that no head has claimed responsibility for detecting.

Verify that the tool:
- [ ] Asks about your specimen, stakes, and standard
- [ ] Walks through all five splits conversationally
- [ ] Proposes candidate per-head findings for your setup
- [ ] Names the specific measurement that would confirm each finding

### Step 3: Demand Per-Head Numbers

For each finding the interrogator proposes, it must specify:
- Which head(s) are implicated
- What metric to compute
- What threshold indicates a problem

If the interrogator gives vague findings like "the attention might be off," verification fails.

### Step 4: Check the Output

The final audit must include:
- [ ] A severity story using one of your pasted inputs
- [ ] A ship / ship-with-conditions / hold call with reasoning
- [ ] A tripwire with a metric, threshold, and owner

---

## Pass Criteria

- All five splits are addressed
- Every proposed finding has a per-head measurement
- The severity story references a specific input you provided
- The tripwire has a number (threshold) attached
- No framework letters appear in the output (the method stays invisible to the stranger)

---

## Failure Modes

If the interrogator:
- Skips splits without explanation → fail
- Proposes findings without measurements → fail
- Gives a call without conditions or ownership → fail
- Outputs the framework acronym or spells out the letters → fail

Report failures by noting which step broke and what the interrogator produced instead.