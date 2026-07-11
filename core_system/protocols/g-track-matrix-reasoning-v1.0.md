---
Version: v1.0
Date: 2026-07-10
Owner: Mindware Lab (Trident G IQ)
Status: Public methodological protocol
Scope: HRP Lab G Track Matrix Reasoning benchmark
"Claims boundary": Non-clinical fluid-reasoning benchmark; sample-calibrated Matrix Index where stated
---

# G Track Matrix Reasoning Benchmark Protocol

## 1) Purpose and scope

The G Track Matrix Reasoning benchmark is a repeated-measures fluid-reasoning assessment used in the HRP Lab G Track battery. It measures nonverbal abstract reasoning using matrix-completion items from the Open Matrices Item Bank.

The public construct label is:

- Matrix Reasoning

The G Track score object uses:

`construct: "matrix_reasoning"`

The benchmark is assessment, not training. It is not a clinical instrument, not a diagnostic test, and not a complete IQ test battery. It should be interpreted as a matrix-reasoning/fluid-reasoning benchmark rather than as a full-scale IQ estimate.

## 2) Source database

The item source is:

- Open Matrices Item Bank (OMIB)
- Koch, M., Spinath, F. M., Greiff, S., & Becker, N. (2022). Development and Validation of the Open Matrices Item Bank. Journal of Intelligence, 10(3), 41.
- DOI: `https://doi.org/10.3390/jintelligence10030041`
- Source materials: `https://osf.io/4km79/`
- Licence: GNU General Public License version 3 (GPL-3.0)

The local G Track distribution includes selected unmodified OMIB SVG item stems and construction elements. The public item metadata include source item number, rule count, difficulty, discrimination, item-rest value, image dimensions, and file hash. Protected metadata include answer-key solution vectors and item-construction codes.

## 3) Battery placement and forms

The programme forms are:

- `matrix-a`
- `matrix-b`
- `matrix-c`

The hook form is:

- `matrix-d`

Forms A-C can serve one of three roles:

- Baseline / pre-training
- Post-training
- Follow-up

The app tracks form identity and measurement role separately. Participants are counterbalanced across Forms A-C so that, over time, each form can contribute baseline data rather than Form A always being the baseline form.

Form D is a separate hook/entry form. It is scored, but it is kept separate from programme benchmark norms and from baseline/post/follow-up gain modelling.

Only valid baseline/pre-training completions of Forms A-C can contribute to IQMindware G Track sample norms. Post-training and follow-up completions are saved, scored, and exportable as proof/change evidence, but are excluded from norm-building.

## 4) Form construction

Each matrix form contains:

- 16 OMIB matrix items
- 20 construction elements available as response options
- no overlapping items within a form
- item metadata from the OMIB calibration set

Each programme form samples the same rule-count structure:

| Rule count | Items per form |
| ---: | ---: |
| 1 | 1 |
| 2 | 4 |
| 3 | 6 |
| 4 | 4 |
| 5 | 1 |

This gives each 16-item form a spread from easier lower-rule items to more complex higher-rule items.

Current form-level metadata:

| Form | Items | Difficulty range | Discrimination range | Role |
| --- | ---: | ---: | ---: | --- |
| matrix-a | 16 | -1.28 to 0.52 | 1.63 to 5.16 | programme |
| matrix-b | 16 | -0.79 to 0.60 | 1.74 to 3.86 | programme |
| matrix-c | 16 | -1.36 to 0.58 | 1.69 to 5.08 | programme |
| matrix-d | 16 | -0.73 to 0.49 | 1.70 to 4.05 | hook |

The exact item IDs and answer keys are not published in this public protocol. Public method disclosure should make the protocol auditable without making repeated attempts easier to game.

## 5) User task

Each item shows a matrix reasoning stem with the bottom-right cell missing. The participant builds the missing cell by selecting every construction element required for the correct answer.

The response bank contains 20 construction elements. For each item, the participant can toggle each element on or off.

The recorded answer is therefore a 20-position binary vector:

- `1` means the construction element was selected
- `0` means the construction element was not selected

The task is untimed for item solving, but the session has a validity window. The app preloads and decodes all 16 item stems and all 20 construction elements before timing begins. Missing assets prevent the test from starting.

## 6) Trial-level scoring

All canonical scoring is performed server-side.

For each item:

1. The submitted 20-position binary vector is checked for valid format.
2. The submitted vector is compared with the protected OMIB-derived solution vector.
3. The item is scored correct only if the submitted vector exactly matches the protected solution.
4. Partial element-level credit is not awarded in v1.

Raw score is:

`raw = number of exactly correct items`

Maximum raw score is:

`maxRaw = 16`

Raw score is useful for transparency but is not the primary score because the selected items differ in difficulty and discrimination.

## 7) IRT/EAP scoring

The primary score is an expected-a-posteriori ability estimate:

`theta`

The model uses the OMIB item parameters in a two-parameter logistic item-response model:

`P(correct | theta) = logistic(discrimination * (theta - difficulty))`

For each candidate theta value, the server calculates:

- the likelihood of the participant's correct/incorrect response pattern
- a standard normal prior density
- the posterior weight for that theta value

The current EAP grid is:

- minimum theta: -4
- maximum theta: 4
- step size: 0.05

The server returns:

- `theta`: posterior mean
- `standardError`: posterior standard deviation
- `raw`: number correct
- `maxRaw`: 16

This EAP estimate is more informative than raw score because it gives more weight to response patterns involving harder and more discriminating items.

## 8) Matrix Index standardisation

The G Track Matrix result displays a Matrix Index when the current calibration status and estimate stability permit it.

The Matrix Index uses:

`standard score = 100 + 15z`

where:

`z = (theta - calibration mean) / calibration SD`

Current launch calibration values:

- model ID: `omib_matrix_2pl_eap`
- model version: `v1`
- calibration label: `brevo_calibration_2026_wave1_pooled`
- calibration N: 33
- calibration mean theta: 0.425678787878788
- calibration SD theta: 0.835307843237141

The displayed confidence interval is derived by applying the same transformation to:

- `theta - standardError`
- `theta + standardError`

This is a provisional sample-calibrated Matrix Index, not a full-population IQ norm.

## 9) Calibration and norm-status rules

Matrix Index status is governed by the size of the eligible calibration sample and the uncertainty of the individual estimate.

Current status labels:

| Eligible calibration N | Status |
| ---: | --- |
| below 30 | not shown |
| 30 to 99 | sample calibration |
| 100 to 299 | provisional sample norm |
| 300 to 499 | moderate-confidence sample norm |
| 500 and above | stronger sample norm |

Confidence labels:

- below 30 eligible calibrations: insufficient data
- standard error above 1.1: unstable estimate
- 30 to 99 eligible calibrations: calibrating
- 100 to 499 eligible calibrations: moderate confidence
- 500 and above: high confidence

The broader CPT norm pipeline also stores live aggregate snapshots. Standard scores from that aggregate path are withheld until the relevant metric reaches at least 100 eligible attempts.

## 10) Norm-building rules

Only these attempts can enter IQMindware G Track norm-building:

- participant opted into cloud benchmark contribution
- construct is `matrix_reasoning`
- form is one of `matrix-a`, `matrix-b`, or `matrix-c`
- role is baseline/pre-training
- attempt passes validity checks
- first valid eligible baseline attempt for that participant/construct

These attempts are excluded from norm-building:

- private guest attempts
- cloud personal attempts without benchmark contribution
- post-training attempts
- follow-up attempts
- hook-form attempts
- invalid or implausibly fast attempts
- repeat baseline attempts after a first valid eligible attempt

This separation is important because the battery is used both for population calibration and for within-person training-gain tracking.

## 11) Gain and confidence policy

The protocol is designed for training-gain measurement using baseline, post-training, and follow-up roles.

Because matrix forms are not identical items, standardised gains should be modelled using paired IQMindware data and form-role information rather than by subtracting raw scores.

Raw change can be reported descriptively:

- post theta minus baseline theta
- follow-up theta minus baseline theta
- post Matrix Index minus baseline Matrix Index, when both values are displayable

Standardised gain scores require enough paired data to estimate:

- form effects
- practice effects
- regression to the mean
- standard error of measurement
- expected retest change
- uncertainty around the individual gain

Recommended future gain reporting:

- theta change
- Matrix Index change
- standardised change against paired baseline/post or baseline/follow-up distributions
- confidence interval or confidence band
- form-role adjustment
- sample size used for the gain norm

Until paired-change calibration is available, the app should avoid overclaiming individual intelligence gains from a single pre/post contrast.

## 12) Proof export and app-stack use

Matrix Reasoning results are stored as G Track score objects under:

`construct: "matrix_reasoning"`

Exported proof can include:

- form ID
- measurement role
- raw score
- max raw score
- theta
- standard error
- provisional Matrix Index when displayable
- confidence label
- norm status
- calibration model metadata

The proof payload can be passed to other IQMindware apps as external reasoning evidence without exposing protected answer keys.

## 13) Claims boundary

Allowed:

- Matrix-reasoning benchmark tracking
- OMIB-based fluid-reasoning evidence
- raw score, theta, and standard error reporting
- clearly labelled Matrix Index when calibration status permits
- baseline/post/follow-up change tracking
- future standardised gain claims only after paired calibration data support them

Not allowed:

- clinical diagnosis
- full-scale IQ claims
- claims of equivalence to WAIS, Raven's, or any closed publisher test
- guaranteed training gains
- population percentile claims before calibration thresholds are met
- treating one short matrix form as a complete measure of intelligence

## 14) Open vs protected

Open:

- source database and licence
- task family and response format
- form size and rule-count structure
- raw scoring rule
- EAP/2PL scoring model
- Matrix Index standardisation formula
- calibration-status thresholds
- norm eligibility rules
- claims boundaries

Protected:

- answer-key solution vectors
- item-construction codes where they enable answer reconstruction
- item-ordering/assignment details that increase gaming risk
- anti-abuse controls
- private deployment infrastructure
- user-level data

