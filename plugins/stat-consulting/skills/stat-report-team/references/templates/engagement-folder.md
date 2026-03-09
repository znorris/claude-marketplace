# Engagement Folder Structure

At the start of every engagement, the Manager creates this folder structure. This is the single
source of truth for all agents. All work products are stored here, and agents read from and write
to their designated sections.

## Directory Structure

```
engagement/
├── config.md                       # Manager: engagement metadata and status
├── research_spec.md                # Design Architect: frozen research specification
├── decision_log.md                 # Manager: all decisions, escalations, rollbacks
├── sampling/
│   ├── design.md                   # Sampling Strategist: sampling strategy and strata
│   ├── power_analysis.md           # Sampling Strategist: sample size calculations
│   └── variables.md                # Sampling Strategist: variable definitions and data manifest
├── sources/
│   ├── inventory.md                # Source Scout: discovered sources and quality ratings
│   ├── coverage_map.md             # Source Scout: stratum coverage status
│   ├── ESCALATIONS.md              # All agents: escalation log for Manager relay to user
│   ├── collection_requests/        # Source Scout: user-facing data collection requests
│   │   └── (CR-001.md, etc.)
│   └── user_submissions/           # User-supplied data in response to collection requests
│       └── (submission_001.csv, etc.)
├── data/
│   ├── validation_log.md           # Collection & Validation: quality checks and flags
│   ├── cleaning_notes.md           # Collection & Validation: transformation log
│   └── datasets/                   # Collection & Validation: cleaned data files
│       └── (cleaned_source_001.csv, etc.)
├── analysis/
│   ├── primary_results.md          # Analyst: main findings
│   ├── sensitivity.md              # Analyst: sensitivity/robustness analyses
│   └── confidence_assessment.md    # Analyst: tier assignments per stratum and aggregate
├── report/
│   ├── draft.md                    # Report Composer: the final report
│   └── limitations.md             # Report Composer: detailed limitations section
└── archive/                        # Manager: rolled-back artifacts
    └── (RB-001_manifest.md, etc.)
```

## Initial File Contents

### config.md

```markdown
# Engagement Configuration

## Metadata
- **Engagement ID**: [auto-generated or assigned]
- **Created**: [date]
- **Client request**: [original request text]
- **Client sophistication**: [to be assessed during Phase 0]
- **Current phase**: Phase 0, Intake
- **Status**: Active

## Phase Status
| Phase | Status | Gate Approved | Date |
|-------|--------|---------------|------|
| 0: Intake | In Progress | N/A | [date] |
| 1: Formulation | Not Started | - | - |
| 2: Sampling Design | Not Started | - | - |
| 3: Data Acquisition | Not Started | - | - |
| 4: Validation | Not Started | - | - |
| 5: Analysis | Not Started | - | - |
| 6: Reporting | Not Started | - | - |

## Active Agents
[Updated as agents are dispatched]

## Rollback Count
- Level 1: 0
- Level 2: 0
```

### decision_log.md

```markdown
# Decision Log

All scope decisions, design choices, escalation resolutions, and rollbacks are recorded here in
chronological order. Each entry is timestamped and attributed to the deciding agent or the Manager.

---

## [DEC-001], Engagement Initiated, [Date]
- **Decision**: Engagement created for client request: "[request summary]"
- **Decided by**: Manager
- **Rationale**: Request assessed as feasible and within system capabilities

---
```

### All other files

Created empty or with a header comment indicating the responsible agent. Agents populate their
files during their respective phases.

## Access Control Summary

| Agent | Writes To | Reads From |
|-------|-----------|------------|
| Manager | config.md, decision_log.md | Everything |
| Design Architect | research_spec.md, decision_log.md | config.md |
| Sampling Strategist | sampling/*, decision_log.md | research_spec.md, config.md, Source Scout feasibility reports |
| Source Scout | sources/*, decision_log.md | sampling/*, research_spec.md, config.md |
| Collection & Validation | data/*, decision_log.md | sources/*, sampling/variables.md |
| Analyst | analysis/*, decision_log.md | sampling/*, data/*, config.md |
| Report Composer | report/* | Everything (read-only for report assembly) |
| Manager (rollback) | archive/*, all files (for updates) | Everything |
