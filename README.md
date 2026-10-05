# Income Trends and Inequality in Denmark, 2014–2023

An exploratory statistics project by **Guido Di Santo**, examining income-band
counts for Danish families, couples, and single people across ten years and
98 municipalities.

Open **[Progetto.ipynb](Progetto.ipynb)** to read the analysis. Saved tables and
figures make it readable directly on GitHub; running it locally reproduces the
results from the supplied data.

## Questions explored

- How did estimated mean income and annual growth change between 2014 and 2023?
- How did a grouped-data Gini coefficient evolve for each family category?
- How closely did income and inequality move together?
- How did Copenhagen's lowest-income-band share change?
- Which municipalities had the highest shares and counts in that band in 2023?

## Extended analyses

| Section | Analysis | Why it matters |
| :--- | :--- | :--- |
| 2.3 | Income-band shares and heatmaps | Shows changes throughout the distribution without midpoint assumptions |
| 2.4 | Exact symmetric decomposition of the aggregate mean | Separates changes within groups from changes in the weights of couples and single people |
| 3.2 | Annual CPI adjustment to 2014 DKK | Distinguishes nominal changes from estimated changes after consumer-price inflation |
| 4.3 | Twelve open-band scenarios | Tests whether income and inequality conclusions depend on chosen representative values |
| 5.2 | Correlations of annual changes and leave-one-out checks | Examines the role of common trends and individual annual observations |
| 7.5 | A ten-year panel of 98 municipalities | Compares endpoint changes, ranking movements, and full trajectories for every family category |

Each section includes English Markdown explaining the question, method,
assumptions, and interpretation, followed by reproducible tables and figures.

## Main findings

Under the notebook's representative-income assumptions, estimated mean income
for all families rises from approximately **DKK 454,762 to DKK 556,395**. Annual
growth slows sharply in 2022 and increases again in 2023. Under the current-price
interpretation, the **22.35% nominal increase becomes 3.91% after CPI adjustment**.
The composition decomposition attributes approximately **+DKK 110,059** to
within-group changes and **−DKK 8,427** to changes in family-category weights.

The baseline Gini declines between the first and last year for every category,
but **alternative open-band values reverse the direction for all families and
single people**. Couples show a decline in all 12 tested scenarios. Correlations
are also specification-dependent: for single people, the nominal correlation
changes from about **−0.933 in levels to +0.337 in annual changes**.

In 2023, **Aarhus has the highest share** of all family units below DKK 200,000
(19.69%), followed by Copenhagen (19.19%). **Copenhagen has the largest count**
in that band (74,540 family units). Shares and counts answer different questions.
Between 2014 and 2023, the lowest-band share falls in all 98 municipalities for
all families and single people, and in 95 municipalities for couples.

These are descriptive results. Means and Gini coefficients approximate grouped
data using fixed representative incomes. A separate CPI-adjusted mean is
provided, conditional on the current-price interpretation; the band shares
retain fixed DKK thresholds. No family-size adjustment is applied. Scenario
ranges and leave-one-out ranges are not confidence intervals. The lowest-band
share is not an official poverty rate, and correlations do not establish
causality. “All families” already includes couples and single people; the three
categories must not be added together.

## Project files

| File | Role |
| :--- | :--- |
| `Progetto.ipynb` | English notebook with methodology, calculations, figures, and limitations |
| `family_income_dk2014-2023.csv` | Semicolon-separated income-band counts |
| `gadm41_DNK_1.json` | Regional map backdrop |
| `dk.json` | Settlement coordinates for municipal markers |
| `denmark_cpi_2014-2023.csv` | Unmodified Statistics Denmark PRIS8 annual CPI response |
| `requirements.txt` | Versions used to execute and validate the notebook |

Keep these files and the `data_sources/` directory together. No network requests
are made during notebook execution. Other files in the working directory are
not required to reproduce the analysis.

## Run locally

Use **Python 3.12** and keep the notebook and data files in the same directory.
From that directory, create an isolated environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

On Windows, use `.venv\Scripts\activate` to activate the environment.

Open `Progetto.ipynb` in a notebook editor such as VS Code, select `.venv` as the
Python kernel, and run all cells in order. Alternatively, install and launch
JupyterLab from the same environment:

```bash
python -m pip install jupyterlab
jupyter lab Progetto.ipynb
```

To execute the notebook from the command line and save a separate result:

```bash
python -m ipykernel install --user --name denmark-income \
  --display-name "Python (Denmark income)"
jupyter nbconvert --to notebook --execute Progetto.ipynb \
  --output Progetto.executed.ipynb \
  --ExecutePreprocessor.kernel_name=denmark-income \
  --ExecutePreprocessor.timeout=180
```

Registering this kernel ensures that command-line execution uses the project
environment, even if another Python installation already provides a default
Jupyter kernel.

The notebook checks record uniqueness, count validity, national totals, family
category reconciliation, required missing values, and map coverage. Missing
values marked `..` remain missing; they are never replaced with zero.
Additional checks verify the archived income-source match, band-share totals,
the exact composition identity, baseline reproduction in the sensitivity grid,
CPI coverage, and the balanced municipal panel.

## Data provenance

An official extract retrieved on **4 October 2026** from
[Statistics Denmark INDKF122](https://www.statistikbanken.dk/statbank5a/SelectVarVal/Define.asp?MainTable=INDKF122)
matches **all 32,603 observed counts** in the local income CSV. This supports
identifying the data as family income before tax, while the original extraction
date and processing history remain unknown. The original file and its 67 missing
values are unchanged. CPI deflation explicitly assumes current-year DKK bands.

The annual-average CPI comes from
[Statistics Denmark PRIS8](https://www.statbank.dk/PRIS8), retrieved on the same
date. Its original index reference is 1900=100; the notebook rebases it to
2014=100 and expresses real-income estimates in 2014 DKK. The saved metadata
retain the official note on greater uncertainty in parts of 2020.

See [data_sources/README.md](data_sources/README.md) for the source archive,
checksums, and instructions for requesting a fresh copy separately.

The regional file identifies itself as a GADM 4.1 Denmark layer. Its five
regional boundaries provide a backdrop; municipal values are displayed at
same-named settlement coordinates, not as municipal boundary polygons.
The coordinate file's original provider and download date remain undocumented.

## Revision notes

The English revision preserves the original study questions and representative
income assumptions while correcting three calculation issues: the omitted
origin segment in the Gini calculation, the Copenhagen single-person series
previously taken from couples, and duplicate counting of the aggregate together
with its components. Saved outputs have been regenerated from the revised code.
Interpretations now distinguish income from wealth, percentages from percentage
points, and lowest-band membership from poverty.
