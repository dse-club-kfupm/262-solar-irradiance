<p align="center">
  <a href="https://github.com/dse-club-kfupm"><img src=".github/assets/logo.svg" alt="Data Engineering &amp; Science Club, KFUPM" width="150"></a>
</p>

<h1 align="center">Solar Irradiance Prediction</h1>

<p align="center">
  <b>Predicting daily solar energy at KFUPM from weather data</b><br>
  A term 262 applied project of the <a href="https://github.com/dse-club-kfupm">Data Engineering &amp; Science Club, KFUPM</a>
</p>

<p align="center">
  <a href="https://docs.python.org/3/tutorial/"><img alt="Python 3.10+" src="https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white"></a>
  <a href="https://jupyter.org"><img alt="Jupyter" src="https://img.shields.io/badge/Jupyter-notebooks-F37626?logo=jupyter&logoColor=white"></a>
  <a href="https://pandas.pydata.org"><img alt="pandas" src="https://img.shields.io/badge/pandas-2.x-150458?logo=pandas&logoColor=white"></a>
  <a href="https://scikit-learn.org"><img alt="scikit-learn" src="https://img.shields.io/badge/scikit--learn-1.3%2B-F7931E?logo=scikitlearn&logoColor=white"></a>
  <a href="https://matplotlib.org"><img alt="Matplotlib and seaborn" src="https://img.shields.io/badge/charts-matplotlib%20%C2%B7%20seaborn-11557C"></a>
  <a href="https://www.manim.community"><img alt="Manim" src="https://img.shields.io/badge/animation-Manim-0E1E3A"></a>
</p>

<p align="center">
  <a href="https://github.com/dse-club-kfupm"><img alt="Term 262" src="https://img.shields.io/badge/term-262-1D3A6B"></a>
  <img alt="Renewable energy" src="https://img.shields.io/badge/field-Renewable%20energy-47ABD6">
  <a href="#evaluation"><img alt="Metric" src="https://img.shields.io/badge/metric-mean%20absolute%20error-3DD2A8"></a>
  <a href="data/README.md"><img alt="Data licence" src="https://img.shields.io/badge/data-NASA%20POWER-16A34A"></a>
  <a href="LICENSE"><img alt="Code licence" src="https://img.shields.io/badge/code-MIT-6B7280"></a>
  <a href="https://github.com/dse-club-kfupm/262-solar-irradiance/actions/workflows/format-check.yml"><img alt="Format check" src="https://github.com/dse-club-kfupm/262-solar-irradiance/actions/workflows/format-check.yml/badge.svg"></a>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#dataset"><b>Dataset</b></a> ·
  <a href="#learning-path"><b>Learning path</b></a> ·
  <a href="#getting-started"><b>Getting started</b></a> ·
  <a href="#workflow"><b>Workflow</b></a> ·
  <a href="#deliverables"><b>Deliverables</b></a> ·
  <a href="#evaluation"><b>Evaluation</b></a> ·
  <a href="#rules"><b>Rules</b></a>
</p>

## Overview

Solar power is a growing part of Saudi Arabia's energy plans, and the Eastern Province receives strong sunshine for most of the year. Dust, humidity and cloud cover still change how much energy reaches a panel from one day to the next, and a solar plant must tell the grid in advance how much power to expect.

In this project you build a regression model that estimates the daily solar energy reaching the ground at KFUPM from that day's weather measurements.

> [!NOTE]
> This is a case study. You will not process satellite images or operate a plant. You work with one daily table that NASA's POWER project derived from satellite and weather-model data for KFUPM's location, and show how the data science workflow (understanding, cleaning, exploring, modelling and communicating) answers the question on that table. The weather columns stand in for a forecast: in practice an operator would use the forecast for the next day.

### Project summary

|  |  |
| --- | --- |
| **Stakeholder** | The operations team of a solar plant in the Eastern Province that submits a daily energy forecast to the grid operator |
| **Question** | Given the expected weather for a day, how much solar energy will reach each square metre of ground? |
| **Target** | `solar_kwh_m2`: daily solar energy on a horizontal surface, in kWh/m² per day |
| **Evaluation** | Average error in kWh/m² per day on 2024 and 2025; lower is better ([details](#evaluation)) |
| **Training data** | 8,400 days × 10 columns in `data/train.csv` |
| **Level** | Introductory: DATA 211, Introduction to Data Science |
| **Assessment** | 100 points: 50 for the prediction, 50 for charts, slides and an animation |

## Dataset

`data/train.csv` holds one row per day from 1 January 2001 to 31 December 2023 for KFUPM, Dhahran (26.31° N, 50.14° E). The committee keeps 2024 and 2025 as the hidden test set.

| Column | Description | Unit |
| --- | --- | --- |
| `date` | Day of the measurement | YYYY-MM-DD |
| `temp_mean_c` | Mean air temperature at 2 m (POWER: T2M) | °C |
| `temp_max_c` | Maximum air temperature at 2 m (T2M_MAX) | °C |
| `temp_min_c` | Minimum air temperature at 2 m (T2M_MIN) | °C |
| `humidity_pct` | Relative humidity at 2 m (RH2M) | % |
| `wind_ms` | Wind speed at 2 m (WS2M) | m/s |
| `pressure_kpa` | Surface pressure (PS) | kPa |
| `precip_mm` | Precipitation (PRECTOTCORR) | mm/day |
| `cloud_pct` | Cloud amount (CLOUD_AMT) | % |
| `solar_kwh_m2` | **Target:** all-sky solar energy on a horizontal surface (ALLSKY_SFC_SW_DWN) | kWh/m²/day |

Test rows contain every column except `solar_kwh_m2`.

### Data considerations

- **The test period is in the future.** The test years come after the training years, so validate on later years as well: for example, train on 2001 to 2020 and validate on 2021 to 2023. A random split lets the model see the days on either side of each validation day, and the score looks better than it will be on the test.
- **Seasonality.** Solar energy follows the calendar, so the month or the day of the year, derived from `date`, is likely to be informative.
- **Missing values.** NASA POWER marks missing values with fill values such as -999. This training file has none, but write your cleaning so that `predict.py` still works if a test day contains one.

**Source.** NASA Prediction Of Worldwide Energy Resources (POWER) project, NASA Langley Research Center, funded by NASA's Earth Science Division. Daily point data, Renewable Energy community. When citing, give the service name, version and date accessed, as NASA requests. <https://power.larc.nasa.gov>

## Learning path

The project assumes the level of DATA 211, Introduction to Data Science. If you have completed it, review the list and open only the topics you are unsure about. Each topic is needed by a specific milestone, so you can learn it just before that milestone. Most Kaggle Learn courses take a few hours and run in the browser.

| When | Topic | Why it is needed | Start with | Go further |
| --- | --- | --- | --- | --- |
| Before you start | Git and GitHub | Every change reaches `main` through a reviewed pull request | [Introduction to GitHub](https://github.com/skills/introduction-to-github) · GitHub Skills | [Pro Git](https://git-scm.com/book/en/v2), chapters 1 to 3 |
| Before you start | Python | The notebooks and `predict.py` are written in Python | [Python](https://www.kaggle.com/learn/python) · Kaggle Learn | [The Python Tutorial](https://docs.python.org/3/tutorial/) · python.org |
| Before you start | Environments | Install the project's packages in isolation | [Virtual environments and packages](https://docs.python.org/3/tutorial/venv.html) · python.org |  |
| Milestone 1 | pandas | Load, inspect and reshape the table | [Pandas](https://www.kaggle.com/learn/pandas) · Kaggle Learn | [10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html) · pandas docs |
| Milestone 1 | Data cleaning | Handle missing and unusual values | [Data Cleaning](https://www.kaggle.com/learn/data-cleaning) · Kaggle Learn | [Working with missing data](https://pandas.pydata.org/docs/user_guide/missing_data.html) · pandas docs |
| Milestone 1 | Domain background | Understand what each column measures | [POWER documentation](https://power.larc.nasa.gov/docs/) · NASA |  |
| Milestone 2 | Visualisation | Show how the features relate to the target | [Data Visualization](https://www.kaggle.com/learn/data-visualization) · Kaggle Learn | [Ten Simple Rules for Better Figures](https://doi.org/10.1371/journal.pcbi.1003833) · PLOS |
| Milestone 3 | Modelling | Train a first model and compare it with the baseline | [Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning) · Kaggle Learn | [Getting started](https://scikit-learn.org/stable/getting_started.html) · scikit-learn |
| Milestone 3 | Better models | Pipelines, categorical features, missing values and leakage | [Intermediate Machine Learning](https://www.kaggle.com/learn/intermediate-machine-learning) · Kaggle Learn | [Feature Engineering](https://www.kaggle.com/learn/feature-engineering) · Kaggle Learn |
| Milestone 3 | Evaluation | Mean absolute error and validation by time | [Mean absolute error](https://scikit-learn.org/stable/modules/model_evaluation.html#mean-absolute-error) · scikit-learn | [Time series split](https://scikit-learn.org/stable/modules/cross_validation.html#time-series-split) · scikit-learn |
| Milestone 4 | Animation | Explain the result in motion, in up to 2 minutes | [Quickstart](https://docs.manim.community/en/stable/tutorials/quickstart.html) · Manim | [Animations using Matplotlib](https://matplotlib.org/stable/users/explain/animations/animations.html) · Matplotlib |
| Milestone 4 | Presentation | Take the stakeholder from the question to the answer in 5 to 7 slides | [Ten Simple Rules for Making Good Oral Presentations](https://doi.org/10.1371/journal.pcbi.0030077) · PLOS |  |

## Getting started

You need Python 3.10 or newer and Git.

```bash
git clone https://github.com/dse-club-kfupm/262-solar-irradiance.git
cd 262-solar-irradiance
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Then open `notebooks/01_data_understanding.ipynb` and work through its questions.

- **No local setup?** Google Colab also works: upload `data/train.csv`, and commit your notebooks back to this repository.
- **Preparing the animation?** Install Manim separately with `pip install manim`, after the system packages listed in the [Manim installation guide](https://docs.manim.community/en/stable/installation.html).
- **Questions?** Ask in the organization's [Discussions](https://github.com/orgs/dse-club-kfupm/discussions) or open an issue with the Question template.

## Workflow

The work is divided into four milestones. Each milestone has its own issues in the [Issues tab](https://github.com/dse-club-kfupm/262-solar-irradiance/issues) and is complete when all of them are closed.

| Milestone | Complete when |
| --- | --- |
| 1. Data understanding | Notebook 01 and the cleaning functions in `src/features.py` are merged |
| 2. Exploration | Notebook 02, with charts and written findings, is merged |
| 3. Prediction submission | Notebook 03 and `src/predict.py` are merged and pass the format check |
| 4. Visual communication | The slides and the animation or video are merged |

For every change:

1. Choose an open issue and assign yourself to it.
2. Create a branch for it: `git switch -c short-description`.
3. Commit in small steps with clear messages, then push the branch.
4. Open a pull request into `main` and write `Closes #<issue number>` in its description.
5. A teammate reviews it. Once it is approved and the format check has passed, merge it.

Files in `data/`, `tests/` and `.github/` are owned by the committee: a pull request that changes them also needs a committee review.

**Validation guidance for milestone 3.** Split by time, as the hidden test does: for example, train on 2001 to 2020 and validate on 2021 to 2023.

## Deliverables

**Part A · Prediction (50 points)**

- [ ] `src/predict.py` with a function `predict(df)`. It receives a table with the test columns and returns a number (kWh/m² per day) for each row, in the same order as the rows of `df`.
- [ ] Notebooks 01 to 03, run from top to bottom, documenting how the model was developed.
- [ ] A passing format check. Run `python tests/check_submission.py` from the repository root before the final pull request; the same check runs automatically on every pull request.

**Part B · Visual communication (50 points)**

- [ ] Clear, labelled charts in the notebooks.
- [ ] 5 to 7 slides that take the stakeholder from the question to the answer, saved as PDF in `visuals/slides/`.
- [ ] An animation or a short video of up to 2 minutes made with Python (Manim or matplotlib), with its source code and the rendered file in `visuals/animation/`. A suggested subject: a year of sunshine over Dhahran, day by day, with the model's prediction drawn against the measured value.

## Evaluation

`predict.py` is run once on the hidden test set after the submission deadline. A team receives the points of the highest level it reaches; the levels are not added together.

| Level | Requirement | Points |
| --- | --- | --- |
| Runs | Returns a valid prediction for every test row | 10 |
| Bronze | Average error below the baseline's 0.52 kWh/m² per day (the baseline predicts the training average for the same calendar month) | 30 |
| Silver | Average error of at most 0.41 kWh/m² per day | 40 |
| Gold | Average error of at most 0.29 kWh/m² per day, the committee's reference model | 50 |

Average error is the mean absolute difference between the prediction and the true value, in kWh/m² per day. Lower is better.

| Part B | Missing | Basic | Good | Excellent |
| --- | --- | --- | --- | --- |
| Charts in the notebooks | 0 | 5 | 10 | 15 |
| Slides, 5 to 7, as PDF | 0 | 5 | 10 | 15 |
| Animation or video, made with Python, up to 2 minutes | 0 | 7 | 14 | 20 |

> [!IMPORTANT]
> Any team member may be asked to explain any part of the work. If no one can, the Part A points do not count.

## Rules

- Train only on `data/train.csv`.
- Do not download POWER data yourselves; use only `data/train.csv`.
- Every change reaches `main` through a pull request reviewed by a teammate.
- `predict.py` may use only pandas, NumPy and scikit-learn, so that it runs in the grading environment. Notebooks and visuals may also use matplotlib, seaborn, plotly and Manim.
- AI tools are allowed if they are declared in the Credits section, and every member must be able to explain the work.
- Follow the organization's [code of conduct](https://github.com/dse-club-kfupm/.github/blob/main/CODE_OF_CONDUCT.md).

## Repository structure

```text
262-solar-irradiance/
├── README.md
├── LICENSE                     MIT licence for the code
├── requirements.txt
├── data/
│   ├── train.csv
│   └── README.md               source and licence of the data
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_cleaning_and_exploration.ipynb
│   └── 03_modelling.ipynb
├── src/
│   ├── features.py             cleaning and feature functions
│   └── predict.py              submission entry point
├── models/                     saved model, if predict.py loads one
├── tests/
│   └── check_submission.py     format check, run before submitting
└── visuals/
    ├── slides/                 presentation as PDF
    └── animation/              animation source code and rendered video
```

## Credits

- **Project leads:** Name (@github), Name (@github)
- **Contributors:** Name (@github)
- **Organized by:** Applied Projects Committee, Data Engineering & Science Club, KFUPM
- **Data:** NASA Prediction Of Worldwide Energy Resources (POWER) project, NASA Langley Research Center, funded by NASA's Earth Science Division. When citing, give the service name, version and date accessed, as NASA requests.
- **AI tools used:** none, or name the tool and what it was used for
- **Licence:** the code is released under the [MIT licence](LICENSE); the data keeps its own licence, described in [data/README.md](data/README.md).

---

<p align="center">
  <sub>Applied Projects Committee · <a href="https://github.com/dse-club-kfupm">Data Engineering &amp; Science Club, KFUPM</a> · نادي هندسة وعلوم البيانات</sub>
</p>
