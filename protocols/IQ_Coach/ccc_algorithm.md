# IQ Coach — Adaptive Cognitive Bandwidth Specification

## Standard and Relational MFT-M-Derived Capacity Layer

**Status:** Draft v0.2
**Product:** IQ Coach / Mindware Flow
**Layer:** Component 1 — Cognitive Bandwidth / Frame Bandwidth
**Purpose:** Implement a short adaptive masked-majority arrow task that estimates standard and relational cognitive-control bandwidth in bits/sec using an MFT-M-derived grouping-search model and rolling multi-session estimates.

---

# 1. Product summary

This component provides two brief adaptive capacity checks:

```text
2 min Standard Direction Bandwidth
+
2 min Relational Frame Bandwidth
```

The standard task adapts the published MFT-M / MFT-M-R information-rate framework.

The relational task applies the same information-rate framework to a novel frame-based judgement. It should be treated as an experimental extension until separately calibrated.

The component should not be presented as a full IQ test, clinical assessment, diagnostic tool or guaranteed IQ-improvement measure.

---

# 2. Core user-facing scores

| User-facing score       | Internal metric    | Evidence status        | Meaning                                                                             |
| ----------------------- | ------------------ | ---------------------- | ----------------------------------------------------------------------------------- |
| **Direction Bandwidth** | `C_abs`            | Strongest              | MFT-M-derived estimate of majority-direction extraction capacity in bits/sec        |
| **Frame Bandwidth**     | `C_rel`            | Experimental extension | MFT-M-derived estimate of relational majority extraction relative to a centre/frame |
| **Frame Cost**          | `C_abs - C_rel`    | Exploratory            | Estimated cost of relational/frame-based processing relative to absolute direction  |
| **Frame Efficiency**    | `C_rel / C_abs`    | Exploratory            | Ratio of relational to absolute bandwidth                                           |
| **Timing Quality**      | `timing_quality`   | Required control       | Whether display timing supports a trustworthy estimate                              |
| **Estimate Confidence** | `confidence_label` | Required control       | Whether the current estimate is calibrating, early, moderate or good confidence     |

Example result:

```text
Direction Bandwidth: 3.2 bits/sec
Confidence: Moderate

Frame Bandwidth: 2.7 relational bits/sec
Confidence: Moderate, experimental

Frame Cost: 0.5 bits/sec
Exploratory

Based on:
4 sessions
187 standard trials
172 relational trials
Timing quality: Good
```

---

# 3. Claims boundary

## 3.1 Allowed claims

The app may say:

```text
Direction Bandwidth adapts the published MFT-M / MFT-M-R information-rate framework.
Frame Bandwidth applies the same framework to a novel relational judgement.
Single-session scores are provisional.
Rolling estimates become more stable across repeated sessions.
The task tracks cognitive-control bandwidth under brief masked visual evidence.
```

## 3.2 Avoid

Do not say:

```text
This is the full published MFT-M.
This is a complete IQ test.
This proves IQ gain.
This diagnoses attention, ADHD, fatigue, flow or brain state.
Frame Bandwidth is already validated as a published measure.
Frame Cost is a validated cognitive-efficiency score.
```

## 3.3 Evidence-safe public wording

Use:

```text
Direction Bandwidth adapts a published cognitive-control capacity task that estimates information processing rate in bits/sec.

Frame Bandwidth is an experimental extension that asks whether the same kind of capacity can be estimated when the judgement depends on a relational frame.

Your single session gives a quick update. Your rolling score is the main estimate.
```

---

# 4. Task architecture

Each daily check contains two blocks.

```text
Block A:
Standard Direction Bandwidth
Duration: 2 min

Block R:
Relational Frame Bandwidth
Duration: 2 min
```

The two blocks share the same renderer, timing engine, masking engine, response logging, adaptive selector and scoring model.

They differ only in the response rule.

---

# 5. Standard Direction Bandwidth

## 5.1 User task

The user sees a brief display of arrows followed by a mask.

They respond:

```text
Left
Right
```

Question:

```text
Were most arrows pointing left or right?
```

## 5.2 Internal task type

```text
task_type = standard_abs
wrapper_id = abs_lr
metric = C_abs
unit = bits/sec
```

## 5.3 Evidence status

This is the most directly MFT-M-derived score.

It uses:

```text
masked majority-arrow displays
left/right judgement
information-rate scoring
grouping-search likelihood
CAT-style adaptive selection
rolling estimate
```

---

# 6. Relational Frame Bandwidth

## 6.1 User task

The user sees a brief display of arrows arranged around a centre point.

They respond:

```text
Out
In
```

Question:

```text
Were most arrows pointing out or in?
```

## 6.2 Internal task type

```text
task_type = relational_frame
wrapper_id = rel_inout
metric = C_rel
unit = relational bits/sec
```

## 6.3 Evidence status

This is a novel relational extension.

It should be treated as:

```text
MFT-M-derived
theoretically coherent
not yet independently validated
requires calibration against standard MFT-M and user outcomes
```

## 6.4 MVP relational wrapper

Use only:

```text
rel_inout
```

Do not include tangential, spiral or mixed relational wrappers in the first capacity-estimation MVP.

Later wrappers can be added only after `rel_inout` is reliable.

---

# 7. Stimulus design

## 7.1 Core display

Use static arrows only.

```text
centre point
8 possible item positions around an octagon
5 sampled positions per trial
one arrow per sampled position
backward mask at each possible position
```

## 7.2 Standard task arrows

For the standard task:

```text
arrow category = left / right
majority category = left or right
```

## 7.3 Relational task arrows

For the relational task:

```text
arrow category = out / in
```

Define each sampled position:

```text
p_i = [x_i, y_i]
centre = c
r_i = unit vector from centre to p_i
v_i = arrow direction vector
```

Classification:

```text
OUT if dot(v_i, r_i) > 0
IN  if dot(v_i, r_i) < 0
```

## 7.4 Geometry constants

For MVP:

```text
set_size = 5
possible_positions = 8
sampled_positions = 5
centre = screen centre
radius = fixed
arrow_size = fixed in CSS pixels but logged in rendered pixels
mask_positions = all 8 possible arrow positions
```

Do not jitter the centre in MVP.

## 7.5 Visual-angle note

The original MFT-M specified visual-angle parameters. A mobile/web implementation cannot assume fixed visual angle across devices unless viewing distance and screen density are controlled.

Therefore:

```text
research mode:
calibrate visual angle where possible

consumer mode:
use responsive pixel geometry
log device, viewport, pixel ratio and rendered size
do not claim exact visual identity with the lab task
```

---

# 8. Majority ratios

## 8.1 MVP five-arrow ratios

Use the five-arrow ratio set:

```text
5:0
4:1
3:2
```

| Ratio | Role            | Use                                      |
| ----- | --------------- | ---------------------------------------- |
| `5:0` | easiest / catch | lapse, attention and p0-monitoring trial |
| `4:1` | medium          | estimation and training                  |
| `3:2` | hard            | estimation and training                  |

## 8.2 Paper-faithful research mode

For closer MFT-M-R comparability, a research mode may include:

```text
2:1
4:1
3:2
×
0.25, 0.5, 1, 2 s
```

This requires set sizes 3 and 5.

## 8.3 MVP recommendation

For the consumer MVP:

```text
use five-arrow displays only
estimate from 4:1 and 3:2
use 5:0 as catch/lapse/asymptote trials
```

Label this as:

```text
five-arrow MFT-M-derived adaptive estimate
```

not:

```text
full MFT-M-R
```

---

# 9. Trial structure

## 9.1 Default trial sequence

```text
fixation
→ arrow stimulus
→ mask
→ response window
→ correctness feedback
→ inter-trial interval
```

## 9.2 Timing

Recommended mobile/web MVP timing:

| Event             |              Value | Notes                       |
| ----------------- | -----------------: | --------------------------- |
| Fixation          |         250–400 ms | short, stable               |
| Stimulus exposure |           adaptive | frame-counted               |
| Mask              |         300–500 ms | same positions as arrows    |
| Response window   | up to 1500–2200 ms | response allowed after mask |
| Feedback          |         150–300 ms | brief correctness pulse     |
| ITI               |         150–300 ms | short, calm                 |

## 9.3 Feedback

Use correctness feedback.

```text
correct:
brief blue/green pulse

incorrect:
brief red pulse or subtle shake

timeout:
brief neutral/amber pulse
```

Do not show scores during the task.

## 9.4 Sound

Sound is optional and off by default for research consistency.

If enabled:

```text
correct = soft click
incorrect = short low tone
timeout = neutral tick
```

Log sound setting.

---

# 10. Response setup / FKS

## 10.1 Keyboard mode

For lab-style keyboard mode:

```text
response hand = right hand preferred
finger mapping = index / middle
number of keys = 2
```

Example mapping:

```text
index = Left / Out
middle = Right / In
```

Counterbalance or make configurable where possible.

Log:

```text
input_mode
response_mapping
handedness_preference
key_codes
response_hand_reported
```

## 10.2 Mobile mode

For mobile:

```text
two large touch targets
left button and right button
large enough for thumb response
consistent placement across trials
```

The app should not assume keyboard and touch timing are equivalent.

Log:

```text
input_mode = touch
button_bounds
screen_size
device_pixel_ratio
orientation
touch_latency_available
```

---

# 11. Exposure-time handling

## 11.1 Required timing variables

Each trial must log:

```text
ET_requested_ms
ET_actual_ms
stimulus_onset_timestamp
stimulus_offset_timestamp
mask_onset_timestamp
mask_offset_timestamp
response_timestamp
RT_ms
frame_count_expected
frame_count_observed
refresh_rate_estimate
timing_quality
```

## 11.2 Effective processing time

Use an effective exposure value in scoring:

```text
tau_eff = ET_actual
```

For sensitivity analysis, compute:

```text
tau_min = min(ET_actual, RT_ms if response occurred before mask onset)
```

Primary scoring should use `ET_actual` if stimulus exposure is fixed and not terminated by response.

If the task allows response-terminated display, primary scoring must use:

```text
tau_eff = min(ET_actual, RT_ms)
```

## 11.3 Recommendation

For the cleanest psychometrics:

```text
do not terminate the stimulus on response
keep exposure fixed
show mask even if response has already occurred
log early responses
exclude implausibly early responses from high-confidence fitting
```

---

# 12. Information-rate model

## 12.1 Grouping-search quantities

For each condition:

```text
N_size = number of arrows
N_maj = majority sample size
N_con = number of arrows in the majority category
P_group = probability of obtaining a congruent majority sample
```

Formula:

```text
P_group = C(N_con, N_maj) / C(N_size, N_maj)
```

Entropy/load:

```text
H_condition = log2(N_maj / P_group)
```

Information rate:

```text
D_trial = H_condition / tau_eff_seconds
```

## 12.2 Five-arrow entropy table

For five-arrow displays:

| Ratio | N_size | N_maj | P_group | H_condition |
| ----- | -----: | ----: | ------: | ----------: |
| `5:0` |      5 |     3 |    1.00 |   1.58 bits |
| `4:1` |      5 |     3 |    0.40 |   2.91 bits |
| `3:2` |      5 |     3 |    0.10 |   4.91 bits |

Store values in a database/config table.

Do not hard-code inside the estimator.

---

# 13. Primary scoring model

## 13.1 Use full grouping-search likelihood

Primary scoring should use the full MFT-M grouping-search model, not the sigmoid approximation.

For each trial:

```text
P_VT = 1 - (1 - P_group)^(2^C × tau_eff / N_maj)
```

Then:

```text
P_correct = P_VT × p0 + (1 - P_VT) × p_guess
```

Where:

```text
C = capacity estimate in bits/sec
p0 = upper/asymptotic performance
p_guess = 0.50
```

## 13.2 p0 handling

For MVP:

```text
p0 prior mean = 0.97
pguess = 0.50 fixed
```

Do not estimate p0 from a single 2-minute block.

Update p0 slowly only from accumulated easy/catch trials if enough data exist.

Suggested p0 model:

```text
p0_user ~ Beta prior centred on 0.97
update from valid 5:0 and very-easy trials only
minimum update threshold = 100 easy/catch trials
```

## 13.3 Trial likelihood

For each trial:

```text
if correct:
    L_j = P_correct
else:
    L_j = 1 - P_correct
```

Total log-likelihood:

```text
LL(C) = Σ log(L_j)
```

Estimate:

```text
C_hat = argmax LL(C)
```

or Bayesian posterior:

```text
posterior(C) ∝ prior(C) × Π L_j
```

## 13.4 Separate estimates

Fit separately:

```text
C_abs
C_rel
```

Do not pool standard and relational trials into one capacity estimate.

---

# 14. Fallback scoring model

A lapse-adjusted sigmoid may be used only as:

```text
engineering fallback
calibration comparison
diagnostic robustness check
```

Fallback form:

```text
P_correct = chance + usable_range × sigmoid(theta - D_trial)
```

Where:

```text
chance = 0.50
usable_range = 1 - chance - lapse_rate
D_trial = H_condition / tau_eff
```

This model should not be the default scientific score unless it has been validated against the full grouping-search estimator in app data.

---

# 15. Adaptive trial selection

## 15.1 Goal

Select trials that are informative about the current capacity estimate while maintaining response balance, ratio balance and timing quality.

## 15.2 Candidate condition pool

For each block:

```text
ratio ∈ {4:1, 3:2}
ET ∈ adaptive exposure grid
target_category balanced
catch trials ∈ {5:0}
```

Suggested exposure grid:

```text
120 ms
160 ms
200 ms
250 ms
320 ms
400 ms
500 ms
650 ms
800 ms
1000 ms
```

Avoid ultra-short exposure values on devices with unreliable timing.

## 15.3 Selection rule

For each candidate condition:

```text
expected_information =
posterior_uncertainty_weight
× expected_discrimination
× timing_reliability_weight
× balance_weight
× anti_repetition_weight
```

Pick probabilistically from the top candidate conditions rather than always choosing the maximum.

This prevents repetitive item selection.

## 15.4 Starting trials

Each 2-minute block begins with a small warm-start set:

```text
6–8 trials
balanced target categories
mix of 4:1 and 3:2
moderate exposure values
```

Use the user’s rolling posterior from previous sessions if available.

If no prior exists:

```text
start around moderate exposure
avoid shortest ETs
fit after warm-start trials
```

## 15.5 Catch/lapse trials

Include:

```text
5–10% catch/easy trials
```

Purpose:

```text
detect lapses
monitor p0
detect misunderstanding
support timing and engagement flags
```

Do not use catch trials as primary capacity evidence.

---

# 16. Two-minute block structure

## 16.1 Standard block

```text
duration = 2 min
task_type = standard_abs
target score = C_abs
```

Expected structure:

```text
6–8 warm-start trials
30–50 adaptive estimation trials
3–6 catch/lapse trials
```

Actual trial count depends on pacing and device performance.

## 16.2 Relational block

```text
duration = 2 min
task_type = relational_frame
wrapper = rel_inout
target score = C_rel
```

Expected structure:

```text
6–8 warm-start trials
30–50 adaptive estimation trials
3–6 catch/lapse trials
```

## 16.3 Single-session interpretation

A single 2-minute block is:

```text
a provisional update
not a stable standalone capacity estimate
```

The app should show:

```text
Today’s update
Rolling estimate
Confidence label
```

---

# 17. Running / rolling estimates

## 17.1 Main principle

The rolling estimate is the primary user-facing score.

A single session updates the estimate, but progress is evaluated across a window.

## 17.2 Windows

Use trial-count-based windows rather than session-count alone.

Recommended windows per task type:

```text
minimum visible estimate = 80 valid trials
moderate-confidence estimate = 150–219 valid trials
good-confidence estimate = 220+ valid trials
```

Session equivalent:

```text
3 sessions = early estimate
5 sessions = usually moderate estimate
7–10 sessions = stable/high-confidence trend estimate
```

## 17.3 Rolling window rule

Primary displayed score:

```text
rolling posterior over most recent 150–220 valid trials
```

or:

```text
last 5 sessions, if trial count is adequate
```

Use whichever gives better stability.

## 17.4 Baseline and final programme comparison

For a 20-session programme:

```text
baseline window = sessions 1–5
midpoint window = sessions 8–12 or 11–15
final window = sessions 16–20
```

Compute:

```text
baseline_C_abs
current_C_abs
final_C_abs

baseline_C_rel
current_C_rel
final_C_rel
```

Improvement:

```text
ΔC_abs = current_C_abs - baseline_C_abs
ΔC_rel = current_C_rel - baseline_C_rel
ΔFrameCost = current_FrameCost - baseline_FrameCost
```

## 17.5 Improvement probability

Report improvement only when:

```text
valid_trials_baseline >= 120
valid_trials_current >= 120
timing_quality not limited
posterior_probability(ΔC > 0) >= 0.80
```

Suggested language:

```text
Likely improvement
Possible improvement
Stable
Too early to tell
Timing limited
```

Do not over-interpret small changes.

---

# 18. Confidence labels

## 18.1 Trial-count confidence

Per task type:

|                                    Valid trials | Label                 |
| ----------------------------------------------: | --------------------- |
|                                           `<80` | Calibrating           |
|                                        `80–149` | Early estimate        |
|                                       `150–219` | Moderate confidence   |
|                                          `220+` | Good confidence       |
| `two or more good windows across 7–10 sessions` | High-confidence trend |

## 18.2 Timing override

If timing quality is poor:

```text
confidence_label = Timing limited
```

even if trial count is high.

## 18.3 Volatility override

If the recent estimate is unstable:

```text
confidence_label = Volatile / still settling
```

Criteria:

```text
posterior_sd high
recent session-to-session change exceeds expected measurement error
lapse rate elevated
timeout rate elevated
```

---

# 19. Timing quality rules

## 19.1 Trial-level flags

Flag a trial if:

```text
frame_count_observed differs from expected by >1 frame
ET_actual error > 12 ms
tab visibility changed
screen refresh estimate unstable
response timestamp missing or implausible
RT < 100 ms
RT > response_window
```

## 19.2 Exclusion rules

Exclude from high-confidence capacity fitting:

```text
timing-contaminated trials
anticipatory responses
timeouts
incorrectly rendered trials
orientation / layout interruptions
```

Timeouts should still be logged and may contribute to engagement/lapse metrics, but should be handled carefully in the capacity model.

## 19.3 Device-level timing flag

If too many trials are timing-contaminated:

```text
timing_quality = limited
```

Suggested threshold:

```text
>15% timing-contaminated trials in current block
```

---

# 20. Error and lapse modelling

Track:

```text
accuracy
balanced_accuracy
median_RT
timeout_rate
lapse_rate
catch_trial_accuracy
fast_guess_rate
post-error slowing
response_bias
```

Do not rely on raw accuracy alone.

## 20.1 Response bias

For each block:

```text
left_response_rate
right_response_rate
out_response_rate
in_response_rate
```

Flag if:

```text
one response >70% without matching target distribution
```

## 20.2 Lapse estimate

Estimate lapse from:

```text
5:0 errors
timeouts on easy trials
implausibly slow responses
```

Use lapse to:

```text
reduce confidence
adjust p0 slowly
route easier trials if needed
```

---

# 21. Adaptive training band

The adaptive engine should keep the user near:

```text
70–82% balanced accuracy
```

Interpretation:

| Performance | Action                              |
| ----------- | ----------------------------------- |
| `>85%`      | harder ratio or shorter ET          |
| `70–82%`    | maintain / refine                   |
| `60–70%`    | slightly easier                     |
| `<60%`      | easier exposure, reduce hard trials |
| high lapse  | add catch trials and slow pacing    |
| poor timing | avoid short ETs                     |

---

# 22. Data model

## 22.1 Tables

Minimum Supabase/Postgres tables:

```text
cband_sessions
cband_blocks
cband_trials
cband_estimates
cband_posteriors
cband_adaptive_events
cband_device_timing
cband_user_baselines
```

## 22.2 Session fields

```text
id
user_id
started_at
ended_at
app_version
algorithm_version
device_id
input_mode
viewport_width
viewport_height
device_pixel_ratio
refresh_rate_estimate
overall_timing_quality
standard_block_id
relational_block_id
created_at
```

## 22.3 Block fields

```text
id
session_id
user_id
task_type
wrapper_id
duration_target_ms
duration_actual_ms
n_trials
n_valid_trials
n_scored_trials
catch_trial_count
accuracy
balanced_accuracy
median_rt_ms
timeout_rate
lapse_rate
response_bias
timing_quality
estimate_confidence
created_at
```

## 22.4 Trial fields

```text
id
session_id
block_id
user_id
trial_index
task_type
wrapper_id
set_size
ratio
n_size
n_maj
n_con
p_group
h_condition_bits
target_category
minority_category
arrow_positions_json
arrow_vectors_json
mask_positions_json
ET_requested_ms
ET_actual_ms
tau_eff_ms
mask_ms
response_window_ms
stimulus_onset_ts
stimulus_offset_ts
mask_onset_ts
mask_offset_ts
response_ts
rt_ms
response
correct_response
is_correct
is_timeout
is_catch_trial
is_scored
frame_count_expected
frame_count_observed
refresh_rate_estimate
timing_flag
feedback_type
input_mode
created_at
```

## 22.5 Estimate fields

```text
id
user_id
session_id
task_type
metric_name
algorithm_version
n_trials_used
n_sessions_used
window_type
window_start
window_end
C_hat
C_posterior_mean
C_posterior_sd
C_ci_lower
C_ci_upper
p_improvement_vs_baseline
baseline_C_mean
delta_from_baseline
confidence_label
timing_quality
lapse_rate
created_at
```

---

# 23. Estimation pseudocode

```text
function scoreCapacity(taskType, userId, window):

    trials = loadValidTrials(userId, taskType, window)

    if count(trials) < 80:
        confidence = "Calibrating"

    prior = loadPrior(userId, taskType)

    for each trial in trials:

        P_group = computePGroup(
            N_con = trial.n_con,
            N_maj = trial.n_maj,
            N_size = trial.n_size
        )

        H = log2(trial.n_maj / P_group)

        tau = trial.tau_eff_ms / 1000

        for each candidate C in C_grid:

            P_VT = 1 - (1 - P_group)^(2^C * tau / trial.n_maj)

            P_correct = P_VT * p0 + (1 - P_VT) * 0.50

            if trial.is_correct:
                logLikelihood[C] += log(P_correct)
            else:
                logLikelihood[C] += log(1 - P_correct)

    posterior = normalise(prior + logLikelihood)

    C_hat = posterior_mean_or_MAP(posterior)

    confidence = computeConfidence(
        n_valid_trials,
        posterior_sd,
        timing_quality,
        lapse_rate,
        volatility
    )

    return estimate
```

---

# 24. Adaptive selection pseudocode

```text
function selectNextTrial(taskType, posterior, blockState):

    candidates = generateCandidateConditions(taskType)

    for candidate in candidates:

        if violatesResponseBalance(candidate, blockState):
            candidate.weight *= 0.5

        if repeatsRecentCondition(candidate, blockState):
            candidate.weight *= 0.7

        if deviceTimingPoor(candidate.ET):
            candidate.weight *= 0.2

        expectedInfo = computeExpectedInformation(
            candidate,
            posterior
        )

        candidate.score =
            expectedInfo
            * balanceWeight(candidate, blockState)
            * timingReliabilityWeight(candidate)
            * antiRepetitionWeight(candidate, blockState)

    return weightedSample(topCandidates)
```

---

# 25. Running-estimate pseudocode

```text
function updateRunningEstimate(userId, taskType):

    recentTrials = loadMostRecentValidTrials(
        userId,
        taskType,
        target = 220,
        minimum = 80
    )

    recentEstimate = scoreCapacity(
        taskType,
        userId,
        recentTrials
    )

    baselineTrials = loadTrialsFromSessions(
        userId,
        taskType,
        sessions = 1..5
    )

    if validTrialCount(baselineTrials) >= 120:
        baselineEstimate = scoreCapacity(
            taskType,
            userId,
            baselineTrials
        )

        delta = recentEstimate.C_mean - baselineEstimate.C_mean

        pImprove = computePosteriorProbability(
            recentEstimate.C > baselineEstimate.C
        )
    else:
        delta = null
        pImprove = null

    saveEstimateSnapshot()
```

---

# 26. UI copy

## 26.1 Start screen

```text
Today’s Cognitive Bandwidth Check

You’ll complete two short blocks.

First:
Direction Bandwidth

Then:
Frame Bandwidth

Total time:
about 4 minutes
```

## 26.2 Standard instructions

```text
You will see five arrows very briefly.

Choose the direction most arrows were pointing.

Respond from your first clear impression.
```

Buttons:

```text
Left
Right
```

## 26.3 Relational instructions

```text
Now judge direction relative to the centre.

Out means arrows point away from the centre.

In means arrows point towards the centre.

Choose which relation most arrows show.
```

Buttons:

```text
Out
In
```

## 26.4 Results screen

```text
Today’s update

Direction Bandwidth
3.2 bits/sec
Moderate confidence

Frame Bandwidth
2.7 relational bits/sec
Experimental
Moderate confidence

Frame Cost
0.5 bits/sec
Exploratory

Your rolling score is based on 187 standard trials and 172 frame trials.
```

## 26.5 Early estimate copy

```text
Still calibrating

Your score will become more stable after a few sessions.

Single sessions can move around. The rolling estimate is the main number.
```

## 26.6 Improvement copy

```text
Likely improvement since baseline

Direction Bandwidth:
+0.4 bits/sec

Frame Bandwidth:
+0.5 relational bits/sec

This compares your latest rolling window with your first baseline window.
```

## 26.7 No-overclaim copy

```text
This is a cognitive-control bandwidth estimate from a brief adaptive task.

It is not a clinical diagnosis or a full IQ test.
```

---

# 27. 20-session progress structure

## 27.1 Programme windows

| Sessions | Role                 |
| -------: | -------------------- |
|      1–5 | Baseline calibration |
|     6–10 | Early change         |
|    11–15 | Consolidating trend  |
|    16–20 | Final comparison     |

## 27.2 Progress display

Show:

```text
Baseline window
Current rolling window
Final window after session 20
```

Metrics:

```text
Direction Bandwidth trend
Frame Bandwidth trend
Frame Cost trend
Confidence labels
Timing quality
```

## 27.3 Improvement logic

Use:

```text
posterior probability of improvement
minimum trial count
timing-quality gate
lapse-rate gate
```

Do not display a strong improvement claim if:

```text
baseline window has <120 valid trials
current window has <120 valid trials
timing quality is limited
lapse rate is high
posterior probability of improvement <0.80
```

---

# 28. Calibration and validation plan

## 28.1 Internal validation phase

Run a validation sample comparing:

```text
full or revised MFT-M / MFT-M-R
vs
app Direction Bandwidth
vs
app Frame Bandwidth
```

Primary questions:

```text
Does app C_abs correlate with MFT-M-R CCC?
Does app C_abs show acceptable test–retest reliability?
Does app C_rel show stable test–retest reliability?
Does C_rel explain variance beyond C_abs?
Does Frame Cost show reliable individual differences?
```

## 28.2 Device timing validation

Validate:

```text
browser timing
mobile timing
frame-count accuracy
ET_actual accuracy
touch latency
keyboard latency
```

Exclude or flag device classes with unreliable short exposure control.

## 28.3 Relational-task validation

Validate:

```text
rel_inout comprehension
response mapping
accuracy curves by ET and ratio
C_rel reliability
C_abs vs C_rel relationship
Frame Cost reliability
```

Do not add more relational wrappers until this is complete.

---

# 29. Build order

```text
1. Implement arrow renderer
2. Implement octagon position sampler
3. Implement standard left/right majority task
4. Implement mask renderer
5. Implement fixed timing and feedback
6. Implement full trial logging
7. Implement grouping-search scorer
8. Implement adaptive selector
9. Implement rolling estimate system
10. Implement confidence labels
11. Implement relational in/out wrapper
12. Implement separate C_rel scorer
13. Implement progress dashboard
14. Implement baseline vs current change logic
15. Implement timing-quality flags
16. Implement internal calibration export
```

---

# 30. MVP exclusions

Do not include in this layer yet:

```text
spiral wrappers
tangential wrappers
diagonal wrappers
Gabor patches
optic flow
n-back
SR path prediction
flow-state labels
HRV
clinical state interpretation
IQ-gain claims
```

This component should stay focused on:

```text
standard and relational masked-majority cognitive bandwidth
```

---

# 31. Final implementation summary

The production MVP should implement:

```text
2 min Direction Bandwidth
+
2 min Frame Bandwidth
```

using:

```text
five-arrow masked majority displays
adaptive ratio / exposure selection
full grouping-search likelihood scoring
fixed p0 prior around .97
pguess = .50
ET_actual / tau_eff timing correction
separate C_abs and C_rel estimates
rolling multi-session posterior estimates
conservative confidence labels
evidence-safe reporting
```

The most important design rule is:

```text
Do not make the single 2-minute block carry the scientific claim.

The rolling estimate is the product score.
```

The clean product promise is:

```text
Measure cognitive-control bandwidth briefly.
Track it across sessions.
Separate standard direction extraction from experimental relational frame extraction.
Show progress only when the data are stable enough.
```
