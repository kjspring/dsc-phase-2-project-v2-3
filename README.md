# Home Price Prediciton using Linear Regression

## Overview
Linear regression is one of the most commonly used methods for predictive modeling. It fits a line to find the best fit among the data. This project's goal is to predict the sales price of of a home based on the features of the home. Ordinary least squares linear regression minimizes the sum of the squared difference between the observed dependent variable and the predicted response to estimate the coefficients to build the linear equation.

![Linear Equation](img/linear-eq.png)

## Business Problem

Bon Jovi Real Estate Advisors is a residential real estate broker in King County, WA. Many of their clients come to them needing to sell their homes but are unsure of the price to list. The real estate broker wants us to design a model where they can take in the features of their client's home and determine which price to begin listing discussion with their client.

### Stakeholders
- President and Managing Director of Bon Jovi Real Estate Advisors
- Bon Jovi real estate agents

## The Data

This project uses the King County House Sales dataset, which can be found in  `kc_house_data.csv` in the data folder in this repository. The description of the column names can be found in `column_names.md` in the same folder.

### Data Cleanup

#### Missing Data

The data in the `waterfront`, `view`, and `yr_renovated` had `NaN` values. There are placeholder values in `yr_renovated` and `sqft_basement`. `yr_renovated` has 0 for over 95% of values which means the property was not renovated. `sqft_basement` has `?` for about 2% of its values.

#### Strategy for clean-up
- `NaN`
    - `waterfront`
        - Binary categorical variable (`YES` or `NO`)
        - replace `NaN` with mode of `NO` as most likely these properties are not waterfront
    - `view`
        - Ordinal categorical variable
        - replace `NaN` with `NONE` 
    - `yr_renovated`
        - Will be converted to a countable numerical variable
        - `0` is the most common value with over 95% of values. 
        - Replace `NaN` with 0 value
- Placeholder
    - `yr_renovated` has `0` for missing or unknown values.
    - `sqft_basement` has `?` for missing or unknown values.

#### Re-coding variable
For regression analysis, the catagorical data need to be in numerical format so they were re-coded to maintain their binary, ordinal, and count information.

- `condition`
    - Ordinal categorical variable
    - dictionary - `{'Poor': 0, 'Fair': 1, 'Average': 2, 'Good': 3, 'Very Good': 4}`
- `grade`
    - Ordinal categorical variable
    - Example: `7 Average`
    - Delete the descriptor, keep the number, and convert it to `int` datatype
- `sqft_basement`
    - Code to binary catagorical ('No': 0, 'Yes':1)
    - `?` makes up about 2% of values and the current value of `0` makes up almost 60%
    - Since 2% is a low value I will replace `?` with the mode of `0`
    - If there is a basement (sq.ft > 0) the value will be set to `1`
- `view`
    - Oridinal catagorical variable
    - dicitonary - `{'NONE': 0, 'FAIR': 1, 'AVERAGE': 2, 'GOOD': 3, 'EXCELLENT': 4}`
- `waterfront`
    - Binary catagorical variable
    - dictionary - `'NO': 0, 'YES': 1`
- `home_age`
    - Create new count variable
    - subtract current year from `yr_built`
    - Drop `yr_built`
- `yr_since_reno`
    - Make new count variable  
    - Subtract current year from `yr_renovated`
        -  `0` is the most common value with over 95% of values.
        - if never renovated then subtract from `yr_built`
    - Drop `yr_renovated`

#### Outliers
An obvious outlier was found that I believe was a data entry error. A property is listed as having 33  bedrooms but only having 1,620 square feet in size. I edited this observation to 3.

## Modeling

### Model 1


### Model 2


### Model 3

## Regression Results

## Conclusion




