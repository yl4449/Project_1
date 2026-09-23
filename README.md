# Project 1: Missing Data Reconstruction in Dairy Milking Records

## Project Overview

The goal of this project is to reconstruct missing information in a large dairy milking dataset using the information available in complete records. The dataset contains approximately 8.5 million observations and 10 variables describing animal, lactation, reproductive, and milking-session characteristics. Initial data inspection identified substantial missingness in variables such as `AnimalNumber`, `LactationNumber`, `DaysInMilk`, and `ReproductionStatus`, while most milking-session variables are complete.
The project will develop a reproducible workflow to identify missing data, evaluate possible reconstruction strategies, and recover as much missing information as reasonably possible while validating the accuracy of the approach.

## Development Environment

This project will be conducted locally on my computer using Python in Visual Studio Code as the primary IDE. Jupyter Notebooks within VS Code will be used for data inspection, cleaning, imputation, modeling, and validation. Git and GitHub will be used for version control and project documentation. Because the dataset is large and should not be included in the public repository, the raw data will be stored locally in the `data/` folder and excluded from GitHub through `.gitignore`.

## Naming Convention

Files and folders will use lowercase `snake_case`. Notebooks will use numerical prefixes to show workflow order, such as `01_data_inspection.ipynb`, `02_data_cleaning.ipynb`, and `03_imputation.ipynb`.

## Data Management and Data Lineage

The raw dataset is stored locally in the `data/` directory, which is excluded from GitHub through `.gitignore`. The original data will remain unchanged. Processing steps will be documented so that the workflow from raw data to reconstructed data can be traced and reproduced.

The planned data lineage is:

Raw data → Data inspection → Data processing → Data cleaning → Imputation/reconstruction → Validation → Final dataset

### Data Cleaning Strategy

The dataset will be cleaned before modeling to improve data quality and reduce potential errors. The cleaning process include:

- Standardizing variable names and converting `EventDate` to a datetime format.

- Removing exact duplicate rows.

- Removing records where `avg_milk_flow` equals zero and null.

- Identifying potential outliers using Z-scores, with observations beyond |Z| > 3 considered potential outliers. Removing rows that contain extreme outliers in selected continuous variables.

### Train/Test Split Strategy

Before splitting the data, observations with missing `animal_id` values will be removed because these records cannot be assigned to a specific animal and therefore cannot be included in an animal-level train/test split.

The remaining cleaned dataset will then be divided into training and testing sets using `animal_id` as the grouping variable. Approximately 80% of the animals will be assigned to the training set and 20% to the testing set.

All observations from the same animal will remain in the same dataset. This means that an animal appearing in the training set will not also appear in the testing set. After the split, the training and testing datasets will be checked to confirm that there is no overlap in `animal_id` and that the distributions of key variables and missing values are reasonably similar between the two sets.


### Modeling Strategy

Several modeling approaches will be explored and compared.

A linear regression model will first be used as a baseline model because it is simple and easy to interpret.

Additional machine learning models may include:

- Random Forest Regression, which can capture nonlinear relationships and interactions among predictors.
- Gradient Boosting models, which may improve predictive performance by sequentially correcting prediction errors.
- Regularized regression methods such as Ridge or Lasso regression, which can help reduce overfitting and evaluate the importance of predictors.

Model performance will be compared using appropriate evaluation metrics such as R-squared.

The final model will be selected based on predictive performance, interpretability, and generalization to the test dataset.

## Testing and Validation

Imputation methods will be evaluated using observations with known values. A subset of known values can be temporarily masked and reconstructed using the proposed method. The reconstructed values will then be compared with the original values using appropriate evaluation metrics. This will help determine whether a method is sufficiently accurate before applying it to truly missing observations.

## Timeline

| Stage | Planned Work |
|---|---|
| Week 1 | Set up the public repository, inspect the dataset, and identify missing-data patterns |
| Week 2 | Investigate relationships within the data and develop rule-based reconstruction methods |
| Week 3 | Explore model-based imputation methods and compare alternative approaches |
| Week 4 | Validate reconstruction methods, finalize the dataset, and document results |
