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

#### Data Merge
- Collecting data 
- Convert collected data to the same format
- Merge data based on Country ID

-> merged_data.xlsx

#### Data Preprocessing
- Initial Analysis(calculate the percentage of missing values for each feature)
- Handling Missing Values(more than 20% missing values are dropped)
- Creating Country-Specific Datasets
- Automated Handling of Missing Values for Each Country
- Merging Country-Specific Datasets and Visualization
Before 
![before](/Users/jinsoo/Desktop/screensh1.png)
After
![after](/Users/jinsoo/Desktop/screensh2.png)

->real_merged_data.xlsx

#### Feature Extraction
- Create new features from existing features
    * Work Leisure Balance Index
    * Labor Market Stability
    * Hosing Affordability Index
- Deletion of Unnecessary Features
    * PerCapitaGDP
    * Population
    * DivorceRate
    
#### Correlation Analysis
- 
- 
- 
- 
- 
#### RFECV
- 
- 
### Final Data Analysis
- 

### Compare performance

-

### Results
-







