---
Version: v1.0
Date: 2026-07-10
Owner: Mindware Lab (Trident G IQ)
Status: Public methodological protocol
Scope: HRP Lab G Track Working Memory benchmark
"Claims boundary": Non-clinical cognitive benchmark; provisional task bands only at launch
---

# G Track Working Memory Benchmark Protocol

## 1) Purpose and scope

The G Track Working Memory benchmark is a repeated-measures assessment used in the HRP Lab G Track battery. It measures two separable working-memory constructs:

- Working Memory Capacity
- Binding Precision

The benchmark is assessment, not training. It is not a clinical test, not a diagnostic test, and not an IQ score. It is designed to provide external proof/change evidence alongside IQMindware training apps such as WM Coach.

The implemented benchmark is:

`WMC-B10 v0.1.0`

## 2) Source paradigms

The benchmark is based on two established task families:

- Visuospatial complex-span tasks, where a participant stores ordered spatial information while performing an intervening processing task.
- Visual-array change-detection tasks, where a participant remembers object-location bindings and then judges whether a probed item is the same or different.

The planning specification used for this implementation is:

- `https://github.com/Mindware-Lab/IQ-Coach/blob/main/specs/tests/HRP_Lab_WM_Test.md`

That source discussed jsPsych, MIT-licensed span-task references, and visual-array change-detection references. The live G Track implementation uses Mindware-authored task generation, stimulus rendering, data-rights handling, proof export, and canonical server-side scoring. The public protocol therefore describes the implemented method and score logic, not a verbatim copy of any third-party task code.

N-back is deliberately not used in this benchmark. IQMindware WM Coach already uses n-back-like app-native training measures. The G Track WM benchmark is intended to provide a partially independent outcome check.

## 3) Battery placement and forms

The benchmark has three repeatable forms:

- `wm-a`
- `wm-b`
- `wm-c`

Each form can serve one of three roles:

- Baseline / pre-training
- Post-training
- Follow-up

The app tracks form identity and measurement role separately. Participants are counterbalanced across the three forms so that, over time, each form can contribute baseline data rather than one form always being the baseline form.

Only valid baseline/pre-training completions can contribute to IQMindware G Track sample norms. Post-training and follow-up completions are saved, scored, and exportable as proof/change evidence, but are excluded from norm-building.

## 4) Task structure

Each Working Memory form contains two task blocks.

### Part A: Visuospatial Complex Span

The participant alternates between:

- a symmetry judgement for an 8 x 8 black-and-white pattern
- a spatial storage item in which one cell in a 4 x 4 grid is highlighted

At the end of each set, the participant recalls the highlighted grid locations in their original order.

The implemented schedule is:

- 2 practice sets: one set of size 2 and one set of size 3
- 12 scored sets
- 3 scored sets at each size: 2, 3, 4, and 5
- 42 total scored serial positions

Set order is constrained so that the first scored set is not too difficult, set sizes do not form a simple ascending/descending pattern, long runs of the same size are avoided, and each part of the task samples multiple set sizes.

### Part B: Visual Binding Change Detection

The participant sees an array of visual tokens at different locations. After a short retention interval, one occupied location is probed. The participant judges whether the token at that location is the same as before or different.

The implemented schedule is:

- 6 practice trials at set size 3
- 54 scored trials
- 3 set sizes: 3, 5, and 7
- 3 conditions: same, novel change, and swap change
- 6 scored trials per set-size x condition cell

This produces:

`3 set sizes x 3 conditions x 6 repeats = 54` scored binding trials.

The swap-change condition is the binding-specific lure: the probe token was present in the memory array, but originally belonged to a different location.

## 5) Stimulus construction

### Complex-span processing stimuli

Symmetry patterns are generated internally as 8 x 8 black-and-white matrices.

Symmetrical patterns are produced by generating one side of the matrix and mirroring it. Asymmetrical patterns are produced by altering a valid symmetrical pattern and verifying that it is no longer vertically symmetrical.

Patterns are rejected if their black-cell density is outside the allowed range. This keeps the task from becoming trivial because of nearly empty or nearly full patterns.

### Complex-span storage stimuli

Spatial storage uses a 4 x 4 grid.

Within a set:

- locations do not repeat
- simple row, column, and diagonal runs are rejected
- the participant recalls the sequence by selecting grid cells in order

The recall screen labels selected cells by order and supports undo, clear, and submit actions.

### Binding stimuli

Binding trials use internally generated visual tokens. Each token combines:

- a colour
- a simple internal pattern or mark

The implementation uses eight token identities and fixed possible display locations. Tokens are unique within a memory array.

The three binding conditions are:

- Same: the original token is probed at the original location.
- Novel change: the probe token was not in the memory array.
- Swap change: the probe token was in the memory array but at another location.

## 6) Timing and response rules

### Complex span

For each processing-storage item:

- symmetry pattern response window: 3000 ms
- location highlight: 650 ms

If no symmetry response occurs within the window, the processing item is recorded as an omission and scored incorrect. The test then continues to the storage item.

Responses:

- `F` or the left-side button: symmetrical
- `J` or the right-side button: not symmetrical

No correctness feedback is given during scored sets.

### Binding

For each binding trial:

- memory array display: 500 ms
- retention interval: 900 ms
- probe response window: 2500 ms

If no probe response occurs within the window, the trial is recorded as an omission and scored incorrect.

Responses:

- `F` or the left-side button: same
- `J` or the right-side button: different

No correctness feedback is given during scored trials.

## 7) Raw scoring

All canonical scoring is performed server-side from submitted trial rows.

### Complex-span scoring

The primary complex-span score is:

`complexSpanPCU = exact serial positions correct / total serial positions`

Because the task contains 42 scored serial positions, this is the proportion of locations recalled in the correct position and order across all scored sets.

Example:

- target sequence: 3, 8, 11, 2
- response sequence: 3, 8, 2, 11
- exact serial positions correct: 2
- set-level PCU contribution: 2 / 4

The main complex-span metrics are:

- `wm_complex_span_pcu`
- `wm_processing_accuracy`
- `wm_transposition_rate`
- `wm_intrusion_rate`
- `complexSpanItemAccuracy`
- `complexSpanWholeSetAccuracy`
- `complexSpanAbsoluteScore`
- `complexSpanMaximumStableSetSize`
- `processingMedianRt`
- `processingOmissionRate`

Definitions:

- Item accuracy: target locations recalled regardless of order divided by total target locations.
- Whole-set accuracy: perfectly recalled scored sets divided by 12.
- Absolute score: sum of set sizes for all perfectly recalled scored sets.
- Maximum stable set size: largest set size at which at least two of three sets are recalled perfectly.
- Transposition: a target location recalled in the wrong serial position.
- Intrusion: a recalled location that was not part of the target set.

### Complex-span validity

Processing accuracy is treated as a validity check because a participant could otherwise prioritise location storage and neglect the symmetry task.

Launch confidence labels:

- processing accuracy below .75: invalid
- processing accuracy from .75 to below .85: limited
- processing accuracy at or above .85: calibrating

Do not interpret high memory recall as strong working-memory capacity if the processing task was not adequately performed.

### Binding scoring

Binding scoring treats change as the signal.

- Hit: a `different` response on a novel-change or swap-change trial.
- False alarm: a `different` response on a same trial.

Signal-detection scores use log-linear correction:

`corrected hit rate = (hits + 0.5) / (change trials + 1)`

`corrected false-alarm rate = (false alarms + 0.5) / (same trials + 1)`

`d-prime = z(corrected hit rate) - z(corrected false-alarm rate)`

The main binding metrics are:

- `wm_binding_dprime`
- `wm_binding_swap_dprime`
- `wm_binding_swap_cost`
- `wm_binding_balanced_accuracy`
- `bindingDPrimeNovel`
- `bindingMedianRt`
- `bindingOmissionRate`
- `bindingResponseBias`
- `swapLureAccuracy`

Swap cost is:

`bindingSwapCost = bindingDPrimeNovel - bindingDPrimeSwap`

Interpretation:

- Higher overall d-prime indicates better change discrimination.
- Higher swap d-prime indicates better token-location binding.
- Larger positive swap cost indicates greater difficulty rejecting a familiar token when it appears at the wrong location.

Exploratory K estimates are computed internally from same versus novel-change trials by set size, but are not used as headline public scores in v0.1.

## 8) Public score labels and launch bands

The public result displays two provisional cards:

- Working Memory Capacity
- Binding Precision

No combined working-memory standard score, percentile, diagnostic label, or IQ interpretation is displayed at launch.

### Working Memory Capacity bands

The launch Working Memory Capacity band is based on complex-span PCU, provided the processing validity gate is not failed.

| PCU | Launch band |
| ---: | --- |
| below .50 | Low task performance |
| .50 to below .65 | Developing |
| .65 to below .80 | Standard range |
| .80 to below .90 | Strong |
| .90 and above | Very strong |

These are provisional task bands, not population percentile boundaries.

### Binding Precision bands

The launch Binding Precision band is based on swap d-prime.

| Swap d-prime | Launch band |
| ---: | --- |
| below .50 | Low task performance |
| .50 to below 1.00 | Developing |
| 1.00 to below 1.50 | Standard range |
| 1.50 to below 2.00 | Strong |
| 2.00 and above | Very strong |

These are conventional/provisional discrimination bands, not population percentile boundaries.

Binding confidence is marked as limited when omissions exceed 10% of scored binding trials. Otherwise it is marked as calibrating.

## 9) Standardisation policy

At launch, WMC-B10 v0.1 displays provisional app bands only.

It does not display:

- standard scores
- percentiles
- IQ-style interpretation
- a combined global working-memory score

The future public standard-score scale, once justified by IQMindware data, is:

`standard score = 100 + 15z`

For higher-better metrics:

`z = (raw - mean) / SD`

For lower-better metrics:

`z = (mean - raw) / SD`

Standard scores should be introduced only after enough valid IQMindware WMC-B10 baseline data exist to estimate stable distributions, form effects, device effects, and reliability.

Recommended calibration stages:

- fewer than 100 valid baseline users: show provisional task bands, raw scores, and confidence labels only
- 100-299 valid baseline users: calculate empirical summaries and confidence intervals, but avoid stable norm claims
- 300-999 valid baseline users: consider provisional sample standard scores if distributions are stable
- 1000+ valid baseline users: fit stronger models for form equivalence, age/device effects, practice effects, reliability, and measurement error

## 10) Norm-building rules

Only these attempts can enter IQMindware G Track norm-building:

- participant opted into cloud benchmark contribution
- construct is `working_memory`
- role is baseline/pre-training
- attempt passes validity checks
- first valid eligible baseline attempt for that participant/construct

These attempts are excluded from norm-building:

- private guest attempts
- cloud personal attempts without benchmark contribution
- post-training attempts
- follow-up attempts
- invalid attempts
- repeat baseline attempts after a first valid eligible attempt

No percentile display is used until the relevant IQMindware calibration sample is at least `n >= 100`. For WMC-B10, launch public output remains provisional bands even before standard-score release.

## 11) Gain and confidence policy

The protocol is designed for training-gain measurement using the participant's baseline, post-training, and follow-up roles.

Raw gain can be computed as:

- post minus baseline for higher-better metrics
- baseline minus post for lower-better metrics
- follow-up minus baseline, or baseline minus follow-up, using the same direction rules

For this benchmark, higher is better for:

- complex-span PCU
- processing accuracy
- binding d-prime
- binding balanced accuracy
- swap-lure accuracy

Lower is better for:

- transposition rate
- intrusion rate
- binding omission rate
- positive swap cost, when both novel and swap discrimination are above chance

Standardised gain scores require enough paired IQMindware data to estimate expected retest change, form effects, practice effects, and uncertainty. Until that paired-change calibration is available, the app should report raw change and calibration status rather than overclaiming standardised improvement.

Recommended future gain reporting:

- raw change
- standardised change against paired baseline/post or baseline/follow-up distributions
- confidence interval or confidence band
- form-role adjustment
- sample size used for the gain norm

## 12) Proof export and app-stack use

Working Memory results are stored as G Track score objects under:

`construct: "working_memory"`

The proof payload can be passed to other IQMindware apps, including Attention Coach and WM Coach. Exported proof includes raw metric fields and public score objects so downstream coach apps can use the benchmark as external evidence without needing protected scoring internals.

## 13) Claims boundary

Allowed:

- repeated working-memory benchmark tracking
- raw complex-span and binding metrics
- clearly labelled provisional task bands
- baseline/post/follow-up change tracking once repeated data exist
- future standardised gain claims only after paired calibration data support them

Not allowed:

- clinical diagnosis
- working-memory impairment classification
- WAIS-equivalent claims
- IQ-score claims
- guaranteed training gains
- percentile or norm claims before calibration thresholds are met
- treating one short assessment as a complete model of working memory

## 14) Open vs protected

Open:

- task families
- trial counts
- construct labels
- raw metric definitions
- provisional band definitions
- standard-score formulas for future calibration
- norm eligibility rules
- claims boundaries

Protected:

- runtime source code
- anti-gaming controls
- abuse-detection thresholds
- private deployment infrastructure
- item-ordering or seed details that would make repeated testing easier to game

