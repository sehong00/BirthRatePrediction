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

📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/merged_data.xlsx" >merged_data.xlsx </a>

### Data Preprocessing - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/data_merge.ipynb" > Code </a>
- Initial Analysis(calculate the percentage of missing values for each feature)
- Handling Missing Values

Removing Columns with more than 20% Missing Values
```ruby
columns_to_drop = data.columns[data.isnull().mean() > 0.2]
data = data.drop(columns_to_drop, axis=1)
data.info()
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
    * Labor Market Stability = ‘EmploymentRate’ / ‘UnemploymentRate’
    * Hosing Affordability Index = ‘HousingPrice’ / ‘PerCapitaGDP’
- Deletion of Unnecessary Features
    * PerCapitaGDP = ‘GDP’ / ‘Population’
    * Population
    * DivorceRate

📁<a href="https://github.com/oosedus/BirthratePrediction/tree/main/Data/after_extraction_data.xlsx" >after_extraction_data.xlsx </a>

### Correlation Analysis - <a href="https://github.com/oosedus/BirthratePrediction/blob/main/Code/Correlation_Analysis.ipynb" > Code </a>
- Correlation Analysis using heatmap
- Excludes specific features for creating 4 new datasets
- Correlation Analysis on Modified Datasets
- Feature Selection utilizing RFECV using a RandomForestRegressor

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







