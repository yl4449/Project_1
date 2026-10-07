# Project 1: Predicting Missing Animal IDs in Dairy Milking Records

## Project Overview

This project trains a classification model to identify which cow produced each milking record. The eventual goal is to predict missing `animal_id` values so that these records can be linked to individual cows.

The dataset contains approximately 8.5 million milking records, with approximately 8.3 million remaining after cleaning.

## Development Environment

The project uses Python and Jupyter Notebooks locally in Visual Studio Code. Git and GitHub provide version control and documentation.

Data files are stored locally and excluded from the public repository through `.gitignore`. File names generally follow `snake_case`, with numerical prefixes used to indicate notebook workflow order.

## Data Processing

The cleaning workflow includes:

1. Standardizing column names and converting `event_date` to datetime.
2. Removing exact duplicate rows.
3. Removing records with missing or zero `avg_milk_flow`.
4. Removing rows with absolute Z-scores greater than 3.

The cleaned dataset is saved as `Data_after_cleaning.csv`. Records are then separated into known-ID and missing-ID groups. Only known-ID records are used for training and evaluation.

## Data Split and Sampling

Known-ID records are sorted by `event_date` and split chronologically:

- **Training:** earliest 70%.
- **Validation:** following 10%.
- **Test:** latest 20%.

For model training, up to 1,600 records are randomly sampled per cow from the training set. Validation uses a random sample of up to 50,000 records from the validation set. These samples do not change the original 70/10/20 split.

## Features and Model

The model uses eight measurement and lactation features:

`flow_30_60_session`, `yield_first_2min_session`, `yield_session`, `duration_session_sec`, `milking`, `avg_milk_flow`, `days_in_milk`, and `lactation_number`.

Additional features are derived from existing data:

- Event month, day of year, weekday, and year.
- **Estimated calving date = milking date − days in milk.** Records from the same cow within the same lactation should have similar estimated calving dates.

Random Forest was explored initially. K-nearest neighbors (KNN) was selected because it ran faster in this project, allowing a larger training sample.

The final approach uses standardized features and **KNN with K = 1**, assigning each record the animal ID of its nearest training record. Model development included increasing the records sampled per cow and adjusting the weight of the estimated calving date feature.

## Evaluation and Limitations

Validation was used to tune the model, and the test set was used for final evaluation. `animal_id` is the target and is excluded from the predictors.

| Evaluation | Accuracy |
|---|---:|
| Validation sample | 57.83% |
| Test set | 21.51% |

The lower test accuracy indicates limited performance on later records. Possible contributors include repeated tuning on validation data, changes in cow characteristics over time, and cows in the test set that were absent from training. KNN cannot predict an animal ID that it has never seen during training.

Predicting IDs for the missing-ID records remains a future step. The current test performance does not support reliable automatic assignment.

## Data Lineage

Raw data → Inspection → Cleaning → Known/missing-ID separation → Chronological split → Feature engineering → Model training and tuning → Test evaluation

## Timeline

| Stage | Planned Work |
|---|---|
| Week 1 | Set up the public repository, inspect the dataset, and identify missing-data patterns |
| Week 2 | Investigate relationships within the data and develop rule-based reconstruction methods |
| Week 3 | Explore model-based imputation methods and compare alternative approaches |
| Week 4 | Validate reconstruction methods, finalize the dataset, and document results |
