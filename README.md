# Who Gets Diagnosed? Autism Patterns in U.S. Children (NSCH 2024)

A beginner-friendly data project in Python. It uses a large U.S. government survey to look at how many children are diagnosed with autism, and whether that changes by age, sex, family income, insurance, or state.

> **Note:** This project only describes patterns in survey answers. It is not medical advice, and a difference between groups does not prove what caused it.

---

## Why I Built This

This project has two goals: **learn real data skills** and **use those skills on a problem that matters.**

### 1. To learn by doing

I wanted to learn Python's main data tools on real, messy data instead of clean practice examples. This project let me practice:

- **Collecting data:** finding a trusted public source, downloading it, and reading a file format I had never used before
- **pandas:** loading, cleaning, renaming, and grouping a large table
- **NumPy:** doing the math, like weighted averages, that turns raw answers into real estimates
- **Jupyter Notebook:** exploring the data one step at a time and writing notes next to the code
- **matplotlib:** turning numbers into charts that people can understand quickly

### 2. To help with a real-life problem

Getting an autism diagnosis early matters. Children who are diagnosed early can start therapy and support sooner, while their brains are still developing quickly. But not every child gets diagnosed at the same age. Some families wait years, depending on where they live, how much money they have, or what insurance covers.

This project uses real survey data to ask:

- **Which children are being diagnosed later than others?**
- **Do family income, insurance, or state make a difference?**

If the data shows that some groups are diagnosed later, that points to where more screening, more doctors, or better insurance coverage could help. Parents, schools, and health programs can use this kind of evidence to decide where to focus.

This project can't fix the problem alone, but it shows how a developer can take public data and turn it into a clear picture of who may be getting left behind.

## What This Project Is About

Every year, the U.S. Census Bureau runs the **National Survey of Children's Health (NSCH)**. Tens of thousands of parents answer questions about their child's health, school, and family life.

One question asks: *"Has a doctor or other health care provider ever told you that this child has autism?"*

This project uses those answers to explore four questions:

1. **How common is an autism diagnosis** among U.S. children?
2. **Are boys diagnosed more often than girls**, and by how much?
3. **At what age** do parents usually first hear the diagnosis?
4. **Do rates differ by family income, insurance, or state?** If they do, it may mean some families have an easier time getting their child seen by a doctor, not that autism itself is more common in those groups.

## Key Findings

<!-- Fill these in after you run the script. Replace each blank with your real numbers. -->

- About **___%** of U.S. children have ever been diagnosed with autism (weighted estimate).
- Boys are diagnosed about **___ times** as often as girls.
- The most common age for a first diagnosis is around **___ years old**.
- Income and insurance: _[what you noticed]_
- States: _[highest and lowest, and how big the gap is]_

![Diagnosis rate by age group](images/rate_by_age.png)

## The Data

| | |
|---|---|
| **Source** | [2024 NSCH, U.S. Census Bureau](https://www.census.gov/programs-surveys/nsch/data/datasets/nsch2024.html) |
| **File used** | Topical file, Stata format (`.dta`) |
| **Who is in it** | Children ages 0 to 17 across all 50 states and D.C. |
| **Cost** | Free and public, no sign-up needed |
| **Variable guide** | [NSCH Codebook](https://www.census.gov/data-tools/demo/uccb/nschdict) |

### The columns this project uses

The survey uses short codes instead of plain names, so the script renames them:

| Survey code | Renamed to | What it means |
|---|---|---|
| `k2q35a` | `ever_told_autism` | Has a doctor ever said the child has autism? |
| `k2q35a_1_years` | `age_first_told` | How old the child was when parents were first told |
| `k2q35b` | `currently_has_autism` | Does the child have autism now? |
| `k2q35d` | `severity` | Mild, moderate, or severe |
| `sc_age_years` | `age` | Child's age |
| `sc_sex` | `sex` | Child's sex |
| `fipsst` | `state` | State (as a number code) |
| `instype` | `insurance_type` | Type of health insurance |
| `fpl_i1` | `income_pct_poverty` | Family income compared to the poverty line |
| `fwc` | `weight` | Survey weight (explained below) |

> Codes can change between survey years. Always double-check them in the codebook.

### What is a survey weight?

The survey does not reach every type of family equally. Some groups answer more often than others. To fix this, every row has a **weight**: a number that says how many real U.S. children that one answer stands for.

- A **normal average** tells you about the families who answered.
- A **weighted average** estimates the numbers for **all U.S. children**.

The script prints both so you can see the difference. All the final numbers use the weighted version.

## Tech Stack

Here is every tool used and what job it does.

| Tool | What it is | What it does in this project |
|---|---|---|
| **Python 3** | The programming language | Runs everything |
| **pandas** | A library for working with tables of data | Loads the survey file, renames columns, cleans values, and groups the data (for example, "rate by state") |
| **NumPy** | A library for fast math on lists of numbers | Calculates weighted averages with `np.average` |
| **matplotlib** | A library for drawing charts | Creates the bar charts and histograms and saves them as images |
| **seaborn** | A chart library built on top of matplotlib | Gives nicer-looking charts with less code (optional) |
| **Jupyter Notebook** | An interactive coding notebook | Lets you run code in small steps and see the results right away |
| **venv** | Python's built-in tool for project environments | Keeps this project's libraries separate from other projects |
| **Git / GitHub** | Version control and code hosting | Saves your progress and shares the project |

**For Java developers:**
- A pandas **DataFrame** is like a `List<Map<String, Object>>` with built-in methods for filtering and grouping.
- `groupby().apply()` works like `Collectors.groupingBy()` in Java Streams.
- `requirements.txt` is the Python version of a `pom.xml` dependency list.
- `venv` keeps libraries inside the project folder, the way Maven keeps them per project.

## How the Code Works

The script `nsch_autism_explorer.py` runs in four steps:

**1. Load**
Reads the Stata file with `pd.read_stata()`, keeps only the 10 columns we need, and gives them readable names. If a column is missing, it stops and tells you which one.

**2. Clean**
- The survey stores yes/no answers as numbers (`1` = yes, `2` = no). The script turns these into `True` / `False`.
- Answers like "don't know" or "skipped" become empty (missing) values.
- Rows with no autism answer or no weight are removed.
- Ages are grouped into **0-5, 6-11, and 12-17**.
- Income is grouped into four levels compared to the poverty line.

**3. Calculate**
Works out the weighted autism rate overall, then splits it by sex, age group, income group, and insurance type.

**4. Chart**
Saves three charts to the `images/` folder:
- `rate_by_age.png`: diagnosis rate for each age group
- `age_first_told.png`: how old children were when first diagnosed
- `rate_by_state.png`: diagnosis rate for each state

## Project Structure

```
nsch-autism-analysis/
├── README.md                  # this file
├── requirements.txt           # list of libraries to install
├── nsch_autism_explorer.py    # the main script
├── data/                      # put the downloaded survey file here (not uploaded to GitHub)
├── notebooks/
│   └── analysis.ipynb         # step-by-step version with notes
└── images/                    # charts the script creates
```

## How to Run It

**1. Get the code**
```bash
git clone https://github.com/<your-username>/nsch-autism-analysis.git
cd nsch-autism-analysis
```

**2. Create a project environment and install the libraries**
```bash
python -m venv venv
source venv/bin/activate          # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

**3. Download the data**
- Go to the [2024 NSCH data page](https://www.census.gov/programs-surveys/nsch/data/datasets/nsch2024.html).
- Download the **Topical data, Stata** zip file.
- Unzip it into the `data/` folder.

**4. Run the script**
```bash
python nsch_autism_explorer.py
```
Results print in the terminal, and the charts appear in `images/`.

> The data file is not stored in this repo because it is large and published by the Census. Add `data/` to your `.gitignore` file.

## Limitations

- **Parent-reported:** Parents answer the questions, so some answers may be remembered wrong.
- **Diagnosis is not the same as autism:** Lower rates in a group may mean those children see doctors less often, not that they have less autism.
- **One year only:** This uses 2024 data. Trends over time would need several years combined.
- **Small groups are less reliable:** Some states or groups have only a few diagnosed children, so their rates can jump around.
- **No significance testing yet:** Differences between groups have not been checked to see whether they could just be random chance.

## What I Learned

## Next Steps

- [ ] Combine several survey years (2016 to 2024) to see trends over time
- [ ] Draw a U.S. map of rates by state
- [ ] Add confidence intervals to show how certain each number is
- [ ] Build a simple interactive dashboard with Streamlit

## Credits

Data: U.S. Census Bureau, *National Survey of Children's Health, 2024*. Sponsored by the Health Resources and Services Administration's Maternal and Child Health Bureau (HRSA MCHB).
