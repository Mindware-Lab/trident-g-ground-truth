# IQ Coach — SR Game Component (Component 3) Specification
## Successor Representation Training Layer / Pattern Binding + SR Path Prediction

**Status:** Draft v0.1  
**Product:** IQ Coach  
**Layer:** Component 3 — SR Training  
**Depends on:**
* Component 1: Arrow-based C-Control / Cognitive Bandwidth
* Component 2: Arrow-based relational n-back / Majority Memory

**Feeds into:**
* Component 4: Explicit Relational Reasoning / Reasoning Gym

**Not included in this spec:**
* Long-form coaching prompts
* Real-world mission planning
* Open-ended writing tasks
* LLM-assisted reasoning
* General IQ test replacement claims
* Clinical interpretation
* Verbal labels or explicit rule instruction during SR blocks

---

# 1. Core Purpose

Component 3 trains implicit relational knowledge through non-verbal visual 
prediction, conditioning the hippocampal successor representation (SR) circuit 
to encode four families of relational invariants. The layer operates entirely 
through **procedural learning** — the user perceives relations directly and 
responds via simple motor actions, without verbal mediation.

The layer moves from:

```text
visual stream exposure
→ prediction error learning
→ procedural relational knowledge
→ wrapper recovery
→ delayed consolidation
→ explicit reasoning bridge (Component 4)
```

Consumer-facing labels:

```text
Pattern Binding
Change Tracking
Path Prediction
SR Training
```

Technical labels:

```text
SR-Chain
SR-Change
SR-Rule
SR-Path
SR-Mixed
SR-WRecovery
SR-Delay
SR-Lure
SR-BindCost
```

---

# 2. Core Design Principles

## 2.1 No Verbal Labels During SR Blocks

The SR layer must not attach verbal labels ("position 2, orientation 45°") 
to stimuli during training. Verbal labels recruit language-based working memory 
and shift processing from visual relational abstraction to explicit declarative 
memory. This contaminates the SR substrate.

**Allowed:** Visual cues (color, shape, motion), spatial layout, temporal rhythm.  
**Forbidden:** Text labels, spoken instructions, symbolic annotations during 
trials.

## 2.2 Motoric Responses Only

All responses must be simple button presses. No verbal responses, no typing, 
no complex multi-step actions.

**Allowed:** Left/right, up/down, A/B, 1/2, spacebar.  
**Forbidden:** Text entry, spoken answers, drag-and-drop, multi-touch gestures.

## 2.3 Implicit Learning Through Prediction Error

Learning occurs through prediction error, not explicit instruction. The user 
learns by predicting the next state and receiving immediate visual/auditory 
feedback on prediction accuracy. The relation becomes "felt" rather than "thought."

## 2.4 Continuous Stream Architecture

Stimuli are presented in a continuous stream with occasional breaks/resets. 
The stream engages the hippocampal CA3→CA1 prediction circuit. Breaks prevent 
overfitting and test consolidation.

---

# 3. Stimulus Design

## 3.1 Gabor Patch Specifications

```text
size: 128-256 pixels (scalable)
spatial_frequency: 0.5 - 4.0 cycles/degree
orientation: 0° - 180° (continuous or stepped)
position: grid-based or continuous (x, y in degrees visual angle)
contrast: 0.6 - 1.0
sigma: 20-40 pixels (Gaussian envelope)
color: grayscale (default), or color-tinted for context cues
```

## 3.2 State Space Definition

A "state" is a specific Gabor configuration defined by:

```text
s = (position, orientation, spatial_frequency, size, color)
```

Dimensions can be held constant or varied depending on the training target. 
The state space should be large enough to prevent memorization but constrained 
enough to learnable structure.

## 3.3 Stimulus Presentation

```text
update_rate: 800-1200ms per stimulus (adaptive)
response_window: 500-800ms after stimulus onset
isi: 200-400ms (inter-stimulus interval)
feedback_duration: 200ms
```

Stimuli are presented centrally or in a grid. Transitions between states are 
animated (e.g., smooth motion, rotation) where possible to enhance predictive 
learning.

---

# 4. The Four SR Training Families

---

# 4A. SR-Chain — Order & Chains

Maps to reasoning domain:

```text
Order / Chain Reasoning (Component 4, Section 5A)
```

Consumer-facing skill:

```text
Feel the flow of a sequence.
```

Internal metric:

```text
SR-Chain
```

## 4A.1 Core Relation

Sequential ordering: states compose in a chain where each successor follows 
from its predecessor according to a consistent rule.

## 4A.2 What the User Sees

A Gabor patch that changes position and/or orientation in a consistent sequence.

**Example:**
```text
Trial 1: [pos 1, ori 0°]
Trial 2: [pos 2, ori 45°]
Trial 3: [pos 3, ori 90°]
Trial 4: [pos 4, ori 135°]
```

The sequence follows a rule: each step advances both position and orientation 
by +1 unit.

## 4A.3 What the User Does

**Prediction version:**
```text
After 3-4 consistent steps, a "prediction cue" appears (screen border flash).
User presses LEFT if they predict the next step continues the chain.
User presses RIGHT if they predict it breaks.
```

**Violation version:**
```text
Stream runs automatically.
User presses SPACEBAR whenever they detect a "wrong" step — a break in the chain.
```

## 4A.4 Response Mapping

```text
Continue / Follows → LEFT arrow
Break / Violation → RIGHT arrow
```

## 4A.5 SR Mechanism

The brain learns M(s,s') where transitions that "feel right" have high 
predicted occupancy. The relation is procedural — the motor system learns to 
expect certain continuations. CA3 predicts the next state; CA1 signals error 
when prediction fails.

## 4A.6 Wrapper Variations

| Wrapper | Surface Change | Relation Preserved |
|---|---|---|
| **Spatial** | Position changes across screen | Ordinal progression |
| **Feature** | Position fixed, orientation changes | Ordinal progression |
| **Frequency** | Spatial frequency increases | Ordinal progression |
| **Size** | Gabor size changes | Ordinal progression |
| **Mixed** | Which dimension chains varies | Ordinal progression (abstracted) |

## 4A.7 Lures

```text
shared_lower_anchor: A > B, C > B (indeterminate)
reversed_relation: B < A presented as A > B
wrong_step_size: +2 instead of +1
chain_broken: Distractor inserted mid-sequence
```

## 4A.8 Level Ladder

| Level | Structure | Example |
|---|---|---|
| 1 | Direct comparison | A > B (single step) |
| 2 | Two-step chain | A > B > C |
| 3 | Three-step chain | A > B > C > D |
| 4 | Reversed order | B < A, C < B |
| 5 | Identity + chain | X = A, A > B > C |
| 6 | Indeterminate | A > B, C > B (cannot tell A vs C) |
| 7 | Delayed mixed | X = A, distractor, A > B > C |

---

# 4B. SR-Change — Transformations & Analogies

Maps to reasoning domain:

```text
Transformation / Analogy Reasoning (Component 4, Section 5B)
```

Consumer-facing skill:

```text
Feel the same change in a new form.
```

Internal metric:

```text
SR-Change
```

## 4B.1 Core Relation

Same transformation across different state pairs: the operation applied to 
transform A into B is the same operation that transforms X into Y.

## 4B.2 What the User Sees

Pairs of Gabor patches presented sequentially.

**Example:**
```text
Reference pair: [ori 0°] → [ori 45°] (transformation: +45° rotation)
Test pair 1: [ori 90°] → [ori 135°] (same +45° rotation)
Test pair 2: [ori 90°] → [ori 45°] (different: -45° rotation)
```

## 4B.3 What the User Does

```text
Press A if the transformation feels identical (SAME CHANGE).
Press B if the transformation feels different (DIFFERENT CHANGE).
```

## 4B.4 Response Mapping

```text
Same change → A key
Different change → B key
```

## 4B.5 SR Mechanism

The SR learns transformation operators T(s) = s'. The predictive map caches 
"what change leads to what result" across multiple starting states. The user 
learns to abstract the operator from the operands.

## 4B.6 Wrapper Variations

| Wrapper | Surface Change | Relation Preserved |
|---|---|---|
| **Orientation** | Rotation by fixed angle | Angular transformation |
| **Spatial** | Translation by fixed vector | Directional shift |
| **Frequency** | Multiplicative scaling | Scaling transformation |
| **Cross-modal** | Orientation→spatial→frequency | Abstract transformation |

## 4B.7 Lures

```text
same_endpoint_different_transformation: 0°→45° vs 90°→45°
same_direction_different_step_size: +45° vs +90°
surface_similarity_without_structural_similarity: looks similar but different change
identity_substitution_wrong_role: wrong starting state
```

## 4B.8 Level Ladder

| Level | Structure | Example |
|---|---|---|
| 1 | Difference size | A→B = +2, C→D = +1 (which is larger?) |
| 2 | Simple progression | A→B = +1, B→C = +1, predict C→D |
| 3 | Same-change analogy | A→B = +2, C→D = +2 (same relation) |
| 4 | Changed surface | Different dimensions, same transformation |
| 5 | Identity + analogy | X = A, A:B :: C:D |
| 6 | Equivalence class | A→C, B→C (A ≡ B w.r.t. C) |
| 7 | Equivalence + substitution | X = A, A ≡ B, predict X behaves like B |

---

# 4C. SR-Rule — Rules & Constraints

Maps to reasoning domain:

```text
Rule / Constraint Reasoning (Component 4, Section 5C)
```

Consumer-facing skill:

```text
Feel what is allowed and what is blocked.
```

Internal metric:

```text
SR-Rule
```

## 4C.1 Core Relation

Valid/invalid state transitions: some transitions are smooth (permitted by 
implicit rules), others are jarring (violate implicit rules).

## 4C.2 What the User Sees

A Gabor patch that transitions between states. Most transitions follow a hidden 
rule; some violate it.

**Example:**
```text
Valid: [pos 1, ori 0°] → [pos 2, ori 45°] (adjacent, monotonic)
Valid: [pos 2, ori 45°] → [pos 3, ori 90°] (adjacent, monotonic)
Invalid: [pos 1, ori 0°] → [pos 3, ori 90°] (skips position — violates adjacency)
```

## 4C.3 What the User Does

```text
Press UP if the transition feels allowed (VALID).
Press DOWN if the transition feels blocked (INVALID).
```

## 4C.4 Response Mapping

```text
Valid → UP arrow
Invalid → DOWN arrow
```

## 4C.5 SR Mechanism

The SR learns which transitions have zero probability (blocked) vs. non-zero 
probability (permitted). The matrix M(s,s') encodes the "feasibility structure" 
of the state space. The user develops a procedural sense of "this is possible" 
vs. "this is impossible."

## 4C.6 Rule Types

| Rule | Description | Visual Signature |
|---|---|---|
| **Adjacency** | Can only move to adjacent positions | Smooth spatial step |
| **Monotonicity** | Orientation must increase | Consistent rotation direction |
| **Conjunction** | Must satisfy multiple constraints | Multi-dimensional consistency |
| **XOR** | Change one dimension OR the other, not both | Single-dimension transitions only |
| **Context** | Rules change based on background cue | Color-dependent validity |

## 4C.7 Lures

```text
single_condition_sufficient: A present, B absent, but C follows (missing condition)
wrong_context: Rule from context K applied in context L
XOR_as_inclusive_OR: Changed both dimensions when only one allowed
identity_mapped_wrong: Wrong label substitution
```

## 4C.8 Level Ladder

| Level | Structure | Example |
|---|---|---|
| 1 | Simple identity | X = A, A has P, therefore X has P |
| 2 | Conjunction | A AND B → C, both present |
| 3 | Missing condition | A AND B → C, A only (invalid lure) |
| 4 | XOR | A XOR B, A true, therefore B false |
| 5 | Hierarchy | Context K: A→B; Context L: A→C |
| 6 | Identity + conjunction | X=A, Y=B, A AND B → C |
| 7 | Hierarchy + XOR | In context K, A XOR B |
| 8 | Conjunction + transitivity | A AND B → C, C > D |
| 9 | Identity + conjunction + transitivity | X=A, Y=B, A AND B → C, C > D |

---

# 4D. SR-Path — Prediction & Strategy

Maps to reasoning domain:

```text
Prediction / Strategic Path Reasoning (Component 4, Section 5D)
```

Consumer-facing skill:

```text
Feel which path is open and which is blocked.
```

Internal metric:

```text
SR-Path
```

## 4D.1 Core Relation

Reachability and likelihood of future states: from the current state, which 
future states are likely, unlikely, or impossible?

## 4D.2 What the User Sees

Current state + two candidate future states displayed simultaneously.

**Example:**
```text
Current: [pos 2, ori 45°, freq medium]

Option A: [pos 3, ori 90°, freq medium] — follows usual path
Option B: [pos 1, ori 0°, freq low] — possible but less likely
Option C: [pos 4, ori 180°, freq high] — blocked by rule
```

## 4D.3 What the User Does

```text
Press 1 to select Option A.
Press 2 to select Option B.
(Option C is never selectable — it is a lure displayed to test understanding.)
```

For binary versions:
```text
Press LEFT for the option that feels more likely/reachable.
Press RIGHT for the option that feels less likely/blocked.
```

## 4D.4 Response Mapping

```text
Option 1 → 1 key
Option 2 → 2 key
More likely → LEFT arrow
Less likely → RIGHT arrow
```

## 4D.5 SR Mechanism

This directly trains the SR matrix M(s,s') as a predictive map. The user learns 
the discounted sum of future occupancies:
- High M(s,s') = s' is frequently visited from s
- Low M(s,s') = s' is rarely visited from s
- Zero M(s,s') = s' is unreachable from s

## 4D.6 Training Progression

| Stage | Task | Response |
|---|---|---|
| 1 | Simple reachability | Binary: reachable vs. blocked |
| 2 | Probabilistic comparison | Select more likely of two options |
| 3 | Multi-step prediction | Predict state after 2-3 steps |
| 4 | Counterfactual | "What if I had chosen X?" |

## 4D.7 Lures

```text
rare_treated_as_impossible: Low-probability but valid path
possible_treated_as_likely: Possible but very unlikely path
old_path_still_reachable: Path that was valid but no longer is
wrong_context_probability: Probability from wrong context applied
```

## 4D.8 Level Ladder

| Level | Structure | Example |
|---|---|---|
| 1 | Simple conditional | P(B\|A) high, A occurred, B likely |
| 2 | Compare two paths | P(B\|A) high, P(C\|A) low |
| 3 | Probabilistic chain | A→B→C, A occurred, C plausibly reachable |
| 4 | Reachability | From A, only through B reaches T; path through C does not |
| 5 | Counterfactual | A→B or A→C possible; B taken; C was possible |
| 6 | Counterfactual + outcome | A→B→X, A→C→Y; B chosen; X actual, Y counterfactual |
| 7 | Equivalence + probability | A ≡ B for outcome C; P(C\|A) high → P(C\|B) high |
| 8 | Hierarchy + probability | Context K: A→B likely; Context L: A→C likely |

---

# 5. Mixed-Operation SR Training

As training progresses, items must combine operations from multiple SR families.

## 5.1 Supported Combinations

```text
identity + transitivity
identity + analogy
identity + conjunction
conjunction + transitivity
XOR + hierarchy
equivalence + probability
counterfactual + probability
hierarchy + probability
identity + conjunction + transitivity
delayed_identity + mixed_operation
```

## 5.2 Mixed Example: Conjunction + Transitivity

**Visual stream:**
```text
Rule: A AND B → C (both conditions must be met for transition to C)
C > D (C outranks D)
Current: A present, B present
Predict: Does current outrank D?

Response: YES (conjunction satisfied, transitivity applies)
```

## 5.3 Mixed Example: Hierarchy + XOR

**Visual stream:**
```text
Context cue: Background color = RED
Rule in RED context: A XOR B (one or the other, not both)
Current: A is true
Predict: Is B allowed?

Response: NO (XOR satisfied, B must be false)
```

---

# 6. SR Capacity Metric

The core metric is:

```text
SR-Capacity = Successor Representation Capacity
```

Definition:

```text
The maximum relational complexity a learner can predict above criterion 
across the four SR families and multiple visual wrappers, including lures 
and delayed re-checks, measured by prediction accuracy and reaction time.
```

User-facing label:

```text
Pattern Prediction Capacity
```

or:

```text
Change Tracking Capacity
```

---

# 7. Demand Model

Each SR item gets a structured demand score.

```text
D_sr =
  α(StateSpaceSize)
+ β(TransitionComplexity)
+ γ(WrapperNovelty)
+ δ(LureDensity)
+ ε(DelayLength)
+ ζ(PredictionHorizon)
+ η(ResponseConflict)
```

Where:

```text
StateSpaceSize
= number of distinct states in the current rule set

TransitionComplexity
= number of dimensions that change simultaneously

WrapperNovelty
= how recently the current wrapper was introduced

LureDensity
= proportion of trials that are lures/violations

DelayLength
= number of trials since the relation was last seen

PredictionHorizon
= how many steps ahead the user must predict

ResponseConflict
= similarity between correct and incorrect options
```

Fit:

```text
P(correct) = chance + usable_range × sigmoid(SR_theta − D_sr)
RT = baseline + slope × (D_sr − SR_theta) [for correct trials]
```

Report:

```text
current level passed
fitted SR capacity
prediction accuracy
RT slope (learning signal)
confidence label
```

---

# 8. Subscale Metrics

Estimate separate subscales:

```text
SR-Chain
SR-Change
SR-Rule
SR-Path
SR-Mixed
```

Also estimate transfer metrics:

```text
SR-WRecovery = wrapper recovery ratio
SR-Delay = delayed recovery ratio
SR-Lure = lure resistance
SR-BindCost = identity-binding cost
```

## 8.1 Wrapper Recovery

```text
SR-WRecovery = RT_after_wrapper_shift / RT_before_wrapper_shift
```

Example:
```text
Spatial wrapper RT = 450ms
Feature wrapper RT = 520ms
Wrapper recovery = 520 / 450 = 1.16

Target: < 1.20 (less than 20% RT cost)
```

## 8.2 Delayed Recovery

```text
SR-Delay = accuracy_after_delay / accuracy_before_delay
```

## 8.3 Lure Resistance

```text
SR-Lure = 1 − false_acceptance_rate_of_lure_transitions
```

## 8.4 Identity-Binding Cost

```text
SR-BindCost = RT_without_identity_bindings − RT_with_identity_bindings
```

---

# 9. Difficulty Ladder

Use a shared 10-level ladder across SR families.

| Level | Structure | Example |
|---|---|---|
| 1 | One relation | A > B |
| 2 | Two premises, same relation | A > B > C |
| 3 | Three premises, same relation | A > B > C > D |
| 4 | Identity + one relation | X = A, A > B |
| 5 | Identity + chain | X = A, A > B > C |
| 6 | Two identities + conjunction | X = A, Y = B, A AND B → C |
| 7 | Mixed operations | A ≡ B, P(C\|A) high |
| 8 | Lure / cannot tell | A > B, C > B (indeterminate) |
| 9 | Delayed mixed relation | Identity survives distractor |
| 10 | Cross-wrapper mixed block | Visual + abstract + nonsense interleaved |

---

# 10. Adaptive Progression

## 10.1 Target Performance Band

```text
Target prediction accuracy = 75-85%
Target RT = 400-600ms (after learning)
Target lure resistance = > 80%
```

## 10.2 Adaptation Rules

```text
IF accuracy > 85% AND RT < 400ms for two mini-blocks:
    → Increase one demand dimension.

IF accuracy 75-85% AND RT 400-600ms:
    → Maintain.

IF accuracy 65-75% OR RT 600-800ms:
    → Repeat with support (e.g., highlight relevant dimension).

IF accuracy < 65% OR RT > 800ms:
    → Reduce demand OR switch to easier wrapper.
```

## 10.3 Demand Dimensions

```text
state_space_size
transition_complexity
wrapper_novelty
lure_density
delay_length
prediction_horizon
response_conflict
```

Only increase one dimension at a time.

---

# 11. Horizontal Wrapper Cycle

Use the same horizontal cycle as Component 4:

```text
Plateau
→ Perturb
→ Recover
→ Mix
→ Plateau
→ Expand
```

For each target relational invariant R:

```text
1. Train R in easiest wrapper until learning curve flattens (RT stabilizes).
2. Shift to second wrapper while preserving R.
3. Expect temporary RT increase (swap cost).
4. When recovery begins, mix wrapper 1 + wrapper 2.
5. Train mixed wrappers until performance stabilizes.
6. Add third wrapper.
7. Mix all wrappers.
8. Bank the relation only if delayed mixed-wrapper performance passes.
```

## 11.1 Wrapper Order

Default:
```text
Gabor patches (visual)
→ Abstract shapes (visual-symbolic)
→ Nonsense patterns (visual-unfamiliar)
→ Mixed
```

Adaptive: If visual is too complex, allow:
```text
Abstract shapes
→ Gabor patches
→ Nonsense patterns
→ Mixed
```

## 11.2 Wrapper Swap Protocol

```text
PHASE 1: ESTABLISH (Block 1-2)
└── Train in Wrapper A until RT stabilizes

PHASE 2: PROBE (Block 3)
├── Insert 20% Wrapper B trials
├── Measure swap cost (RT increase)
└── If RT increase < 20%, proceed; else, scaffold

PHASE 3: MIX (Block 4-5)
├── 50/50 A/B mix
├── Add lures that test wrapper-specific vs. relation-specific responses
└── Measure mixed-wrapper stability

PHASE 4: EXPAND (Block 6+)
├── Add Wrapper C
├── Random A/B/C mix
└── Measure generalization slope
```

---

# 12. Relationship to Component 4 (Reasoning)

Every SR block should inform the next reasoning block.

## 12.1 Same-Day Bridge

```text
If SR block trained: SR-Chain
Then reasoning block uses: Order / Chain Reasoning

If SR block trained: SR-Change
Then reasoning block uses: Transformation / Analogy Reasoning

If SR block trained: SR-Rule
Then reasoning block uses: Rule / Constraint Reasoning

If SR block trained: SR-Path
Then reasoning block uses: Prediction / Strategic Path Reasoning
```

## 12.2 Bridge Block Design

```text
Duration: 2-4 minutes
Structure:
  ├── Brief instruction: "You just practiced this pattern visually. 
  │   Now see if you recognize it in a new form."
  ├── 2-3 reasoning items (symbolic wrapper)
  ├── Same relation family as SR block
  └── Brief feedback only (no long explanations)
```

The bridge should feel like a **recognition test**, not a learning test. The 
user should experience an "aha" moment: "Oh, that's what I was doing!"

## 12.3 Mixed-Day Review

Later sessions deliberately mix SR families:

```text
SR: SR-Change (visual)
Reasoning: analogy + equivalence + conditional probability (mixed)
```

This tests whether trained relation families can combine.

---

# 13. SR Block Design

## 13.1 Standard SR Block

```text
Pre-block instruction: 10-20 sec (visual demo, no verbal rules)
20-30 SR items
Brief visual feedback after each item
Post-block summary (RT trend, accuracy, lure resistance)
```

Duration:

```text
3-5 minutes
```

## 13.2 Full Benchmark Block

Use every 5-7 sessions.

```text
30-40 SR items
Mixed wrappers
Mixed SR families
Delayed items from earlier sessions
Lure-heavy (30-40%)
```

Duration:

```text
8-12 minutes
```

## 13.3 Daily Bridge Block

Use after SR training, before reasoning block.

```text
Same SR family as preceding block
One wrapper focus
1-2 lures
One mixed item if ready
```

Duration:

```text
2-4 minutes
```

---

# 14. Feedback Design

## 14.1 Immediate Feedback (Non-Verbal)

```text
CORRECT PREDICTION:
├── Visual: Brief green flash (50ms)
├── Auditory: Soft positive tone (200ms)
└── Motor: Smooth transition animation

INCORRECT PREDICTION:
├── Visual: Brief red flash (50ms)
├── Auditory: Soft negative tone (200ms)
└── Motor: Jarring transition animation

LURE CAUGHT (correctly rejected invalid transition):
├── Visual: Blue highlight (100ms)
├── Auditory: Distinct "insight" chime
└── Message: None (non-verbal)

LURE MISSED (falsely accepted invalid transition):
├── Visual: Orange flash (100ms)
├── Auditory: Warning tone
└── Message: None (non-verbal)
```

## 14.2 RT Feedback

```text
Faster than average: Smooth animation speedup
Slower than average: Animation remains normal
```

## 14.3 Block Summary

```text
POST-BLOCK DASHBOARD
├── Prediction accuracy: X%
├── Lure resistance: Y%
├── Wrapper recovery: Z (RT ratio)
├── Delayed recovery: W (accuracy ratio)
├── Average RT: XXXms
├── RT trend: improving / stable / declining
└── Next block preview: Visual cue only
```

No verbal explanations. All information conveyed through visual trends and icons.

---

# 15. Item Schema

Each item should be generated from a formal template.

## 15.1 SR Item Fields

```text
id
sr_family
reasoning_domain_target
operation_tags
wrapper_type
stimulus_sequence
correct_response
response_options
difficulty_level
d_sr
state_space_size
transition_complexity
wrapper_novelty
lure_density
delay_length
prediction_horizon
response_conflict
lure_type
delay_condition
source_relation_id
explanation_short (for internal use only)
explanation_long (for internal use only)
```

## 15.2 Operation Tags

```text
identity
comparison
transitivity
reversal
indeterminate
difference
progression
analogy
equivalence
conjunction
xor
hierarchy
conditional_probability
reachability
counterfactual
delay
mixed
```

---

# 16. Data Model

## 16.1 Tables

```text
sr_sessions
sr_blocks
sr_items
sr_trials
sr_estimates
sr_wrapper_states
sr_delayed_probes
```

## 16.2 Trial Fields

```text
id
user_id
session_id
block_id
item_id
trial_index
sr_family
wrapper_type
operation_tags_json
difficulty_level
d_sr
state_space_size
transition_complexity
wrapper_novelty
lure_density
delay_length
prediction_horizon
response_conflict
lure_type
delay_condition
response
correct_response
is_correct
rt_ms
confidence_rating_optional
feedback_type
created_at
```

## 16.3 Estimate Fields

```text
id
user_id
session_id
metric_name
sr_family
wrapper_type
capacity_estimate
capacity_unit
prediction_accuracy
rt_slope
rt_baseline
d_prime
false_acceptance_rate
wrapper_recovery_ratio
delayed_recovery_ratio
confidence_label
created_at
```

---

# 17. Adaptive Item Selection

## 17.1 Select Next Item

```text
function selectSRItem(userState, blockGoal):

    identify active SR family
    identify active wrapper
    estimate current SR_theta
    build candidate item set
    remove recently repeated surface items
    favour items near current threshold
    include controlled lure rate
    include one bridge item for Component 4
    return highest expected information item
```

## 17.2 Adapt After Mini-Block

```text
function updateSRLevel(blockScore):

    if prediction_accuracy > 0.85
       and rt < 400ms
       and lure_resistance > 0.80:
           increase one demand dimension

    else if prediction_accuracy between 0.75 and 0.85:
           maintain

    else if prediction_accuracy between 0.65 and 0.75:
           repeat with support

    else:
           reduce demand
```

---

# 18. Lure Taxonomy

## 18.1 Believable-Invalid Lure

The transition looks smooth but violates a hidden rule.

## 18.2 Valid-Unbelievable Lure

The transition follows the rule but looks unusual.

## 18.3 Cannot-Tell Lure

The premises are insufficient to determine validity.

## 18.4 Missing-Condition Lure

A conjunction is treated as satisfied when one condition is absent.

## 18.5 Wrong-Identity Lure

The user substitutes the wrong temporary label.

## 18.6 Wrong-Context Lure

A local rule is applied under the wrong global context.

## 18.7 Over-Equivalence Lure

An equivalence class is applied beyond its specified outcome.

## 18.8 Counterfactual-Confusion Lure

A possible alternative path is treated as the actual path.

---

# 19. User-Facing Outputs

## 19.1 Daily Result Summary

```text
Pattern Prediction: Level 4
Current focus: Chain Flow
Wrapper: visual-spatial
Lure Resistance: improving
Next: same rule in feature wrapper
```

## 19.2 Weekly Profile

```text
Chain Flow
Change Flow
Rule Flow
Path Flow
Mixed Flow
Wrapper Recovery
Delayed Recovery
```

## 19.3 Technical Profile

```text
SR-Chain
SR-Change
SR-Rule
SR-Path
SR-Mixed
SR-WRecovery
SR-Delay
SR-Lure
SR-BindCost
```

---

# 20. Integration with Total IQ Coach Dashboard

Component 3 sits in the vertical stack:

```text
Cognitive Bandwidth (Component 1)
→ Frame Bandwidth (Component 2)
→ Pattern Binding (Component 3a: SR-Chain, SR-Change)
→ Change Tracking (Component 3b: SR-Rule)
→ Path Prediction (Component 3c: SR-Path)
→ Reasoning Transfer (Component 4)
→ Transfer Score
```

Dashboard groups:

```text
Control & Focus:
Cognitive Bandwidth
Frame Bandwidth

Pattern & Prediction:
Pattern Binding
Change Tracking
Path Prediction

Reasoning & Transfer:
Reasoning Transfer
Lure Resistance
Wrapper Recovery
Delayed Recovery
Reasoning Benchmark
```

---

# 21. Build Order

```text
1. Gabor patch renderer
2. State space generator
3. Continuous stream controller
4. SR-Chain templates
5. SR-Change templates
6. SR-Rule templates
7. SR-Path templates
8. Non-verbal feedback system
9. RT measurement system
10. Adaptive item selection
11. SR capacity scoring
12. Wrapper recovery controller
13. Mixed-operation templates
14. Delayed SR probes
15. Bridge block to Component 4
16. Full benchmark mode
```

---

# 22. MVP Scope

MVP should include:

```text
Gabor patch stimuli
Continuous stream presentation
Motoric responses only (no verbal)
Non-verbal feedback (visual/auditory)

SR-Chain (Order & Chains)
SR-Change (Transformations & Analogies)
SR-Rule (Rules & Constraints)
SR-Path (Prediction & Strategy)

Continue/Break
Same/Different
Valid/Invalid
Option 1/2

identity + transitivity
identity + analogy
conjunction + transitivity
XOR + hierarchy
equivalence + probability
counterfactual + probability

SR capacity scoring
wrapper recovery
delayed recovery
lure resistance
RT-based adaptation

Bridge block to Component 4 (same-day)
```

MVP should not include:

```text
verbal labels during SR blocks
explicit rule instruction during SR blocks
text entry responses
spoken responses
drag-and-drop interactions
long-form written explanations
open-ended reasoning tasks
LLM-generated item creation
clinical labels
official IQ-score claims
```

---

# 23. Claims Boundary

The app may say:

```text
Pattern Binding trains implicit relational prediction through visual streams.
It conditions the brain to predict state transitions across multiple visual 
wrappers without verbal mediation.
It tracks prediction capacity through accuracy, reaction time, lure resistance, 
wrapper recovery, and delayed consolidation.
```

The app should not say:

```text
This is a full IQ test.
This proves IQ increase.
This diagnoses cognitive impairment.
This guarantees far transfer.
This replaces explicit reasoning training.
```

Safe technical wording:

```text
The SR layer estimates how well users predict, detect violations, and recover 
relational structure across multiple visual wrappers and delayed re-checks 
through implicit procedural learning.
```

---

# 24. Final Summary

Component 3 trains implicit relational knowledge through non-verbal visual 
prediction, conditioning four families of successor representations:

```text
Order & Chains
→ SR-Chain
→ Order / Chain Reasoning

Transformations & Analogies
→ SR-Change
→ Transformation / Analogy Reasoning

Rules & Constraints
→ SR-Rule
→ Rule / Constraint Reasoning

Prediction & Strategy
→ SR-Path
→ Prediction / Strategic Path Reasoning
```

It uses continuous visual streams with motoric responses:

```text
Gabor stream → prediction → motor response → feedback → SR update
```

It supports wrapper recovery:

```text
visual → abstract → nonsense → mixed
```

Its core metric is:

```text
SR-Capacity = implicit relational prediction capacity
```

defined as:

```text
the maximum relational complexity predicted above criterion across SR families, 
wrappers, lures, and delayed re-checks, measured by accuracy and reaction time.
```

This feeds the full IQ Coach stack:

```text
evidence extraction
→ relational evidence extraction
→ associative binding
→ relational WM
→ SR path prediction
→ explicit relational reasoning (Component 4)
→ wrapper recovery
→ delayed transfer
```
