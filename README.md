# Project 1: Missing Data Reconstruction in Dairy Milking Records

## Project Overview
The goal of this project is to reconstruct missing information in a large dairy milking dataset using the information available in complete records. The dataset contains approximately 8.5 million observations and 10 variables describing animal, lactation, reproductive, and milking-session characteristics. Initial data inspection identified substantial missingness in variables such as `AnimalNumber`, `LactationNumber`, `DaysInMilk`, and `ReproductionStatus`, while most milking-session variables are complete.
The project will develop a reproducible workflow to identify missing data, evaluate possible reconstruction strategies, and recover as much missing information as reasonably possible while validating the accuracy of the approach.

## Development Environment

The project will be conducted locally using Python and Jupyter Notebooks in Visual Studio Code. GitHub will be used for version control and project documentation. The raw dataset will remain local and will not be uploaded to the public repository.

## Data Management and Data Lineage

The raw dataset is stored locally in the `data/` directory, which is excluded from GitHub through `.gitignore`. The original data will remain unchanged. Processing steps will be documented so that the workflow from raw data to reconstructed data can be traced and reproduced.

The planned data lineage is:

Raw data → Data inspection → Missing-data assessment → Data cleaning → Imputation/reconstruction → Validation → Final dataset

## Analysis and Modeling Strategy

I will first examine patterns of missingness and relationships among observations and variables. Because the dataset contains repeated milking records, I will first investigate whether missing values can be reconstructed using information from related records and logical relationships within the data.

Rule-based or deterministic reconstruction will be prioritized when values can be reliably inferred from existing records. For observations that cannot be reconstructed using these relationships, I will explore model-based imputation using available variables such as milk yield, milk flow, session duration, and milking number.

Different approaches may be required for different target variables. For example, `DaysInMilk` is numerical, while `LactationNumber` and `ReproductionStatus` may require discrete or classification-based approaches.

## Testing and Validation

Imputation methods will be evaluated using observations with known values. A subset of known values can be temporarily masked and reconstructed using the proposed method. The reconstructed values will then be compared with the original values using appropriate evaluation metrics. This will help determine whether a method is sufficiently accurate before applying it to truly missing observations.

## Naming Convention

Files and folders will use lowercase `snake_case`. Notebooks will use numerical prefixes to show workflow order, such as `01_data_inspection.ipynb`, `02_data_cleaning.ipynb`, and `03_imputation.ipynb`.

## Timeline

Week 1: Set up repository, inspect data, and identify missing-data patterns 
Week 2: Investigate data relationships and develop rule-based reconstruction methods 
Week 3: Explore model-based imputation and compare alternative approaches 
Week 4: Validate reconstruction methods, finalize the dataset, and document results 

