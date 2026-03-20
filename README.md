# Predicting Isolated & Reclusive Youth in Seoul

**NIA Data Analyst Training Program (Dataitgirls, 2023) — Team Project**  
Award: NIA Director's Award (과학기술정보통신부 산하 한국지능정보사회진흥원 원장상)

## Project Summary

South Korea faces a growing crisis of isolated and reclusive youth (고립은둔청년) — an estimated 540,000 young adults (age 19–39) who withdraw from social participation for 6+ months. Depression among youth has doubled in 5 years; youth suicide averages 4.3 per day. Despite increasing government attention, the characteristics that define and predict this population remain vaguely defined, and most support systems rely on self-reporting — meaning those who need help the most are the least likely to ask for it.

This project analyzed the **Seoul Metropolitan Government's Isolated/Reclusive Youth Survey** to build a predictive model and propose a shift from reactive to proactive intervention.

## What We Did

1. **Built classification models** on survey data (5,513 respondents × 250 features) to identify isolated youth
2. **Reduced 382 dummy features → 216 (top 90% importance) → 20 key predictive characteristics** across 6 categories (demographic, emotional, economic, behavioral, health, relational)
3. **Analyzed 5,830 YouTube comments** on isolation-related videos using text classification to find real isolated youth in the wild
4. **Text mining pipeline** (KoBERT sentiment analysis, TF-IDF, KoNLPy/Okt preprocessing)

## Key Findings

**Survey model (objective responses):**
- Naive Bayes: Accuracy 0.94, F1 0.70, ROC-AUC 0.96 (after correlation-based feature selection)
- XGBoost with HyperOpt: Accuracy 0.97, F1 0.98, ROC-AUC 0.99
- Top predictive feature by far: **isolation duration** (고립기간). Followed by: no social interaction, relationship difficulties, mental illness, job loss

**YouTube comment analysis (text responses):**
- From 5,830 comments, the model identified **25 individuals** showing strong indicators of isolation
- Isolated youth write in **formal/polite language** (88% used 존댓말 vs 31% in general comments)
- Isolated youth write **fact-based, neutral sentences** (75% vs 36%)
- Isolated youth and those close to them express **anxiety** — every comment classified as isolated-youth had the "anxiety" feature set to True

**Multi-dimensional nature confirmed:** No single category dominates — effective intervention requires addressing social, economic, psychological, and health dimensions simultaneously.

## Models Used

| Task | Models | Best Result |
|------|--------|-------------|
| Survey classification (objective) | Naive Bayes, XGBoost | XGBoost: F1 0.98, ROC-AUC 0.99 |
| Text classification (subjective) | Logistic Regression, Random Forest | Accuracy ~0.90, but F1 low (0.04–0.06) due to natural class imbalance |
| Sentiment analysis | KoBERT | Emotion labeling on YouTube comments |

Note on text model performance: High accuracy but low F1 was expected — the real-world distribution of isolated youth in YouTube comments is naturally imbalanced. The team intentionally did not apply resampling, as the goal was to find actual isolated individuals in real data, not to optimize metrics.

## My Contributions

This was a **7-person team project** (Team BlueYouth). This repository contains only my direct contributions.

| Area | Contribution |
|------|-------------|
| **Project Direction** | Proposed initial project ideas; structured the topic selection decision-making process |
| **Team Infrastructure** | Set up Notion workspace, established ground rules, check-in systems, and GTD-based task management |
| **Data Collection** | Sourced and gathered public datasets: Seoul survey data, demographic statistics (KOSIS), spatial data |
| **Variable Selection & Analysis** | Designed selection criteria for survey variables (Unit 2: sections A13–B12); documented rationale for each inclusion/exclusion decision → see [`preprocessing/variable_selection.md`](preprocessing/variable_selection.md) |
| **Preprocessing Specifications** | Wrote preprocessing specifications for assigned variables (B2–B12): defined missing value treatment strategy, conditional logic for survey skip patterns, eligibility validation rules. Implementation was done by the development lead. |
| **Presentation Structure** | Created the initial PPT draft, defined the presentation outline (accepted by the team), and led final editing |
| **Presentation Script** | Wrote the presentation script; co-led rehearsal with team leader |
| **Team Branding** | Designed team name "BlueYouth" (블루유스) logo concept |

### What I Did NOT Do (Other Team Members' Work)

- ML model implementation (Naive Bayes, XGBoost, Random Forest, HyperOpt tuning)
- YouTube comment crawling and text preprocessing code
- Hyperparameter tuning and model evaluation

## Repository Structure

```
├── README.md
├── data/
│   ├── raw/                          # Public datasets I collected
│   │   ├── 201812_202212_연령별인구현황_연간.csv
│   │   ├── 202310_202310_연령별인구현황_월간.csv
│   │   ├── 연령_및_성별_인구_....csv
│   │   └── 서울시_생활권계획_시설_공간정보.csv
│   └── processed/                    # Final preprocessed outputs (team output)
│       ├── preprocessing_final.xlsx
│       └── preprocessing_final_dummy.xlsx
├── preprocessing/
│   ├── variable_selection.md         # Variable selection criteria & decisions
│   └── preprocessing_guide_unit2.xlsx # Preprocessing specification sheet
└── presentation/
    └── final_presentation_KA_editing.pptx
```

## Data Sources

| Dataset | Source | License |
|---------|--------|---------|
| Seoul Isolated/Reclusive Youth Survey | Seoul Metropolitan Government | Not redistributed — available via [Seoul Open Data](https://data.seoul.go.kr/) |
| Youth Socioeconomic Survey (2021) | Korea Institute for Health and Social Affairs | Not redistributed — via [KIHASA](https://www.kihasa.re.kr/) |
| Population by Age (Annual/Monthly) | KOSIS (Statistics Korea) | Public domain (CC BY) |
| Spatial Facility Data (Parks) | Seoul Open Data | Public domain |
| YouTube comments (isolation-related videos, top 6 by comment count) | YouTube | Crawled for research purposes |

> Note: The core Seoul survey dataset is not included due to redistribution restrictions.

## Tech Stack

- Python (pandas), Google Colab
- Notion (project management)
- PowerPoint (presentation — my editing)

> Full team stack also included: scikit-learn, XGBoost, HyperOpt, KoNLPy (Okt), KoBERT, matplotlib, wordcloud. These were used by other team members for model implementation.

## Context

- **Program**: SW Women Data Analyst Training, Dataitgirls 7th (과학기술정보통신부 / Ministry of Science and ICT → NIA 한국지능정보사회진흥원), 2023
- **Team**: BlueYouth (블루유스) — 7 members
- **Duration**: Oct–Nov 2023 (~4 weeks)
- **Result**: NIA Director's Award (한국지능정보사회진흥원 원장상)
