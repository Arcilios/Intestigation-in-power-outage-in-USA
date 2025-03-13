# Intestigation In Power Outage In USA

## Introduction

### Project Focus

This project is centered around analyzing power outages in the United States using a dataset that contains detailed records of outages, their causes, durations, and impacts across different states and regions. This dataset provides valuable insights into how different factors influence the duration of power outages, helping us understand what leads to prolonged service disruptions.

The dataset used in this project comes from Laboratory for Advancing Sustainable Critical Infrastructure from Purdue University, containing records of major power outages in the continental U.S. between January 2000 and July 2016. The dataset captures detailed information about outage causes, durations, affected areas, and energy losses, providing a comprehensive look into the factors influencing power disruptions across different states.

My research question is: **"What is the most important factor affecting the duration of power outages in the U.S.?"** Understanding the primary drivers of outage duration is crucial because of multiple reasons:

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
  
## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
To ensure the dataset is ready for analysis, we performed the following cleaning steps:

1. Remove unwanted rolls and columns(head of table, redundant index, unit of features) and set features as column names:
   
```python
data = pd.read_excel("outage.xlsx")
data = data[4:] #The first four lines are head of the table. Removed
data.columns = data.iloc[0] #Setting the features as column names
data = data[2:].drop(columns=['variables']) #Remove redundant index and columns
```

- The first four rows contained metadata and headers, which were not actual data.
- The variables column was not needed and was removed.
- This step ensures a clean tabular format for analysis.

2. For the ease of analysis, add a new column called ```season``` from ```MONTH```

```python
def season_check(month):
    if month in [12, 1, 2]:
        return 'Winter'
    elif month in [3, 4, 5]:
        return 'Spring'
    elif month in [6, 7, 8]:
        return 'Summer'
    else:
        return 'Fall'
data['SEASON'] = data['MONTH'].transform(season_check)
```

- Instead of analyzing MONTH as a numerical value, categorizing by SEASON allows better trend analysis.
- This helps identify seasonal patterns in outage durations.

Here is the head of data
<iframe
  src="assets/head.html"
  width="800"
  height="600"
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

1. **This bar chart illustrates the number of power outages in each season.**

<iframe
  src="assets/Outage_Count_by_Season.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- Summer experiences the highest number of outages, possibly due to extreme weather events like heatwaves and storms.
- Winter also has a significant number of outages, likely caused by snowstorms and ice accumulation.

2. **The following pivot table summarizes the number of outages per season for each climate category.**

<iframe
  src="assets/pivot_table.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

3. **This grouped bar chart shows how outages vary by season across different climate regions (cold, normal, and warm).**
   
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

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

