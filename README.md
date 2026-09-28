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

The dataset will first be separated into two groups: records with a known `animal_id` and records with a missing `animal_id`.

Only records with known animal IDs will initially be used for model development because their true identities are available for training and evaluation.

The known-ID records will be sorted chronologically by `EventDate`. The data will then be divided into training, validation, and testing sets using a time-based split:

- Approximately 70% of the earlier records will be used as the training set.
- Approximately 10% of the following records will be used as the validation set.
- Approximately 20% of the most recent records will be reserved as the test set.

Using a chronological split helps prevent data leakage because the model will be trained on earlier observations and evaluated on later observations that were not available during training.

The distribution of important variables and missing-value patterns will also be checked across the training, validation, and test sets to make sure that the datasets are reasonably comparable.


### Modeling Strategy

The main modeling goal is to learn patterns from records with known animal IDs and use those patterns to help identify the most likely animal associated with records where `animal_id` is missing.

The model will use information such as `EventDate`, milk production variables, milking characteristics, lactation information, and other available cow-level variables to learn temporal and production patterns for individual animals.

Several modeling approaches will be explored and compared. A simple baseline model will first be developed to provide a reference for performance. Additional machine learning methods may include Random Forest and Gradient Boosting models because they can capture nonlinear relationships and interactions among multiple predictors.

The model will be trained using only the training dataset. The validation dataset will be used to compare modeling approaches and tune model settings. After the final model is selected, it will be applied to the test dataset for final performance evaluation.

If model performance is acceptable, the final trained model will then be used to estimate the most likely animal identity for records where `animal_id` is missing.

## Testing and Validation

Model performance will be evaluated using the validation and test datasets containing records with known animal IDs.

During validation, the true `animal_id` values will be retained for comparison but will not be used as predictors. The model will predict which animal is most likely associated with each observation, and the predictions will be compared with the known animal IDs.

The validation set will be used during model development to compare different modeling techniques and adjust model settings. The test set, which contains the most recent observations, will be reserved for the final evaluation and will not be used during model training or tuning.

Performance will be assessed using classification metrics such as overall accuracy and, if appropriate, precision, recall, and confusion matrices. Model errors will also be examined to determine whether certain animals or time periods are more difficult to identify.

After the model has been validated and tested on records with known animal IDs, it can be applied to observations with missing `animal_id` values. Predictions with low confidence may be flagged for further review rather than automatically assigning an animal identity.

## Timeline

| Stage | Planned Work |
|---|---|
| Week 1 | Set up the public repository, inspect the dataset, and identify missing-data patterns |
| Week 2 | Investigate relationships within the data and develop rule-based reconstruction methods |
| Week 3 | Explore model-based imputation methods and compare alternative approaches |
| Week 4 | Validate reconstruction methods, finalize the dataset, and document results |
