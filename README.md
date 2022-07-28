# Home Price Prediciton using Linear Regression

## Overview
The project's goal is to predict the sales price of a home in King County, WA based on the features of the home. One way to most common method to predict continuous values is through linear regression. Linear regression explains the relationship of independent predictor variables to a dependent predicted variable using a linear equation. The linear equation represents the best fit among the data, where given a set of predictor variables the predicted value would be found.  Ordinary least squares linear regression finds this linear equation by minimizing the sum of the squared difference between the actual observed dependent variable and the predicted value, denoted as y-hat.

![Linear Equation](img/linear-eq.png)

This project will use a dataset of home sales and design three linear regression models. The models will be compared and the best model will be used for a backend function for a dashboard to predict the sales range for a home.

## Business Problem

Bon Jovi Real Estate Advisors is a residential real estate broker in King County, WA. Many of their clients come to them needing to sell their homes but are unsure of the price to list. The real estate broker wants us to design a model where they can take in the features of their client's home and determine which price to begin listing discussion with their client.

### Stakeholders
- President and Managing Director of Bon Jovi Real Estate Advisors
- Bon Jovi real estate agents that will use the model for price predictions

## The Data

This project uses the [King County House Sales dataset](https://www.kaggle.com/harlfoxem/housesalesprediction), which can be found as  `kc_house_data.csv` in the data folder in this repository. The description of the column names can be found in `column_names.md` in the same folder.

## Variable Names and Descriptions for King County Data Set
* `id` - Unique identifier for a house
* `date` - Date house was sold
* `price` - Sale price (prediction target)
* `bedrooms` - Number of bedrooms
* `bathrooms` - Number of bathrooms
* `sqft_living` - Square footage of living space in the home
* `sqft_lot` - Square footage of the lot
* `floors` - Number of floors (levels) in house
* `waterfront` - Whether the house is on a waterfront
  * Includes Duwamish, Elliott Bay, Puget Sound, Lake Union, Ship Canal, Lake Washington, Lake Sammamish, other lake, and river/slough waterfronts
* `view` - Quality of view from house
  * Includes views of Mt. Rainier, Olympics, Cascades, Territorial, Seattle Skyline, Puget Sound, Lake Washington, Lake Sammamish, small lake / river / creek, and other
* `condition` - How good the overall condition of the house is. Related to maintenance of house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each condition code
* `grade` - Overall grade of the house. Related to the construction and design of the house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each building grade code
* `sqft_above` - Square footage of house apart from basement
* `sqft_basement` - Square footage of the basement
* `yr_built` - Year when house was built
* `yr_renovated` - Year when house was renovated
* `zipcode` - ZIP Code used by the United States Postal Service
* `lat` - Latitude coordinate
* `long` - Longitude coordinate
* `sqft_living15` - The square footage of interior housing living space for the nearest 15 neighbors
* `sqft_lot15` - The square footage of the land lots of the nearest 15 neighbors

### Data Cleanup

The median house sold in King County, WA between 2014 to 2015 was for \$450,000. The median house sold was 1910 square feet, 3 bedroom, 2.25 bathrooms, and 47 years old. The home sale price range was \\$78,000 and to \\$7,700,000.

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

### Encoding Variables
For regression analysis, the catagorical data needs to be in numerical format so catagorical variables were encoded to to meet this requirement but still maintain their binary, ordinal, and count information.

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
- `zipcod`
    - Used [dummy variables](https://en.wikipedia.org/wiki/Dummy_variable_(statistics))

#### Outliers
An obvious outlier was found that I believe was a data entry error. A property is listed as having 33  bedrooms but only having 1,620 square feet in size. I edited this observation to 3 to match the median number of bedrooms in the dataset.

## Modeling

### Model 1 (M1)
M1 is the baseline model and it was chosen from the variables correlated with sales price of a house.

![Correlation Heatmap](img/corr-heatmap.png)

### Model 2


### Model 3


## Regression Results

## Conclusion




