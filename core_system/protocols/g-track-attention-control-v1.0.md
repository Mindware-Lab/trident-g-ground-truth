---
Version: v1.0
Date: 2026-07-10
Owner: Mindware Lab (Trident G IQ)
Status: Public methodological protocol
Scope: HRP Lab G Track Attention Control benchmark
"Claims boundary": Non-clinical cognitive benchmark; provisional calibration where stated
---

# G Track Attention Control Benchmark Protocol

## 1) Purpose and scope

The G Track Attention Control benchmark is a repeated-measures attention assessment used in the HRP Lab G Track battery. It is designed to measure attention-control and sustained-attention evidence before, after, and after a delay from cognitive training.

It is assessment, not training. It is not a diagnostic instrument and should not be interpreted as a clinical measure of ADHD, neurological status, fatigue disorder, or treatment response.

The public construct labels are:

- Executive Attention
- Sustained Stability
- Alerting
- Orienting
- Attention Benchmark Composite

## 2) Source paradigms

The benchmark combines two established research paradigms and open implementation references:

- A short Attention Network Test style task, drawing on the Fan et al. Attention Network Task family and open web implementations such as Experiment Factory's MIT-licensed attention-network-test.
- A Sustained Attention to Response Task, drawing on the Robertson et al. SART paradigm and open web implementations such as MIT-licensed jsSART-style tasks.

These sources are methodological references. The G Track implementation uses Mindware-authored task generation, scoring, data-rights handling, and proof-export code. The public protocol therefore describes the abstract task method and scoring, not a verbatim copy of any third-party codebase.

The planning source used for this implementation is:

- `https://github.com/Mindware-Lab/IQ-Coach/blob/main/specs/tests/HRP_Lab_attention_test.md`

## 3) Battery placement and forms

The benchmark has three repeatable forms:

- `attention-a`
- `attention-b`
- `attention-c`

Each form can serve one of three roles:

- Baseline / pre-training
- Post-training
- Follow-up

The app tracks form identity and measurement role separately. Participants are counterbalanced across the three forms so that, over time, each form can contribute baseline data rather than one form always being the baseline form.

Only valid baseline/pre-training completions can contribute to IQMindware G Track sample norms. Post-training and follow-up completions are saved, scored, and exportable as proof/change evidence, but are excluded from norm-building.

## 4) Task structure

Each Attention Control form contains two task blocks.

### Short ANT block

The short ANT block contains:

- 32 practice trials
- 128 scored trials

The scored design crosses:

- 4 cue conditions: no cue, centre cue, double cue, spatial cue
- 2 flanker conditions: congruent, incongruent
- 2 target locations: above fixation, below fixation
- 2 target directions: left, right
- 4 repetitions of the full cell set

This produces:

`4 x 2 x 2 x 2 x 4 = 128` scored ANT trials.

The participant responds to the direction of the centre arrow while ignoring flankers. The centre arrow can point left or right. Flankers are either congruent with the centre arrow or incongruent with it. Cues manipulate alerting and orienting information before the target.

### SART block

The SART block contains:

- 18 practice trials
- 225 scored trials

The stimulus set is the digits 1 through 9. The no-go digit is 3. Participants press for every digit except 3 and withhold the response for 3.

The scored block contains 25 appearances of each digit:

- 200 go trials
- 25 no-go trials

The public method treats SART as a sustained-attention and response-inhibition/lapse-stability benchmark. It is not interpreted as a standalone diagnostic test.

## 5) Trial-level response rules

### ANT

For each ANT trial the recorded fields include:

- cue type
- flanker type
- target location
- target direction
- participant response
- response made or omitted
- reaction time in milliseconds
- correctness
- practice/scored status

Only scored trials enter the public summary metrics. Correct scored trials with plausible reaction times are used for reaction-time network scores.

### SART

For each SART trial the recorded fields include:

- digit
- go/no-go status
- participant response made or withheld
- reaction time in milliseconds when a response is made
- correctness
- practice/scored status

Only scored trials enter public summary metrics.

## 6) Raw scoring

All canonical scoring is performed server-side from the submitted trial rows.

### ANT raw metrics

ANT scoring uses the 128 scored ANT trials.

Correct scored trials with reaction times from 200 ms to 1700 ms are treated as valid reaction-time trials for median RT summaries.

The main raw ANT metrics are:

- `ant_accuracy`: proportion correct across all scored ANT trials
- `ant_median_rt_ms`: median reaction time across valid correct scored trials
- `ant_conflict_cost_ms`: median incongruent RT minus median congruent RT
- `ant_alerting_benefit_ms`: median no-cue RT minus median double-cue RT
- `ant_orienting_benefit_ms`: median centre-cue RT minus median spatial-cue RT

Interpretation direction:

- Lower conflict cost is better.
- Higher alerting benefit is better when interpreted as efficient use of alerting cues.
- Higher orienting benefit is better when interpreted as efficient use of spatial information.

The executive-attention public score is based on conflict cost. Alerting and orienting are shown as secondary network scores and should be interpreted more cautiously from a short ANT.

### SART raw metrics

SART scoring uses the 225 scored SART trials.

The main raw SART metrics are:

- `sart_commission_error_rate`: responses made on no-go trials divided by no-go trials
- `sart_omission_error_rate`: missed responses on go trials divided by go trials
- `sart_median_go_rt_ms`: median reaction time on valid go responses
- `sart_go_rt_cv`: coefficient of variation of go-trial reaction time
- `sart_time_on_task_error_slope`: linear slope of error rate across five 45-trial quintiles

Interpretation direction:

- Lower commission error rate is better.
- Lower omission error rate is better.
- Lower go RT coefficient of variation is generally interpreted as more stable responding.
- A more positive time-on-task error slope indicates greater worsening across the task.

## 7) Standardisation

The public standard-score scale is:

`standard score = 100 + 15z`

For higher-better metrics:

`z = (raw - mean) / SD`

For lower-better metrics:

`z = (mean - raw) / SD`

Display values may be clamped for user-interface presentation, but canonical raw metrics and unclamped standard-score values are retained in the score object where available.

### ANT launch anchors

At launch, the app uses conservative ANT seed anchors for provisional standard scores:

| Public score | Raw metric | Mean | SD | Direction |
| --- | --- | ---: | ---: | --- |
| Executive Attention | conflict cost | 84 ms | 25 | lower better |
| Alerting | alerting benefit | 47 ms | 18 | higher better |
| Orienting | orienting benefit | 51 ms | 21 | higher better |

These are labelled as seed-calibrated scores. They are intended as conservative launch anchors, not final IQMindware norms.

### SART launch calibration

SART metrics are based on the published SART paradigm and standard sustained-attention scoring conventions. Standardised SART scores are calibrated against the current IQMindware G Track sample as the sample grows.

At launch:

- SART raw metrics are shown.
- Sustained Stability is marked as collecting/calibrating.
- The attention composite is withheld until SART sample calibration is available.

## 8) Composite policy

The planned public composite is:

- Executive Attention from ANT conflict cost
- Sustained Stability from SART lapse, error, and reaction-time stability metrics
- Alerting from ANT alerting benefit
- Orienting from ANT orienting benefit

At launch, the app may compute an internal ANT seed composite from the three seed-calibrated ANT network scores, but the public Attention Benchmark Composite is withheld until SART calibration is available. This avoids presenting a composite that looks more mature than the available norming data justify.

## 9) Norm-building rules

Only these attempts can enter IQMindware G Track norm-building:

- participant opted into cloud benchmark contribution
- construct is `attention_control`
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

No percentile display is used until the relevant IQMindware calibration sample is at least `n >= 100`. Early calibration labels can be shown before percentile release, but should remain visibly provisional.

## 10) Gain and confidence policy

The protocol is designed for training-gain measurement using the participant's baseline, post-training, and follow-up roles.

Raw gain can be computed as:

- post minus baseline for higher-better metrics
- baseline minus post for lower-better metrics
- follow-up minus baseline, or baseline minus follow-up, using the same direction rules

Standardised gain scores require enough paired IQMindware data to estimate expected retest change, form effects, practice effects, and uncertainty. Until that paired-change calibration is available, the app should report raw change and calibration status rather than overclaiming standardised improvement.

Recommended future gain reporting:

- raw change
- standardised change against paired baseline/post or baseline/follow-up distributions
- confidence interval or confidence band
- form-role adjustment
- sample size used for the gain norm

## 11) Proof export and app-stack use

Attention Control results are stored as G Track score objects under:

`construct: "attention_control"`

The proof payload can be passed to other IQMindware apps, including Attention Coach and WM Coach. Exported proof includes raw metric fields and public score objects so downstream coach apps can use the attention benchmark as external evidence without needing item keys or protected scoring internals.

## 12) Claims boundary

Allowed:

- Repeated attention benchmark tracking
- Raw ANT/SART metric reporting
- Clearly labelled provisional ANT seed standard scores
- Clearly labelled SART calibration status
- Baseline/post/follow-up change tracking once repeated data exist

Not allowed:

- Clinical diagnosis
- ADHD classification
- Treatment-response claims
- Guaranteed training gains
- Percentile or norm claims before calibration thresholds are met
- Treating one short ANT completion as a definitive profile of all attention networks

## 13) Open vs protected

Open:

- Task families
- Trial counts
- Construct labels
- Raw metric definitions
- Standard-score formulas
- Norm eligibility rules
- Claims boundaries

Protected:

- Runtime source code
- Anti-gaming controls
- Abuse-detection thresholds
- Private deployment infrastructure
- Any item-ordering or seed details that would make repeated testing easier to game
