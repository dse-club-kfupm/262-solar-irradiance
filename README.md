# SunCast: How Much Solar Energy Will Reach the Farm?

Data Engineering & Science Club, Applied Projects Committee, KFUPM. Term 262. Prediction case study.

Solar power is a growing part of Saudi Arabia's energy plans, and the Eastern Province receives strong sunshine for most of the year. Dust, humidity and cloud still change how much energy reaches a panel from one day to the next, and a solar farm must tell the grid in advance how much it will deliver.

In this project you build a model that predicts the solar energy reaching the ground at KFUPM on a given day, from that day's weather.

## Scope

This is a case study. You will not process satellite images or run a real plant. You work with one daily table that NASA's POWER project derived from satellite and weather-model data for KFUPM's location, and you show how the data science workflow (understanding, cleaning, exploring, modelling and communicating) answers the question on that table. The weather columns stand in for a forecast: in practice an operator would use the forecast for the next day.

## The client and the question

| Item | Description |
| --- | --- |
| Client | The operations team of a solar farm in the Eastern Province, which tells the grid each day how much energy to expect |
| Question | Given the weather expected for a day, how much solar energy will reach each square metre of ground? |
| Prediction | `solar_kwh_m2`: daily solar energy on a horizontal surface, in kWh/m² per day |
| Score | Average error in kWh/m² per day on the hidden test set (see Grading); lower is better |

## The data

`data/train.csv` holds one row per day from 1 January 2001 to 31 December 2023 for KFUPM, Dhahran (26.31° N, 50.14° E). The committee holds 2024 and 2025 as the hidden test set.

Because the test years come after the training years, split your own validation set by time as well. Missing values in the source are marked with negative fill values such as -999; part of your work is to find and handle them.

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
| `solar_kwh_m2` | Target: all-sky solar energy on a horizontal surface (ALLSKY_SFC_SW_DWN) | kWh/m²/day |

Test rows contain every column except `solar_kwh_m2`.

Source: NASA Prediction Of Worldwide Energy Resources (POWER) project, NASA Langley Research Center, funded by NASA's Earth Science Division. Daily point data, Renewable Energy community. When citing, give the service name, version and date accessed, as NASA requests. <https://power.larc.nasa.gov>

## Prerequisites

Learn these before you start. Each row gives one source to start with and one to go further.

| Skill | Start with | Go further |
| --- | --- | --- |
| Git and GitHub | [Introduction to GitHub](https://github.com/skills/introduction-to-github), GitHub Skills | [Pro Git](https://git-scm.com/book/en/v2), chapters 1 to 3 |
| Python | [Python](https://www.kaggle.com/learn/python), Kaggle Learn | [The Python Tutorial](https://docs.python.org/3/tutorial/) |
| pandas | [Pandas](https://www.kaggle.com/learn/pandas), Kaggle Learn | [10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html) |
| Data cleaning | [Data Cleaning](https://www.kaggle.com/learn/data-cleaning), Kaggle Learn | [Working with missing data](https://pandas.pydata.org/docs/user_guide/missing_data.html) |
| Charts | [Data Visualization](https://www.kaggle.com/learn/data-visualization), Kaggle Learn | [Ten Simple Rules for Better Figures](https://doi.org/10.1371/journal.pcbi.1003833) |
| Prediction | [Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning), Kaggle Learn | [scikit-learn: Getting started](https://scikit-learn.org/stable/getting_started.html) |
| Animation | [Manim Community quickstart](https://docs.manim.community/en/stable/tutorials/quickstart.html) | [Animations using Matplotlib](https://matplotlib.org/stable/users/explain/animations/animations.html) |
| Slides | [Ten Simple Rules for Making Good Oral Presentations](https://doi.org/10.1371/journal.pcbi.0030077) |  |

Background on the data: the [POWER documentation](https://power.larc.nasa.gov/docs/), which describes each parameter.

## Getting started

```bash
git clone https://github.com/dse-club-kfupm/262-suncast.git
cd 262-suncast
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Open `notebooks/01_data_understanding.ipynb`. Google Colab is also acceptable; upload `data/train.csv` and keep your notebooks in the repository.

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

## What you submit

Part A, prediction (50 points)

- `src/predict.py` containing a function `predict(df)`. It receives a table with the test columns described above and returns a number (kWh/m² per day) per row, in the same order.
- The three notebooks showing how you reached the model.
- Run `python tests/check_submission.py` before your final pull request. It confirms that `predict.py` loads and returns predictions in the right format.

Part B, visual communication (50 points)

- Clear, labelled charts in the notebooks.
- Slides: 5 to 7 slides that take the client from the question to the answer, saved as PDF in `visuals/slides/`.
- An animation or a short video of up to 2 minutes made with Python (Manim or matplotlib), with its source code and the rendered file in `visuals/animation/`. A good subject: a year of sunshine over Dhahran day by day, with your model's prediction drawn against the measured value.

## Grading

Your `predict.py` is run once on the hidden test set after the submission deadline.

| Level | Requirement | Points |
| --- | --- | --- |
| Runs | Returns a valid prediction for every test row | 10 |
| Bronze | Scores below the baseline, which predicts the training average for that calendar month: 0.52 kWh/m² per day | 30 |
| Silver | Average error of at most 0.41 kWh/m² per day | 40 |
| Gold | Average error of at most 0.29 kWh/m² per day, the committee's reference model | 50 |

Average error is the mean absolute difference between your prediction and the true value, in kWh/m² per day. Lower is better.

| Part B | Missing | Basic | Good | Excellent |
| --- | --- | --- | --- | --- |
| Charts in the notebooks | 0 | 5 | 10 | 15 |
| Slides, 5 to 7, as PDF | 0 | 5 | 10 | 15 |
| Animation or video, made with Python, up to 2 minutes | 0 | 7 | 14 | 20 |

Any team member may be asked to explain any part of the work. If no one can, the Part A points do not count.

## Milestones

Progress is tracked with four milestones, each with its own issues.

| Milestone | Complete when |
| --- | --- |
| 1. Data understanding | Notebook 01 and the cleaning functions are merged |
| 2. Exploration | Notebook 02, with charts and written findings, is merged |
| 3. Prediction submission | Notebook 03 and `src/predict.py` are merged and pass the format check |
| 4. Visual communication | The slides and the animation or video are merged |

## Rules

- Train only on `data/train.csv`.
- Do not download POWER data yourselves; use only `data/train.csv`.
- Every change reaches `main` through a pull request reviewed by a teammate.
- AI tools are allowed if declared in the Credits section, and every member must be able to explain the work.
- `predict.py` may use only pandas, NumPy and scikit-learn, so that it runs in the grading environment. Notebooks and visuals may also use matplotlib, seaborn, plotly and Manim.

## Credits

- Project Leads: Name (@github), Name (@github)
- Contributors: Name (@github)
- Organized by: Applied Projects Committee, Data Engineering & Science Club, KFUPM
- Data: NASA Prediction Of Worldwide Energy Resources (POWER) project, NASA Langley Research Center, funded by NASA's Earth Science Division. Daily point data, Renewable Energy community. When citing, give the service name, version and date accessed, as NASA requests.
- AI tools used: none, or name the tool and what it was used for
