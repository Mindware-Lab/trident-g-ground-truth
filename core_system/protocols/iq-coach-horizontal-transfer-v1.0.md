---
Version: v1.0
Date: 2026-07-11
Owner: Mindware Lab (Trident G IQ)
Status: Public implementation protocol
Scope: IQ Coach horizontal representational transfer protocol for Attention Coach and Working Memory Coach
"Claims boundary": Behavioural transfer-training protocol; non-clinical, non-diagnostic, and not proof of broad real-world far transfer
---
# IQ Coach Horizontal Transfer Protocol v1.0

## Status

This document is the implementation-facing specification for the IQ Coach horizontal transfer protocol used by Attention Coach and Working Memory Coach.

It is based on:

- `core_system/horizontal_transfer_implementation.docx`
- `IQ_Coach_Representational_Transfer_Protocol_v1.0 (1).docx`
- the implemented Attention Coach and WM Coach transfer controllers in `apps/attention-coach` and `apps/wm-coach`

The protocol is behavioural and product-facing. It does not claim to measure neural geometry, prove IQ gain, or demonstrate broad real-world far transfer. It tests whether task-relevant relations can be recovered across controlled changes in carrier, reference frame, wrapper mixing, response-mapping opportunity, and delay.

## Core Rule

Do not advance because a fixed number of sessions has elapsed.

Advance when evidence shows that the current representational condition is stable enough to challenge, and when the next perturbation can be introduced without confounding the result.

The governing cycle is:

```text
build base fluency
detect flattening
probe a matched new wrapper
measure first-contact dip
recover without increasing difficulty
return to base
mix wrappers progressively
test held-out recombination
run full-factorial mix
re-check after delay
maintain or repair weak wrappers
```

The target is not a smooth monotonic learning curve. The target is a progressively stronger ability to recover the same latent task relation when the surface format changes.

## Scope

This specification governs horizontal representational transfer inside IQ Coach:

- Attention Coach guided mode
- Working Memory Coach guided mode
- local progress migration
- synced JSON progress
- trial telemetry
- user-facing transfer-stage language
- post-block feedback requirements

It does not govern:

- free-play progression scoring
- broad external transfer claims
- production deployment
- clinical or diagnostic interpretation

Practice/free-play may be excluded from guided progression, but it cannot be excluded from exposure accounting for held-out conditions.

## Atomic Wrappers

The protocol has four atomic carrier x frame wrappers:

```ts
type WrapperId =
  | "arrow_abs"
  | "flow_abs"
  | "arrow_rel"
  | "flow_rel";
```

`mixed` is not a wrapper. A mix is a distribution over atomic wrappers:

```ts
type WrapperMix = {
  wrapperRatios: Partial<Record<WrapperId, number>>;
  randomised: boolean;
};
```

This distinction is mandatory. A generic `mixed` wrapper would erase the difference between:

```text
arrow_abs + flow_abs
arrow_abs + arrow_rel
all four wrappers
```

Those conditions answer different transfer questions and must have separate histories.

## Wrapper Matrix

```text
                  arrow carrier                 optic-flow carrier
absolute frame    arrow_abs                      flow_abs
relative frame    arrow_rel                      flow_rel
```

The held-out carrier x frame composition is cohort-specific. The default pathway is arrows-first, but a small stable optic-flow-first cohort is required so the programme can estimate direction-of-transfer baselines rather than treating arrow-to-flow transfer as the only observed ordering.

## Start-Carrier Counterbalancing

Fresh clean v1 users are assigned a persistent `startCarrier` at first guided progression initialization:

```text
arrows_first:      85-90% of clean v1 users, default target 87.5%
optic_flow_first:  10-15% of clean v1 users, default target 12.5%
```

Assignment must be deterministic and stable for the user, for example by hashing a cloud user id or local installation id with the protocol version. A user must not be re-randomised after clearing ordinary session state, changing devices, or syncing progress.

The two cohorts are mirror paths:

```text
arrows_first:
  base wrapper:        arrow_abs
  carrier target:      flow_abs
  frame target:        arrow_rel
  held-out composition: flow_rel

optic_flow_first:
  base wrapper:        flow_abs
  carrier target:      arrow_abs
  frame target:        flow_rel
  held-out composition: arrow_rel
```

The optic-flow-first cohort is not a separate product mode. It is an analysis counterbalance that allows estimates of whether transfer cost is specific to optic flow, specific to arrows, or produced by changing wrappers in either direction. Trial and session telemetry must include `startCarrier`, `startWrapper`, `carrierTargetWrapper`, `frameTargetWrapper`, and `heldOutWrapper`.

The clean v1 path must protect the cohort-specific held-out composition. For arrows-first users this is `flow_rel`; for optic-flow-first users this is `arrow_rel`. The held-out wrapper must not be trained, previewed, demonstrated, or made available in clean-user practice before the formal held-out composition event.

## Held-Out Exposure Status

The app must track held-out status globally and relative to the assigned `heldOutWrapper`:

```ts
type HeldOutStatus =
  | "clean"
  | "first_exposure_logged"
  | "contaminated"
  | "legacy_exposed";
```

Any rendering of the cohort-specific held-out mapping counts as exposure unless it is the formal held-out event.

This includes:

- guided sessions
- practice/free-play
- tutorials
- demos
- debug screens in production
- example animations
- migrated legacy progress

For clean v1 users:

```text
no tutorial preview of heldOutWrapper
no free-play access to heldOutWrapper
no mixed practice containing heldOutWrapper
no production debug exposure of heldOutWrapper
```

before the formal held-out composition probe.

If exposure occurs outside the formal event, mark the status `contaminated`. If the user has old beta history with the assigned `heldOutWrapper`, mark `legacy_exposed`.

## Transfer Controller State

Each app stores versioned transfer state in local progress and synced JSON progress:

```ts
type TransferControllerState = {
  version: "horizontal-transfer-v1.0";
  activeBaseWrapper: WrapperId | null;
  activeTargetWrapper: WrapperId | null;
  phase: TransferPhase;
  mixRatio: number | null;
  activeMix: WrapperMix | null;
  wrapperStates: Record<WrapperId, WrapperState>;
  transferEvents: TransferEvent[];
  delayedRechecks: DelayedRecheck[];
  startCarrier: "arrows" | "optic_flow";
  startWrapper: WrapperId;
  carrierTargetWrapper: WrapperId;
  frameTargetWrapper: WrapperId;
  heldOutWrapper: WrapperId;
  heldOutCompositionLogged: boolean;
  heldOutStatus: HeldOutStatus;
  legacyHeldOutExposure: boolean;
  legacyStatus:
    | "none"
    | "legacy_active"
    | "legacy_exposed"
    | "legacy_mixed_unknown"
    | "rebaseline_required";
  completedAtSession: number | null;
  maintenanceSessionCount: number;
};
```

## Transfer Phases

The controller uses protocol phases rather than fixed legacy phase progression:

```ts
type TransferPhase =
  | "base_fluency"
  | "diagnostic_probe"
  | "flattening"
  | "transition_probe"
  | "expected_dip"
  | "recovering"
  | "return_to_base"
  | "mix_80_20"
  | "mix_60_40"
  | "mix_50_50"
  | "random_mix"
  | "held_out_composition"
  | "full_factorial_mix"
  | "delayed_recheck"
  | "portable"
  | "maintenance_mix";
```

Legacy public phases may remain as compatibility labels, but the controller decision must come from transfer state.

## Canonical Progression

For a fresh clean v1 arrows-first user:

```text
1. arrow_abs base fluency
2. optional 5-10% flow_abs diagnostic probe
3. base flattening gate
4. 80/20 arrow_abs + flow_abs transition probe
5. expected first-contact dip
6. flow_abs recovery
7. return-to-base arrow_abs check
8. 80/20, 60/40, 50/50, random absolute carrier mix
9. arrow_rel frame probe and recovery
10. arrow_abs + arrow_rel frame mix
11. held-out flow_rel first-exposure composition probe
12. flow_rel recovery
13. full-factorial random mix across all four wrappers
14. delayed mixed re-check
15. portable or maintenance mix
```

For a fresh clean v1 optic-flow-first user, mirror the same logic:

```text
1. flow_abs base fluency
2. optional 5-10% arrow_abs diagnostic probe
3. base flattening gate
4. 80/20 flow_abs + arrow_abs transition probe
5. expected first-contact dip
6. arrow_abs recovery
7. return-to-base flow_abs check
8. 80/20, 60/40, 50/50, random absolute carrier mix
9. flow_rel frame probe and recovery
10. flow_abs + flow_rel frame mix
11. held-out arrow_rel first-exposure composition probe
12. arrow_rel recovery
13. full-factorial random mix across all four wrappers
14. delayed mixed re-check
15. portable or maintenance mix
```

If progress is fast, sessions up to and beyond the 20-session envelope become:

- mixed transfer practice
- delayed re-checks
- early/late cue benchmark sessions
- weak-wrapper repair
- full-factorial maintenance

If progress is slow, the programme continues beyond session 20 without forced advancement.

Session 20 is an envelope, not a completion rule.

## Stage-Specific Gates

Do not use the full 240-trial flattening gate for every stage. Use the gate that matches the controller question.

```text
base wrapper -> transition probe:
  full flattening gate

target wrapper -> recovery/mix:
  recovery gate

mixed condition -> next dimension:
  mixed-stability gate

full factorial mix -> portable:
  delayed-portability gate
```

## Default Gate Configuration

The Attention Coach and WM Coach share the same controller grammar but have separately versioned gate defaults.

### Attention Coach Defaults

```ts
const attentionTransferDefaults = {
  baseFlatteningMinValidTrials: 240,
  baseFlatteningMinWindows: 4,
  baseFlatteningMaxAbsSlope: 0.02,
  accuracyBandMin: 0.70,
  accuracyBandMax: 0.82,
  diagnosticProbeRatio: 0.05,
  transitionProbeRatio: 0.20,
  firstContactTrials: 5,
  earlyTransitionMinTrials: 12,
  targetRecoveryMinTrials: 40,
  targetRecoveryMinWindows: 2,
  recoveryStartingRatio: 0.70,
  readyToMixRatio: 0.80,
  mixedStabilityMinTrials: 80,
  mixedStabilityRatio: 0.80,
  mixedStabilityMinAccuracy: 0.70,
  delayedRecheckMinSessionsAfterMix: 1
};
```

### Working Memory Coach Defaults

```ts
const wmTransferDefaults = {
  baseFlatteningMinValidTrials: 240,
  baseFlatteningMinWindows: 4,
  baseFlatteningMaxAbsSlope: 0.02,
  accuracyBandMin: 0.70,
  accuracyBandMax: 0.82,
  diagnosticProbeRatio: 0.05,
  transitionProbeRatio: 0.20,
  firstContactTrials: 5,
  earlyTransitionMinTrials: 12,
  targetRecoveryMinTrials: 44,
  targetRecoveryMinWindows: 2,
  recoveryStartingRatio: 0.70,
  readyToMixRatio: 0.80,
  mixedStabilityMinTrials: 88,
  mixedStabilityRatio: 0.80,
  mixedStabilityMinAccuracy: 0.70,
  delayedRecheckMinSessionsAfterMix: 1
};
```

The WM values are not assumed to be canonical for all time. They must be calibrated against WM-specific evidence, because n-level, relation alphabet, lure rate, exposure duration, target frequency, and buffer trials change the meaning of accuracy and slope.

## Flattening Gate

The base flattening gate permits a major perturbation only when all required conditions pass:

```text
valid trials >= baseFlatteningMinValidTrials
rolling windows >= baseFlatteningMinWindows
abs(recent capacity slope) < baseFlatteningMaxAbsSlope
balanced accuracy in calibrated band
lapse/error profile acceptable
timing quality not poor
no global fatigue/timing blocker
```

Flattening means:

```text
stable enough to challenge
```

It does not mean:

```text
mastered
neural transition detected
ready to complete the programme
```

## Diagnostic Probe

A diagnostic probe may occur before full flattening:

```text
dose: 5-10% target-wrapper trials
state effect: no advancement
banking effect: none
```

It is used to estimate readiness and does not unlock a wrapper, mark transfer, or count as recovery.

## Transition Probe

A formal transition probe cannot begin until the base flattening gate passes.

The transition session must separate:

```text
anchor/base trials
initial 20% transition probe trials
feedback-enabled recovery trials
return-to-base trials
```

The initial transition probe estimate must not be contaminated by later trained recovery trials.

Trial labels:

```ts
probeStatus:
  | "base"
  | "diagnostic_probe"
  | "transition_probe"
  | "recovery"
  | "return_to_base"
  | "mix"
  | "held_out"
  | "delayed_recheck";
```

## First-Contact Dip

The first 3-5 target-wrapper trials are a descriptive first-contact dip measure.

They are not sufficient for controller advancement.

Progression uses a larger matched transition estimate:

```ts
firstContactTrials: 5;        // descriptive
earlyTransitionMinTrials: 12; // controller estimate floor
```

Transition comparisons must match or explicitly model:

- relation set
- response alternatives
- majority ratio
- exposure demand
- masking
- lure pressure
- device timing
- n-level for WM

Otherwise the observed transfer cost may simply be a difficulty difference.

## Difficulty Freeze During Wrapper Shifts

During wrapper shifts, freeze unrelated difficulty:

```text
no exposure speed increase
no n-level increase
no harder majority ratio
no additional lure pressure
no extra response alternatives
```

For WM, n-level should be held constant or stepped down during wrapper transitions.

## Recovery States

Recovery is a two-step concept:

```text
recovery_starting:
  positive recovery evidence or recovery ratio around 0.70

ready_to_mix:
  sufficient target-wrapper trials
  reliable recovery evidence
  stronger recovered proportion
  acceptable lure/error balance
  acceptable timing
```

The 0.70 ratio is a recovery-starting floor, not a ready-to-mix threshold.

Ready-to-mix requires the configured `readyToMixRatio`, enough target trials, enough windows, acceptable timing, and acceptable error/lure profile.

## Mixed Stability

Mixed stability is relative, not merely "70% accurate".

Compute:

```text
mixed stability =
  observed mixed capacity
  /
  weighted blocked-wrapper capacity
```

Then require:

```text
adequate mixed stability ratio
adequate balanced accuracy
stable windows
acceptable lure balance
acceptable timing
```

A low-demand mixed block may reach 70% accuracy while still showing a large cost relative to blocked-wrapper performance. That is not enough for portability.

## Delayed Re-Check

Banking requires mixed stability plus a delayed re-check.

The delayed re-check should use both:

```text
elapsed session count
elapsed time when available
```

The minimal implemented gate is:

```ts
delayedRecheckMinSessionsAfterMix: 1;
```

Delayed recovery should be recorded as a metric, not inferred from simply unlocking a later wrapper.

## Attention Coach Session Generation

Guided Attention Coach sessions retain 80 trials:

```text
60 Attention Control trials
20 Binding Focus trials
```

The controller generates blocks from transfer events rather than fixed phase numbers.

Required block patterns:

```text
base:
  current wrapper only

diagnostic:
  mostly base with 5-10% target wrapper

transition:
  anchor/base block
  20% target-wrapper probe block
  recovery block
  optional return-to-base

recovery:
  target wrapper only, difficulty frozen

return_to_base:
  base wrapper only

mix:
  explicit 80/20, 60/40, 50/50, random wrapper distributions

held_out_composition:
  first formal flow_rel exposure
  then feedback-enabled recovery

full_factorial_mix:
  all four atomic wrappers

delayed_recheck:
  full-factorial or weak-wrapper targeted re-check
```

## Working Memory Coach Session Generation

Guided WM Coach sessions retain 88 displayed trials.

Relational n-back is the primary horizontal-transfer layer. Binding phases are later benchmark or vertical-bridge content, not the driver of horizontal wrapper advancement.

WM-specific rules:

- hold n-level constant or step down during wrapper shifts
- buffer trials before `trial_index > n` do not count as valid scored evidence
- flattening windows must hold constant or model n-level, relation alphabet, target frequency, lure rate, exposure duration, majority ratio, and wrapper
- slope should use a stable capacity estimate rather than raw accuracy while n-level changes
- lure imbalance must block ready-to-mix

Each WM trial must record the actual atomic wrapper, carrier, frame, probe status, mix ratio, n-level, lure type, and mapping timing.

## Telemetry Contract

Each trial should carry:

```ts
wrapperId: WrapperId;
carrier: "arrow" | "optic_flow";
frame: "absolute" | "relative" | "mixed";
probeStatus:
  | "base"
  | "diagnostic_probe"
  | "transition_probe"
  | "recovery"
  | "return_to_base"
  | "mix"
  | "held_out"
  | "delayed_recheck";
mixRatio: number | null;
mappingTiming: "early" | "late" | null;
lureType: string | null;
transferEventId: string | null;
```

Supabase trial tables use nullable columns to avoid breaking old inserts:

```text
wrapper_id
carrier
frame
probe_status
mix_ratio
mapping_timing
lure_type
transfer_event_id
```

Required transfer metric keys:

```text
initial_dip
recovery_slope
recovery_ratio
return_strength
mixed_wrapper_stability
compositional_transfer
delayed_recovery
late_cue_cost
early_cue_reinstatement
```

## Migration Rules

Migration must preserve access and progress without manufacturing transfer evidence.

Legacy phase mapping:

```text
P1_ARROW_ABS:
  base_fluency / arrow_abs

P2_FLOW_ABS:
  legacy_active; target flow_abs; no automatic recovery credit

P3_ARROW_REL:
  legacy_active; target arrow_rel; no automatic recovery credit

P4_FLOW_REL:
  legacy_exposed; held-out status legacy_exposed

P5/P7 mixed:
  legacy_mixed_unknown; do not assume all four wrappers were actually presented

P6/P11 delayed:
  delayed_recheck compatibility state; do not assume delayed recovery passed
```

If a user has already trained `flow_rel`, do not pretend it is held out.

Use:

```ts
heldOutCompositionLogged = false;
legacyHeldOutExposure = true;
heldOutStatus = "legacy_exposed";
legacyStatus = "legacy_exposed" | "legacy_mixed_unknown";
```

Run a composition re-check rather than claiming first-exposure held-out transfer.

## User Narrative

The public narrative should be clear, modest, and motivational:

```text
Building
Ready for a change
New format dip
Recovering
Return check
Ready to mix
Stable across formats
Re-check due
Portable
```

User-facing copy rules:

- do not expose neural claims
- explain dips as expected and recoverable
- describe session 20 as an envelope, not completion
- if ahead of schedule, show "mixed transfer practice" or "return check"
- if behind schedule, continue without forcing advancement
- practice/free-play does not change guided progress
- practice/free-play can contaminate held-out first-exposure status

Safe wording:

```text
A temporary drop is expected while you recover the same rule in a new format.
```

Unsafe wording:

```text
Your brain has learned a new neural code.
This proves IQ improvement.
You have completed transfer training because session 20 ended.
```

## Post-Block Feedback Requirement

After every guided block, the app should continue to show motivating performance feedback.

The implemented apps preserve the existing two-metric curve feedback path:

```text
guided block completes
-> recordGuidedBlockFeedback(...)
-> append blockFeedbackHistory
-> renderBlockBreak(...)
-> show current block performance plus recent curve context
```

This feedback is motivational and progress-oriented. It should not imply that a single block proves transfer.

For protocol consistency, block feedback should remain separate from transfer banking:

```text
block feedback:
  immediate motivation and local performance trend

transfer controller:
  evidence-based stage advancement and banking
```

## Early/Late Mapping Benchmark

The representational-flexibility benchmark is periodic, not part of every training block.

```text
early cue:
  mapping cue -> stimulus -> delay -> response

late cue:
  stimulus -> delay -> mapping cue -> response

reinstatement:
  return to early cue after late-cue block
```

Metrics:

```text
late_cue_cost =
  late-cue performance - early-cue baseline

early_cue_reinstatement =
  post-late early-cue performance / original early-cue baseline
```

The optional late-cue held-out-wrapper condition is a joint wrapper-and-coding-flexibility probe. It is compositional transfer inside the app family, not proof of broad far transfer.

## Claims Boundary

Allowed claims:

```text
IQ Coach trains and tests whether task-relevant relations can be recovered across controlled changes in visual carrier, reference frame, response mapping opportunity, and delay.
```

Allowed internal metrics:

```text
wrapper recovery
representational flexibility
held-out compositional transfer
mixed-wrapper stability
delayed recovery
```

Disallowed claims:

```text
direct hippocampal training
measured hippocampal-prefrontal geometry
guaranteed IQ gain
guaranteed broad far transfer
neural decoding from response time alone
criticality inferred from RT heavy tails
```

## Acceptance Tests

Minimum controller tests:

- transition probe cannot start before base flattening passes
- diagnostic probes do not advance wrapper state or count as banking
- the cohort-specific `heldOutWrapper` is not generated before held-out composition for clean v1 users
- practice/free-play exposure to the cohort-specific `heldOutWrapper` contaminates clean held-out status
- legacy users with prior held-out-wrapper exposure migrate with `legacyHeldOutExposure = true`
- wrapper shifts freeze exposure, n-level, ratio, and lure pressure
- first-contact dip uses first 3-5 valid target-wrapper trials descriptively only
- controller decisions use at least `earlyTransitionMinTrials`
- recovery-starting is distinct from ready-to-mix
- mix progresses 80/20 -> 60/40 -> 50/50 -> random
- mixed stability is relative to weighted blocked-wrapper capacity
- delayed re-check is required before portable
- fast progress fills remaining envelope with maintenance or benchmark sessions
- session 20 does not force portability or programme completion
- WM buffer trials do not count as scored evidence
- WM n-level changes do not create artificial flattening or recovery decisions

Minimum integration tests:

- fresh Attention Coach user can progress base -> carrier probe -> mix -> frame probe -> held-out composition
- fresh WM Coach user can progress through relational n-back horizontal transfer without binding phases driving advancement
- migrated beta users load without losing progress or breaking cloud sync
- Supabase block submit accepts nullable transfer fields
- export includes transfer fields
- delete/reset clears local and cloud transfer state

Minimum copy tests:

- no public copy claims IQ gain
- no public copy claims neural decoding
- no public copy claims broad far transfer
- post-20 screens say envelope/maintenance/re-check, not programme completion
- dips are described as expected

## Implementation Notes

As implemented in the current Attention Coach and WM Coach work:

- `wap.ts` is a thin compatibility wrapper over `transferController.ts`
- transfer state is stored in local progress and cloud JSON progress
- guided generation reads `TransferControllerState`
- `mixed` is represented as `activeMix`, not `WrapperId`
- the cohort-specific `heldOutWrapper` is protected by held-out status
- old progress is migrated into compatibility states
- trials record transfer telemetry
- score snapshots include `transferMetrics`
- Supabase migrations add nullable transfer columns
- post-block two-metric performance feedback remains active after guided blocks

## Final Compression

```text
stabilise
perturb
measure dip
recover
return to base
mix progressively
protect held-out composition
test full-factorial flexibility
re-check after delay
bank only what survives
```



