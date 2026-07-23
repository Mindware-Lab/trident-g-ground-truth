
The governing operation remains:

```text
SIGNAL
Detect the emotionally relevant information.

ACCUMULATE
Integrate threat and safety evidence and revise the current interpretation.

COMMIT
Reach a proportionate judgement at the appropriate time.
```

This preserves the distinction between signal capacity, evidence updating and stopping control in the overarching architecture.

## Important scientific reframing

I would not define the main intervention target as:

> dampening amygdala reactivity.

A behavioural task cannot establish that amygdala activity has changed, and lower amygdala activation is not always the desirable outcome. In one attention-bias-modification neuroimaging study, clinical improvement was associated with **increased** amygdala differentiation between negative and neutral stimuli in some participants, suggesting that better discrimination may matter more than globally reducing responsiveness. ([PMC][1])

The more defensible target is:

> **Improve flexible threat discrimination, reduce false-positive threat generalisation, strengthen updating from threat to safety, and calibrate decisions under emotional ambiguity.**

That preserves genuine threat detection rather than training emotional blunting.

```text
Not:
notice threat less

But:
detect genuine threat accurately
avoid classifying ambiguity as threat automatically
use subsequent safety evidence
disengage when threat is no longer supported
respond proportionately under pressure
```

A 2024 study found that attention training towards pleasant information altered gaze behaviour and attenuated skin-conductance responses to unpleasant images, showing that attentional training can affect behavioural and autonomic responding—but this is not direct evidence of amygdala change or clinical efficacy. ([PubMed][2])

# 1. Shared emotional stimulus grammar

## Core emotion categories

For the initial implementation:

```text
ANGRY
HAPPY
NEUTRAL
```

I would not combine angry and fearful expressions initially. They have different social meanings:

```text
anger:
possible direct interpersonal threat

fear:
another person detecting environmental threat
```

Keeping the target to anger produces a cleaner construct.

## Recommended binary response

Rather than:

```text
THREAT / SAFE
```

use:

```text
ANGRY
NO CLEAR ANGER
```

or:

```text
THREAT CUE
NO CLEAR THREAT CUE
```

This matters because neutral expressions are ambiguous rather than intrinsically safe. Neutral faces can receive negative appraisals and engage amygdala-related processing depending on the judgement being made. ([PubMed][3])

## Five-face majority display

Each MFT-M sample contains:

```text
5 faces
different identities
one expression per face
majority ratio:
5:0
4:1
3:2
```

Examples:

```text
4 angry + 1 happy
→ majority angry

3 neutral + 2 angry
→ majority no clear anger

4 happy + 1 angry
→ majority no clear anger
```

Happy and neutral should remain separately coded even when both currently support the same response. This allows you to distinguish:

```text
positive-safety evidence
from
ambiguous non-threat evidence
```

## Stimulus controls

Identity, sex, age band, ethnicity, gaze direction, lighting and image source should be balanced independently of emotional category.

Each identity should ideally have matched:

```text
angry
happy
neutral
```

versions, with expression intensity calibrated through validated morph continua where possible.

The task must prevent shortcuts such as:

```text
visible teeth = happy
high contrast = angry
one identity = threat
direct gaze = always angry
```

---

# 2. Component A: Emotional Signal Control

## Public label

**Emotional Signal Control**

## Core question

> **Can you distinguish genuine threat cues from competing emotional information?**

## Task

The ordinary single-display MFT-M becomes:

```text
fixation
→ five emotional faces
→ backward mask
→ ANGRY / NO CLEAR ANGER response
→ feedback
```

Difficulty adapts through:

```text
exposure duration
majority ratio
expression intensity
angry–happy versus angry–neutral contrast
identity variability
mask strength
```

## Recommended progression

### Stage 1 — Clear emotional discrimination

```text
angry versus happy
high expression intensity
5:0 and 4:1
comfortable exposure
```

### Stage 2 — Majority interference

```text
4:1
→ 3:2
```

### Stage 3 — Ambiguity

```text
angry versus neutral
lower-intensity anger morphs
neutral–anger boundary cases
```

### Stage 4 — Mixed emotional context

```text
angry
happy
neutral
mixed within the same block
```

## Key metrics

| Metric                                 | Interpretation                                                    |
| -------------------------------------- | ----------------------------------------------------------------- |
| **Emotion discrimination sensitivity** | Ability to distinguish angry from non-angry majorities            |
| **Anger hit rate**                     | Detection of genuine angry-majority displays                      |
| **Threat false-alarm rate**            | Neutral or happy majorities incorrectly judged angry              |
| **Neutral ambiguity cost**             | Extra errors or slowing for angry–neutral relative to angry–happy |
| **Expression-intensity threshold**     | Minimum anger intensity classified reliably                       |
| **Threat overgeneralisation**          | Judging weak or ambiguous cues as angry                           |
| **Threat miss rate**                   | Failure to recognise clear anger                                  |
| **Emotional interference cost**        | Cost introduced by minority emotional distractors                 |
| **RT variability**                     | Stability of emotional classification                             |
| **Post-error recovery**                | Recovery after a threat false alarm or miss                       |

## Training target

The goal is not simply to produce fewer angry responses.

It is:

```text
preserve sensitivity to clear anger
+
reduce false anger classifications of neutral cues
+
maintain stable performance across identities and intensity
```

A successful profile therefore looks like:

```text
high threat sensitivity
low threat false alarms
low neutral overgeneralisation
good recovery after ambiguity
```

---

# 3. Component B: Emotional Evidence Control

## Public label

**Emotional Evidence Control**

## Core question

> **Can you combine emotional evidence and update when the social signal changes?**

## Latent-state formulation

Each episode has a hidden state:

```text
HOSTILE / THREAT-SUPPORTING
```

or:

```text
NON-HOSTILE / THREAT-NOT-SUPPORTED
```

The user sees a sequence of emotional MFT-M samples. Individual samples may be misleading.

Example:

```text
Hidden state:
NO CLEAR THREAT

Sample 1:
angry majority

Sample 2:
neutral majority

Sample 3:
happy majority

Sample 4:
neutral majority
```

The task is not to respond to the most emotionally salient sample. It is to infer the state from the sequence.

Recommended response:

```text
THREAT SUPPORTED
NO CLEAR THREAT
```

The second response is preferable to `SAFE`, because the available evidence may merely be insufficient to support threat.

## Reliability contexts

Use two hidden contexts:

| Context    | Meaning                                                          |
| ---------- | ---------------------------------------------------------------- |
| **Stable** | Emotional samples usually reflect the hidden state               |
| **Noisy**  | Misleading angry, neutral or happy samples occur more frequently |

Illustrative starting values:

```text
stable:
P(sample supports hidden state) = .85

noisy:
P(sample supports hidden state) = .65
```

These are calibration values rather than fixed theoretical constants.

## Critical episode structures

### Threat followed by safety evidence

```text
angry
→ neutral
→ happy
→ neutral
```

Tests whether an initial threat interpretation can be revised.

### Safety followed by threat evidence

```text
happy
→ neutral
→ angry
→ angry
```

Tests whether genuine emerging threat is detected rather than ignored.

### Persistent ambiguity

```text
neutral
→ weak anger
→ neutral
→ weak happiness
```

Tests whether ambiguity is tolerated without forced threat closure.

### Context change

```text
previously reliable angry cues become noisy
or
previously noisy cues become reliable
```

Tests latent reliability updating.

## User instruction

> **Watch the emotional evidence. Decide when the overall pattern is clear enough.**

## Key metrics

| Metric                             | Interpretation                                                       |
| ---------------------------------- | -------------------------------------------------------------------- |
| **Emotional evidence sensitivity** | Degree to which accumulated emotional evidence controls the decision |
| **Threat-evidence weighting**      | Weight given to angry samples                                        |
| **Safety-evidence weighting**      | Weight given to happy and non-threatening samples                    |
| **Negative weighting asymmetry**   | Whether one angry sample outweighs several benign samples            |
| **Safety updating rate**           | Speed of revision after credible benign evidence                     |
| **Threat updating rate**           | Speed of revision when threat evidence becomes credible              |
| **Threat hysteresis**              | Persistence of threat judgement after evidence changes               |
| **Context-switch lag**             | Time taken to adapt when stream reliability changes                  |
| **Premature threat commitment**    | Threat judgement before sufficient evidence                          |
| **Premature reassurance**          | Non-threat judgement before genuine threat evidence is resolved      |
| **Excess safety checking**         | Continued sampling after threat is no longer supported               |
| **Confidence calibration**         | Whether confidence tracks evidence strength                          |
| **First-trial adaptation**         | Immediate response after a latent context change                     |

## Desired training signature

The intended improvement is:

```text
less domination by isolated angry cues
+
greater use of cumulative evidence
+
faster updating from threat to safety
+
preserved updating from safety to genuine threat
+
better-calibrated confidence
```

This is more defensible as an anxiety-relevant mechanism than merely training users to look away from angry faces.

---

# 4. Component C: Emotional Decision Timing

## Public label

**Emotional Decision Timing**

## Core question

> **Can you reach a proportionate judgement without reacting too early or checking indefinitely?**

## Basic task

Use the same sequential emotional evidence engine, but add variable deadlines.

```text
emotional samples arrive
→ user may continue sampling
→ user commits to:
   THREAT SUPPORTED
   or
   NO CLEAR THREAT
→ deadline expires
```

User instruction:

> **Use the emotional evidence, but do not rush and do not miss the deadline.**

## Deadline conditions

```text
long
medium
short
```

Later:

```text
fixed deadlines
variable deadlines
revisable decisions
irreversible decisions
```

## Primary failure poles

### Fast brittle

```text
isolated angry face
→ immediate threat commitment
→ later benign evidence never sampled
```

### Slow compensatory

```text
threat no longer supported
→ continue checking
→ delay or omission
```

### Globally overloaded

```text
emotional evidence remains unstable
→ threshold becomes erratic
→ alternating threat and non-threat judgements
```

### Regulated

```text
slow down when evidence is genuinely ambiguous
commit promptly when the pattern is clear
preserve sensitivity to consequential threat
```

## Key metrics

| Metric                           | Interpretation                                                 |
| -------------------------------- | -------------------------------------------------------------- |
| **Threat decision threshold**    | Evidence required before committing to threat                  |
| **Non-threat threshold**         | Evidence required before concluding threat is not supported    |
| **Threshold asymmetry**          | Difference between threat and non-threat evidence requirements |
| **Deadline adjustment**          | Whether thresholds change appropriately with available time    |
| **Fast threat false alarms**     | Premature threat choices under urgency                         |
| **Late non-threat closure**      | Continued checking despite adequate benign evidence            |
| **Threat misses under pressure** | Failure to detect clear anger under short deadlines            |
| **Selective slowing**            | Extra time used for ambiguous rather than clear displays       |
| **Omission rate**                | No completed response before the deadline                      |
| **Urgency slope**                | Growth in response probability as the deadline approaches      |
| **Threshold variability**        | Stability of the emotional decision policy                     |
| **Threshold hysteresis**         | Carry-over of the previous deadline or threat context          |
| **Decision utility**             | Accuracy balanced against delay and omission cost              |

## Advanced fallback mode

A later task could require the user to switch from:

```text
continue observing
```

to:

```text
make a provisional judgement now
```

with a short variable response-implementation cost.

That would measure whether the person waits too long before moving from emotional monitoring into action, but it should be introduced only after the simpler timed-commit task is stable.

---

# 5. Horizontal transfer without reference frames

You do not need absolute versus relational frames for the emotional lane.

Instead, use surface changes that preserve the emotional-control operation.

## Recommended wrapper factors

### Factor 1 — Presentation carrier

```text
static facial photographs
dynamic facial-expression morphs or short clips
```

### Factor 2 — Identity exposure

```text
trained identity set
held-out identity set
```

This gives a simple matrix:

| Condition              | Presentation | Identity status    |
| ---------------------- | ------------ | ------------------ |
| `face_static_trained`  | Static       | Trained identities |
| `face_static_novel`    | Static       | Novel identities   |
| `face_dynamic_trained` | Dynamic      | Trained identities |
| `face_dynamic_novel`   | Dynamic      | Novel identities   |

The held-out composition could be:

```text
dynamic expressions
+
novel identities
```

The progression becomes:

```text
BASE
static faces from trained identity set

IDENTITY PERTURBATION
static faces from novel identities

RETURN AND MIX
trained + novel static identities

PRESENTATION PERTURBATION
dynamic expressions from trained identities

HELD-OUT COMPOSITION
dynamic expressions from novel identities

FULL MIX
static/dynamic × trained/novel

DELAYED RE-CHECK
fresh mixed identities and expressions

BANK
only if emotional discrimination,
updating and timing survive
```

Identity generalisation is weaker evidence than transfer across genuinely different carriers, but it is a practical MVP.

A stronger later transfer ladder would be:

```text
facial expressions
→ whole-body emotional posture
→ emotional vocal prosody
→ contextual social scenes
```

The target relation would remain:

```text
threat supported
versus
threat not supported
```

That would test whether the control policy generalises beyond facial-expression recognition.

---

# 6. Relationship to the neutral Attention Coach

Keep both lanes.

```text
Neutral Attention Control
→ establishes general signal, accumulation and timing capacity

Emotional Attention Control
→ estimates how emotional salience changes those same operations
```

This permits useful difference measures:

```text
Emotional Signal Cost
=
emotional signal performance
-
matched neutral performance

Threat Updating Cost
=
emotional context-switch performance
-
neutral context-switch performance

Emotional Threshold Cost
=
emotional deadline adjustment
-
neutral deadline adjustment
```

Without the neutral anchor, poor emotional performance could reflect:

```text
general visual extraction difficulty
general evidence-accumulation difficulty
general deadline difficulty
face-recognition variability
emotion-specific bias
```

and these would be difficult to separate.

---

# 7. The anxiety-relevant profile

The most meaningful candidate profile would not be “high amygdala reactivity”.

It would be a behavioural pattern such as:

```text
Signal Control:
clear anger detected normally
but neutral faces frequently classified as angry

Evidence Control:
early angry evidence receives excessive weight
and later safety evidence produces weak updating

Decision Timing:
threat threshold is unusually low
while the threshold for accepting non-threat remains unusually high
```

Compressed:

```text
easy entry into threat
+
slow exit from threat
+
high certainty required for safety
```

This is a much more specific intervention target than general emotional reactivity.

A desirable change would be:

```text
preserved detection of clear threat
reduced neutral-threat false alarms
faster updating from threat to benign evidence
lower threat hysteresis
less excessive safety checking
better deadline calibration
```

---

# 8. Add an interpretation bridge

Faces alone primarily train perceptual discrimination and evidence control. For anxiety and resilience, I would add a short transfer bridge:

```text
face evidence
+
social context
→
provisional interpretation
```

Example:

```text
You make a suggestion in a meeting.

One person briefly looks angry.
Two others appear neutral.
A later response is supportive.

What is currently justified?

A. The group rejected the suggestion.
B. There is mixed evidence; rejection is not established.
C. The group strongly approves.
```

This tests whether the trained emotional evidence policy transfers into interpretation.

That matters because a 2024 review of cognitive-bias modification judged the evidence stronger for interpretation-bias modification in anxiety than for assuming that simple attention-bias training alone will produce reliable clinical change; it also emphasised that proposed mechanisms still require stronger testing. ([dbc.library.uu.nl][4])

A clean vertical bridge would therefore be:

```text
Emotional Signal Control
detect the expression accurately

Emotional Evidence Control
integrate several emotional cues

Emotional Decision Timing
reach proportionate provisional closure

Interpretation Bridge
decide what the evidence actually supports

Applied Resilience Check
recover the same policy in a realistic social scenario
```

---

# 9. Safety and implementation boundaries

For an early non-clinical build:

```text
use mild-to-moderate posed anger
avoid trauma-specific images
avoid graphic or personally targeted content
provide an immediate stop option
limit continuous threat exposure
include balanced positive and neutral material
end with a low-load neutral or positive block
monitor distress and dropout
```

Do not present repeated angry-face exposure as inherently therapeutic. Very brief masked emotional faces can affect amygdala responses even without explicit awareness, so exposure dosage, masking and participant acceptability need prospective testing rather than assumption. ([PubMed][5])

Use validated, licensed face sets and report performance by demographic stimulus group internally. Otherwise identity familiarity or uneven emotion-recognition accuracy could be mistaken for an anxiety-relevant effect.

---

# 10. Validation hierarchy

## Stage 1 — Manipulation checks

Show that:

```text
clear anger is detected
neutral creates greater ambiguity than happiness
noisy streams produce greater sampling
safety evidence changes prior threat judgements
short deadlines change stopping thresholds
```

## Stage 2 — Reliability

Test:

```text
test–retest reliability
identity-set generalisation
expression-intensity parameter recovery
threat false-alarm stability
safety-update stability
deadline-threshold stability
```

## Stage 3 — Mechanism outcomes

Use independent measures of:

```text
threat generalisation
attention disengagement
interpretation bias
confidence calibration
skin conductance
pupil response
heart-rate response
```

These physiological measures index autonomic reactivity, not amygdala activity specifically.

## Stage 4 — Clinical or resilience outcomes

Only in appropriately governed studies:

```text
validated state and trait anxiety measures
social-evaluative stress response
recovery after stress
behavioural avoidance
functional interference
delayed follow-up
```

## Stage 5 — Neural outcomes

Claims about amygdala reactivity require:

```text
fMRI
or another direct validated neural measurement
```

Behavioural improvement alone must not be labelled amygdala dampening.

---

## Recommended final architecture

```text
NEUTRAL ANCHOR

Signal Control
Can the relation be extracted?

Evidence Control
Can reliability be inferred and evidence integrated?

Decision Timing
Can the threshold adapt to time pressure?


EMOTIONAL LANE

Emotional Signal Control
Can genuine anger be distinguished from happy and ambiguous faces?

Emotional Evidence Control
Can threat and safety evidence be integrated and updated?

Emotional Decision Timing
Can the person avoid both premature threat closure
and excessive safety checking?


TRANSFER BRIDGE

Ambiguous social context
→ evidence-sensitive interpretation
→ proportionate provisional action
→ delayed re-check
```

The strongest intervention target is therefore:

> **Preserve sensitivity to real threat while reducing false alarms, improving safety updating and strengthening proportionate commitment under emotional uncertainty.**

That is more precise, measurable and defensible than attempting simply to suppress emotional attention or dampen amygdala activity.

[1]: https://pmc.ncbi.nlm.nih.gov/articles/PMC8320806/?utm_source=chatgpt.com "What’s in a Face? Amygdalar Sensitivity to an Emotional Threatening Faces Task and Transdiagnostic Internalizing Disorder Symptoms in Participants Receiving Attention Bias Modification Training - PMC"
[2]: https://pubmed.ncbi.nlm.nih.gov/38244853/?utm_source=chatgpt.com "The impact of attention bias modification training on behavioral and physiological responses - PubMed"
[3]: https://pubmed.ncbi.nlm.nih.gov/19709644/?utm_source=chatgpt.com "Preferential amygdala reactivity to the negative assessment of neutral faces - PubMed"
[4]: https://dbc.library.uu.nl/handle/1874/452810?utm_source=chatgpt.com "Towards implementation of cognitive bias modification in mental health care: State of the science, best practices, and ways forward"
[5]: https://pubmed.ncbi.nlm.nih.gov/9412517/?utm_source=chatgpt.com "Masked presentations of emotional facial expressions modulate amygdala activity without explicit knowledge - PubMed"
