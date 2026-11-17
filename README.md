<p align="center">
  <a href="https://github.com/dse-club-kfupm"><img src=".github/assets/logo.svg" alt="Data Engineering &amp; Science Club, KFUPM" width="150"></a>
</p>

<h1 align="center">SunCast</h1>

<p align="center">
  <b>How much solar energy will reach the farm?</b><br>
  A term 262 learning project of the <a href="https://github.com/dse-club-kfupm">Data Engineering &amp; Science Club, KFUPM</a>
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
  <a href="#grading"><img alt="Score" src="https://img.shields.io/badge/score-mean%20absolute%20error-3DD2A8"></a>
  <a href="data/README.md"><img alt="Data licence" src="https://img.shields.io/badge/data-NASA%20POWER-16A34A"></a>
  <a href="https://github.com/dse-club-kfupm/262-suncast/actions/workflows/format-check.yml"><img alt="Format check" src="https://github.com/dse-club-kfupm/262-suncast/actions/workflows/format-check.yml/badge.svg"></a>
</p>

<p align="center">
  <a href="#at-a-glance"><b>At a glance</b></a> ·
  <a href="#the-data"><b>Data</b></a> ·
  <a href="#learn-first"><b>Learn first</b></a> ·
  <a href="#get-started"><b>Get started</b></a> ·
  <a href="#how-the-work-flows"><b>Workflow</b></a> ·
  <a href="#what-you-submit"><b>Submit</b></a> ·
  <a href="#grading"><b>Grading</b></a> ·
  <a href="#rules"><b>Rules</b></a>
</p>

Solar power is a growing part of Saudi Arabia's energy plans, and the Eastern Province receives strong sunshine for most of the year. Dust, humidity and cloud still change how much energy reaches a panel from one day to the next, and a solar farm must tell the grid in advance how much it will deliver.

In this project you build a model that predicts the solar energy reaching the ground at KFUPM on a given day, from that day's weather.

> [!NOTE]
> This is a case study. You will not process satellite images or run a real plant. You work with one daily table that NASA's POWER project derived from satellite and weather-model data for KFUPM's location, and show how the data science workflow (understanding, cleaning, exploring, modelling and communicating) answers the question on that table. The weather columns stand in for a forecast: in practice an operator would use the forecast for the next day.

## At a glance

|  |  |
| --- | --- |
| **Client** | The operations team of a solar farm in the Eastern Province, which tells the grid each day how much energy to expect |
| **Question** | Given the weather expected for a day, how much solar energy will reach each square metre of ground? |
| **You predict** | `solar_kwh_m2`: daily solar energy on a horizontal surface, in kWh/m² per day |
| **Scored by** | Average error in kWh/m² per day on 2024 and 2025 (lower is better) ([details](#grading)) |
| **Training data** | 8,400 days × 10 columns in `data/train.csv` |
| **Level** | DATA 211, Introduction to Data Science |
| **Graded out of** | 100: 50 for the prediction, 50 for charts, slides and an animation |

## The data

`data/train.csv` holds one row per day from 1 January 2001 to 31 December 2023 for KFUPM, Dhahran (26.31° N, 50.14° E). The committee keeps 2024 and 2025 as the hidden test set.

| Column | Meaning | Unit |
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

### Watch out for

- **The test is in the future.** The test years come after the training years, so split your validation set by time as well: for example, train on 2001 to 2020 and validate on 2021 to 2023. A random split lets the model see the days on either side of each validation day, and the score looks better than it will be on the test.
- **Seasons matter.** Sunshine follows the calendar, so the month or the day of the year, taken from `date`, is likely to help.
- **Missing values.** NASA POWER marks missing values with fill values such as -999. This training file has none, but write your cleaning so that `predict.py` still works if a test day has one.

**Source.** NASA Prediction Of Worldwide Energy Resources (POWER) project, NASA Langley Research Center, funded by NASA's Earth Science Division. Daily point data, Renewable Energy community. When citing, give the service name, version and date accessed, as NASA requests. <https://power.larc.nasa.gov>

## Learn first

Everything here is at the level of DATA 211, Introduction to Data Science. If you have passed it, skim the list and open only what you are unsure about. You do not need everything on day one: learn each row before the milestone that needs it. Most Kaggle Learn courses take a few hours and run in the browser.

| When | Skill | Why you need it | Start with | Go further |
| --- | --- | --- | --- | --- |
| Before you start | Git and GitHub | Every change reaches `main` through a reviewed pull request | [Introduction to GitHub](https://github.com/skills/introduction-to-github) · GitHub Skills | [Pro Git](https://git-scm.com/book/en/v2), chapters 1 to 3 |
| Before you start | Python | The notebooks and `predict.py` are Python | [Python](https://www.kaggle.com/learn/python) · Kaggle Learn | [The Python Tutorial](https://docs.python.org/3/tutorial/) · python.org |
| Before you start | Environments | Install the project's packages without breaking anything else | [Virtual environments and packages](https://docs.python.org/3/tutorial/venv.html) · python.org |  |
| Milestone 1 | pandas | Load, inspect and reshape the table | [Pandas](https://www.kaggle.com/learn/pandas) · Kaggle Learn | [10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html) · pandas docs |
| Milestone 1 | Data cleaning | Decide what to do with missing and unusual values | [Data Cleaning](https://www.kaggle.com/learn/data-cleaning) · Kaggle Learn | [Working with missing data](https://pandas.pydata.org/docs/user_guide/missing_data.html) · pandas docs |
| Milestone 1 | The field | Know what each column means in the real world | [The POWER documentation](https://power.larc.nasa.gov/docs/) · NASA, describes each parameter |  |
| Milestone 2 | Charts | Show how the features relate to the target | [Data Visualization](https://www.kaggle.com/learn/data-visualization) · Kaggle Learn | [Ten Simple Rules for Better Figures](https://doi.org/10.1371/journal.pcbi.1003833) · PLOS |
| Milestone 3 | Prediction | Train a first model and compare it with the baseline | [Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning) · Kaggle Learn | [Getting started](https://scikit-learn.org/stable/getting_started.html) · scikit-learn |
| Milestone 3 | Better models | Pipelines, categorical columns, missing values and leakage | [Intermediate Machine Learning](https://www.kaggle.com/learn/intermediate-machine-learning) · Kaggle Learn | [Feature Engineering](https://www.kaggle.com/learn/feature-engineering) · Kaggle Learn |
| Milestone 3 | Your score | Average error, and a validation split by time | [Mean absolute error](https://scikit-learn.org/stable/modules/model_evaluation.html#mean-absolute-error) · scikit-learn | [Time series split](https://scikit-learn.org/stable/modules/cross_validation.html#time-series-split) · scikit-learn |
| Milestone 4 | Animation | Tell the story in motion, up to 2 minutes | [Quickstart](https://docs.manim.community/en/stable/tutorials/quickstart.html) · Manim | [Animations using Matplotlib](https://matplotlib.org/stable/users/explain/animations/animations.html) · Matplotlib |
| Milestone 4 | Slides | Take the client from the question to the answer in 5 to 7 slides | [Ten Simple Rules for Making Good Oral Presentations](https://doi.org/10.1371/journal.pcbi.0030077) · PLOS |  |

## Get started

You need Python 3.10 or newer and Git.

```bash
git clone https://github.com/dse-club-kfupm/262-suncast.git
cd 262-suncast
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Then open `notebooks/01_data_understanding.ipynb` and work through its questions.

- **No setup on your laptop?** Google Colab works: upload `data/train.csv`, and commit your notebooks back to this repository.
- **Making the animation?** Install Manim on its own with `pip install manim`, after the system packages listed in the [Manim installation guide](https://docs.manim.community/en/stable/installation.html).

## How the work flows

The work is split into four milestones. Each one has its own issues in the [Issues tab](https://github.com/dse-club-kfupm/262-suncast/issues), and it is done when all of them are closed.

| Milestone | Done when |
| --- | --- |
| 1. Data understanding | Notebook 01 and the cleaning functions in `src/features.py` are merged |
| 2. Exploration | Notebook 02, with charts and written findings, is merged |
| 3. Prediction submission | Notebook 03 and `src/predict.py` are merged and pass the format check |
| 4. Visual communication | The slides and the animation or video are merged |

For every change:

1. Pick an open issue and assign yourself to it.
2. Create a branch for it: `git switch -c short-description`.
3. Commit in small steps with clear messages, then push the branch.
4. Open a pull request into `main` and write `Closes #<issue number>` in its description.
5. A teammate reviews it. When it is approved and the format check has passed, merge it.

**Validation tip for milestone 3.** Split by time, as the hidden test does: for example, train on 2001 to 2020 and validate on 2021 to 2023.

## What you submit

**Part A · Prediction (50 points)**

- [ ] `src/predict.py` with a function `predict(df)`. It receives a table with the test columns and returns a number (kWh/m² per day) per row, in the same order as the rows of `df`.
- [ ] Notebooks 01 to 03, run from top to bottom, showing how you reached the model.
- [ ] A passing format check. Run `python tests/check_submission.py` from the repository root before your final pull request; the same check also runs automatically on every pull request.

**Part B · Visual communication (50 points)**

- [ ] Clear, labelled charts in the notebooks.
- [ ] 5 to 7 slides that take the client from the question to the answer, saved as PDF in `visuals/slides/`.
- [ ] An animation or a short video of up to 2 minutes made with Python (Manim or matplotlib), with its source code and the rendered file in `visuals/animation/`. A good subject: a year of sunshine over Dhahran day by day, with your model's prediction drawn against the measured value.

## Grading

Your `predict.py` is run once on the hidden test set after the submission deadline. You get the points of the highest level you reach; they are not added up.

| Level | You need | Points |
| --- | --- | --- |
| Runs | Returns a valid prediction for every test row | 10 |
| Bronze | Average error below the baseline's 0.52 kWh/m² per day (the baseline predicts the training average for that calendar month) | 30 |
| Silver | Average error of at most 0.41 kWh/m² per day | 40 |
| Gold | Average error of at most 0.29 kWh/m² per day, the committee's reference model | 50 |

Average error is the mean absolute difference between your prediction and the true value, in kWh/m² per day. Lower is better.

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
- AI tools are allowed if you declare them in the Credits section, and every member must be able to explain the work.

## Repository structure

```text
262-suncast/
├── README.md
├── requirements.txt
├── data/
│   ├── train.csv
│   └── README.md               source and licence
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_cleaning_and_exploration.ipynb
│   └── 03_modelling.ipynb
├── src/
│   ├── features.py             cleaning and feature functions
│   └── predict.py              submission entry point
├── models/                     saved model, if predict.py loads one
├── tests/
│   └── check_submission.py     run before submitting
└── visuals/
    ├── slides/                 presentation as PDF
    └── animation/              animation source code and rendered video
```

## Credits

- **Project leads:** Name (@github), Name (@github)
- **Contributors:** Name (@github)
- **Organized by:** Applied Projects Committee, Data Engineering & Science Club, KFUPM
- **Data:** NASA Prediction Of Worldwide Energy Resources (POWER) project, NASA Langley Research Center, funded by NASA's Earth Science Division. Daily point data, Renewable Energy community. When citing, give the service name, version and date accessed, as NASA requests.
- **AI tools used:** none, or name the tool and what it was used for

---

<p align="center">
  <sub>Applied Projects Committee · <a href="https://github.com/dse-club-kfupm">Data Engineering &amp; Science Club, KFUPM</a> · نادي هندسة وعلوم البيانات</sub>
</p>
