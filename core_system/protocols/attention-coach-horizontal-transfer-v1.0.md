---
Version: v1.0
Date: 2026-07-11
Owner: Mindware Lab (Trident G IQ)
Status: Public app protocol specification
Scope: Attention Coach guided training protocol under IQ Coach Horizontal Transfer v1.0
"Claims boundary": Behavioural attention-control training and within-app transfer evidence; non-clinical, non-diagnostic, and not proof of general intelligence gain or broad real-world far transfer
Canonical method: iq-coach-horizontal-transfer-v1.0.md
Implementation checked: IQ-Coach apps/attention-coach source and deployed iqmindware attention-coach bundle, 2026-07-11
---

# Attention Coach Horizontal Transfer Protocol v1.0

## 1) Purpose and Scope

Attention Coach is a guided cognitive-training app that trains attention control under controlled changes in stimulus wrapper. It implements the IQ Coach horizontal transfer method as an attention-control intervention: the learner first stabilises a local rule, then the display format changes while the underlying control demand is held as constant as possible.

The protocol is designed to generate behavioural evidence about whether a user can recover task-relevant relations across:

- visual carrier: static arrows vs optic flow
- reference frame: screen-centred absolute direction vs centre-relative direction
- wrapper mixing: blocked formats vs unpredictable format switching
- delay/re-entry: later checks after mixed-wrapper exposure

The app is not an assessment of ADHD, neurological status, clinical attention disorder, IQ, or employability. It does not directly measure neural mechanisms. It creates behavioural conditions intended to exercise recovery of the same attention-control relation under changed surface formats.

## 2) Relationship to the Canonical Horizontal Transfer Protocol

This app-specific protocol instantiates the shared method described in:

- `core_system/protocols/iq-coach-horizontal-transfer-v1.0.md`

The governing cycle is:

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

The key methodological point is that the swapped wrapper is not trained to a full separate asymptote before returning to the original wrapper. It is trained enough to show recovery from the dip, then the protocol checks whether the original representation remains available and whether performance can survive mixed-wrapper switching.

## 3) Current Implementation Status

The live Attention Coach implementation checked on 2026-07-11 contains:

- transfer controller version `horizontal-transfer-v1.0`
- atomic wrappers `arrow_abs`, `flow_abs`, `arrow_rel`, and `flow_rel`
- phases for diagnostic probe, transition probe, expected dip, recovery, return-to-base, progressive mix, held-out composition, full-factorial mix, delayed re-check, portable state, and maintenance mix
- trial telemetry for wrapper, carrier, frame, probe status, mix ratio, transfer event id, exposure timing, response, correctness, and timing quality
- deterministic cohort assignment for clean v1 users, with approximately 10% assigned to the optic-flow-first validation path and the remainder assigned to arrows-first paths
- path-aware held-out exposure protection for the cohort-specific held-out wrapper
- persisted `startCarrier`, `startCohort`, `startWrapper`, `carrierTargetWrapper`, `frameTargetWrapper`, `heldOutWrapper`, and `heldOutStatus` fields for Supabase-backed aggregate analysis
- local-first data rights with optional cloud sync and benchmark contribution

Current implementation note:

- The source and deployed bundle now implement the required 10-15% counterbalanced optic-flow-first start-carrier cohort.
- Supabase schema support is required for the new cohort/path fields before cloud benchmark records can preserve the full counterbalancing metadata.
- Transfer comparisons should be estimated against aggregated population-level benchmark pools stratified by protocol group, start cohort, and wrapper path. They should not be interpreted as direct individual-to-individual comparisons.

## 4) Task Family

Attention Coach contains two task constructs inside guided sessions.

### Attention Control

Attention Control is the primary progression construct. On each trial the user reports the majority direction relation in a small array of displayed motion or arrow signals. The task is designed to train extraction of the relevant direction relation while ignoring surface changes in carrier and frame.

### Binding Focus

Binding Focus is a secondary construct that asks the user to preserve direction-colour conjunctions while extracting the dominant pattern. It is included to keep direction information bound to another task feature during the same wrapper sequence. Binding Focus supports the training ecology but the transfer controller advances primarily from Attention Control evidence.

## 5) Stimulus Wrappers

The live Attention Coach uses four atomic wrappers.

```text
arrow_abs:
  static arrow carrier
  absolute screen-centred frame
  response relation: left or right

flow_abs:
  optic-flow carrier
  absolute screen-centred frame
  response relation: left or right

arrow_rel:
  static arrow carrier
  centre-relative frame
  response relation: out or in

flow_rel:
  optic-flow carrier
  centre-relative frame
  response relation: out or in
```

`mixed` is not a fifth wrapper. It is a distribution over the atomic wrappers. The distinction matters because different mixes test different transfer questions.

## 6) Guided Session Structure

Each guided session contains 80 trials:

```text
60 Attention Control trials
20 Binding Focus trials
```

The 60 Attention Control trials are organised as three 20-trial mini-blocks. The 20 Binding Focus trials form a fourth mini-block. Depending on the transfer phase, the mini-blocks may be blocked, diagnostic, transition-probe, recovery, return-to-base, mixed, held-out, or delayed-recheck blocks.

Trial difficulty varies by majority ratio and exposure duration. The app records requested and actual exposure timing, estimated display refresh rate, dropped-frame count, response accuracy, reaction time, and timing-quality flags.

## 7) Counterbalanced Progressions Implemented Live

Most clean v1 users follow the arrows-first path.

```text
1. arrow_abs base fluency
2. optional small flow_abs diagnostic probe
3. base flattening gate
4. 20% arrow_abs + flow_abs transition probe
5. expected first-contact dip
6. flow_abs recovery
7. return-to-base arrow_abs check
8. progressive arrow_abs + flow_abs mix: 80/20, 60/40, 50/50, random
9. arrow_rel recovery
10. progressive arrow_abs + arrow_rel mix
11. held-out flow_rel composition probe
12. flow_rel recovery
13. full-factorial mix across all four wrappers
14. delayed mixed re-check
15. portable or maintenance mix
```

The counterbalanced validation cohort follows the optic-flow-first mirror path.

```text
1. flow_abs base fluency
2. optional small arrow_abs diagnostic probe
3. base flattening gate
4. 20% flow_abs + arrow_abs transition probe
5. expected first-contact dip
6. arrow_abs recovery
7. return-to-base flow_abs check
8. progressive flow_abs + arrow_abs mix: 80/20, 60/40, 50/50, random
9. flow_rel recovery
10. progressive flow_abs + flow_rel mix
11. held-out arrow_rel composition probe
12. arrow_rel recovery
13. full-factorial mix across all four wrappers
14. delayed mixed re-check
15. portable or maintenance mix
```

The held-out event is the first formal exposure to the cohort-specific held-out recombination wrapper. For arrows-first users this is `flow_rel`; for optic-flow-first users this is `arrow_rel`. Free-play or other non-formal exposure to the held-out wrapper before that event contaminates clean held-out status.

## 8) Advancement Gates

Advancement is evidence-gated rather than session-count-gated. The 20-session programme envelope is used for product pacing, not as a forced completion rule.

### Base Flattening Gate

The base wrapper is considered ready for challenge when Attention Control evidence approximately satisfies:

```text
valid trials: at least 240
rolling windows: at least 4
recent capacity slope: absolute value below 0.02
balanced accuracy: 0.70 to 0.82
lapse rate: at most 0.18
timing quality: not poor
```

This gate is intended to avoid introducing a wrapper shift while the base rule is still unstable or while performance is limited by display timing or fatigue.

### Diagnostic Probe

After early base exposure, the controller may insert a small target-wrapper diagnostic probe. This probe is used to sample readiness and does not by itself bank transfer or advance the pathway.

### Transition Probe and Expected Dip

After flattening, the app introduces the target wrapper at a controlled dose. The expected behavioural signature is a temporary drop when the old surface-specific policy is no longer sufficient. The target is not to avoid the dip; it is to recover the relation after the dip.

### Recovery Gate

Recovery in the target wrapper is evaluated relative to source-wrapper performance. The live controller looks for early recovery evidence and then for a stronger ready-to-mix threshold, including:

```text
target valid trials: at least 40
target rolling windows: at least 2
recovery ratio: at least 0.80 of source capacity
balanced accuracy: at least 0.70
lapse rate: at most 0.20
timing quality: not poor
```

When recovery is sufficient after the first carrier swap, the protocol returns to the original base wrapper before beginning progressive mixing.

### Mixed Stability Gate

Mixed-wrapper phases require evidence that performance remains stable when wrappers are interleaved. The app estimates mixed-wrapper stability against weighted blocked-wrapper performance and requires adequate evidence for wrappers present in the mix.

### Delayed Re-check

After full-factorial mixed-wrapper stability, the app schedules a delayed re-check. Passing this check moves the controller toward `portable` status or maintenance mix, depending on session number and available evidence.

## 9) Transfer Events and Metrics

The app records transfer events including:

- diagnostic probe
- transition probe
- recovery
- return to base
- mix step
- held-out composition
- delayed re-check
- banked/portable status where available

The app derives internal transfer metrics including:

- initial dip
- recovery slope
- recovery ratio
- return strength
- mixed-wrapper stability
- compositional transfer
- delayed recovery

These metrics are behavioural summaries. They should be reported as within-app training and recovery indicators, not as proof of broad far transfer.

## 10) Data, Consent, and Research Use

The app is local-first. Users can keep data in the browser, enable cloud personal baseline sync, or opt into benchmark contribution. Benchmark contribution is optional and governed by the data-rights page.

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
- phase label and phase status
- wrapper id
- carrier
- frame
- probe status
- mix ratio
- transfer event id
- trial timing fields
- correctness and response time
- held-out status
- legacy or contamination status
- data mode and benchmark consent state

Benchmark comparisons should be calculated only from consented aggregate population-level records. The counterbalanced design allows researchers to estimate transfer effects more cleanly by comparing matched cohort/path benchmark pools, reducing confounding from generic practice effects, task familiarity, and first-wrapper advantage.

## 11) Interpretation Boundaries

Appropriate interpretation:

```text
The app trains and tests whether attention-control relations can be recovered across controlled changes in visual carrier, reference frame, wrapper mixing, and delay.
```

Inappropriate interpretation:

```text
The app proves IQ gain.
The app diagnoses attention disorder.
The app directly measures hippocampal-prefrontal geometry.
The app proves broad real-world far transfer from app practice alone.
```

## 12) Implementation Notes for Reproducibility

The current live implementation was checked against the deployed Attention Coach bundle and the source files:

```text
IQ-Coach/apps/attention-coach/src/transferController.ts
IQ-Coach/apps/attention-coach/src/generator.ts
IQ-Coach/apps/attention-coach/src/types.ts
IQ-Coach/apps/attention-coach/src/main.ts
trident-g-platform/products/trident-g-iq/websites/iqmindware/attention-coach/index.html
```

The deployed app bundle referenced by the live static index at the time of review was:

```text
/attention-coach/assets/index-DtRjOMFE.js
/attention-coach/assets/index-B9VaHg_T.css
```

The live protocol state should be rechecked whenever the transfer controller version, cohort assignment rule, Supabase schema, or deployed bundle changes.
