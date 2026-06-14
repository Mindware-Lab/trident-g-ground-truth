# IQ Coach — Gabor Patch SR Component Specification

## Component 3: Pattern Binding + Path Prediction

**Status:** Draft v0.1
**Product:** IQ Coach / Trident G app
**Layer:** Component 3 — Gabor Pattern Binding + SR Path Prediction
**Depends on:**

* Component 1: Cognitive Bandwidth / Frame Bandwidth
* Component 2: Frame Memory / Majority Memory

**Primary user-facing labels:**

* Pattern Binding
* Change Tracking
* Path Prediction
* Lure Resistance
* Wrapper Recovery
* Delayed Recovery

**Internal metrics:**

* `A_Bind`
* `C_Bind`
* `R_Bind`
* `S_Horizon`
* `I_Lure`
* `W_Recovery`
* `D_Retention`

---

# 1. Core purpose

This component trains the user to:

```text
bind visual pattern features
track how patterns transform
learn predictable transition structures
use current states to anticipate future states
recover the same relation after delay or wrapper change
```

The component is not a standard n-back game. It is an SR-inspired visual prediction layer.

The user should experience it as:

```text
watch patterns unfold
learn the hidden rule
predict where the pattern is going
spot when the path breaks
```

The internal training target is:

```text
state → transition → successor state → reachable path
```

---

# 2. Key design decision: continuous gameplay blocks

This component should **not** ask the user a question after every stimulus.

Avoid this pattern:

```text
pattern appears
question
pattern appears
question
pattern appears
question
```

Use this instead:

```text
continuous stream block
→ sparse prediction probe
→ continue stream
→ occasional lure or path break
→ block summary
```

The task should feel like a flowing pattern game, not a sequence of interrupted test items.

## 2.1 Probe density rule

Recommended MVP rule:

```text
80–90% of events = continuous unqueried stream events
10–20% of events = scored probes, decision points or path-break checks
```

In early tutorials, probe density can be higher briefly. In scored gameplay, keep probes sparse.

## 2.2 Block-level structure

Each block should contain:

```text
1. Observe phase
2. Predictive stream phase
3. Sparse probe phase
4. Lure / break phase
5. Short recovery or summary
```

Example 90-second block:

```text
0–15 sec: learn the sequence
15–60 sec: continuous pattern stream
60–75 sec: 2–3 sparse prediction probes
75–90 sec: lure or path-break challenge
```

---

# 3. Stimulus system

## 3.1 Gabor token

Each Gabor patch is a visual state token.

Recommended MVP state vector:

```text
S = [location, orientation, spatial_frequency]
```

Optional later dimensions:

```text
contrast
phase
clarity/noise
field density
```

Do not introduce too many dimensions in MVP.

## 3.2 Recommended feature bins

### Location

Use four fixed positions:

```text
top
right
bottom
left
```

or:

```text
upper-left
upper-right
lower-left
lower-right
```

For MVP, cardinal positions are cleaner because they map easily onto order, path and transition structures.

### Orientation

Use four orientation bins:

```text
0°
45°
90°
135°
```

Because pure Gabor orientation is 180° periodic, avoid treating 0° and 180° as different unless a directional cue is added.

### Spatial frequency

Use four log-spaced levels:

```text
f1 = coarse
f2 = medium-coarse
f3 = medium-fine
f4 = fine
```

The user should not see these labels. They simply see patterns.

## 3.3 Internal state entropy

With four values per dimension:

```text
location = log2(4) = 2 bits
orientation = log2(4) = 2 bits
spatial_frequency = log2(4) = 2 bits
```

Full token state:

```text
6 feature bits
```

Use these values for internal demand calculation, not for user-facing explanation.

---

# 4. Core game modes

Component 3 has four modes.

```text
Mode 3A: Pattern Binding
Mode 3B: Change Tracking
Mode 3C: Path Prediction
Mode 3D: Wrapper / Delay Recovery
```

These should be implemented as related blocks, not as separate unrelated games.

---

# 5. Mode 3A — Pattern Binding

## 5.1 Purpose

Train the ability to remember what belongs with what.

Examples:

```text
orientation belongs with location
orientation belongs with spatial frequency
location belongs with spatial frequency
full token belongs with prior state
```

## 5.2 User-facing task

The user watches short pattern pairs or fields.

Occasional probe:

```text
Did this pattern belong here?
```

Buttons:

```text
Same pairing
Swapped pairing
```

or:

```text
Belongs
Doesn’t belong
```

## 5.3 Gameplay structure

Avoid asking after every pair. Use a short stream of bindings first.

Example:

```text
A appears at top
B appears at right
C appears at bottom
A returns at top
B returns at right
probe: A appears at bottom
```

Correct response:

```text
Doesn’t belong
```

## 5.4 Lures

Use:

```text
same feature, wrong location
same location, wrong orientation
same orientation, wrong frequency
recent correct pair, wrong temporal position
partial match without full binding match
```

## 5.5 Metric

```text
A_Bind = H_binding × lag_or_delay at criterion
```

User-facing label:

```text
Pattern Binding
```

---

# 6. Mode 3B — Change Tracking

## 6.1 Purpose

Train the ability to track transformations rather than isolated states.

The user learns:

```text
what changed?
what stayed invariant?
did the same change continue?
did the pattern break?
```

## 6.2 Transformation types

Use simple transformations first:

```text
orientation +1 step
orientation -1 step
frequency +1 step
frequency -1 step
location clockwise
location anticlockwise
```

Then combine dimensions:

```text
orientation +1 and frequency -1
location clockwise and orientation +1
frequency +1 while orientation stays fixed
```

## 6.3 User-facing task

During the stream, the user watches a pattern evolve.

Sparse probes ask:

```text
Is the change still continuing?
```

Buttons:

```text
Still follows
Breaks pattern
```

Alternative copy:

```text
Same change
Different change
```

## 6.4 Continuous block example

Hidden rule:

```text
orientation rotates +45° each step
```

Stream:

```text
0° → 45° → 90° → 135°
```

No question appears during the first four events.

Then probe:

```text
next pattern = 0°
```

Correct response:

```text
Still follows
```

Lure:

```text
next pattern = 90°
```

Correct response:

```text
Breaks pattern
```

## 6.5 Metric

```text
R_Bind = H_relation × horizon_or_lag
```

User-facing label:

```text
Change Tracking
```

---

# 7. Mode 3C — Path Prediction / SR Stream

## 7.1 Purpose

Train behavioural SR-style successor prediction.

The user learns that current pattern states imply likely next states.

The target is:

```text
current state
→ likely successor
→ future reachable state
→ target still reachable or blocked
```

This is not a direct neural SR measure. It is a behavioural successor-prediction task.

## 7.2 Transition map

Each block uses a hidden transition graph.

Example simple chain:

```text
A → B → C → D
```

Example branching path:

```text
A → B → D
A → C → E
```

Example probabilistic transition:

```text
A → B with 80%
A → C with 20%
```

Example blocked path:

```text
A → B → C → target
A → D → blocked
```

## 7.3 Gameplay rule

The user watches the stream and gradually learns which states tend to follow which.

Do not ask:

```text
Is this the expected next state?
```

after every step.

Instead, ask occasionally:

```text
Which pattern is most likely next?
```

or:

```text
Can this path still reach the target?
```

## 7.4 Probe types

Use sparse probes.

### Probe A — next-state prediction

Prompt:

```text
What comes next?
```

Responses:

```text
two or three Gabor options
```

### Probe B — target reachability

Prompt:

```text
Can this path still reach the target?
```

Responses:

```text
Still reachable
Blocked
Not sure
```

### Probe C — path continuation

Prompt:

```text
Does this step fit the path?
```

Responses:

```text
Fits
Breaks
```

### Probe D — delayed successor

Prompt:

```text
Two steps ahead, where should the path go?
```

Responses:

```text
two or three Gabor options
```

Use Probe D only after the user has stabilised one-step prediction.

## 7.5 Metric

```text
SR_load = -log2 P(S_future | S_current) × horizon
```

Primary score:

```text
S_Horizon = maximum horizon solved at criterion
```

User-facing label:

```text
Path Prediction
```

Consumer explanation:

```text
How far ahead you can use a pattern’s current state to predict where it can go next.
```

---

# 8. Mode 3D — Wrapper / Delay Recovery

## 8.1 Purpose

Test whether the user recovers the same relation after:

```text
surface change
layout change
feature emphasis change
delay
interference
```

This is the transfer-critical part of Component 3.

## 8.2 Gabor-only wrapper changes for MVP

Before adding optic flow, use internal Gabor wrapper perturbations:

```text
single patch → two-patch display
central stream → four-location field
orientation-first → frequency-first
high contrast → lower contrast
fixed location → rotating location
clean pattern → mild noise
```

The underlying relation remains the same.

Example:

```text
Wrapper A:
orientation increases by +1 each step

Wrapper B:
spatial frequency increases by +1 each step

Invariant:
ordered +1 progression
```

## 8.3 Recovery block

Structure:

```text
train relation in Wrapper A
brief switch to Wrapper B
expect temporary dip
continue until recovery
mix A/B trials
delayed re-check later
```

## 8.4 Metric

```text
W_Recovery = performance_after_wrapper_shift / performance_before_wrapper_shift
```

Also compute:

```text
swap_cost_accuracy
swap_cost_RT
recovery_slope
trials_to_recovery
```

User-facing label:

```text
Wrapper Recovery
```

---

# 9. Four SR relation families

The Gabor SR component should train the same four families used by the reasoning layer.

## 9.1 Order & Chains

Hidden operation:

```text
A > B > C
```

Gabor implementation:

```text
f4 finer than f3
f3 finer than f2
therefore f4 finer than f2
```

Gameplay:

```text
stream shows ordered states
sparse probe asks whether a later state is still above / below another
```

User-facing label:

```text
Chain Flow
```

## 9.2 Transformations & Analogies

Hidden operation:

```text
A:B :: C:D
```

Gabor implementation:

```text
A changes to B by +1 orientation step
C changes to D by +1 orientation step
```

Gameplay:

```text
stream shows transformations
sparse probe asks whether the same change appeared again
```

User-facing label:

```text
Change Flow
```

## 9.3 Rules & Constraints

Hidden operation:

```text
if condition X holds, path is valid
if condition X fails, path is blocked
```

Gabor implementation:

```text
orientation + frequency combination defines valid state
wrong conjunction defines blocked state
```

Gameplay:

```text
user learns which pattern combinations allow continuation
sparse probe asks whether the path remains valid
```

User-facing label:

```text
Rule Flow
```

## 9.4 Prediction & Strategy

Hidden operation:

```text
current state constrains future path
```

Gabor implementation:

```text
A usually leads to B
B usually leads to target
C leads away from target
```

Gameplay:

```text
user predicts which successor keeps the path alive
```

User-facing label:

```text
Path Flow
```

---

# 10. Trial and event structure

Because the gameplay is continuous, distinguish between:

```text
stream events
probe events
lure events
block outcomes
```

Do not treat every visual event as a question trial.

## 10.1 Stream event

A stream event is an unqueried pattern presentation.

Fields:

```text
event_id
block_id
event_index
state_id
state_vector
transition_from_previous
expected_successor
wrapper_id
display_ms
iti_ms
```

## 10.2 Probe event

A probe event is a scored decision point.

Fields:

```text
probe_id
block_id
event_index
probe_type
target_state
foil_state
correct_response
user_response
rt_ms
correct
confidence_optional
```

## 10.3 Lure event

A lure event is an attractive wrong state.

Fields:

```text
lure_type
lure_similarity
wrong_feature_match
wrong_relation_match
wrong_lag_match
wrong_path_match
```

## 10.4 Block summary

At the end of a block, compute:

```text
probe_accuracy
balanced_accuracy
median_rt
lure_false_acceptance
horizon_level
relation_family
wrapper_id
difficulty_level
```

---

# 11. Recommended block types

## 11.1 Binding Block

Duration:

```text
60–90 seconds
```

Content:

```text
12–20 stream events
3–5 sparse probes
1–2 swap lures
```

Purpose:

```text
train feature-role binding
```

## 11.2 Change Block

Duration:

```text
90–120 seconds
```

Content:

```text
16–24 stream events
3–6 sparse probes
2–3 transformation lures
```

Purpose:

```text
train transformation tracking
```

## 11.3 Path Block

Duration:

```text
120–180 seconds
```

Content:

```text
20–35 stream events
4–8 sparse probes
2–4 path lures
```

Purpose:

```text
train successor prediction
```

## 11.4 Recovery Block

Duration:

```text
90–150 seconds
```

Content:

```text
Wrapper A refresher
Wrapper B perturbation
A/B mixed recovery
sparse probes
```

Purpose:

```text
measure wrapper recovery
```

---

# 12. Session structure

## 12.1 Standard Component 3 session

Duration:

```text
8–10 minutes
```

Recommended structure:

| Block                   | Duration | Purpose                                |
| ----------------------- | -------: | -------------------------------------- |
| Pattern Binding warm-up |   90 sec | stabilise feature-role binding         |
| Change Tracking block   |    2 min | train transformations                  |
| Path Prediction stream  |    3 min | train successor prediction             |
| Lure / Recovery block   |   90 sec | test near-miss resistance and recovery |
| Summary                 |   30 sec | show progress                          |

## 12.2 Full core-session placement

Inside the wider IQ Coach session:

```text
Pattern Binding: 2 min
Change Tracking: 3 min
Path Prediction stream: 4 min
```

This matches the wider architecture while keeping Component 3 as one coherent gameplay layer.

## 12.3 Benchmark session

Every 5–7 sessions, include:

```text
mixed relation families
mixed Gabor wrappers
delayed probes from prior sessions
path prediction at several horizons
lure-controlled probes
```

Benchmark duration for Component 3:

```text
5–7 minutes inside a 20–25 minute benchmark session
```

---

# 13. Adaptive difficulty

## 13.1 Training band

Use:

```text
target balanced accuracy = 70–82%
```

Adapt at block level, not after every event.

## 13.2 Difficulty dimensions

Increase only one dimension at a time.

Recommended ladder:

```text
1. Increase stream length
2. Increase probe horizon
3. Add lures
4. Add feature dimensions
5. Add branching transitions
6. Add probabilistic successors
7. Add wrapper perturbation
8. Add delay / re-entry
```

## 13.3 Difficulty examples

### Easy

```text
one feature dimension
deterministic chain
one-step prediction
no lures
same wrapper
```

### Medium

```text
two feature dimensions
two-step prediction
one lure type
same relation under mild wrapper shift
```

### Hard

```text
three feature dimensions
branching path
probabilistic successor
three-step horizon
near-miss lures
mixed wrapper
delayed re-check
```

---

# 14. Feedback design

## 14.1 During stream

Keep the stream clean.

Do not show:

```text
scores
technical labels
capacity estimates
difficulty numbers
```

Use only:

```text
subtle progress ring
soft transition animation
minimal sound or haptic pulse
```

## 14.2 After sparse probe

Use brief feedback:

```text
correct = soft blue/cyan pulse
incorrect = brief red or amber pulse
```

Feedback should be short enough not to break the flow.

## 14.3 After block

Show one plain-language result.

Examples:

```text
You tracked the pattern well.
The next block will stretch the path by one step.

You caught most near-misses.
Next: same rule, new surface.

The path became harder after the switch.
We’ll practise recovering the same rule.
```

---

# 15. User-facing copy

## Component card

Title:

```text
Pattern Path
```

or:

```text
Path Prediction
```

Subtitle:

```text
Track how visual patterns change and predict where they are going next.
```

Buttons:

```text
Start Path Prediction
Practise Pattern Binding
Review Progress
```

## Tutorial copy

```text
Patterns will change over time.
Your job is to notice the hidden rule.

Most of the time, just watch the stream.
Sometimes you’ll be asked what should come next
or whether the pattern has broken.

Try to follow the relation, not the surface.
```

## Probe copy

Use simple prompts:

```text
What comes next?
```

```text
Does this still fit the path?
```

```text
Can this path still reach the target?
```

Avoid technical prompts:

```text
successor representation
SR horizon
transition surprisal
relational invariant
```

These remain internal.

---

# 16. Metrics and scoring

## 16.1 Pattern Binding

```text
A_Bind = H_binding × delay_or_lag at criterion
```

Tracks:

```text
correct binding probes
swap-lure rejection
partial-match false alarms
binding recovery after delay
```

## 16.2 Change Tracking

```text
R_Bind = H_relation × horizon_or_lag
```

Tracks:

```text
same-change detection
transformation continuation
transformation break detection
cross-feature transformation recovery
```

## 16.3 Path Prediction

```text
S_Horizon = max k where successor prediction performance ≥ criterion
```

Trial demand:

```text
SR_load = -log2 P(S_future | S_current) × horizon
```

Tracks:

```text
one-step prediction
two-step prediction
three-step prediction
reachability judgement
blocked-path detection
```

## 16.4 Lure Resistance

```text
I_Lure = 1 - false_acceptance_rate_of_lures
```

Lure subtypes:

```text
feature lure
binding lure
same-change lure
wrong-successor lure
wrong-path lure
wrong-wrapper lure
```

## 16.5 Wrapper Recovery

```text
W_Recovery = post_shift_performance / pre_shift_performance
```

Additional recovery metrics:

```text
swap_cost_accuracy
swap_cost_rt
trials_to_recovery
recovery_slope
mixed_wrapper_stability
```

## 16.6 Delayed Recovery

```text
D_Retention = delayed_probe_performance / immediate_probe_performance
```

Use after:

```text
same session delay
next session delay
next day delay
changed-wrapper delay
```

---

# 17. Data model

## 17.1 Tables

```text
gabor_sessions
gabor_blocks
gabor_stream_events
gabor_probe_events
gabor_transition_maps
gabor_lure_events
gabor_block_summaries
gabor_capacity_estimates
gabor_wrapper_states
```

## 17.2 `gabor_blocks`

Fields:

```text
id
session_id
user_id
block_type
relation_family
wrapper_id
difficulty_level
transition_map_id
start_time
end_time
duration_ms
probe_density
training_band_target
```

## 17.3 `gabor_stream_events`

Fields:

```text
id
block_id
event_index
state_id
location_bin
orientation_bin
frequency_bin
contrast_bin
state_vector_json
expected_transition_id
display_ms
iti_ms
timing_quality
```

## 17.4 `gabor_probe_events`

Fields:

```text
id
block_id
probe_index
probe_type
horizon
target_state_id
foil_state_ids
correct_response
user_response
rt_ms
correct
lure_type
wrapper_shift_flag
delayed_probe_flag
```

## 17.5 `gabor_transition_maps`

Fields:

```text
id
map_name
relation_family
states_json
transitions_json
transition_probabilities_json
target_states_json
blocked_states_json
difficulty_level
version
```

## 17.6 `gabor_capacity_estimates`

Fields:

```text
id
user_id
session_id
A_Bind
C_Bind
R_Bind
S_Horizon
I_Lure
W_Recovery
D_Retention
confidence_label
trial_count
probe_count
timing_quality
created_at
```

---

# 18. Adaptation rules

## 18.1 Block-level adaptation

After each block:

```text
if balanced_accuracy >= 82%
and lure_false_acceptance is low:
    increase one difficulty dimension

if balanced_accuracy between 70% and 82%:
    maintain current level

if balanced_accuracy < 70%
or lure_false_acceptance is high:
    reduce difficulty or add scaffold
```

## 18.2 SR horizon adaptation

```text
if one-step prediction stable:
    introduce two-step probe

if two-step stable:
    introduce target reachability probe

if reachability stable:
    introduce branching or probabilistic successor

if branching stable:
    introduce delayed re-check or wrapper perturbation
```

## 18.3 Lure adaptation

If user accepts too many lures:

```text
hold difficulty
increase contrast between valid relation and near-miss
give one tutorial reminder
then reintroduce lure gradually
```

Micro-hint:

```text
That looked similar, but it did not follow the same path.
```

Use sparingly.

---

# 19. Relationship to other components

## 19.1 From Component 2 into Component 3

Component 2 trains:

```text
hold and compare relations across n-back delay
```

Component 3 extends this into:

```text
use relations to predict successor states and reachable paths
```

Bridge:

```text
Frame Memory → Pattern Binding → Change Tracking → Path Prediction
```

## 19.2 From Component 3 into Component 4

Component 3 trains four visual relation families:

```text
order/chains
transformations/analogies
rules/constraints
prediction/strategy
```

Component 4 maps these into explicit reasoning.

Example:

```text
Gabor stream:
A becomes B by the same change that C becomes D

Reasoning bridge:
A:B :: C:D
```

The bridge should be short and occasional, not inserted after every block.

---

# 20. Claim boundary

Use:

```text
This task trains pattern binding, change tracking and path prediction using controlled visual pattern streams.
```

Use:

```text
Path Prediction is a behavioural SR-inspired score. It measures how well the user learns and uses transition patterns in the app.
```

Use:

```text
Transfer is tested through wrapper shifts, lures, delayed probes and reasoning-transfer checks.
```

Avoid:

```text
This directly measures hippocampal successor representations.
This proves far transfer.
This guarantees IQ gains.
This diagnoses cognitive ability.
```

---

# 21. MVP scope

## Include in MVP

```text
single-patch and small-field Gabor streams
orientation and spatial-frequency dimensions
Pattern Binding block
Change Tracking block
Path Prediction stream
sparse probes only
basic lures
block-level adaptation
A_Bind, R_Bind, S_Horizon, I_Lure
basic delayed probes
```

## Defer until later

```text
optic-flow wrapper
complex probabilistic maps
large multi-patch fields
real-time state classifier
HRV integration
full proof dashboard
LLM-generated reasoning bridges
clinical or diagnostic interpretations
```

---

# 22. Final user loop

The Component 3 loop is:

```text
Watch the pattern.
Learn the relation.
Predict the path.
Catch the near-miss.
Recover after the surface changes.
Re-check later.
```

The internal loop is:

```text
state encoding
→ feature binding
→ transformation tracking
→ successor prediction
→ lure resistance
→ wrapper recovery
→ delayed recovery
```

This makes the Gabor SR component feel like a continuous pattern-prediction game while still collecting clean trial-level data for the Trident G transfer protocol.
