Use the attached **Attention Coach Horizontal Transfer Protocol — Minimal carrier × reference-computation revision** as the implementation specification for the existing Attention Coach app.

First inspect the current code and map:

* the four wrapper generators;
* wrapper metadata and public labels;
* protocol/controller states;
* flattening, recovery, return-strength and mixed-stability functions;
* held-out contamination logic;
* delayed re-check and status-assignment logic;
* trial logging and timing calculations;
* existing tests.

Then implement the smallest changes necessary to satisfy the specification.

## Constraints

1. Preserve the existing internal wrapper IDs:

```text
arrow_abs
flow_abs
arrow_rel
flow_rel
```

2. Do not replace or rewrite the evidence-gated controller where the current code already implements the required behaviour.

3. Preserve existing database records, saved user progression and protocol state wherever possible. Prefer additive metadata or compatibility mappings over destructive migrations.

4. Treat the four conditions as:

```text
arrow_abs = fixed-axis LEFT/RIGHT arrows
flow_abs  = fixed-axis LEFT/RIGHT local motion patches
arrow_rel = common-centre IN/OUT arrows
flow_rel  = common-centre IN/OUT local motion patches
```

5. Remove clockwise/anticlockwise and spiral conditions from this four-wrapper protocol route. They may remain elsewhere in the codebase but must not be generated under these four wrapper IDs.

6. Preserve the current:

```text
base stability gate
diagnostic flow probe
formal 20% transition probe
recovery gate
return-to-base test
progressive mixing
held-out flow_rel protection
full four-wrapper mixing
timing-quality checks
```

7. Do not allow session number alone to advance the protocol.

8. Do not allow accuracy above 82% to prevent progression when the other stability criteria pass.

9. During mixed fixed-axis/common-centre blocks, show a clear pre-stimulus rule cue:

```text
LEFT / RIGHT
```

or:

```text
IN / OUT
```

The rule cue must not be counted as part of stimulus exposure time or the MFT-M capacity calculation.

10. Keep `flow_rel` unavailable before its protected probe. Any premature exposure must mark the held-out condition as contaminated or rebaseline-needed.

11. Change delayed status assignment so that completing the delayed session is insufficient by itself. Assign `portable` only when fresh delayed all-four mixed evidence passes the existing mixed-stability and timing-quality checks.

12. Do not add new carriers, displaced centres, response-remapping tasks, evidence-accumulation modes, working-memory demands or research-only diagnostics in this update.

## Implementation process

Before changing code, produce a brief mapping table:

| Specification requirement | Current code location | Current behaviour | Required change |
| ------------------------- | --------------------- | ----------------- | --------------- |

Then implement the changes in small commits or clearly separated patches.

Where the current code conflicts with the specification, follow the specification but preserve backwards compatibility wherever practical.

Do not invent new progression thresholds. Reuse the current configured values unless the specification explicitly requires a change.

## Required validation

Run the existing test suite and add tests corresponding to the acceptance tests in the specification, particularly:

* absolute classification independent of item position;
* centre-relative classification dependent on item position;
* diagnostic trials excluded from recovery evidence;
* held-out `flow_rel` protection;
* actual atomic wrapper stored for mixed trials;
* rule cues matched to the active response axis;
* fresh delayed evidence required for portable status.

At completion, report:

1. files changed;
2. controller behaviour changed;
3. renderer/classification changes;
4. tests added and results;

--

### Additional implementation constraints

* Because target files contain pre-existing edits, preserve unrelated hunks and stage only revision-specific hunks using `git add -p` or an equivalent patch-based workflow.
* Show per-trial rule cues only in blocks where the response axis can vary between `LEFT / RIGHT` and `IN / OUT`. Do not add per-trial cues to `arrow_abs / flow_abs` fixed-axis carrier mixing.
* Exclude only diagnostic target-wrapper trials from target-wrapper recovery and progression evidence. Continue logging them, and retain valid base-wrapper trials from the same block.
* Before the protected `flow_rel` probe, disable only free-play routes capable of generating `flow_rel`. Already-unlocked pair mixes that exclude `flow_rel` may remain available.
* Compute delayed portability exclusively from trials belonging to the fresh delayed-recheck block. Distinguish insufficient delayed evidence from adequate evidence that fails.
* Increment the Attention Coach protocol version. Do not retrospectively reinterpret legacy portable statuses or unverifiable held-out tests under the revised criteria.
* Use the existing return-strength evaluator for the return-to-base guard. If return evidence is insufficient, continue collecting return evidence rather than treating it as either a pass or failure.

5. migrations or compatibility handling;
6. any specification requirement not fully implemented.
