# Variable Selection Criteria & Preprocessing Decisions

## Overview

This document records the variable selection criteria and preprocessing decisions made during the survey data analysis phase. The source dataset is the **Seoul Metropolitan Government Isolated/Reclusive Youth Survey (2023)**, which contains 300+ features across demographic, behavioral, psychological, and social dimensions.

Our team divided the survey variables into units (2-person teams), each responsible for evaluating which variables to include in the predictive model.

## My Role

- **Unit 2 (with teammate Hyunju)**: Responsible for sections A13–B12
- **Variable selection criteria design**: Established the analytical framework used by our unit
- **Preprocessing logic**: Defined missing value treatment strategies for assigned variables

## Selection Criteria (Unit 2)

Four criteria were applied to evaluate each variable:

1. **Is the response quantitative?** (Even if filtered through respondent memory, does it yield a measurable value?)
2. **Can the response be reduced to yes/no?**
3. **For subjective/psychological responses, is there no quantitative alternative available?**
4. **Is this a significant predictor of isolation/reclusion based on prior research?**

## Variable Decisions (A13–B12)

### Included Variables with Rationale

| Variable | Description | Rationale |
|----------|-------------|-----------|
| A13_1–A13_4 | Face-to-face social interaction frequency | Objective measure; social exchange is critical for isolation assessment |
| A14 | Online interaction frequency | Objective measure of social connectivity |
| A15 | Past isolation experience | Prior isolation is a significant predictor of recurrence; related to chronicity |
| A16_1–A16_11 | Self-reported causes of isolation | Subjective but reveals perceived causality; necessary for analysis |
| A17 | Online communication (yes/no) | Binary response, cleanly separable |
| A17_1_1–A17_1_5 | Online communication methods | Relevant for downstream enterprise recommendations |
| A18_1–A18_4 | Subjective feelings of isolation | Only "feeling isolated" retained as representative; no quantitative proxy exists |
| A19_1–A19_6 | Negative experiences before adulthood | Binary (yes/no); pre-adult adversity is a significant predictor |
| A20_1–A20_8 | Negative experiences in adulthood | Binary (yes/no); adult adversity is also a significant predictor |
| B1_1–B1_2 | Subjective version of A13 interaction questions | Cross-validates objective measures |
| B1_3–B1_5 | Sleep quality and life quality | Subjective wellbeing proxy; no alternative quantitative measure available |
| B2 | Sleep duration (hours) | Continuous variable; preferred over B2R (categorized version) for model input |
| B3, B4 | Peak activity time, outing time | Relevant for downstream insights; Seoul city analyzed telecom data for similar detection |
| B5_1–B5_3 | Primary activities | Behavioral pattern indicator |
| B6_1–B6_6 | PC/mobile activities | Behavioral pattern indicator |
| B7, B8 | Eating habits | Behavioral indicator; relevant for enterprise service recommendations |
| B9_1–B9_4 | Personal hygiene frequency | Objective behavioral indicator; relevant for service design |
| B10 | Subjective physical health | No quantitative alternative; retained alongside B11 |
| B11 | Subjective mental health | Higher priority than B10; retained |
| B11_1 | Duration of medication | Quantitative; relevant for chronicity analysis |
| B12_1–B12_9 | PHQ-9 depression scale items | Quantitative; established clinical instrument |

### Excluded Variables with Rationale

| Variable | Reason |
|----------|--------|
| A15_1 | Onset age of isolation — model-dependent, unclear if needed at this stage |
| B2R | Categorized version of B2; continuous variable preferred |

## Missing Value Treatment Strategy

### General Principles (Team-wide)
- Prefer continuous variables over categorized equivalents
- When both subjective and objective measures exist, include both for cross-validation
- For conditional questions (e.g., "answer only if A7 = 5–8"), missing values for non-applicable respondents are coded as **"not applicable"**, distinct from true missing values
- Missing value imputation: cosine similarity-based imputation using related feature vectors (not mean/median replacement)

### Unit 2 Specific Decisions
- **B2 (sleep duration)**: Used raw continuous values; B2R (categorized) excluded
- **B3 (activity time)**: 159 missing rows — imputed via cosine similarity with related behavioral features rather than deletion (both options were "critical" — deletion loses too many rows, column deletion loses valuable feature)
- **B4 (outing time)**: Respondents with A7 >= 7 (never goes out) assigned value 6 ("does not go out"); remaining missing values imputed via cosine similarity
- **A16 (isolation causes)**: Respondents who answered A16 despite not meeting eligibility criteria (A15 != 1) — responses deleted per survey design logic
