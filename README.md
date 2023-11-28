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

### Data Merge
- Collecting data 
- Convert collected data to the same format
- Merge data based on Country ID  
```ruby
data.describe()
```
5 Examples  
|        | FertilityRate | AvgHoursWorked | BothWorking | FirstBirthAge | MarriageAge |
|--------|---------------|----------------|-------------|---------------|-------------|
| conunt |    480.000000 |     453.000000 |   191.00000 |    455.000000 |  174.000000 |
|   mean |      1.466667 |    1714.458318 |    53.27801 |     29.860879 |   28.442529 |
|    std |      0.199506 |     165.567786	|     8.97173 |      1.456630 |    2.434869 |
|    min |      0.810000 |    1319.131000 |    33.90000 |     25.600000 |   22.200000 |
|    25% |      1.330000 |    1592.659000	|    46.70000 |     29.000000 |   26.600000 |
|    50% |      1.420000 |    1716.341000 |    52.00000 |     29.900000 |   28.800000 |
|    75% |      1.570000 |    1811.852000 |    61.90000 |     30.900000 |   30.475000 |
|    max |      2.020000 |    2228.000000 |    71.00000 |     33.400000 |   32.900000 |  

```ruby
data.info()
```
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 480 entries, 0 to 479  
Data columns (total 28 columns):  
| #   | Column                        | Non-Null Count | Dtype    |
| --- | ----------------------------- | -------------- | -------- |
| 0   | ID                            | 480            | object   |
| 1   | Year                          | 480            | int64    |
| 2   | Country Name                  | 480            | object   |
| 3   | FemaleLaborParticipationRate  | 480            | float64  |
| 4   | AvgHoursWorked                | 453            | float64  |
| 5   | BothWorking                   | 191            | float64  |
| 6   | FirstBirthAge                 | 455            | float64  |
| 7   | MarriageAge                   | 174            | float64  |
| 8   | MarriageRate                  | 406            | float64  |
| 9   | EmploymentRate                | 465            | float64  |
| 10  | UnemploymentRate              | 434            | float64  |
| 11  | HousingPrice                  | 414            | float64  |
| 12  | InterestRate                  | 432            | float64  |
| 13  | PartTimeRate                  | 462            | float64  |
| 14  | FamilyExpenditure             | 445            | float64  |
| 15  | HealthExpenditure             | 318            | float64  |
| 16  | LaborMarketExpenditure        | 415            | float64  |
| 17  | UnemploymentExpenditure       | 446            | float64  |
| 18  | GDI                           | 470            | float64  |
| 19  | GDP                           | 479            | float64  |
| 20  | GNI                           | 455            | float64  |
| 21  | PovertyGap                    | 308            | float64  |
| 22  | EduExpenditureOfGDP           | 415            | float64  |
| 23  | EduExpenditureOfGov           | 370            | float64  |
| 24  | TotalLaborParticipationRate   | 480            | float64  |
| 25  | InflationRate                 | 480            | float64  |
| 26  | Population                    | 480            | float64  |
| 27  | FertilityRate                 | 480            | float64  |  
  
dtypes: float64(25), int64(1), object(2)  
memory usage: 105.1+ KB  
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/merged_data.xlsx" >merged_data.xlsx </a>

### Data Preprocessing - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/data_preprocessing(handle_missing_value).ipynb" > Code </a>
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
![image](https://github.com/oosedus/BirthratePrediction/assets/52899383/fca2dfcb-c754-40aa-948e-0bbfab01b552)
- Excludes specific features for creating 4 new datasets
- Correlation Analysis on Modified Datasets
- Feature Selection utilizing Recursive Feature Elimination with Cross-Validation (RFECV) using a RandomForestRegressor

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
Result:  

| Feature                     | RandomForest | XGBRegressor | DecisionTree |
| --------------------------- | ------------- | ------------- | ------------ |
| FemaleLaborParticipationRate| 1             | 1             | 1            |
| WorkLeisureBalanceIndex     | 1             | 4             | 8            |
| TotalLaborParticipationRate | 1             | 1             | 4            |
| EduExpenditureOfGDP         | 1             | 1             | 1            |
| GDP                         | 1             | 1             | 1            |
| GDI                         | 1             | 1             | 1            |
| UnemploymentExpenditure     | 1             | 1             | 1            |
| LaborMarketStability        | 1             | 3             | 6            |
| FamilyExpenditure           | 1             | 1             | 1            |
| InterestRate                | 1             | 7             | 2            |
| HousingPrice                | 1             | 1             | 1            |
| UnemploymentRate            | 1             | 1             | 1            |
| EmploymentRate              | 1             | 1             | 1            |
| FirstBirthAge               | 1             | 1             | 1            |
| AvgHoursWorked              | 1             | 1             | 1            |
| HousingAffordabilityIndex   | 1             | 1             | 5            |
| MarriageRate                | 2             | 2             | 9            |
| InflationRate               | 3             | 8             | 1            |
| GNI                         | 4             | 6             | 7            |
| PartTimeRate                | 5             | 5             | 3            |



📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/data_rf.xlsx" >data_rf.xlsx </a>   
📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/data_xgb.xlsx" >data_xgb.xlsx </a>



### Final Data Analysis - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/Model.ipynb" > Code </a>

#### Train/Valid/Test Dataset
- Train : 1990-2025
- Valid : 2016-2028
- Test : 2019-2021

#### Model Training
1. RandomForestRegressor
- a machine learning model that uses an ensemble of decision trees to make predictions, particularly useful for regression tasks. It's known for its high accuracy and ability to handle large datasets with numerous input variables.

- Hyperparameter
    - 'n_estimators': The number of trees in the forest
    - 'max_depth': The maximum depth of each tree
    - 'min_samples_split': The minimum number of samples required to split an internal node
    - 'min_samples_leaf': The minimum number of samples required to be at a leaf node

- Best Hyperparameter
    - Check all combinations to determine best hyperparameters based on MSE
    - 'n_estimators': 200
    - 'max_depth': 10
    - 'min_samples_split': 5
    - 'min_samples_leaf': 2

- Performance
    - MSE : 0.033935
    - R2 : 0.145540
    - MAE : 0.132456

2. XGBoostRegressor
- This model uses a gradient boosting method that combines multiple decision trees to make predictions, and is known for its high performance and fast processing speed. XGBoostRegressor provides high accuracy even in complex datasets, and can optimize model performance by adjusting various hyperparameters.

- Hyperparameter
    - 'n_estimators': The number of boosting stages to be run
    - 'max_depth': The maximum depth of a tree
    - 'learning_rate': The step size shrinkage used in updating tree weights

- Best Hyperparameter
    - 'n_estimators': 200
    - 'max_depth': 3
    - 'learning_rate': 0.2

- Performance
    - MSE : 0.012513
    - R2 : 0.684921
    - MAE : 0.087852

3. DecisionTreeRegressor
- a machine learning model used for regression tasks, which predicts continuous output values. It builds a tree-like model of decisions, where each node represents a feature, and branches represent decision rules, ultimately leading to a prediction at the leaves. This model is known for its simplicity, interpretability, and ability to capture non-linear relationships in data.

- Hyperparameter
    - 'max_depth': The maximum depth of each tree
    - 'min_samples_split': The minimum number of samples required to split an internal node
    - 'min_samples_leaf': The minimum number of samples required to be at a leaf node

- Best Hyperparameter
    - 'max_depth': 10
    - 'min_samples_split': 10
    - 'min_samples_leaf': 4

#### Model Selection
- **XGBoostRegressor** : Based on the results, the XGBoostRegressor is the best choice among the three models. It achieves the lowest mean squared error (MSE) on the test set (0.012513) and the highest R² score (0.684921).


### Compare performance

#### RandomForest Regressor
![RandomForest](https://github.com/oosedus/BirthratePrediction/assets/75584814/abd6e5b8-c901-463d-a1fd-7911ddc940d1)

#### XGBoost Regressor
![XGBoost](https://github.com/oosedus/BirthratePrediction/assets/75584814/516abb96-0012-4d2e-a739-b8b78d304761)

#### DecisionTree Regressor
![DecisionTree](https://github.com/oosedus/BirthratePrediction/assets/75584814/478d6577-8e12-4eab-a856-d8bcf73b3981)

### Results

#### SHAP(XGBoost Regressor)
- Why we used "SHAP" 
    - We used SHAP to determine which characteristics are most influential on fertility. By calculating the impact of each trait on the forecast, we can understand the importance of the traits and use this to inform policy discussions around traits.

    - SHAP (SHapley Additive exPlanations) is a tool that helps explain model predictions and understand how much each trait contributes to the prediction. 
SHAP values evaluate the contribution of any combination of traits, so it can be computationally expensive for large datasets or complex models, but we felt that we were able to account for relatively all of them, and it helped us understand the importance of traits.

![SHAP1](https://github.com/oosedus/BirthratePrediction/assets/75584814/cdcd2bd9-253f-4255-a6ad-8db537f2b90c)

![SHAP2](https://github.com/oosedus/BirthratePrediction/assets/75584814/824ab16f-938c-46a0-ab1f-1ef86b622806)

The results are not surprising, but they are generally understood. The most influential is the age at first birth, with higher ages having a negative effect on fertility. The data below can be interpreted in a similar way to the age at first birth.

### Limitations

The time-based interpolation method was used to fill in the missing values of the data taken from OECD data, but the accuracy of the data and the reliability of the predicted values are poor. To secure this, it seems that a more accurate data set should be obtained during data collection and a more accurate interpolation method should be used during data preprocessing. 
In addition, there are too many factors that contribute to Korea's declining fertility rate.
It is doubtful that the specific characteristics of the 15-country dataset can be applied to Korea and lead to a positive change in the fertility rate.








