---
Version: v1.0
Date: 2026-07-11
Owner: Mindware Lab (Trident G IQ)
Status: Public app protocol specification
Scope: WM Coach guided training protocol under IQ Coach Horizontal Transfer v1.0
"Claims boundary": Behavioural working-memory training and within-app transfer evidence; non-clinical, non-diagnostic, and not proof of general intelligence gain or broad real-world far transfer
Canonical method: iq-coach-horizontal-transfer-v1.0.md
Implementation checked: IQ-Coach apps/wm-coach source and deployed iqmindware wm-coach bundle, 2026-07-11
---

# WM Coach Horizontal Transfer Protocol v1.0

## 1) Purpose and Scope

WM Coach is a guided working-memory training app that trains relational n-back and binding-memory performance under controlled changes in stimulus wrapper. It instantiates the IQ Coach horizontal transfer method in a working-memory setting: the user first stabilises a local n-back relation, then the display format changes while the underlying match rule is held as constant as possible.

The protocol is designed to generate behavioural evidence about whether relational working-memory performance can recover across:

- visual carrier: static arrows vs optic flow
- reference frame: screen-centred absolute direction vs centre-relative direction
- wrapper mixing: blocked formats vs unpredictable format switching
- delay/re-entry: later checks after mixed-wrapper exposure

The app is not an IQ test, ADHD test, neurological assessment, or employment/selection instrument. Its transfer outputs are within-app training and recovery indicators.

## 2) Relationship to the Canonical Horizontal Transfer Protocol

This app-specific protocol instantiates:

- `core_system/protocols/iq-coach-horizontal-transfer-v1.0.md`

The controlling cycle is:

```text
base fluency
-> flattening gate
-> small diagnostic probe
-> controlled transition probe
-> expected dip
-> recovery in the new wrapper
-> return-to-base check
-> progressive wrapper mix
-> held-out recombination
-> full-factorial mix
-> delayed re-check
-> maintenance or portable status
```

The swapped wrapper is not trained to a full independent asymptote before returning to the original wrapper. It is trained enough to show recovery from the dip, then the protocol checks whether the original representation remains available and whether performance survives mixed-wrapper switching.

## 3) Current Implementation Status

The live WM Coach implementation checked on 2026-07-11 contains:

- transfer controller version `horizontal-transfer-v1.0`
- atomic wrappers `arrow_abs`, `flow_abs`, `arrow_rel`, and `flow_rel`
- phases for diagnostic probe, transition probe, expected dip, recovery, return-to-base, progressive mix, held-out composition, full-factorial mix, delayed re-check, portable state, and maintenance mix
- deterministic cohort assignment for clean v1 users, with approximately 10% assigned to the optic-flow-first validation path and the remainder assigned to arrows-first paths
- path-aware held-out exposure protection for the cohort-specific held-out wrapper
- trial telemetry for wrapper, carrier, frame, probe status, mix ratio, mapping timing, transfer event id, n-level, lure type, correctness, and timing quality
- persisted `startCarrier`, `startCohort`, `startWrapper`, `carrierTargetWrapper`, `frameTargetWrapper`, `heldOutWrapper`, and `heldOutStatus` fields for Supabase-backed aggregate analysis
- local-first data rights with optional cloud sync and benchmark contribution when Supabase is configured

Transfer comparisons should be estimated against consented aggregate population-level benchmark pools stratified by protocol group, start cohort, and wrapper path. They should not be interpreted as direct individual-to-individual comparisons.

## 4) Task Family

WM Coach contains two task constructs inside guided sessions.

### Relational Memory

Relational Memory is the primary progression construct. Users respond to whether the current relational stimulus matches the stimulus N trials back. The wrapper can change while the n-back relation remains the target invariant.

### Binding Memory

Binding Memory is a secondary construct that asks the user to preserve relation-colour binding under working-memory load. It contributes to the training ecology, but the transfer controller advances primarily from Relational Memory evidence.

## 5) Stimulus Wrappers

The live WM Coach uses four atomic wrappers.

```text
arrow_abs:
  static arrow carrier
  absolute screen-centred frame

flow_abs:
  optic-flow carrier
  absolute screen-centred frame

arrow_rel:
  static arrow carrier
  centre-relative frame

flow_rel:
  optic-flow carrier
  centre-relative frame
```

`mixed` is not a fifth wrapper. It is a distribution over the atomic wrappers.

## 6) Counterbalanced Progressions

Most clean v1 users follow the arrows-first path.

```text
1. arrow_abs base fluency
2. optional small flow_abs diagnostic probe
3. base flattening gate
4. arrow_abs + flow_abs transition probe
5. expected first-contact dip
6. flow_abs recovery
7. return-to-base arrow_abs check
8. progressive arrow_abs + flow_abs mix
9. arrow_rel recovery
10. progressive arrow_abs + arrow_rel mix
11. held-out flow_rel composition probe
12. flow_rel recovery
13. full-factorial mix across all four wrappers
14. delayed mixed re-check
15. portable or maintenance mix
```

The optic-flow-first validation cohort follows the mirrored path.

```text
1. flow_abs base fluency
2. optional small arrow_abs diagnostic probe
3. base flattening gate
4. flow_abs + arrow_abs transition probe
5. expected first-contact dip
6. arrow_abs recovery
7. return-to-base flow_abs check
8. progressive flow_abs + arrow_abs mix
9. flow_rel recovery
10. progressive flow_abs + flow_rel mix
11. held-out arrow_rel composition probe
12. arrow_rel recovery
13. full-factorial mix across all four wrappers
14. delayed mixed re-check
15. portable or maintenance mix
```

For arrows-first users the held-out wrapper is `flow_rel`; for optic-flow-first users it is `arrow_rel`. Free-play or non-formal exposure to the held-out wrapper before that event contaminates clean held-out status.

## 7) Advancement Gates

Advancement is evidence-gated rather than session-count-gated. The 20-session envelope is product pacing, not a forced completion rule.

WM-specific gates use the shared horizontal-transfer grammar plus n-back constraints:

```text
base valid trials: at least 240
rolling windows: at least 4
recent capacity slope: absolute value below 0.02
balanced accuracy: 0.70 to 0.82
lapse rate: at most 0.18
timing quality: not poor
current n-level: no more than one level above stable n-level
```

Recovery and mixed-stability gates additionally check lure error rate and n-level stability so a wrapper advance is not driven by unstable n-back demand.

## 8) Research Use and Benchmarking

Research exports or analyses should preserve at minimum:

- protocol version
- protocol group
- start carrier
- start cohort
- start wrapper
- carrier-target wrapper
- frame-target wrapper
- held-out wrapper
- transfer controller phase
- wrapper id
- carrier
- frame
- probe status
- mix ratio
- mapping timing
- transfer event id
- n-level and lure type
- correctness and response time
- held-out status
- data mode and benchmark consent state

Benchmark comparisons should be calculated only from consented aggregate population-level records. The counterbalanced design supports cleaner estimates of transfer by separating wrapper-order effects, generic practice effects, task familiarity, and first-wrapper advantage.

## 9) Implementation Notes

The current implementation was checked against:

```text
IQ-Coach/apps/wm-coach/src/transferController.ts
IQ-Coach/apps/wm-coach/src/generator.ts
IQ-Coach/apps/wm-coach/src/types.ts
IQ-Coach/apps/wm-coach/src/main.ts
IQ-Coach/apps/wm-coach/supabase/functions/submit-wm-block/index.ts
IQ-Coach/apps/wm-coach/supabase/functions/finalize-wm-session/index.ts
trident-g-platform/products/trident-g-iq/websites/iqmindware/wm-coach/index.html
```

The deployed static bundle referenced by the live index at the time of review was:

```text
/wm-coach/assets/index-D5_L-rta.js
/wm-coach/assets/index-C1fE9KQf.css
```

Supabase schema support is required for cloud benchmark records to preserve the full counterbalancing metadata.
