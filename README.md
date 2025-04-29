# Intestigation In Power Outage In USA

## Introduction

### Project Focus

This project is centered around analyzing power outages in the United States using a dataset that contains detailed records of outages, their causes, durations, and impacts across different states and regions. This dataset provides valuable insights into how different factors influence the duration of power outages, helping us understand what leads to prolonged service disruptions.

The dataset used in this project comes from Laboratory for Advancing Sustainable Critical Infrastructure from Purdue University, containing records of major power outages in the continental U.S. between January 2000 and July 2016. The dataset captures detailed information about outage causes, durations, affected areas, and energy losses, providing a comprehensive look into the factors influencing power disruptions across different states.

My research question is: **"What are the major factors affecting the duration of power outages in the U.S.?"** Understanding the primary drivers of outage duration is crucial because of multiple reasons:

- Utilities can improve response strategies to restore power faster.
- Policymakers can allocate resources better to prevent extended disruptions.
- Citizens can be better informed about risks in different regions.

### Dataset

In the dataset there are 57 columns and 1570 rows, recording 1534 cases of power outage and 55 features for each case (excluding index/unit and head of the table). Those features includes the time, location, cause and duration of power outage, also the number of people it influences and the demography of the region experienced power outage. Some key features include:

- ```OUTAGE.DURATION``` : The length of the power outage (target variable, measured in minutes.
- ```CAUSE.CATEGORY.DETAIL``` :Specific cause of the outage (e.g., storms, equipment failure, human error).
- ```MONTH```: The month in which the outage occurred.
- ```CLIMATE.REGION```: The geographical climate classification of the affected area.
- ```CUSTOMERS.AFFECTED```: The number of customers impacted by the outage.
- ```POPULATION```: Population of the affected region, which may influence restoration efforts.
- ```TOTAL.PRICE```:The total electricity price per unit.
- ```DEMAND.LOSS.MW```: The amount of power lost during the outage
  
## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

To ensure the dataset is ready for analysis, I performed the following cleaning steps:

Remove unwanted rolls and columns(head of table, redundant index, unit of features) and set features as column names:

- The first four rows contained metadata and headers, which were not actual data.
- The variables column was not needed and was removed.
- This step ensures a clean tabular format for analysis.

For the ease of analysis, add a new column called ```season``` from ```MONTH```

- Instead of analyzing MONTH as a numerical value, categorizing by SEASON allows better trend analysis.
- This helps identify seasonal patterns in outage durations.

Here is the head of data
<iframe
  src="assets/head.html"
  width = "800"
  heighth = "400"
  frameborder="0"
></iframe>

### Exploratory Data Analysis

After cleaning the data, I performed exploratory data analysis to understand trends and relationships between key variables. Below are some of my insights:

**Power Outages by State**
The bar chart below shows the total number of power outages recorded in each U.S. state from 2000 to 2016.

<iframe
  src="assets/Outage_Count_by_State.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Key Insights:

- California has the highest number of outages, significantly more than any other state.
- Texas and Washington also experience a high frequency of outages.
- States like South Dakota, Montana, and North Dakota report very few outages.
  
To better understand the geographic spread of outages, I created an interactive choropleth map that visualizes the outage count by state.

<iframe
  src="assets/outage_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To further explore seasonal trends in power outages, I created the following visualizations:

**This bar chart illustrates the number of power outages in each season.**

<iframe
  src="assets/Outage_Count_by_Season.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- Summer experiences the highest number of outages, possibly due to extreme weather events like heatwaves and storms.
- Winter also has a significant number of outages, likely caused by snowstorms and ice accumulation.

**The following pivot table summarizes the number of outages per season for each climate category.**

<iframe
  src="assets/pivot_table.html"
  height = "300"
  width = "400"
  frameborder="0"
></iframe>

<a id="outage-count"></a>
**This grouped bar chart shows how outages vary by season across different climate regions (cold, normal, and warm).**

<iframe
  src="assets/pivot_table_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- In cold climates, winter has the highest outage count, which aligns with expectations due to snow and ice-related failures.
- In normal climates, summer has the highest outages, likely due to heatwaves and increased electricity demand.
- In warm climates, outages are distributed more evenly across all seasons.

## Assessment of Missingness

When exploring the data, I found that there are quite a lot missing values for ```OUTAGE.DURATION```. I believe that those missing values in the dataset are Not Missing at Random (NMAR). This is because the likelihood of missing values in this column may be directly related to the cause of the outage itself or the reporting practices of power companies.
For example:

- If an outage was not fully documented, the duration might not have been recorded.
- In some cases, utilities may choose not to disclose outage duration data, especially in situations where extended downtime could impact their reputation.

By further exploring the data, I found that when there is power outage because of certain ```CAUSE.CATEGORY```, the proportion of missingness is greater for ```OUTAGE.DURATION```, as shownn in following plot.

<iframe
  src="assets/miss_proportion.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Hypothesis Test

To analyze whether this is caused by coincidence and whether the missing values in ```OUTAGE.DURATION``` depend on ```CAUSE.CATEGORY```, I performed a permutation test with:

- **Null Hypothesis**: Missingness in ```OUTAGE.DURATION``` is independent of ```CAUSE.CATEGORY```
- **Alternative Hypothesis**: Certain causes are more likely to have missing duration values.

For the test statistic, I choosed **total variance difference** between the missing proportions per cause category and the overall missing rate and use the significance level of 0.05. The result of permutation test is as following:
<iframe
  src="assets/perm_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Observed Test Statistic: 0.3272

P-value: 0.0000

Since the P-value is below 0.05, I reject the null hypothesis and take the alternative hypothesis, which is ```OUTAGE.DURATION``` is depedent on ```CAUSE.CATEGORY```.

## Hypothesis Testing

As shown in [exploratory data analysis section](#outage-count), the number of power outages is different in different season and in the same season in different climate region, this implies the climate region have a strong impact on the happening of power outage, and very likely the duration of power outage. To further understand the existance of potential relationship between ```CLIMATE.REGION``` and ```OUTAGE.DURATION```, I perform a hypothesis test with:

- **Null Hypothesis**: The outage duration does not depend on climate region. Any differences observed in the data are due to random chance.
- **Alternative Hypothesis** The  outage duration does depend on climate region. Certain climate regions are associated with systematically longer or shorter outages.

Since the two features are categorical, I used **total variance difference** as test statistic and perform a permuation test with a significance level of 0.05.

Results:
<iframe
  src="assets/perm_test2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Observed TVD: 4561.5569

P-value: 0.0060

Since p < 0.05, we reject the null hypothesis, suggesting that outage duration is significantly influenced by climate region.

## Framing a Prediction Problem

My objective is to predict the duration of a power outage (```OUTAGE.DURATION```) based on factors that are known at the time the outage occurs. This is a regression problem since the target variable is continuous, representing the duration in minutes.

Understanding which factors influence the duration of an outage can help utility companies optimize response efforts, reduce restoration time, and improve infrastructure resilience.

I expanded my feature selection to include both categorical and numerical predictors:

**Categorical Features**:

- ```CAUSE.CATEGORY.DETAIL```: The specific cause of the outage (e.g., storms, equipment failure, human error). This is critical as different causes may lead to varying restoration times.
- ```SEASON```: The season in which the outage occurred (Winter, Spring, Summer, Fall), as weather conditions may impact restoration efforts.
- ```CLIMATE.REGION```: The climate classification of the affected area, which could influence outage duration.

**Numerical Features**:

- ```CUSTOMERS.AFFECTED```: The number of customers impacted, as larger outages may take longer to restore.
- ```DEMAND.LOSS.MW```: The amount of power lost during the outage, which could correlate with the complexity of restoration efforts.
- ```POPULATION```: The population of the affected region, as densely populated areas may have better infrastructure and faster response times.

The evaluation matrix I used is root mean squared error (RMSE). RMSE penalizes large prediction errors more than MAE, making it a better choice when large mispredictions need to be minimized. Outage durations can vary significantly, and RMSE accounts for these variations by giving higher weight to larger errors. RMSE is a standard metric for regression problems and helps measure how well the model generalizes.

The selected features are available at the time of the outage and do not require post-outage data. Features such as recovery times, response efforts, and post-outage conditions are excluded to ensure the model is predictive rather than reactive.

By using a mix of categorical and numerical features, this model aims to provide a more accurate prediction of power outage durations while maintaining real-world applicability.

## Baseline Model

### Hyperparameters

I trained a baseline model using a Random Forest Regressor with the following features: ```SEASON``` and ```CLIMATE.CATEGORY```, two categorical nomial data. The target variable is ```OUTAGE.DURATION```.
Those categorical nomial variables were one-hot encoded. A Randome Forest Regressor was trained using GridSearchCV for hyperparameter tuning. RMSE was used as the primary evaluation metric.

### Result 
After the regression, the best hyperparameters and the model performance is as following:

``` text
max_depth: None
min_samples_leaf: 1
min_samples_split: 2
n_estimators: 200

Best Cross-Validated RMSE: 28956746.9688
Train RMSE: 5321.8626
Test RMSE: 7790.0323
```

<iframe
  src="assets/base_predicted.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The performance of the model is horrible but expected. The best cross-validation RMSE of 28956746.9688 is very high. This means that with only these two parameters, it is impossible to predict a accurate and precise duration of power outage. This makes sense since the power outage is due to multiple factors, whi...ch are not fully captured by just season and climate category. These two variables alone provide limited information about the severity of the outage, the cause, and the response efforts, all of which significantly impact the duration of a power outage.

Given the complexity of power outages, it is expected that additional factors—such as the cause of the outage, the number of customers affected, demand loss, and regional population density—would improve the model’s performance. This is why, in the final model, I introduced more meaningful features that better explain variations in outage duration.

Also, the train RSME is smaller than the test RSME, indicating overfitting. In the final model, I aim to reduce the test RMSE significantly while ensuring the training RMSE does not indicate excessive overfitting. A better generalization would mean the model is actually learning meaningful relationships rather than memorizing noise in the training data.

## Final Model

After the baseline model, I trained a final model incorporating additional features and better preprocessing techniques. The selected features include:

### Features Selection

- ```CAUSE.CATEGORY.DETAIL```: Categorical, Ordinal
Different causes (e.g., storms vs. equipment failure) lead to varying outage durations.
- ```SEASON```: Categorical, Ordinal
Seasonal weather conditions influence restoration times (e.g., winter storms vs. summer heat).
- ```CLIMATE.REGION```: Categorical Ordinal.
Certain regions (e.g., hurricane-prone areas) may experience longer outages.
- ```CUSTOMERS.AFFECTED```: Quantitative
Larger outages often require more time to restore.
- ```DEMAND.LOSS.MW```: Quantitative.
High-demand outages may be prioritized differently.
- ```POPULATION```: Quantitative.
Urban areas may recover faster due to better infrastructure.

These features align with the data-generating process of power outages, improving model performance by capturing real-world outage patterns.

### Hyperparameters

I chose Random Forest Regression for predicting outage duration. This model is robust to non-linearity, handles categorical and numerical features well, and performs well with missing or imbalanced data.

I used GridSearchCV with 10-fold cross-validation to systematically explore the best combination of hyperparameters. This approach tested multiple values for each hyperparameter and selected the combination that minimized the negative mean squared error (MSE).

For the hyperparameters, I first started with very broad ranges to get a rough estimate of good hyperparameter values. This allowed me to identify general trends without getting stuck in a narrow range too early.

After obtaining initial results, I refined the search space by focusing on promising values.

### Result

After the regression, the best hyperparameters and the model performance is as following:

``` text
max_depth: None
min_samples_leaf: 2
min_samples_split: 5
n_estimators: 100

Best Cross-Validated RMSE: 3888.6256
Train RMSE: 2473.6263
Test RMSE: 3586.3817
```

<iframe
  src="assets/predicted.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The Final Model significantly improves upon the Baseline Model in terms of predictive accuracy and generalization.Below is a comparison of key metrics:

**Best Cross-Validation RMSE**： 
- **Baseline Model**: 29,019,658.1
- **Final Model**: 3,888.63
- **Improvement**: Drastically reduced error
  
**Train RMSE**
- **Baseline Model**: 5,321.30 
- **Final Model**: 2,473.63 
- **Improvement**: Better fit to training data

**Test RMSE**
- **Baseline Model**: 7,798.45    
- **Final Model**: 3,586.38 
- **Improvement**: Improved generalizatio

The Best Cross-Validation RMSE decreased dramatically from 29,019,658.1 in the Baseline Model to 3,888.63 in the Final Model, indicating a substantial reduction in prediction error. Additionally, the Train RMSE improved from 5,321.30 to 2,473.63, while the Test RMSE decreased from 7,798.45 to 3,586.38. This reduction in the gap between training and test errors suggests that the Final Model generalizes better and is less prone to overfitting compared to the Baseline Model.

These improvements can be attributed to better feature selection, hyperparameter tuning, and preprocessing. The Final Model incorporates more relevant variables, such as ```CAUSE.CATEGORY.DETAIL```, ```CUSTOMERS.AFFECTED```, and ```DEMAND.LOSS.MW```, which provide a clearer picture of factors influencing outage duration. Additionally, GridSearchCV helped optimize key hyperparameters, leading to a more balanced model.

Overall, the Final Model is more accurate and generalizes better, making it a more reliable predictor of outage duration.

While the Final Model performs significantly better than the Baseline Model, there are several areas for further enhancement. One major improvement would be expanding feature engineering by incorporating external weather data, such as storm severity and temperature, which could provide a clearer picture of prolonged outages caused by extreme conditions. Additionally, integrating infrastructure-related factors, such as grid resilience and power company response times, could refine predictions. Another potential improvement is utilizing historical outage data to capture regional outage patterns over time.

To further reduce overfitting, adjusting hyperparameters such as increasing ```min_samples_leaf``` and ```min_samples_split``` could help the model generalize better by preventing it from learning noise. Additionally, limiting ```max_depth``` may simplify tree structures, reducing unnecessary complexity. Regularization techniques, such as tree pruning or Ridge regression for linear alternatives, could also be explored.

Lastly, improving data quality and collection is essential. Handling missing values with advanced imputation methods, such as KNN imputation, instead of simply dropping them could provide more stability in the predictions. Expanding the dataset to include more recent power outage data would also enhance the model’s relevance and accuracy for future predictions. With these improvements, the model could achieve greater accuracy, better generalization, and increased reliability in predicting power outage durations.

## Fairness Analysis

To assess whether my model is fair, I performed a fairness analysis by comparing its performance between low-income and high-income regions. I used Root Mean Squared Error (RMSE) as the evaluation metric to measure how well the model predicts outage durations across different income groups.

**Null Hypothesis**: My model is fair, meaning that the RMSE for low-income and high-income areas is roughly the same, and any differences are due to random chance.
**Alternative Hypothesis** (H₁): My model is unfair, meaning that the RMSE for low-income areas is significantly higher than for high-income areas, indicating worse performance for lower-income regions.

### Results

```
High-Income RMSE: 2658.2707
Low-Income RMSE: 4332.8719
Observed RMSE Difference (Low - High): 1674.6013
P-value: 0.0620
```

<iframe
  src="assets/perm_test3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed RMSE difference was 1624.64 (low-income RMSE - high-income RMSE). The p-value obtained from the permutation test was 0.0938, which is greater than 0.05.

Since the p-value is not statistically significant, I fail to reject the null hypothesis, meaning there is no strong evidence that my model is biased against low-income regions.

My model does not exhibit significant disparities in predictive accuracy across income groups. However, fairness is a complex issue, and further analysis (e.g., considering intersectional factors such as population density and outage cause) may provide deeper insights into potential biases.

<iframe
  src="assets/actual_predicted.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
