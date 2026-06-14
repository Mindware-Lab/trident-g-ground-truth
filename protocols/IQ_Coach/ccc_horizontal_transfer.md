# 32. Extension: Horizontal Transfer Training and CCC Far-Transfer Metrics

## 32.1 Purpose

This extension turns the Cognitive Bandwidth task from a brief capacity estimate into a training task aligned with the Trident-G Far Transfer Protocol.

The goal is not only to improve performance inside one response wrapper.

The goal is to test whether the same controlled evidence-extraction skill survives a change in surface format.

In this component, the target invariant is:

```text
controlled majority extraction under masked, time-limited uncertainty
```

For the standard task, the invariant is:

```text
extract the majority absolute screen direction
```

For the relational task, the invariant is:

```text
extract the majority direction relative to a centre/frame
```

The training sequence should therefore be:

```text
train one wrapper
→ detect flattening or stable no-change
→ introduce a controlled wrapper perturbation
→ measure dip and recovery
→ mix wrappers
→ re-check after delay
→ compute CCC transfer metric
```

This is a within-layer horizontal-transfer cycle. It is not yet full protocol-level vertical transfer into working memory, SR inference or reasoning.

---

# 33. Wrapper terminology

## 33.1 Standard / absolute CCC wrappers

The standard task begins with horizontal left/right judgement.

```text
A1: abs_lr
User labels: Left / Right
Question: Were most arrows pointing left or right?
```

When the learning curve flattens or shows stable no-change, introduce the vertical screen-axis wrapper:

```text
A2: abs_ud
User labels: Up / Down
Question: Were most arrows pointing up or down?
```

Important terminology:

```text
abs_ud is a vertical screen-axis wrapper.
It is horizontal transfer inside the CCC layer.
It is not vertical transfer in the Trident-G protocol sense.
```

Later optional extension:

```text
A3: abs_diag
User labels: Diagonal A / Diagonal B
```

## 33.2 Relational / frame CCC wrappers

The relational task begins with in/out judgement relative to the centre.

```text
R1: rel_inout
User labels: Out / In
Question: Were most arrows pointing out or in?
```

When the learning curve flattens or shows stable no-change, introduce the rotational / tangential wrapper:

```text
R2: rel_cwccw
User labels: Circle Right / Circle Left
Question: Were most arrows pointing around the circle to the right or left?
```

The arrows remain static. The user is not judging motion. They are judging whether each arrow’s direction is tangentially clockwise or anticlockwise relative to its position around the centre.

Later optional extension:

```text
R3: rel_spiral
User labels: Spiral Out / Spiral In
```

Do not add spiral wrappers until `rel_inout` and `rel_cwccw` are reliable.

---

# 34. Horizontal transfer progression

## 34.1 Standard CCC progression

Use:

```text
abs_lr
→ abs_ud probe
→ abs_ud recovery
→ abs_lr / abs_ud mix
→ delayed mixed re-check
```

Later:

```text
abs_lr / abs_ud
→ abs_diag probe
→ abs_diag recovery
→ abs_lr / abs_ud / abs_diag mix
→ delayed mixed re-check
```

## 34.2 Relational CCC progression

Use:

```text
rel_inout
→ rel_cwccw probe
→ rel_cwccw recovery
→ rel_inout / rel_cwccw mix
→ delayed mixed re-check
```

Later:

```text
rel_inout / rel_cwccw
→ rel_spiral probe
→ rel_spiral recovery
→ rel_inout / rel_cwccw / rel_spiral mix
→ delayed mixed re-check
```

## 34.3 Rationale

The app should not simply ask:

```text
Did the user improve in the trained wrapper?
```

It should ask:

```text
Can the user recover the same majority-extraction operation when the response frame changes?
Can the recovery cost reduce over time?
Can the skill survive mixed-wrapper presentation?
Can it survive a delayed re-check?
```

---

# 35. Flattening and no-change detection

## 35.1 Why no power-law assumption

Do not assume that every user will show a clear power-law learning curve.

Some users may show:

```text
rapid early gain
slow noisy improvement
flat but stable performance
high volatility
under-engagement
timing-limited estimates
state-dependent dips
```

Therefore, wrapper switching should not depend on fitting a power-law curve.

Use a learning-state classifier based on slope, uncertainty, stability, timing and lapse checks.

## 35.2 Learning-state labels

Classify each wrapper into one of the following states:

| Learning state       | Meaning                                       | Action                                  |
| -------------------- | --------------------------------------------- | --------------------------------------- |
| **Improving**        | Rolling estimate is rising beyond noise       | Continue current wrapper                |
| **Flattening**       | Estimate has stabilised near local asymptote  | Prepare wrapper probe                   |
| **Stable no-change** | No meaningful change across enough data       | Consider wrapper probe or scaffold      |
| **Unstable**         | High volatility or wide posterior uncertainty | Continue calibration                    |
| **Timing limited**   | Display timing undermines estimate            | Avoid switch, fix timing issue          |
| **Lapse limited**    | Catch/easy errors or timeouts are high        | Slow pacing or simplify                 |
| **Overloaded**       | Accuracy collapses or RT instability rises    | Reduce difficulty                       |
| **Under-challenged** | Accuracy too high with low uncertainty        | Shorten ET or increase difficult trials |

## 35.3 Minimum data before detecting flattening

Do not trigger wrapper perturbation until the current wrapper has at least:

```text
minimum = 120 valid scored trials
preferred = 150–220 valid scored trials
minimum sessions = 3
timing_quality ≠ limited
lapse_rate acceptable
```

## 35.4 Smallest worthwhile change

Define a smallest worthwhile change threshold:

```text
SWD = max(0.15 bps, 0.25 × posterior_sd_pooled)
```

For relational scores:

```text
SWD_rel = max(0.15 relational bps, 0.25 × posterior_sd_pooled)
```

This prevents the app from overreacting to trivial changes.

## 35.5 Flattening rule

A wrapper is classed as flattening when:

```text
P(slope_recent > slope_min) < 0.30
AND
P(C_next_window - C_current > SWD) < 0.30
AND
balanced_accuracy is stable in or above the training band
AND
timing_quality is acceptable or good
AND
lapse_rate is not elevated
```

Suggested slope window:

```text
last 3–5 sessions
or
last 150–220 valid trials
```

## 35.6 Stable no-change rule

A wrapper is classed as stable no-change when:

```text
abs(C_current - C_previous_window) < SWD
AND
posterior_probability_of_improvement is between 0.40 and 0.60
AND
this pattern persists across at least two update windows
AND
timing_quality is not limited
AND
lapse_rate is acceptable
```

This is useful for users who do not show a clean improvement curve.

## 35.7 Do not perturb when failure is probably not learning-related

Do not introduce a new wrapper if no-change is probably caused by:

```text
timing-limited estimates
high lapse rate
high timeout rate
response bias
poor comprehension
low valid trial count
very wide posterior uncertainty
accuracy below 60% on current wrapper
```

In those cases, route to:

```text
more calibration
easier exposure times
more 5:0 catch/easy trials
slower pacing
instruction refresh
or rest / retry later
```

## 35.8 Probe rather than full switch

When flattening or stable no-change is detected, begin with a small probe rather than a full switch.

Example:

```text
80% current wrapper
20% new wrapper
```

Do not fully replace the current wrapper until the new wrapper shows recoverable performance.

---

# 36. Wrapper probe and recovery phases

## 36.1 Phase A: blocked current wrapper

Example standard task:

```text
100% abs_lr
```

Example relational task:

```text
100% rel_inout
```

Goal:

```text
establish local learning curve and stable rolling estimate
```

## 36.2 Phase B: new-wrapper probe

Example standard task:

```text
80% abs_lr
20% abs_ud
```

Example relational task:

```text
80% rel_inout
20% rel_cwccw
```

Goal:

```text
measure initial transfer dip
```

## 36.3 Phase C: recovery training

If the new wrapper is recoverable, increase its share.

Example:

```text
50% old wrapper
50% new wrapper
```

or:

```text
30% old wrapper
70% new wrapper
```

Goal:

```text
train recovery without losing the anchor estimate
```

## 36.4 Phase D: mixed-wrapper stability

Use unpredictable mixed blocks.

Example:

```text
random abs_lr / abs_ud
```

or:

```text
random rel_inout / rel_cwccw
```

Goal:

```text
test whether the majority-extraction operation survives wrapper uncertainty
```

## 36.5 Phase E: delayed mixed re-check

Re-test after at least one later session.

Example:

```text
session N:
mixed wrapper recovery

session N+1 or later:
delayed mixed wrapper re-check
```

Goal:

```text
test whether the recovered control operation survives time
```

---

# 37. Wrapper-specific estimates

Do not immediately collapse all wrappers into one score.

Estimate separately:

```text
C_abs_lr
C_abs_ud
C_abs_mix
C_abs_common
```

and:

```text
C_rel_inout
C_rel_cwccw
C_rel_mix
C_rel_common
```

The common score becomes meaningful only when the user has enough data across wrappers.

Minimum for common score:

```text
at least 120 valid trials in each active wrapper
at least one mixed-wrapper block
timing_quality not limited
```

User-facing names:

| User-facing score                | Internal metric                     | Meaning                                 |
| -------------------------------- | ----------------------------------- | --------------------------------------- |
| **Direction Bandwidth**          | `C_abs_common` or current `C_abs_*` | Absolute majority extraction capacity   |
| **Frame Bandwidth**              | `C_rel_common` or current `C_rel_*` | Relational majority extraction capacity |
| **Flexible Direction Bandwidth** | `C_abs_mix`                         | Standard CCC under wrapper switching    |
| **Flexible Frame Bandwidth**     | `C_rel_mix`                         | Relational CCC under wrapper switching  |
| **Wrapper Recovery**             | `W_recovery_abs`, `W_recovery_rel`  | Recovery after wrapper change           |

---

# 38. CCC far-transfer metrics

## 38.1 Interpretation boundary

The term “far-transfer metric” in this layer means:

```text
within-layer CCC transfer across changed wrappers and delayed re-checks
```

It does not mean:

```text
far transfer to IQ
far transfer to reasoning
far transfer to real-world performance
```

A safer internal term is:

```text
CCC Transfer Index
```

A user-facing term can be:

```text
Transfer Strength
```

## 38.2 Standard CCC Transfer Index

Internal name:

```text
FT_abs
```

User-facing name:

```text
Direction Transfer Strength
```

Meaning:

```text
How well standard direction-bandwidth skill survives a change from left/right to up/down, mixed presentation and delayed re-check.
```

## 38.3 Relational CCC Transfer Index

Internal name:

```text
FT_rel
```

User-facing name:

```text
Frame Transfer Strength
```

Meaning:

```text
How well relational frame-bandwidth skill survives a change from in/out to circle right/circle left, mixed presentation and delayed re-check.
```

---

# 39. Component metrics for CCC transfer

Each transfer index is built from several component metrics.

## 39.1 Initial switch cost

Measures the immediate performance dip when the wrapper changes.

```text
SwitchCost = max(0, C_A_pre - C_B_probe) / C_A_pre
```

Where:

```text
C_A_pre = recent stable estimate in trained wrapper
C_B_probe = first valid estimate in new wrapper probe
```

Interpretation:

```text
low switch cost = strong immediate portability
moderate switch cost + later recovery = useful transfer-learning signal
high switch cost + poor recovery = wrapper-bound performance
```

Do not treat a dip as a failure by itself. A moderate dip followed by recovery is expected and useful.

## 39.2 Wrapper recovery ratio

# Replacement for §39.2: Wrapper Recovery Profile

## 39.2 Wrapper recovery profile

Wrapper recovery should be treated as a graded transfer signal, not a binary pass/fail threshold.

The original trained wrapper usually has a familiarity advantage. A genuinely novel wrapper may initially produce a meaningful dip even when the underlying CCC operation is recoverable. Therefore, near-parity with the trained wrapper should be interpreted as strong recovery, not as the minimum criterion for adequate recovery.

Compute:

```text
RecoveryRatio = C_B_recovered / C_A_pre
```

Where:

```text
C_A_pre = recent stable estimate in trained wrapper
C_B_recovered = rolling estimate in the new wrapper after recovery training
```

Use posterior distributions where available:

```text
P_recovery_band = posterior probability that RecoveryRatio falls within each recovery band
```

Recommended recovery bands:

| Recovery band         |                        Criterion | Interpretation                                                         |
| --------------------- | -------------------------------: | ---------------------------------------------------------------------- |
| **Strong recovery**   |  `C_B_recovered ≥ C_A_pre - SWD` | New wrapper has recovered to near-parity with the trained wrapper      |
| **Moderate recovery** | `C_B_recovered ≥ 0.80 × C_A_pre` | Clear recovery under the new wrapper; useful transfer-learning signal  |
| **Weak recovery**     | `C_B_recovered ≥ 0.60 × C_A_pre` | Some recovery, but performance remains substantially wrapper-sensitive |
| **Poor recovery**     | `C_B_recovered < 0.60 × C_A_pre` | Performance remains strongly tied to the trained wrapper               |

Use the highest band with sufficient posterior support.

Suggested posterior rule:

```text
Assign a recovery band when P(band or better) ≥ 0.70
```

Example:

```text
If P(RecoveryRatio ≥ .80) = .76
and P(C_B_recovered ≥ C_A_pre - SWD) = .42

Label:
Moderate recovery
```

This avoids requiring near-parity for a positive transfer interpretation.

---

# Update to §39.2 interpretation

Do not treat a moderate dip as failure.

A useful wrapper-change pattern is:

```text
initial dip
→ partial recovery
→ reduced future switch cost
→ improved mixed-wrapper stability
→ delayed re-check survival
```

Interpretation:

| Pattern                                 | Meaning                                         |
| --------------------------------------- | ----------------------------------------------- |
| Small dip + strong recovery             | High immediate portability                      |
| Moderate dip + moderate/strong recovery | Good transfer-learning signal                   |
| Large dip + weak recovery               | Wrapper-bound performance likely                |
| Any dip + no delayed retention          | Same-session recovery may not have consolidated |

Near-parity is desirable, but it is not required for the app to detect useful horizontal-transfer progress.

---

## 39.3 Recovery efficiency

Measures how quickly the user recovers after the wrapper perturbation.

```text
RecoveryEfficiency = 1 - min(1, trials_to_recovery / target_recovery_trials)
```

Suggested target:

```text
target_recovery_trials = 150
```

Higher values mean faster recovery.

If recovery has not occurred:

```text
RecoveryEfficiency = 0
```

## 39.4 Mixed-wrapper stability

Measures whether the skill survives unpredictable wrapper switching.

```text
MixedStability = min(1, C_mix / mean(C_A_recent, C_B_recovered))
```

Where:

```text
C_mix = capacity estimate from mixed-wrapper trials
```

Interpretation:

```text
high mixed stability = user is extracting the invariant rather than relying on blocked-wrapper preparation
```

## 39.5 Delayed retention

Measures whether the recovered skill survives time.

```text
DelayedRetention = min(1, C_delayed_mix / C_mix_immediate)
```

Where:

```text
C_mix_immediate = mixed-wrapper estimate during initial recovery session
C_delayed_mix = mixed-wrapper estimate during later re-check
```

Suggested delayed re-check:

```text
next session
or
24+ hours later where possible
```

## 39.6 Data-quality weight

Transfer scores should be quality-weighted.

```text
Q = timing_quality_weight × confidence_weight × lapse_weight
```

Suggested values:

```text
timing_quality_weight:
good = 1.00
acceptable = 0.85
limited = 0.40

confidence_weight:
good = 1.00
moderate = 0.85
early = 0.60
calibrating = 0.30

lapse_weight:
low = 1.00
moderate = 0.80
high = 0.50
```

Do not display a strong transfer claim when `Q < 0.70`.

---

# 40. Composite CCC Transfer Index

## 40.1 Purpose

The Composite CCC Transfer Index estimates how well the underlying cognitive-control capacity operation survives a change in task wrapper.

It should not measure only whether the new wrapper reaches near-parity with the trained wrapper. In the Trident-G transfer logic, a temporary dip after wrapper change is expected. The important signal is whether the user recovers the same control operation, maintains it under mixed-wrapper conditions, and retains it after delay.

Do not include `SwitchCost` directly in the main composite. Report switch cost separately because an initial dip is a normal and useful part of transfer training.

---

## 40.2 Recovery score

Replace the previous single `WrapperRecovery` term with a graded recovery score.

First compute:

```text
RecoveryRatio = C_B_recovered / C_A_pre
```

Where:

```text
C_A_pre = recent stable estimate in the trained wrapper
C_B_recovered = rolling estimate in the new wrapper after recovery training
```

Convert the recovery band into a numeric component:

```text
Strong recovery   = 1.00
Moderate recovery = 0.75
Weak recovery     = 0.45
Poor recovery     = 0.15
```

Recommended MVP rule:

```text
Use banded RecoveryScore for user-facing labels.
Use continuous RecoveryRatio internally.
```

Optional continuous internal version:

```text
RecoveryScore = clamp(RecoveryRatio, 0, 1)
```

---

## 40.3 Composite formula

For each task type:

```text
FT = 100 × Q × weighted_mean(
  RecoveryScore,
  RecoveryEfficiency,
  MixedStability,
  DelayedRetention
)
```

Suggested weights:

```text
RecoveryScore:       30%
RecoveryEfficiency:  15%
MixedStability:      30%
DelayedRetention:    25%
```

Rationale:

```text
Recovery matters, but mixed-wrapper stability and delayed retention should carry more weight than near-parity alone.
```

---

## 40.4 Standard task formula

For standard / absolute CCC:

```text
FT_abs = 100 × Q_abs × weighted_mean(
  RecoveryScore_abs,
  RecoveryEfficiency_abs,
  MixedStability_abs,
  DelayedRetention_abs
)
```

User-facing label:

```text
Direction Transfer Strength
```

Interpretation:

```text
How well Direction Bandwidth survives a change from left/right to up/down, mixed presentation, and delayed re-check.
```

---

## 40.5 Relational task formula

For relational / frame CCC:

```text
FT_rel = 100 × Q_rel × weighted_mean(
  RecoveryScore_rel,
  RecoveryEfficiency_rel,
  MixedStability_rel,
  DelayedRetention_rel
)
```

User-facing label:

```text
Frame Transfer Strength
```

Interpretation:

```text
How well Frame Bandwidth survives a change from out/in to circle right/circle left, mixed presentation, and delayed re-check.
```

---

## 40.6 Interpretation

The transfer score should not answer only:

```text
Did the new wrapper reach the old wrapper?
```

It should answer:

```text
Did the underlying CCC operation recover under changed wrapper conditions,
remain usable under mixed presentation,
and survive a later re-check?
```

A useful transfer pattern is:

```text
initial dip
→ recovery
→ reduced future switch cost
→ mixed-wrapper stability
→ delayed retention
```

Near-parity is desirable, but it is not required for a positive transfer interpretation. Moderate recovery can still be a meaningful transfer-learning signal if mixed-wrapper stability and delayed retention are improving.

---

# 41. Transfer interpretation bands

Use cautious interpretation bands.

| Transfer score | User-facing label        | Interpretation                                                 |
| -------------: | ------------------------ | -------------------------------------------------------------- |
|          `<40` | **Still wrapper-bound**  | Performance is still closely tied to the trained wrapper       |
|        `40–59` | **Recovering**           | The skill is beginning to recover under wrapper change         |
|        `60–74` | **Partly transferable**  | The skill survives some wrapper change but is not yet stable   |
|        `75–89` | **Strong transfer**      | The skill recovers well across wrapper change and mixing       |
|          `90+` | **Very strong transfer** | The skill survives wrapper change, mixing and delayed re-check |

Add confidence:

```text
Transfer Strength: Strong
Confidence: Moderate
```

Avoid saying:

```text
This proves far transfer.
```

Use:

```text
This suggests the skill is becoming less wrapper-bound.
```

---

# 42. User-facing explanation

## 42.1 Standard CCC transfer copy

```text
Direction Transfer Strength shows whether your direction-bandwidth skill still holds when the direction frame changes.

If it improves, you may be relying less on one familiar left/right pattern and more on the underlying majority-extraction skill.
```

## 42.2 Relational CCC transfer copy

```text
Frame Transfer Strength shows whether your frame-bandwidth skill still holds when the relational frame changes.

If it improves, you may be getting better at extracting relations relative to a centre, rather than only learning the first in/out format.
```

## 42.3 Switch-cost copy

```text
When the task changes, performance often dips. That is expected.

The important signal is whether you recover the same skill under the new format.
```

## 42.4 Delayed re-check copy

```text
A delayed re-check tests whether the skill is still available later, not just during the same session.
```

---

# 43. Horizontal transfer algorithm

## 43.1 Training-state update

```text
function updateWrapperTrainingState(userId, taskType, wrapperId):

    estimate = getRollingEstimate(userId, taskType, wrapperId)

    slope = estimateRecentSlope(userId, taskType, wrapperId)

    volatility = estimateVolatility(userId, taskType, wrapperId)

    quality = getDataQuality(userId, taskType, wrapperId)

    if quality.timing == "limited":
        return "timing_limited"

    if quality.lapse_rate == "high":
        return "lapse_limited"

    if estimate.valid_trials < 120:
        return "calibrating"

    if posteriorP(slope > slope_min) >= 0.70:
        return "improving"

    if posteriorP(nextGain > SWD) < 0.30:
        return "flattening"

    if abs(estimate.current - estimate.previous) < SWD
       and estimate.p_improve between 0.40 and 0.60
       for two windows:
        return "stable_no_change"

    if volatility.high:
        return "unstable"

    return "continue_current"
```

## 43.2 Wrapper-switch decision

```text
function decideWrapperPerturbation(userId, taskType):

    state = updateWrapperTrainingState(userId, taskType, currentWrapper)

    if state == "flattening" or state == "stable_no_change":

        if taskType == "standard_abs" and currentWrapper == "abs_lr":
            return "probe_abs_ud"

        if taskType == "relational_frame" and currentWrapper == "rel_inout":
            return "probe_rel_cwccw"

        if currentWrapperPairRecovered:
            return "start_mixed_wrapper"

    if state == "timing_limited":
        return "fix_timing_or_avoid_short_ET"

    if state == "lapse_limited":
        return "slow_pacing_or_instruction_refresh"

    if state == "unstable":
        return "continue_calibration"

    return "continue_current"
```

## 43.3 Mixed-wrapper decision

```text
function decideMixedWrapperProgression(userId, taskType):

    recovery = computeWrapperRecovery(userId, taskType)

    if recovery.WrapperRecovery >= 0.85
       and recovery.confidence >= "moderate":
        return "increase_mixing"

    if recovery.WrapperRecovery < 0.65:
        return "return_to_blocked_with_scaffold"

    return "continue_recovery_phase"
```

---

# 44. Data-model additions

## 44.1 Wrapper state table

Add:

```text
cband_wrapper_states
```

Fields:

```text
id
user_id
task_type
wrapper_id
phase
n_valid_trials
rolling_C_mean
rolling_C_sd
recent_slope
learning_state
last_switch_decision
ready_for_probe
created_at
updated_at
```

Phase values:

```text
blocked
probe
recovery
mixed
delayed_recheck
retired
```

## 44.2 Transfer metrics table

Add:

```text
cband_transfer_metrics
```

Fields:

```text
id
user_id
task_type
from_wrapper_id
to_wrapper_id
C_A_pre
C_B_probe
C_B_recovered
C_mix
C_mix_immediate
C_delayed_mix
switch_cost
wrapper_recovery
recovery_efficiency
mixed_stability
delayed_retention
quality_weight
transfer_index
transfer_label
confidence_label
n_trials_used
n_sessions_used
created_at
```

## 44.3 Adaptive event additions

In `cband_adaptive_events`, add event types:

```text
flattening_detected
stable_no_change_detected
wrapper_probe_started
wrapper_recovery_started
mixed_wrapper_started
delayed_recheck_scheduled
transfer_index_updated
```

---

# 45. Session scheduling logic

The daily 4-minute CCC block should retain both measurement and training functions.

## 45.1 Standard block scheduling

Possible states:

```text
blocked abs_lr
probe abs_ud
recovery abs_ud
mixed abs_lr / abs_ud
delayed mixed re-check
```

Example allocation inside 2 min:

```text
blocked:
100% current wrapper

probe:
80% current wrapper
20% new wrapper

recovery:
50% current wrapper
50% new wrapper

mixed:
random 50/50 or adaptive mix

delayed re-check:
random mixed with no instruction change beyond normal task rule
```

## 45.2 Relational block scheduling

Possible states:

```text
blocked rel_inout
probe rel_cwccw
recovery rel_cwccw
mixed rel_inout / rel_cwccw
delayed mixed re-check
```

Example allocation inside 2 min:

```text
blocked:
100% current wrapper

probe:
80% current wrapper
20% new wrapper

recovery:
50% current wrapper
50% new wrapper

mixed:
random 50/50 or adaptive mix

delayed re-check:
random mixed with standard instructions
```

---

# 46. Instruction updates for new wrappers

## 46.1 Standard up/down instruction

```text
Now the direction frame changes.

Instead of left and right, choose whether most arrows point up or down.

The task is still the same: find the majority direction from a brief display.
```

Buttons:

```text
Up
Down
```

## 46.2 Relational circle instruction

```text
Now the frame changes.

The arrows are still static.

Choose whether most arrows point around the circle to the right or around the circle to the left.

The task is still the same: find the majority relation relative to the centre.
```

Buttons:

```text
Circle Right
Circle Left
```

## 46.3 Mixed-wrapper instruction

```text
The direction frame may change from trial to trial.

Read the button labels carefully and respond to the majority relation shown on that trial.
```

---

# 47. Progress display additions

Add a transfer panel after enough data exist.

## 47.1 Transfer panel

```text
Transfer Strength

Direction:
Recovering
Score: 58 / 100

Frame:
Partly transferable
Score: 66 / 100

These scores show how well your bandwidth skills survive changed task formats.
```

## 47.2 Early transfer copy

```text
Transfer score coming soon

First, the app needs enough data in the original format and the changed format.
```

## 47.3 Switch-cost panel

```text
New format introduced

A temporary dip is normal when the task changes.

The key signal is how quickly your score recovers.
```

---

# 48. 20-session transfer progression

Example progression:

| Sessions | Standard CCC                    | Relational CCC                         | Transfer purpose                     |
| -------: | ------------------------------- | -------------------------------------- | ------------------------------------ |
|      1–3 | `abs_lr` baseline               | `rel_inout` baseline                   | Establish initial estimates          |
|      4–6 | `abs_lr` training               | `rel_inout` training                   | Build stable local learning curves   |
|      7–9 | `abs_ud` probe if ready         | continue `rel_inout` or probe if ready | Test first absolute wrapper transfer |
|    10–12 | `abs_lr/abs_ud` recovery or mix | `rel_cwccw` probe if ready             | Test relational wrapper transfer     |
|    13–15 | mixed absolute                  | relational recovery or mix             | Strengthen mixed-wrapper stability   |
|    16–18 | delayed absolute re-check       | mixed relational                       | Test delayed transfer                |
|    19–20 | final mixed re-check            | final mixed re-check                   | Final transfer comparison            |

This schedule is adaptive. Do not force a wrapper switch if the user lacks enough data or data quality is poor.

---

# 49. Final extension summary

This extension adds a horizontal-transfer training layer to the CCC task.

The training target becomes:

```text
not just higher C_abs or C_rel in one wrapper

but recovery of C_abs and C_rel after wrapper change,
mixed-wrapper presentation
and delayed re-check
```

Standard CCC:

```text
left/right
→ up/down
→ mixed
→ delayed re-check
→ Direction Transfer Strength
```

Relational CCC:

```text
out/in
→ circle right/circle left
→ mixed
→ delayed re-check
→ Frame Transfer Strength
```

The main scientific safeguard is:

```text
trained-wrapper improvement is not treated as transfer.
Transfer requires recovery under changed wrapper conditions.
```

The main user-facing message is:

```text
Your score is not only about getting better at one version of the task.

The app also checks whether the skill survives when the format changes.
```
