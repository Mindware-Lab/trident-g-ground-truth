# Trident-G Far Transfer Protocol

## 1. Horizontal principle

The horizontal far-transfer principle is:

> **A skill has not transferred horizontally until the same relational invariant can be recovered across changed surfaces, modalities, boundary cases and delayed re-checks.**

In standard cognitive training, performance often improves because the learner compiles a surface-specific routine. This may be useful, but it is not yet genuine far transfer. In Trident-G terms, this is **fast Gc** or thin automation: efficient, task-bound, and vulnerable to collapse when the surface changes.

The horizontal-transfer protocol tries to prevent this by changing the wrapper at the point where the learner is beginning to overfit the current format.

The key mechanism is:

```text
surface-specific policy begins to automate
→ wrapper changes
→ old policy is partly invalidated
→ learner re-enters exploratory search
→ deeper invariant must be recovered
→ successful recovery strengthens portable relational structure
```

The Trident-G claim is that this controlled re-entry is a small-scale bifurcation event. The system is pushed out of brittle exploitation and back into the Ψ-band, where entropy/exploration and mutual-information constraint can rebalance around the deeper structure rather than the surface policy.

This should be framed as a **design principle and testable hypothesis**, not as an already proven far-transfer mechanism.

---

## 2. Wrappers — definition

We can define **wrapper** as the **surface container through which the same underlying relation is expressed**.

A wrapper can be:

| Wrapper level                | Meaning                                                           | Example                                                   |
| ---------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------- |
| **Feature wrapper**          | Same modality, different perceptual feature                       | Gabor orientation → Gabor spacing                         |
| **Stimulus wrapper**         | Same relation, different stimulus family                          | Gabor patch → optic flow                                  |
| **Modality wrapper**         | Same relation, different representational mode                    | visuospatial → verbal → numerical                         |
| **Task wrapper**             | Same operation, different task format                             | n-back relation tracking → logic puzzle constraint search |
| **Semantic/context wrapper** | Same control policy, different real-world meaning                 | puzzle “blocked path” → work “blocked project path”       |
| **Niche wrapper**            | Same policy in a real environment with live cues and consequences | app task → real-world mission                             |

So, in the horizontal-transfer section, **wrapper** should mean the surface format that carries the invariant, not the invariant itself.

This matches the Trident-G summary, where horizontal transfer is defined as the same relational invariant surviving surface variation, changed wrappers, boundary cases, novel contexts and delayed re-checks. It also fits the perceptual-learning literature, where transfer is often limited by trained features, locations or task conditions unless training design promotes broader generalisation. Yu’s review notes the classic location- and orientation-specificity problem, while Zhang et al. propose a rule-based account in which transfer depends partly on whether higher-level decision rules can connect to new inputs. ([PMC][1])

Or even shorter:

> **Wrapper = changed surface, same relation.**

---

## 3. Entropy–mutual information interpretation

Zhang and Tang’s recent work models learning as a nonequilibrium process shaped by a balance between maximum-entropy exploration and mutual-information constraint. In their neural-network account, productive learning involves neither unconstrained exploration nor rigid optimisation, but information-driven self-organisation in which relevant updates are constrained by task information while still allowing broad search. ([PNAS][2])

In Trident-G terms:

| Learning dynamic                    | Trident-G interpretation          | Failure mode               |
| ----------------------------------- | --------------------------------- | -------------------------- |
| Entropy / exploration               | search the relational space       | scattered exploration      |
| Mutual information / constraint     | filter by task-relevant structure | premature closure          |
| Heavy-tailed / intermittent updates | possible restructuring events     | unstable if unregulated    |
| Local optimisation                  | efficient fast Gc                 | brittle surface automation |
| Re-entry after perturbation         | Ψ-band recovery                   | transfer opportunity       |

This is the theoretical reason not to train one wrapper endlessly. If the learner stays too long in one surface format, mutual-information constraint can become too narrow: performance improves, but the improvement may reflect a local surface policy. Wrapper change reintroduces entropy, but in a controlled way because the underlying invariant is preserved.

So the principle is:

```text
same invariant
+ changed surface
= controlled entropy injection
```

The learner must then use mutual-information constraint to recover what still matters.

---

## 4. Piecewise learning curves and breakpoint timing

The best moment for a wrapper swap is not the very beginning, and not long after complete automation. It is around **local flattening**, when improvement within the current wrapper is slowing and the learner is beginning to consolidate a surface-specific strategy.

Donner and Hardy found that individual learning curves are often better described by **piecewise power laws** than by a single smooth curve, implying that learning can contain phase shifts, transitions or local reorganisations rather than one continuous improvement process. ([PMC][3])

This supports the Trident-G idea of watching for a behavioural signature such as:

```text
local improvement
→ slowing / asymptote
→ perturbation
→ temporary dip
→ recovery
→ better portability
```

<img width="782" height="823" alt="image" src="https://github.com/user-attachments/assets/8953dd77-5c27-49ae-be26-ed8b91a05bbf" />

The wrapper swap should be timed near the flattening point because:

| Timing                    | Likely result                                                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Too early**             | the learner has not yet formed the target relation, so the swap is just confusion                              |
| **Too late**              | the surface-specific policy may be deeply automated                                                            |
| **Near local flattening** | the learner has enough structure to recover the invariant, but has not fully over-compiled the surface routine |

So the protocol should monitor the learning curve and trigger a wrapper swap when gains start to diminish, not randomly.

---

## 5. Variable construction, evidence-quality perturbation and invariance training

The original distinction remains useful, but it should be sharpened:

> **Speed, discrimination gap, contrast and coherence are mainly evidence-quality / efficiency / attention-control levers. Wrapper and modality swaps are mainly depth / invariance levers.**

The revision is important because fine perceptual-threshold training can easily become surface-specific. Ahissar and Hochstein showed that more difficult perceptual-learning conditions can become more specific to trained orientation and position, while easier conditions generalise more. ([Weizmann Institute of Science][4]) Jeter et al. similarly found that specificity increased after extensive orientation-discrimination training, while earlier learning showed more transfer to a new location and opposite orientation. ([ScienceDirect][5])

So contrast and discrimination should not be treated as the main route to far transfer. They are better used in two more limited ways.

### 5.1 Variable construction

Early in learning, discrimination helps the learner identify the relevant variable:

```text
which feature changed?
what matters?
what is noise?
where is the boundary?
what relation should be tracked?
```

This supports **state-space construction**. It prepares the variables over which WM binding, SR-style inference and later wrapper transfer can operate.

In Trident-G terms:

```text
surface cue
→ variable abstraction
→ WM binding
→ SR-style inference
→ boundary test
→ portable schema
```

The danger is:

```text
surface cue
→ premature perceptual shortcut
→ brittle surface policy
```

So discrimination is valuable, but only when it helps carve the right variables rather than overtraining the current perceptual surface.

### 5.2 Evidence-quality and attention-quality perturbation

After the relation is already established, lower contrast, lower coherence, shorter exposure or mild speed pressure can be used as controlled perturbations.

Their role is not to train the minimum perceivable threshold as an endpoint. Their role is to test whether the learner can still recover the same relation when sensory evidence is weaker, less redundant or more demanding.

The core question becomes:

```text
Can the learner maintain high-quality attentional sampling
and recover the same relational invariant
under lower-quality evidence?
```

This is an **attention-quality challenge**, not a perceptual-threshold endpoint.

The attention literature supports this framing. Nguyen, Watanabe and Andersen found that both exogenous and endogenous attention facilitated task-relevant visual perceptual learning and transfer to an untrained feature. ([PLOS][6]) Roberts and Carrasco similarly found that exogenous attention helped generalise perceptual learning beyond trained spatial locations in adults with amblyopia. ([ScienceDirect][7])

So the stronger Trident-G interpretation is:

```text
low contrast / low coherence
= controlled evidence-quality perturbation

not:

low contrast threshold
= far-transfer mechanism
```

### 5.3 Closed-loop adaptation rule

The system should adapt difficulty based on the **error signature**, not just raw accuracy.

A simple rule is:

| Error pattern                               | Interpretation                   | Adaptation                                                            |
| ------------------------------------------- | -------------------------------- | --------------------------------------------------------------------- |
| Stable high accuracy                        | relation is established          | reduce gap, add mild speed pressure, or prepare wrapper swap          |
| Accuracy drop with slow RT / lapses         | attention-quality or state drift | use brief lower-contrast/coherence vigilance probe, then rebound test |
| Accuracy drop with relation-specific errors | invariant not yet stable         | widen gap, simplify relation, repeat examples                         |
| Accuracy drop with fast impulsive errors    | response-control issue           | slow response window, emphasise accuracy, reduce speed pressure       |
| Sustained poor accuracy                     | task/state mismatch              | reset, rest, or Zone re-check                                         |

So the key design rule is:

> **Do not make the task harder simply because accuracy drops. Make it harder only when there is evidence that the learner already owns the relation and the current failure is likely attentional drift rather than relational confusion.**

This makes the closed-loop logic more precise. Lower contrast or lower coherence can be useful after stable performance, but it should not be the default response to poor accuracy.

### 5.4 Efficiency / robustness training

These levers make the current relation more accurate, faster and more robust within a known representational space:

```text
smaller discrimination gap
faster presentation
shorter response window
lower contrast / coherence
more lures
higher n-back lag
```

They ask:

```text
Can you run the same operation more efficiently?
Can you preserve accuracy under less evidence or less time?
Can you resist lures under pressure?
Can you maintain attention quality when evidence is degraded?
```

Contrast and coherence therefore belong mainly in the **robustness** lane. They test whether the relation survives poorer evidence.

They do not, by themselves, demonstrate far transfer.

### 5.5 Visual-function lane versus far-transfer lane

Perceptual-threshold training can still be valuable, especially in a visual-performance or cognitive-longevity context. DeLoss, Watanabe and Andersen trained older adults on an orientation-discrimination task with variable contrast and noise, reporting improved contrast sensitivity and transfer to an untrained orientation. ([Sage Journals][8]) A more recent study by Tang, Liang and Zhou reported that perceptual learning improved spatial contrast sensitivity and visual acuity in older adults. ([Frontiers][9])

But this should be framed as:

```text
visual function / perceptual robustness training
```

not:

```text
general far-transfer training
```

In Trident-G terms, perceptual-threshold work may improve the quality of evidence entering the system. It does not, by itself, prove that a relational invariant has become portable across wrappers, contexts, delays or niches.

---

## 6. Depth / invariance training

Wrapper swaps change the surface while preserving the relation.

Examples:

```text
Gabor orientation change
→ optic-flow angle change

spacing proportional change
→ speed proportional change

number-relation n-back
→ spatial-vector relation n-back

Towers-style constraint search
→ Pattern-style constraint search
```

They ask:

```text
Can you recover the same relation in a different medium?
Can you survive the surface change?
Can you recover faster on later swaps?
```

This is the stronger horizontal-transfer test.

The Seeing Patterns specification uses exactly this logic: tuning/honing improves precision, reconfiguration changes dimension or rule, portability validation tests the same relation across Gabor, flow and conjunction variants, and consolidation supports slow gist extraction.

So the distinction is:

| Protocol lever              | Main function                 | Transfer meaning                                 |
| --------------------------- | ----------------------------- | ------------------------------------------------ |
| Smaller gap                 | precision                     | sharper evidence extraction                      |
| Faster timing               | efficiency                    | faster relation operation                        |
| Lower contrast/coherence    | evidence-quality perturbation | relation survives poorer evidence                |
| Higher n / lure pressure    | workspace stability           | relation survives interference                   |
| Closed-loop vigilance probe | attention-quality test        | relation survives temporary state drift          |
| Wrapper swap                | abstraction                   | relation survives surface change                 |
| Modality swap               | invariance                    | relation survives representational medium change |
| Delayed re-check            | consolidation                 | relation survives time                           |

The main transfer claim should attach to wrapper swaps, modality swaps, delayed re-checks and real-world/niche probes, not to threshold improvement alone.

---

## 7. Why random mixing is not enough

There is evidence that variable practice, interleaving and contextual interference can improve retention and transfer, but these effects are not a licence to randomise everything. The contextual-interference literature shows a common pattern: harder, more variable practice can impair immediate performance while improving later retention or transfer, but the effect is heterogeneous and depends on task, learner and schedule. Czyż’s 2024 systematic review and meta-analysis emphasises both the long-standing promise and the heterogeneity of contextual-interference effects. ([Frontiers][10])

The “desirable difficulties” literature makes a similar point: conditions that slow apparent learning can improve long-term retention and transfer, but only when the challenge is desirable rather than overwhelming.

For Trident-G, the design implication is:

```text
not random variation
but invariant-preserving variation
```

The surface should change only after the learner has enough grip on the target relation. Otherwise the variation becomes noise rather than productive entropy.

---

## 8. Supporting evidence from perceptual-learning specificity and transfer

The horizontal principle is supported indirectly by the perceptual-learning literature.

Perceptual learning often shows specificity to trained features, locations or orientations. Yu notes that visual perceptual learning has classically been treated as location- and orientation-specific, but also reviews work showing that procedures such as double training and task-plus-exposure can enable transfer to new locations or orientations. ([PMC][1])

Zhang et al. proposed a rule-based account of perceptual-learning transfer in which a higher-level decision unit learns how to reweight perceptual inputs. This is very compatible with the Trident-G idea that what transfers is not raw sensory tuning alone, but a higher-level rule or control policy over the relevant variables. ([PubMed][11])

So the evidence pattern is:

```text
fixed low-level training
→ often specific

structured variation / decision-rule learning
→ more potential transfer
```

This is a useful mainstream translation of the Trident-G horizontal principle.

---

## 9. Protocol summary: horizontal transfer cycle

A clean protocol version is:

### Phase 1: Establish the invariant

Train the target operation in one wrapper until the learner can perform it reliably.

Example:

```text
Gabor orientation:
Is the amount of angle change the same?
```

Goal:

```text
relation understood
accuracy stable
lure errors manageable
basic variable identified
```

The target here is not perceptual threshold. The target is variable abstraction and relation recovery.

---

### Phase 2: Hone efficiency within the wrapper

Increase speed, reduce gap size, add mild lures or mildly reduce signal quality.

Goal:

```text
faster relation extraction
more precise discrimination
better stability under pressure
more reliable evidence sampling
```

This is mutual-information-dominant refinement: the learner is narrowing onto the relevant structure and improving efficiency.

But perceptual difficulty should remain secondary. It should support the relation, not replace it.

---

### Phase 3: Add brief evidence-quality perturbations

Once the relation is stable, introduce short, controlled perturbations such as:

```text
slightly lower contrast
slightly lower motion coherence
shorter presentation
mild speed pressure
mild lure increase
```

Goal:

```text
test whether the same relation survives poorer evidence
test attention-quality under controlled stress
detect state drift
avoid pure comfort-zone performance
```

This phase should be closed-loop. It should respond to error signatures.

```text
stable relation + lapse-like errors
→ brief evidence-quality challenge
→ return to normal signal
→ rebound check
```

If errors are relation-specific, do not lower contrast or increase difficulty. Instead, simplify the relation or widen the gap.

---

### Phase 4: Detect flattening

Monitor for reduced learning-rate or local asymptote:

```text
accuracy no longer improves
RT stabilises
gap threshold stabilises
same error pattern repeats
attention-quality probes show little new information
```

Goal:

```text
avoid over-banking thin fast Gc
```

At this point, more threshold refinement may simply deepen surface skill. The protocol should prepare a wrapper swap instead.

---

### Phase 5: Wrapper swap

Change the surface but preserve the relational invariant.

Example:

```text
Gabor angle change
→ optic-flow angle change

spatial-frequency proportional change
→ flow-speed proportional change
```

Goal:

```text
force re-entry into exploratory search
test whether invariant survives
prevent overfitting to the original wrapper
```

The wrapper swap is the main horizontal-transfer test.

---

### Phase 6: Measure swap cost and recovery

The key metric is not just post-swap accuracy. It is:

```text
swap cost
recovery slope
final recovered performance
lure pattern after swap
error type after swap
delayed survival
```

Interpretation:

| Result                                                  | Interpretation                                                  |
| ------------------------------------------------------- | --------------------------------------------------------------- |
| Low swap cost                                           | relation may already be abstracted                              |
| High cost + fast recovery                               | promising transfer-learning signature                           |
| High cost + no recovery                                 | surface-bound policy                                            |
| Improved recovery across later swaps                    | growing invariant representation                                |
| Good low-contrast performance but poor wrapper recovery | perceptual robustness without horizontal transfer               |
| Good wrapper recovery but poor low-contrast performance | invariant may transfer, but evidence-quality robustness is weak |

This distinction is important. A learner may become very good at degraded evidence in one wrapper without transferring the relation. Conversely, a learner may transfer the relation across wrappers but still struggle under poor signal quality. These are different training targets.

---

### Phase 7: Delayed re-check

Re-test later, preferably after sleep or spacing.

Goal:

```text
test slow schematic Gc candidate
```

Without delay, the protocol risks measuring short-term adaptation rather than portable structure.

A delayed probe should include:

```text
same wrapper re-check
changed wrapper re-check
boundary case
optional low-contrast / low-coherence robustness probe
```

---

## 10. Updated compact protocol logic

The revised horizontal-transfer protocol can be summarised like this:

```text
1. Establish relation at clear signal.
2. Use discrimination to construct the relevant variable.
3. Hone efficiency within the current wrapper.
4. Add brief evidence-quality perturbations only after stable accuracy.
5. Detect local flattening before surface-specific automation hardens.
6. Change wrapper while preserving the invariant.
7. Measure swap cost, recovery slope and error type.
8. Re-test after delay.
9. Treat only delayed cross-wrapper recovery as evidence for slow schematic Gc.
```

Or more compactly:

```text
Discrimination builds variables.
Contrast tests evidence-quality robustness.
Speed tests throughput.
Lures test control.
Wrapper swaps test horizontal transfer.
Prompts support vertical transfer.
Delayed probes test slow Gc.
```

---

## 11. Summary

The horizontal principle can be summarised as:

> **Horizontal transfer trains and tests whether a relational invariant survives surface change. During early learning, discrimination helps construct the relevant variables. During efficiency training, speed, gap reduction and mild signal-quality pressure refine operation within a wrapper. But lower contrast or coherence should be treated as a controlled attention-quality and evidence-quality perturbation, not as the main far-transfer mechanism. When learning begins to flatten, the protocol changes the wrapper while preserving the invariant, producing a controlled perturbation. The resulting swap cost and recovery curve indicate whether the learner had acquired a surface-specific policy or a deeper portable representation. Repeated successful recovery across wrappers, boundary cases and delays is treated as evidence for slow schematic Gc rather than thin automation.**

Even more compactly:

```text
Efficiency training:
same wrapper, harder evidence.

Evidence-quality perturbation:
same relation, poorer signal.

Invariance training:
new wrapper, same relation.

Horizontal transfer:
the relation survives the surface change.

Slow schematic Gc candidate:
the relation survives wrapper variation and delayed re-check.
```

---

## 12. Evidence-grounded summary

> The horizontal-transfer principle is grounded in the distinction between surface-specific skill improvement and portable relational abstraction. Perceptual-learning research shows that training can be highly specific to trained stimulus features, locations or task demands, especially after extensive or high-precision practice. However, transfer can improve under some forms of structured variation, attention-guided learning, exposure-plus-training and rule-based reweighting. Learning-curve research suggests that individual skill acquisition can include piecewise phases and transition points rather than one smooth curve, supporting the idea of timed perturbations near local asymptote. More broadly, desirable-difficulty and contextual-interference findings suggest that challenges which reduce immediate performance can support later retention or transfer when they are well calibrated. In Trident-G, wrapper swaps operationalise this idea: when the learner begins to compile a surface-specific strategy, the task changes surface while preserving the underlying relation. Lower contrast, lower coherence and speed pressure should be used as secondary evidence-quality or attention-quality perturbations, not as the main transfer target. The resulting swap cost, recovery slope and delayed survival are treated as evidence about whether the learner has acquired a portable relational invariant or only a thin surface policy.

---

## 13. Design rule for the app

The app should implement the following rule:

```text
Do not adapt only on accuracy.
Adapt on the error signature.
```

A useful internal controller would be:

```text
if accuracy_high_and_stable:
    reduce_gap_or_prepare_wrapper_swap()

elif lapse_pattern_or_slow_rt_drift and relation_previously_stable:
    run_brief_evidence_quality_probe()
    return_to_clear_signal()
    check_rebound()

elif relation_specific_errors:
    widen_gap()
    simplify_relation()
    repeat_variable_construction()

elif fast_impulsive_errors:
    reduce_speed_pressure()
    emphasise_accuracy()
    add_response_delay_if_needed()

elif sustained_poor_accuracy:
    route_to_reset_or_zone_recheck()
```

The practical claim boundary should be:

> Contrast and discrimination training may improve perceptual precision, visual robustness and attention quality under degraded evidence, but horizontal far transfer is only supported when the same relational invariant survives wrapper variation, boundary cases and delayed re-checks.

---

# References

Ahissar, M., & Hochstein, S. (1997). Task difficulty and the specificity of perceptual learning. *Nature, 387*, 401–406. [https://doi.org/10.1038/387401a0](https://doi.org/10.1038/387401a0)

Bjork, E. L., & Bjork, R. A. (2011). Making things hard on yourself, but in a good way: Creating desirable difficulties to enhance learning. In M. A. Gernsbacher, R. W. Pew, L. M. Hough, & J. R. Pomerantz (Eds.), *Psychology and the real world: Essays illustrating fundamental contributions to society* (pp. 56–64). Worth Publishers.

Czyż, S. H., Wójcik, A. M., Solarská, P., & Kiper, P. (2024). The effect of contextual interference on transfer in motor learning: A systematic review and meta-analysis. *Frontiers in Psychology, 15*, Article 1377122. [https://doi.org/10.3389/fpsyg.2024.1377122](https://doi.org/10.3389/fpsyg.2024.1377122)

DeLoss, D. J., Watanabe, T., & Andersen, G. J. (2015). Improving vision among older adults: Behavioural training to improve sight. *Psychological Science, 26*(4), 456–466. [https://doi.org/10.1177/0956797614567510](https://doi.org/10.1177/0956797614567510)

Donner, Y., & Hardy, J. L. (2015). Piecewise power laws in individual learning curves. *Psychonomic Bulletin & Review, 22*(5), 1308–1319. [https://doi.org/10.3758/s13423-015-0811-x](https://doi.org/10.3758/s13423-015-0811-x)

Jeter, P. E., Dosher, B. A., Petrov, A., & Lu, Z.-L. (2009). Task precision at transfer determines specificity of perceptual learning. *Journal of Vision, 9*(3), Article 1. [https://doi.org/10.1167/9.3.1](https://doi.org/10.1167/9.3.1)

Jeter, P. E., Dosher, B. A., Liu, S.-H., & Lu, Z.-L. (2010). Specificity of perceptual learning increases with increased training. *Vision Research, 50*(19), 1928–1940. [https://doi.org/10.1016/j.visres.2010.06.016](https://doi.org/10.1016/j.visres.2010.06.016)

Magill, R. A., & Hall, K. G. (1990). A review of the contextual interference effect in motor skill acquisition. *Human Movement Science, 9*(3–5), 241–289. [https://doi.org/10.1016/0167-9457(90)90005-X](https://doi.org/10.1016/0167-9457%2890%2990005-X)

Nguyen, K. N., Watanabe, T., & Andersen, G. J. (2020). Role of endogenous and exogenous attention in task-relevant visual perceptual learning. *PLOS ONE, 15*(8), Article e0237912. [https://doi.org/10.1371/journal.pone.0237912](https://doi.org/10.1371/journal.pone.0237912)

Roberts, M., & Carrasco, M. (2022). Exogenous attention generalises location transfer of perceptual learning in adults with amblyopia. *iScience, 25*(3), Article 103839. [https://doi.org/10.1016/j.isci.2022.103839](https://doi.org/10.1016/j.isci.2022.103839)

Tang, Y., Liang, J., & Zhou, Y. (2025). Perceptual learning improves spatial contrast sensitivity in older adults. *Frontiers in Neuroscience, 19*, Article 1681856. [https://doi.org/10.3389/fnins.2025.1681856](https://doi.org/10.3389/fnins.2025.1681856)

Yu, C. (2011). Visual perceptual learning and its specificity and transfer. *i-Perception, 2*(7), 873–875. [https://doi.org/10.1068/ic406](https://doi.org/10.1068/ic406)

Zhang, J.-Y., Zhang, G.-L., Xiao, L.-Q., Klein, S. A., Levi, D. M., & Yu, C. (2010). Rule-based learning explains visual perceptual learning and its specificity and transfer. *The Journal of Neuroscience, 30*(37), 12323–12328. [https://doi.org/10.1523/JNEUROSCI.0704-10.2010](https://doi.org/10.1523/JNEUROSCI.0704-10.2010)

Zhang, X.-Y., & Tang, C. (2025). Heavy-tailed update distributions arise from information-driven self-organisation in nonequilibrium learning. *Proceedings of the National Academy of Sciences, 122*(51), Article e2523012122. [https://doi.org/10.1073/pnas.2523012122](https://doi.org/10.1073/pnas.2523012122)

 

---

## 1. Verticle Principle

The vertical far-transfer principle is:

> **A trained cognitive skill is more likely to transfer when it is stacked across levels of control: from perceptual evidence extraction, through relational workspace and predictive mapping, into explicit strategy, real-world deployment, feedback and delayed reuse.**

The central problem in ordinary cognitive training is that a learner may improve at one layer without being able to deploy the gain elsewhere. A person may become faster at a perceptual task, better at a working-memory game, or more fluent at a puzzle, but still fail to apply the relevant operation in a real-world problem. Vertical transfer is designed to prevent this by turning task skill into a cross-layer **control policy**.

In compact form:

```text
CCC / perceptual evidence control
→ discrimination and variable abstraction
→ attention + working-memory binding
→ relational transformation tracking
→ SR-style predictive maps
→ optional formal inference tools
→ strategic problem-space control
→ metacognitive monitoring
→ implementation intention
→ real-world mission
→ outcome feedback
→ delayed reuse
```

This should be treated as a **testable engineering principle**, not as an already proven full-stack intervention.

---

## 2. Layer 1: CCC, perceptual evidence control and discrimination

The lower layer is about getting enough usable evidence into the system. The MFT-M / CCC literature is relevant here because Wu et al. (2016) modelled cognitive-control capacity as a limited information channel and estimated it at roughly 3–4 bits per second in a perceptual decision task. ([Nature][1])

In Trident-G terms, this is not “intelligence” by itself. It is a **controlled evidence-throughput layer**:

```text
Can the learner extract the relevant signal?
Can they do it under masking, time pressure or ambiguity?
Can they avoid being captured by irrelevant features?
```

This layer supports later reasoning because later reasoning needs reliable inputs. But it does not automatically become reasoning. The Trident-G documents make this point clearly: discrimination builds variables, WM binds variables, SR training learns transitions over variables, and boundary testing validates the relation. 

So the first vertical move is:

```text
raw surface
→ discriminated feature
→ usable variable
```

---

## 3. Layer 2: Association, abstraction and working-memory binding

The next layer is **variable binding**. Once the system can identify what matters, it must hold those variables together across time.

Oberauer’s binding account is directly relevant: working-memory capacity appears to limit the maintenance of temporary bindings, such as which item belongs with which position, context or partner (Oberauer, 2019). ([Journal of Cognition][2]) This supports the Trident-G move from ordinary item n-back to relational workspace training:

```text
item identity
→ hold and match

becomes

variable / relation
→ bind, update and compare
```

This is also why ordinary WM training should be treated cautiously. Meta-analytic evidence indicates that WM training reliably improves trained or closely related WM tasks, but does not provide good evidence for broad far transfer to intelligence or real-world cognitive skills by itself (Melby-Lervåg et al., 2016). ([Sage Journals][3])

The vertical principle therefore says:

> **WM is not the endpoint. It is the workspace in which variables, relations, constraints, appraisals and predictions can be held long enough to be used by higher-level control policies.**

---

## 4. Layer 3: Relational processing and SR-style predictive maps

The third layer is where the system begins to move from binding to **predictive structure**.

Successor representation theory is useful here because it represents a state in terms of the states likely to follow from it. Stachenfeld et al. (2017) argue that the hippocampus can function as a predictive map, representing each current state in terms of expected successor states. ([Gershman Lab][4]) Gershman (2018) similarly frames successor representations as encoding predictive relationships among states.

For Trident-G, this gives the middle-layer target:

```text
state
→ transition
→ consequence
→ next reachable state
```

rather than:

```text
stimulus
→ response
```

This is the bridge from relational working memory into problem solving. The learner is no longer only remembering a relation. They are learning how a relation changes the reachable future state-space.

This is also where relational integration training becomes important. Wang et al. (2025) found that relational integration training modulated frontoparietal EEG microstate dynamics associated with fluid intelligence, supporting the view that relation-preserving n-back-like training targets a more Gf-relevant substrate than item n-back alone. ([PubMed][5])

The safe claim is:

> **Relational and SR-style training targets a more transfer-relevant middle layer than item memory, because it trains structured relations over time.**

Not:

> “This has already been proven to raise g.”

---

## 5. Optional layer: explicit inference tools

Your suggested inference-skill layer fits, but I would keep it **optional and modular**, not as the compulsory core of the vertical stack.

This layer would include object-level reasoning tools such as:

```text
deductive inference
necessary / sufficient conditions
probabilistic reasoning
Bayesian updating
expected value
causal inference
confound checking
argument mapping
```

There is evidence that some inferential rules can be taught and transferred, especially when they are presented as usable schemas rather than purely formal logic. Nisbett et al. (1987) argued that even brief training in inferential rules can improve their use in everyday reasoning. ([PubMed][6]) Fong et al. (1986) found that statistical training increased the frequency and quality of statistical reasoning across everyday problems. ([ScienceDirect][7]) Cheng and Holyoak’s pragmatic reasoning schema work also suggests that abstract permission schemas can facilitate conditional reasoning in Wason-style tasks. ([PubMed][8])

In Trident-G terms, these are **mindware scripts**: explicit, language-accessible tools that can be broadcast into the global workspace when the task calls for them. They should not replace the meta-epistemic prompt layer. They sit underneath or alongside it as domain-general reasoning tools.

Example:

```text
Meta-prompt:
“What would make this wrong?”

Optional inference tool:
“Check the necessary condition.”
“Check the base rate.”
“Compute expected value.”
“Ask whether the evidence discriminates between hypotheses.”
```

So this layer is valuable, but the protocol should avoid becoming a formal logic course. The core vertical stack is broader: it trains when and how to deploy the right control policy.

---

## 6. Layer 4: Strategic planning and problem-space inference

The next layer is **problem-space control**. Here, trained variables and predictive relations are applied in bounded or real problem spaces:

```text
current state
→ goal state
→ constraints
→ operators
→ possible paths
→ next test
```

This is the level where puzzles and real-world problems become important. The Trident-G protocol notes that problem-space puzzles provide constrained search environments, while meta-epistemic prompts convert successful lower-level operations into reusable mindware scripts. 

The empirical support here comes from strategy instruction, inductive reasoning, self-explanation and metacognitive training. Donker et al. (2014) reviewed 58 studies and found that cognitive, metacognitive and management strategy instruction can improve academic performance. ([ScienceDirect][9]) Bisra et al. (2018) found a positive overall effect for self-explanation prompts across 69 effects from 64 reports. ([Semantic Scholar][10]) Klauer and Phye’s review/meta-analysis summarised 74 inductive-reasoning training experiments and reported support for training comparison-based inductive reasoning. ([Sage Journals][11])

This gives the vertical transition:

```text
implicit relation tracking
→ explicit prompt
→ strategic move in a problem space
```

The prompt does not “contain” the intelligence. It is a control handle that coordinates lower-level processes. The Trident-G protocol explicitly describes prompts such as “What must be true?”, “Which feature changed?” and “Can this path still reach the goal?” as portable handles that coordinate attention, WM binding, SR search and action. 

---

## 7. Layer 5: Implementation intentions and real-world missions

Implementation intentions are the clearest evidence base for turning strategy possession into strategy deployment. Gollwitzer and Sheeran’s meta-analysis found that implementation intentions had a medium-to-large effect on goal attainment, *d* = .65, across 94 independent tests (Gollwitzer & Sheeran, 2006). ([kops.uni-konstanz.de][12]) Mental contrasting with implementation intentions also has meta-analytic support, with Wang et al. (2021) reporting a small-to-medium effect on goal attainment across 21 studies and 15,907 participants. ([Frontiers][13])

In Trident-G terms, implementation intentions are not reasoning skills. They are **deployment scripts**:

```text
If cue X appears,
then I will run script Y.
```

A mission version adds action and feedback:

```text
If I face a confusing task today,
then I will write:
state → goal → constraints → next test,
take one small action,
and record what happened.
```

This is where vertical transfer becomes niche transfer. The app can train a strategy, but a real-world mission tests whether the strategy activates under live cues, survives noise, guides action and generates useful feedback. Transfer-of-training evidence supports this general concern: Blume et al. (2010) found that transfer depends on trainee characteristics, intervention design and the work/environmental context, not just the training event itself. ([Sage Journals][14])

---

## 8. Efficiency compounding through the stack

A useful way to express the vertical principle is **efficiency compounding**.

Each layer makes the next layer less costly:

| Layer                            | Efficiency contribution                                 |
| -------------------------------- | ------------------------------------------------------- |
| **CCC / perceptual control**     | more usable evidence per unit time                      |
| **Discrimination / abstraction** | faster identification of relevant variables             |
| **WM binding**                   | less loss of relation structure under load              |
| **SR mapping**                   | faster prediction of reachable next states              |
| **Inference tools**              | reusable reasoning shortcuts for common structures      |
| **Problem-space control**        | reduced search through better constraints and operators |
| **Metacognition**                | less time wasted on failing strategies                  |
| **Implementation intentions**    | less friction between intention and action              |
| **Missions**                     | real-world feedback refines deployment                  |
| **Delayed reuse**                | policy becomes easier to recover later                  |

The compounding claim is not that each layer magically transfers upward. It is that each layer reduces friction for the next layer **when explicitly connected**.

A weak stack is:

```text
better perception
+ better WM
+ puzzles
+ prompts
```

A vertical stack is:

```text
better evidence
→ better variable selection
→ better relation holding
→ better prediction
→ better problem-space move
→ better prompt selection
→ better real-world action
→ better feedback update
```

This is why the protocol should score more than final correctness. It should score cue detection, variable selection, relation accuracy, probe efficiency, prompt use, mission completion, feedback quality and delayed reuse.

---

## 9. Reinforcement and vertical credit assignment

The reinforcement principle is that feedback should strengthen the **whole chain**, not just the final answer.

The Trident-G source says vertical transfer is strengthened when feedback reinforces cross-layer coordination: state readiness, attention-control stability, variable binding, SR-style inference, epistemic prompt selection, mission action, real-world implementation and delayed reuse. 

This creates **vertical credit assignment**:

```text
Did the learner succeed because they were in the right state?
Because they sampled the right feature?
Because they held the right relation?
Because they used the right prompt?
Because they chose the right probe?
Because the cue fired in real life?
Because feedback revised the policy?
```

Feedback research supports the need for this specificity. Hattie and Timperley (2007) argue that feedback is powerful but can be positive or negative depending on whether it helps learners understand the goal, their progress and the next action. ([Sage Journals][15]) Reinforcement-learning theory also supports the idea that prediction-error signals update policies when outcomes are better or worse than expected (Schultz et al., 1997; Schultz, 2016). ([PubMed][16])

So, in product terms:

```text
Do not only reward:
“Correct.”

Reward:
“You selected the right variable.”
“You avoided a lure.”
“You chose a useful probe.”
“You used the prompt at the right cue.”
“You logged informative feedback.”
“You reused the policy after delay.”
```

That is the vertical reinforcement mechanism.

---

## 10. Protocol summary: vertical stacking cycle

A clean protocol sequence would be:

### Phase 1: Gate the state

Use Zone/CCC-style readiness to determine whether the learner should train, reduce difficulty, reset or rest.

```text
state readiness
→ train / stabilise / reduce / delay
```

### Phase 2: Build the variable layer

Train discrimination and abstraction:

```text
Which feature changed?
What is relevant?
What is noise?
```

### Phase 3: Bind and update relations

Use relational WM / n-back variants:

```text
What belongs with what?
What changed across time?
What relation must be held?
```

### Phase 4: Build predictive maps

Train successor-style inference:

```text
Given this state, what follows?
Can this path still reach the goal?
What action changes the reachable future state?
```

### Phase 5: Add explicit inference tools where useful

Introduce compact mindware scripts:

```text
What must be true?
What would make this wrong?
What is the base rate?
What is the expected value?
What evidence distinguishes the hypotheses?
```

### Phase 6: Apply in a problem space

Use puzzles, reasoning tasks or appraisal-space tasks:

```text
state → goal → constraint → operator → next test
```

### Phase 7: Deploy with an implementation intention

Bind the strategy to a cue:

```text
If I feel stuck,
then I will identify the state, goal, constraint and next test.
```

### Phase 8: Run a real-world mission

Convert the script into action:

```text
detect cue
→ run script
→ take action
→ log outcome
```

### Phase 9: Reinforce and re-test

Use feedback and delay:

```text
what worked?
which layer failed?
does the policy return tomorrow / next week?
```

---

## 11. Compact formulation

The vertical principle can be written as:

> **Vertical transfer trains the conversion of capacity into deployable policy. It begins with controlled evidence extraction and variable discrimination, then stacks working-memory binding, relational/SR-style prediction, explicit inference tools, problem-space strategy, metacognitive prompting, implementation intentions and real-world missions. Each layer should make the next layer more efficient, and feedback should reinforce the whole chain rather than merely reward final correctness. A skill is vertically transferred only when the learner can enter the right state, select the right variables, hold the right relation, run the right inference, choose the right prompt, act in the real niche, learn from feedback and recover the policy after delay.**

Even more compactly:

```text
Horizontal transfer:
Does the relation survive a new surface?

Vertical transfer:
Can the policy be rebuilt from perception to action?

Niche transfer:
Does the policy work in the real environment?
```

---

## 12. Evidence-grounded wording for the protocol document

> The vertical-transfer principle is grounded in the view that cognitive training gains must be connected across levels of control. CCC/MFT-M gives a model of controlled evidence throughput (Wu et al., 2016), while working-memory binding theory suggests that temporary relation maintenance is a central capacity limit (Oberauer, 2019). Successor-representation theory provides a computational model for learning state–transition–future-state structure (Stachenfeld et al., 2017; Gershman, 2018). Strategy instruction, self-explanation and inductive-reasoning training support the value of explicit language-accessible control policies (Bisra et al., 2018; Donker et al., 2014; Klauer & Phye, 2008). Implementation intentions and mental contrasting support cue-linked real-world deployment (Gollwitzer & Sheeran, 2006; Wang et al., 2021). Feedback and reinforcement-learning research support the importance of process-level outcome information and prediction-error-based policy updating (Hattie & Timperley, 2007; Schultz et al., 1997). The full Trident-G vertical stack remains an evidence-generating hypothesis, but each layer has independent support, and the stack gives a clear way to test whether capacity becomes deployable adaptive policy.

---

# Key references

Bisra, K., Liu, Q., Nesbit, J. C., Salimi, F., & Winne, P. H. (2018). Inducing self-explanation: A meta-analysis. *Educational Psychology Review, 30*, 703–725. [https://doi.org/10.1007/s10648-018-9434-x](https://doi.org/10.1007/s10648-018-9434-x)

Blume, B. D., Ford, J. K., Baldwin, T. T., & Huang, J. L. (2010). Transfer of training: A meta-analytic review. *Journal of Management, 36*(4), 1065–1105. [https://doi.org/10.1177/0149206309352880](https://doi.org/10.1177/0149206309352880)

Cheng, P. W., & Holyoak, K. J. (1985). Pragmatic reasoning schemas. *Cognitive Psychology, 17*(4), 391–416. [https://doi.org/10.1016/0010-0285(85)90014-3](https://doi.org/10.1016/0010-0285%2885%2990014-3)

Donker, A. S., de Boer, H., Kostons, D., Dignath van Ewijk, C. C., & van der Werf, M. P. C. (2014). Effectiveness of learning strategy instruction on academic performance: A meta-analysis. *Educational Research Review, 11*, 1–26. [https://doi.org/10.1016/j.edurev.2013.11.002](https://doi.org/10.1016/j.edurev.2013.11.002)

Fong, G. T., Krantz, D. H., & Nisbett, R. E. (1986). The effects of statistical training on thinking about everyday problems. *Cognitive Psychology, 18*(3), 253–292. [https://doi.org/10.1016/0010-0285(86)90001-0](https://doi.org/10.1016/0010-0285%2886%2990001-0)

Gershman, S. J. (2018). The successor representation: Its computational logic and neural substrates. *The Journal of Neuroscience, 38*(33), 7193–7200. [https://doi.org/10.1523/JNEUROSCI.0151-18.2018](https://doi.org/10.1523/JNEUROSCI.0151-18.2018)

Gollwitzer, P. M., & Sheeran, P. (2006). Implementation intentions and goal achievement: A meta-analysis of effects and processes. *Advances in Experimental Social Psychology, 38*, 69–119. [https://doi.org/10.1016/S0065-2601(06)38002-1](https://doi.org/10.1016/S0065-2601%2806%2938002-1)

Hattie, J., & Timperley, H. (2007). The power of feedback. *Review of Educational Research, 77*(1), 81–112. [https://doi.org/10.3102/003465430298487](https://doi.org/10.3102/003465430298487)

Klauer, K. J., & Phye, G. D. (2008). Inductive reasoning: A training approach. *Review of Educational Research, 78*(1), 85–123. [https://doi.org/10.3102/0034654307313402](https://doi.org/10.3102/0034654307313402)

Melby-Lervåg, M., Redick, T. S., & Hulme, C. (2016). Working memory training does not improve performance on measures of intelligence or other measures of “far transfer”: Evidence from a meta-analytic review. *Perspectives on Psychological Science, 11*(4), 512–534. [https://doi.org/10.1177/1745691616635612](https://doi.org/10.1177/1745691616635612)

Nisbett, R. E., Fong, G. T., Lehman, D. R., & Cheng, P. W. (1987). Teaching reasoning. *Science, 238*(4827), 625–631. [https://doi.org/10.1126/science.3672116](https://doi.org/10.1126/science.3672116)

Oberauer, K. (2019). Working memory capacity limits memory for bindings. *Journal of Cognition, 2*(1), Article 40. [https://doi.org/10.5334/joc.86](https://doi.org/10.5334/joc.86)

Schultz, W. (2016). Dopamine reward prediction error coding. *Dialogues in Clinical Neuroscience, 18*(1), 23–32. [https://doi.org/10.31887/DCNS.2016.18.1/wschultz](https://doi.org/10.31887/DCNS.2016.18.1/wschultz)

Schultz, W., Dayan, P., & Montague, P. R. (1997). A neural substrate of prediction and reward. *Science, 275*(5306), 1593–1599. [https://doi.org/10.1126/science.275.5306.1593](https://doi.org/10.1126/science.275.5306.1593)

Stachenfeld, K. L., Botvinick, M. M., & Gershman, S. J. (2017). The hippocampus as a predictive map. *Nature Neuroscience, 20*(11), 1643–1653. [https://doi.org/10.1038/nn.4650](https://doi.org/10.1038/nn.4650)

Wang, G., Wang, Y., & Gai, X. (2021). A meta-analysis of the effects of mental contrasting with implementation intentions on goal attainment. *Frontiers in Psychology, 12*, Article 565202. [https://doi.org/10.3389/fpsyg.2021.565202](https://doi.org/10.3389/fpsyg.2021.565202)

Wang, Z., Sun, T., & Xiao, F. (2025). Relational integration training modulated the frontoparietal network for fluid intelligence: An EEG microstates study. *Brain Topography, 38*, Article 24. [https://doi.org/10.1007/s10548-024-01099-3](https://doi.org/10.1007/s10548-024-01099-3)

Wu, T., Dufford, A. J., Mackie, M.-A., Egan, L. J., & Fan, J. (2016). The capacity of cognitive control estimated from a perceptual decision making task. *Scientific Reports, 6*, Article 34025. [https://doi.org/10.1038/srep34025](https://doi.org/10.1038/srep34025)

 

