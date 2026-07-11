---
Version: v1.0
Date: 2026-03-06
Owner: Mindware Lab (Trident G IQ)
Status: Public battery protocol publication
Scope: Repeatable cognitive evidence battery for baseline/follow-up and trajectory tracking
"Claims boundary": Module-specific validation status; non-clinical interpretation only
---

# G Track Evidence Battery Protocol

## 1) Purpose and scope

G Track is a local-first cognitive evidence battery for repeated measurement across a training or performance cycle.

It is assessment, not training.

## 2) Battery architecture

1. Pre/post outcome checks
2. Repeatable trackers (weekly/fortnightly)
3. Optional external benchmark checks

## 3) Modules and constructs

- SgS-12 A/B: brief fluid-reasoning snapshot
- Matrix Reasoning: OMIB-based fluid-reasoning benchmark
- CRS-10: cognitive resilience under pressure
- Psi-CBS Core/AD/AI: applied cognitive bandwidth and related markers
- EDHS A/B: everyday decision-habit profile
- Attention Control: short ANT plus SART attention benchmark
- Working Memory: visuospatial complex span plus visual binding change detection
- Optional Mensa online benchmark: external reference only

## 4) Validation status registry

Status labels:
- `published_foundation`
- `published_summary`
- `in_testing`
- `external_reference`

Current status:
- SgS-12 A/B: `published_foundation` (ICAR item-family psychometric foundation)
- Matrix Reasoning: `published_foundation` (Open Matrices Item Bank item-source foundation) with IQMindware sample calibration
- CRS-10: `published_summary`
- Psi-CBS modules: `in_testing`
- EDHS A/B: `in_testing`
- Attention Control: `in_testing` with provisional ANT seed calibration and SART sample calibration collecting
- Working Memory: `in_testing` with provisional app bands only
- Mensa online benchmark: `external_reference`

## 5) Distinction from in-app scores

The battery is designed as an evidence layer and should not be treated as equivalent to in-app performance scores.

## 6) Validation and replication policy

Independent validation studies are encouraged.

Priority domains:
- Convergent/discriminant validity
- Test-retest stability
- Sensitivity to change
- Trend robustness under routine variability

## 7) Copyright and licensing posture

- Mindware-authored modules are not tied to closed test-publisher validation lockouts
- Independent validation studies are encouraged with proper citation and conservative claims
- Third-party sources follow their own terms (ICAR/OMIB/Mensa)

## 8) Open vs protected

Open:
- Battery architecture, constructs, validation-status labels, interpretation families

Protected:
- Item-selection seeds, anti-gaming controls, internal implementation constants

## 9) Claims boundary

Allowed:
- Repeated cognitive outcome tracking with explicit module-level validation status

Not allowed:
- Blanket "fully validated battery" claims
- Clinical diagnosis/treatment claims
- Guaranteed IQ/performance outcomes

## 10) Related governance

- `../../governance/RESEARCH_AND_EVIDENCE_POLICY.md`
- `../../governance/CLAIMS_AND_SAFETY.md`
- `../../governance/DATA_PUBLICATION_POLICY.md`

## 11) Related public protocols

- `g-track-attention-control-v1.0.md`
- `g-track-working-memory-v1.0.md`
- `g-track-matrix-reasoning-v1.0.md`
