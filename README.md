# BirthratePrediction



## 🖥️ Project Introduction

The purpose of creating a birth rate prediction model is to analyze and identify the factors that most significantly influence birth rates.

Proposing Solutions from a Socio-Economic Standpoint Based on the Identified Causes.
<br>

### ⚙️ Environment Setting
- Python 3
- Interpreter : Jupyter Notebook

### 🧑‍🤝‍🧑 Members
 - Team member 1 : 19102085 Park Sehong
 - Team member 2 : 19102088 Park Jinsoo 
 - Team member 3 : 21102044 Oh Seyeon

## 📌 Process
### Sources - <a href="https://github.com/oosedus/BirthratePrediction/wiki/Sources" > links </a>
- Period : 1990 ~ 2021
- 15 countries(OECD)

| Country Code | Country Name |
|--------------|--------------|
| AUT          | Austria      |
| CAN          | Canada       |
| CHE          | Switzerland  |
| DEU          | Germany      |
| ESP          | Spain        |
| FIN          | Finland      |
| FRA          | France       |
| GRC          | Greece       |
| HUN          | Hungary      |
| ITA          | Italy        |
| JPN          | Japan        |
| KOR          | Korea        |
| LUX          | Luxembourg   |
| POL          | Poland       |
| PRT          | Portugal     |

## Libraries Used
- pandas: Utilized for data manipulation and analysis.
- numpy: Used for numerical operations and calculations.
- matplotlib: Employed for creating visualizations, including scatter plots.
- scikit-learn (sklearn): Applied for machine learning tasks, such as model training, evaluation, and hyperparameter tuning.
- xgboost: Integrated for implementing XGBoost regression models.
- eli5: Used for permutation importance analysis.
- shap: Employed for interpreting SHAP values and creating summary plots.

### Data Merge - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/data_merge.ipynb" > Code </a>
- Collecting data 
- Convert collected data to the same format
- Merge data based on Country ID  

```ruby
data.info()
```
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 480 entries, 0 to 479  
Data columns (total 30 columns):  
| #  | Column                       | Non-Null Count |  Dtype  |
|----|------------------------------|----------------|---------|
| 0  | ID                           | 480 non-null   | Object  |
| 1  | Year                         | 480 non-null   | int64   |
| 2  | Country Name                 | 480 non-null   | Object  |
| 3  | BirthRate                    | 480 non-null   | float64 |
| 4  | FemaleLaborParticipationRate | 480 non-null   | float64 |
| 5  | AvgHoursWorked               | 453 non-null   | float64 |
| 6  | BothWorking                  | 191 non-null   | float64 |
| 7  | FirstBirthAge                | 455 non-null   | float64 |
| 8  | MarriageAge                  | 174 non-null   | float64 |
| 9  | MarriageRate                 | 406 non-null   | float64 |
| 10 | EmploymentRate               | 465 non-null   | float64 |
| 11 | UnemploymentRate             | 434 non-null   | float64 |
| 12 | HousingPrice                 | 414 non-null   | float64 |
| 13 | InterestRate                 | 432 non-null   | float64 |
| 14 | PartTimeRate                 | 462 non-null   | float64 |
| 15 | FamilyExpenditure            | 445 non-null   | float64 |
| 16 | HealthExpenditure            | 318 non-null   | float64 |
| 17 | LaborMarketExpenditure       | 415 non-null   | float64 |
| 18 | UnemploymentExpenditure      | 446 non-null   | float64 |
| 19 | GDI                          | 470 non-null   | float64 |
| 20 | GDP                          | 479 non-null   | float64 |
| 21 | GNI                          | 455 non-null   | float64 |
| 22 | PovertyGap                   | 308 non-null   | float64 |
| 23 | EduExpenditureOfGDP          | 415 non-null   | float64 |
| 24 | EduExpenditureOfGov          | 370 non-null   | float64 |
| 25 | WeeksPaidLeaveForMothers     | 480 non-null   | float64 |
| 26 | TotalLaborParticipationRate  | 480 non-null   | float64 |
| 27 | DivorceRate                  | 441 non-null   | float64 |
| 28 | InflationRate                | 480 non-null   | float64 |
| 28 | Population                   | 480 non-null   | float64 |  

dtypes: float64(27), int64(1), object(2)  
memory usage: 112.6+ KB  
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/merged_data.xlsx" >merged_data.xlsx </a>

### Data Preprocessing - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/data_merge.ipynb" > Code </a>
- Initial Analysis(calculate the percentage of missing values for each feature)
- Handling Missing Values

Removing Columns with more than 20% Missing Values
```ruby
columns_to_drop = data.columns[data.isnull().mean() > 0.2]
data = data.drop(columns_to_drop, axis=1)
```
Method: Dropped columns with more than 20% missing values.  
Reason: Columns with a high percentage of missing values may not provide meaningful insights.

Imputing Missing Values by Country
```ruby
# Interpolated missing values for each column based on time, considering the direction of NaNs
for dataset in country_data_list:
    dataset['Year'] = pd.to_datetime(dataset['Year'], format='%Y')
    dataset.set_index('Year', inplace=True)
    
    for column in dataset.columns:
        dataset[column] = interpolate_with_direction(dataset[column])
```
Method: Interpolated missing values based on time, considering the direction of NaNs for each column.  
Reason: Time-based interpolation is suitable for sequential data, and considering the direction of NaNs helps maintain the trend.

- Creating Country-Specific Datasets
- Automated Handling of Missing Values for Each Country
- Merging Country-Specific Datasets and Visualization

#### Before 
![before](https://user-images.githubusercontent.com/75584814/284490475-15f99b8e-f792-4a55-baf5-4eea333c7c06.png)
#### After
![after](https://user-images.githubusercontent.com/75584814/284490589-fd1ed4f0-8b1e-490c-8417-e0ec241044bb.png)

📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/real_merged_data.xlsx" >real_merged_data.xlsx </a>

### Feature Extraction - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/Feature%20Extraction.ipynb" > Code </a>
- Create new features from existing features
    * Work Leisure Balance Index = 'AvgHoursWorked' / 'TotalLaborParticipationRate'  
 It reflects the interplay between work intensity and leisure time in the labor market. A high index suggests a scenario with prolonged working hours and reduced participation, indicating an environment of intense work and limited leisure. This condition may contribute to lower birth rates. In contrast, a low index points to shorter working hours and higher participation, indicating a balanced environment with ample leisure time. Such conditions may foster a more favorable atmosphere for higher birth rates.
      
    * Labor Market Stability = ‘EmploymentRate’ / ‘UnemploymentRate’  
 It is served as a crucial indicator for evaluating the health and stability of the labor market. A higher ratio signifies a stable labor market characterized by high employment and low unemployment rates. Conversely, a lower ratio indicates an unstable labor market with lower employment and higher unemployment rates. 
      
    * Hosing Affordability Index = ‘HousingPrice’ / ‘PerCapitaGDP’  
  It offers comprehensive measure of the economic feasibility of homeownership relative to individual or household income levels. A lower index value suggests that purchasing a house is more affordable, potentially alleviating financial stress on families and encouraging higher birth rates. Conversely, a higher index value indicates that buying a house is financially burdensome in relation to personal income, possibly leading to increased financial strain and acting as a deterrent to family expansion.
      
- Deletion of Unnecessary Features
    * PerCapitaGDP = ‘GDP’ / ‘Population’
    * Population
    * DivorceRate

📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/after_extraction_data.xlsx" >after_extraction_data.xlsx </a>

### Correlation Analysis - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/Correlation_Analysis.ipynb" > Code </a>
- Correlation Analysis using heatmap
```ruby
plt.figure(figsize=(12, 10))
sns.heatmap(corr_matrix, annot=True, fmt=".2f", cmap='coolwarm',
            xticklabels=corr_matrix.columns,
            yticklabels=corr_matrix.columns)
plt.title('Correlation Matrix')
plt.xticks(rotation=45, ha='right')
plt.yticks(rotation=0)
plt.tight_layout()
plt.show()
```
Purpose: Examine pairwise feature correlations.  
Methodology: Heatmap using Seaborn.  
Result:  
![correlation matrix](https://github.com/oosedus/BirthratePrediction/assets/52899383/a97a4379-4fae-4a76-8ed0-e9943b56f6d2)
- Excludes specific features for creating 4 new datasets
- Correlation Analysis on Modified Datasets
- Feature Selection utilizing Recursive Feature Elimination with Cross-Validation (RFECV) using a RandomForestRegressor
```ruby
# Model selection
models = {
    'RandomForestRegressor': RandomForestRegressor(random_state=42),
    'XGBRegressor': XGBRegressor(random_state=42),
    'DecisionTreeRegressor': DecisionTreeRegressor(random_state=42)
}

# Feature ranking
feature_rankings = pd.DataFrame()

for model_name, model in models.items():
    rfecv = RFECV(estimator=model, step=1, cv=5, scoring='neg_mean_squared_error')
    rfecv.fit(X, y)
    
    # Create a ranking dataframe
    ranking_df = pd.DataFrame({
        'Feature': X.columns,
        f'Ranking_{model_name}': rfecv.ranking_
    })
    
    # Merge rankings
    if feature_rankings.empty:
        feature_rankings = ranking_df
    else:
        feature_rankings = feature_rankings.merge(ranking_df, on='Feature')

# Sort and display rankings
feature_rankings = feature_rankings.sort_values(by='Ranking_RandomForestRegressor')
feature_rankings
```
Purpose: Identify vital features for predicting FertilityRate.  
Methodology: RFECV applied using RandomForest, XGBoost, and DecisionTree models.

-> after_correlation_data(01,02,03,04).xlsx   
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/after_correlation_data01.xlsx" >after_correlation_data01.xlsx </a>   
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/after_correlation_data02.xlsx" >after_correlation_data02.xlsx </a>   
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/after_correlation_data03.xlsx" >after_correlation_data03.xlsx </a>   
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/after_correlation_data04.xlsx" >after_correlation_data04.xlsx </a>   
### Preliminary Experiment - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/Preliminary%20Experiment.ipynb" > Code </a>
- Modeling with XGBoost regression to predict 'BirthRate'
- Evaluates the model's performance

### Correlation Analysis_RFECV - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/Correlation_Analysis_RFECV.ipynb" > Code </a>
- Feature Selection using RandomForestRegressor and XGBRegressor
- Ranks features based on their importance
- Removes less important features identified by both RandomForestRegressor and XGBRegressor 


📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/data_rf.xlsx" >data_rf.xlsx </a>   
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/data_xgb.xlsx" >data_xgb.xlsx </a>
### Final Data Analysis
- 

### Compare performance

-

### Results
-







