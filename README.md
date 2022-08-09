# Home Price Prediciton of King County, WA single-family homes using Linear Regression

## Overview
The project's goal is to predict the sales price of a home in King County, WA based on the features of the home, determine the coefficients to make actionable recommendations on how to improve the sale price of a house, and who to target with marketing.

One of the  most common methods to predict continuous values is through linear regression. Linear regression explains the relationship of independent predictor variables to a dependent predicted variable using a linear equation. The linear equation represents the best fit among the data, where given a set of predictor variables the predicted value would be found.  Ordinary least squares linear regression finds this linear equation by minimizing the sum of the squared difference between the actual observed dependent variable and the predicted value.

This project will use a dataset of home sales and design three linear regression models. The models will be compared and the best model will be used for a backend function for a dashboard to predict the sales range for a home.

## Business Problem
Bon Jovi Real Estate Advisors is a residential real estate broker in King County, WA. Many of their clients come to them needing to sell their homes but are unsure of the price to list. The real estate broker wants us to design a model where they can take in the features of their client's home and determine which price to begin listing discussion with their client.

### Stakeholders
- President and Managing Director of Bon Jovi Real Estate Advisors
- Bon Jovi real estate agents

## Deliverables
- [Presenatation](https://docs.google.com/presentation/d/1j2AvBxCo7cBzQYDgb4XhTaN4kj6pb4SyR1TKwX8m39I/edit?usp=sharing) to stakeholders
- Jupyter Notebook
- GitHub Repository

## Repository Navigation
- [/img](/img) - contains image files
- [/data](/data) - house sale data from King County, WA
- [/license](/license) - license information
- project_notebook.ipynb - Jupyter notebook

## The Data
This project uses the [King County House Sales dataset](https://www.kaggle.com/harlfoxem/housesalesprediction), which can be found as  `kc_house_data.csv` in the data folder in this repository. The description of the column names can be found in `column_names.md` in the same folder.

### King County Home Sales May 2014 - May 2015
![King County, WA Sales](img/KC_home_sale_map.png)

## Variable Names and Descriptions for King County Data Set
See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each condition code

| Variable | Data Type | Description |
|:---------|:----------|:------------|
|`id`| catagorical | Unique identifier for a house |
|`date`|continuous | Date house was sold |
| `price` | continuous | Sale price (prediction target)
| `bedrooms` | discrete | Number of bedrooms
| `bathrooms` | discrete | Number of bathrooms
| `sqft_living` | continuous | Square footage of living space in the home
| `sqft_lot` | continuous | Square footage of the lot
| `floors` - | discrete | Number of floors (levels) in house
| `waterfront` | ordinal | Whether the house is on a waterfront |
| `view` | ordinal | Quality of view from house |
| `condition` | ordinal | How good the overall condition of the house is. Related to maintenance of house |
| `grade` | ordinal | Overall grade of the house. Related to the construction and design of the house |
| `sqft_above` | continuous | Square footage of house apart from basement |
| `sqft_basement` | continuous | Square footage of the basement |
| `yr_built` | catagorical | Year when house was built |
| `yr_renovated` | catagorical |Year when house was renovated |
| `zipcode` | catagorical | ZIP Code used by the United States Postal Service |
| `lat` | catagorical | Latitude coordinate |
| `long` | catagorical | Longitude coordinate |
| `sqft_living15` | continuous | The square footage of interior housing living space for the nearest 15 neighbors |
| `sqft_lot15` | continuous | The square footage of the land lots of the nearest 15 neighbors | 

### Data Cleanup

The median house sold in King County, WA between 2014 to 2015 was for \$450,000. The median house sold was 1910 square feet, 3 bedroom, 2.25 bathrooms, and 47 years old. The home sale price range was $78,000 and to $7,700,000.

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
For regression analysis, catagorical data needs to be in numerical format so catagorical variables were encoded to to meet this requirement but still maintain their binary, ordinal, and count information.

| variable | Data Type | Plan |
|:---------|:----------|:-----|
|`condition`| ordinal | Recode to dictionary.<br>`{'Poor': 0, 'Fair': 1, 'Average': 2, 'Good': 3, 'Very Good': 4}` |
|`grade` | ordinal |Delete the descriptor, keep the number, and convert it to `int` datatype.<br>Example: `7 Average` becomes `7` |
|`basement`| binary | If there is a basement (sq.ft > 0) the value will be set to `1`.<br> No basement (sq.ft = 0) set to `0`. <br>`?` makes up about 2% of values and the current value of `0` makes up almost 60%.<br>Replace `?` with the mode of `0`. |
| `view` | oridinal | Recode to dictionary.<br>`{'NONE': 0, 'FAIR': 1, 'AVERAGE': 2, 'GOOD': 3, 'EXCELLENT': 4}` |
| `waterfront` | binary | Recode to dictionary.<br>`{NO': 0, 'YES': 1}`. |
| `home_age` | discrete | Create variable from `yr_built`.<br>Subtract current year from `yr_built`. <br>Drop `yr_built` |
| `yr_since_reno` | discrete | Create variable from `yr_renovated`.<br>Subtract current year from `yr_renovated`.<br>`0` is the most common value with over 95% of values.<br>If never renovated then subtract from `yr_built`.<br>Drop `yr_renovated`. |
|`zipcode`|catagorical| Used [dummy variables](https://en.wikipedia.org/wiki/Dummy_variable_(statistics))|

### Outliers
An obvious outlier was found that I believe was a data entry error. A property is listed as having 33  bedrooms but only having 1,620 square feet in size. I edited this observation to 3 to match the median number of bedrooms in the dataset.

### Data Limitations
- Data is only from 2014 to 2015. Models to predict future sales price would need to be updated with newer data.
- Some data might be missing, such as for-sale-by-owner or owner-financed sales.
- Ordinal data might be highly variable based on examinter's subjective experience.

## Modeling

### Model 1 (M1)
M1 is the baseline model and it was chosen from variables with a Pearson's correlation greater than 0.6 with sales price of a house.

![Correlation Heatmap](img/corr-heatmap.png)

Independent variables correlated with `price` with Pearson's correlation (r) greater than 0.6  are `sqft_living`, `sqft_above`, `grade`. As the heatmap shows, `sqft_living` and `sqft_above` are highly correlated with each other (r = 0.88) so this will likely create multicollinearity.

### Model 2 (M2)
M2 uses an automated stepwise regression strategy.  All the variables are fed into the model and fitted. The predictor variable with highest p-value is removed if the p-value is greater than 0.05. This is repeated until all the p-values of the predictor variables are less than 0.05. 

M2 has many more features than M1 and utulizes the Zip code dummy variables.

### Model 3 (M3)
M3 builds on M1 by adding interaction effects to the main effects. It also removes `sqft_above` as a predictor variable because of multicollinearity. Interaction effects occur when the effect of one predictor variable depends on the value of another variable. For example, condition of a home may be dependent on the age of the home. There may be a dependency between the square feet of living space and the number of bathrooms.

## Statistics used to compare models

### R-squared
R-squared is a statistical measurement of how close the observed data is to the regression line. It tells us if the model has can explain the variance seen in the data. Adjusted R-squared is similar to R-squared but it takes into account the number of independent variables used in the model as R-squared has a tendency to increase each time a new variable is added to the model. R-squared range from 0 to 1 with the higher the value, the more the model explains the variance.

![R-squared](img/R-squared.png)

### Prediction interval
A prediction interval (PI) is the range where a single new observation is likely to fall given specific values of the indpendent variables. The prediciton interval can be use to assess if the predicitons are sufficiently in a narrow range to satisfy the client's requirement. Prediction intervals can be compared across models. Smaller intervals indicate tighter predictive range. Large prediction intervals tell us the model could have a wide range in its predictions and would not meet the client's needs.

![Prediction interval equation](img/PI.png)

### Root Mean Squared Error (RMSE)
RMSE is a measure of the mean error rate of a regression model that penalizes larger errors. It is the square root of the average squared difference between the predicted dependent value and the actual values in the dataset. The smaller the RMSE value, the closer the fitted line from the linear equation is to the actual data. Like Mean Squared Error (MSE), this statistic squares the residual error before it is averaged, which gives a high weight to large errors, but because it the square root is taken, the statistic is in the same units as the dependent variable, sale price ($USD). The lower the score of RMSE the closer the model fits the data. 

![Root Mean Squared Error equation](img/RMSE.png)

## Regression Results

![Regression results](img/regression-results.png)

### M1
The p-values indicate that each of the variables chosen has a statistical significant relationship with the dependent variable. The residual plot has a random pattern and the QQ-plot of the residuals indicate that they are normally distributed. Log transforming `price` has worked and the assumptions are met. There does seem to be a  high level of multicollinearity.

The linear function of M1 is:

![Model 1 linear equation](img/M1-eq.PNG)

The coefficient for `grade` of a home is 0.201. This means for every one-unit increase in the `grade` of the home, there is a 22.8% increase in sales price. The coefficient for `sqft_living` is 0.0003. This means for every square foot increase in the living space in a house there is a 0.03% increase in sale price. Lastly, the coefficient for `sqft_above` is 0.0001 so for every square foot increase in the above ground area of a house there is a 0.01% reduction in sale price. Interestingly, the area of the space above the home is penalized in this model. This may be due to multicolinearity between the `sqft_living` and `sqft_above`.  These values are so low as to be insignificant.

$R^2$ value is low at 0.56, RMSE is 0.35, and prediciton interval is 0.68.

### M2
There are many more independent variables used in M2 as compared to M1, which may overfit the data. This model used all the independent variables expect for some of the Zip codes after recoding Zip code to dummy variables. `price` is still log transformed. Though there are many variables, the p-values indicate that each of the variables chosen has a statistical significant relationship with the dependent variable, `price`. The residual plot has a random pattern so the homoskedasticity assumption is met. The QQ-plot of the residuals indicate that they are not normally distributed. $R^2$ is 0.88 for M2. This is tremendous improvement over M1. RMSE is 0.19 and the prediction interval is 0.36.

The coefficient for `condition` for M2 is 0.0587. This means that for ever one-unit increase in condition of a house the sale price is expected to increase on average 6%. Adding an additional full bathroom would increase the sale price of a home by about 3.9%. 

M2 can tell us which customers to target when marketing. Waterfront properties have a sale price over 59% more homes not on water. A 1-unit increase in the score of a view increases the sale price by about 6%. Some of the highest coefficients with M2 is in the Zip code variables. For example, with all other independent variables held constant a home in 98039 would sell for 228% more than Zip code 98003. The real estate agents should avoid marketing to property owners in 98002, 98003, 98023, 98032, 98042, and 98198 as these sell for much less than other Zip codes in King County, WA as these sell for much less than other Zip codes in King County, WA.

### M3

M3 includes interaction effects along with main effects. The interaction effect of `bathrooms` and `bedrooms` was not included as the p-value was above 0.05, indicating that the interaction with `price` is not statistically significant. Likewise, the variable `bedrooms` is not have a statistically significant main interaction with `price`. The residual plot has a random pattern so there is homoskedasticity. The QQ-plot of the residuals indicate that they are normally distributed. These results show the model meets the assumptions of OLS. There is multicollinearity between some of the variables.

$R^2$ is 0.62. This is an improvement over M1 but is less than M2. RMSE is 0.32 and the prediction interval is 0.63.

The coefficient for `sqft_living` is 0.0003, the same as M1. The coefficient for `condition` with M3 is -0.077 so for each one-unit increase in the condition of a house the sale price is expected to **decrease** about 8% on average.

## Recommendations

### Summary
- Our client wants to be able to predict sales price, identify where to market in King County, WA, and how customers can improve their home to increase sale price.
- Ordinary least squares linear regression was used to create three models.
- The three models were compared using $R^2$, Prediction Intervals (PI), and Root Mean Squared Error (RMSE).
- Model 2 (M2) is the best model as it has the best predictive capabilities, R-squared of 0.88, low RMSE and PI, though the error is not normally distributed.

### Actionable Recommendations
1. Improving the condition of a house by one-unit will increase the sale price  by about 6%.
2. Adding an additional full bathroom would increase the sale price of a house by about 3.9%.
3. Marketing should be focused throughout King County, WA except in Zip codes 98002, 98003, 98023, 98032, 98042, and 98198 as the value of the homes sold in these Zip codes are well under the surrounding Zip codes.
4. Market real estate services toward owners of waterfront properties as these sell for 59% more than homes not waterfront.

### Next Steps
This model could be improved to make better predictions by adding more data and additional variables such as crime rate in the geographic location of the home, the zoned public school ranking, and time the house was on the market until it was sold. The GPS coordinates of the sold house could be used to collect the first two of these variables. The Multiple Listing Service may be a source for more recent data and on how long a house was on the market from day of listing to closing date. 

Another source of data could be in the internal data of our brokerage client. They possibly have data of properties they have sold or bid on, this would include the data of the asking and bidding price of the property.