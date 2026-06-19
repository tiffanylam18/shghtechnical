# UK Voting Intention Survey Analysis

An analysis of a survey of 1,437 UK respondents examining voting intention and the demographic, attitudinal, and behavioural factors that predict it. A logistic regression model was trained to predict how respondents would vote if there was a general election tomorrow, with results presented in an interactive dashboard.

**Dashboard:** https://tiffanylam18.github.io/shghtechnical/

---

## Repository Structure

```
├── index.html                            # Interactive dashboard
├── Stonehaven_tech_test_survey_data.csv  # Survey dataset
├── notebook.ipynb                        # Analysis notebook
└── README.md
```

---

## Running the Notebook

**Requirements**

```
python >= 3.9
pandas
numpy
matplotlib
seaborn
scikit-learn
altair
```

Install dependencies:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn altair
```

**Steps**

1. Clone the repository
2. Ensure `Stonehaven_tech_test_survey_data.csv` and `notebook.ipynb` are in the same directory
3. Open `notebook.ipynb` and run all cells in order

---

## Exploratory Data Analysis Overview

**1. Data Loading**

- Column names are renamed for readability and serial number is set as the index

**2. Preprocessing**

- 32 duplicate rows are identified and dropped, leaving 1,468 rows
- Three columns contain missing values: work sector (76.5%), property ownership (60.1%), and EU referendum (24.7%), each handled based on the reason for missingness
- Income currency encoding is corrected
- Features are encoded based on their variable type
  - Ordinal variables (income, political interest, attitude columns) are mapped to numeric scales, preserving natural ordering
  - Binary statement columns are encoded as 0/1
  - Nominal variables (gender, region, working status, etc.) are one-hot encoded
  - Residual missing values in income and attitude columns are handled via binary flags and imputation

**3. Data Visuals**

- Three visualisations are produced examining vote intention distribution, voting patterns by subgroup, and EU referendum vote vs voting intention

**4. Modelling**

- Correlation checks are performed across attitude columns and the full feature matrix to identify multicollinearity
- Redundant features identified through the correlation check are dropped, leaving 86 features
- Data is split 80/20 into training and test sets with stratification to preserve class distribution
- Features are standardised using StandardScaler, fit on training data only to prevent data leakage
- A logistic regression model with L2 regularisation is trained for multiclass classification across 8 voting intention classes
- Model coefficients are extracted and interpreted at both the overall and per-party level
- A Random Forest model is trained as an additional check

**5. Dashboard**

- Four interactive Altair charts are built and exported as JSON specs
- Charts are embedded inline into `index.html` for deployment to GitHub Pages

---

## Updating the Dashboard

To update the dashboard after making changes to the notebook:

1. Run all cells in `notebook.ipynb` to regenerate chart specs and model outputs
2. The final cells generate and save a new `index.html`
3. If the dataset filename has changed, update the filename in the data loading cell:

```python
election_df = pd.read_csv('your_new_filename.csv')
```

4. Push the updated `index.html` to GitHub:

```bash
git add index.html
git commit -m "Update dashboard"
git push
```

GitHub Pages will redeploy automatically within a few minutes.
