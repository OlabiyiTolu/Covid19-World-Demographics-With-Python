# Covid_World_Demographics Analysis

![GitHub](https://img.shields.io/badge/Language-Python-blue) 
![GitHub](https://img.shields.io/badge/Library-Pandas-green) 
![GitHub](https://img.shields.io/badge/Library-Seaborn-orange) 
![GitHub](https://img.shields.io/badge/Library-Plotly-red)

This repository contains an in-depth analysis of COVID-19 demographic data, focusing on the geographical distribution of confirmed cases, deaths, and recoveries. The project leverages Python libraries such as Pandas, Matplotlib, Seaborn, and Plotly to explore, visualize, and interpret the data.

## Table of Contents

1. [Introduction](#introduction)
2. [Libraries and Data Import](#libraries-and-data-import)
3. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
4. [Handling Missing Values](#handling-missing-values)
5. [Data Visualization](#data-visualization)
   - [Countplot Visualization](#countplot-visualization)
   - [Geographical Visualization](#geographical-visualization)
6. [Conclusion](#conclusion)
7. [Installation](#installation)
8. [Usage](#usage)
9. [Contributing](#contributing)
10. [License](#license)

## Introduction

The COVID-19 pandemic has had a profound impact on global health and economies. This project aims to analyze the demographic distribution of COVID-19 cases, deaths, and recoveries across different countries and regions. By visualizing the data, we can gain insights into the spread and impact of the virus.

## Libraries and Data Import

The project utilizes several Python libraries for data processing and visualization:

```python
import pandas as pd  # Data processing, CSV file I/O (e.g., pd.read_csv)
import numpy as np  # Linear algebra
import matplotlib.pyplot as plt  # Data visualization
import seaborn as sns  # Data visualization
import plotly.graph_objects as go  # Interactive geographical visualization
```

The dataset used in this analysis is `COVID-19_geo_timeseries_ver_0311.csv`, which contains information on COVID-19 cases, including geographical coordinates, confirmed cases, deaths, and recoveries.

```python
# Import the dataset
covid_lat_long = pd.read_csv('COVID-19_geo_timeseries_ver_0311.csv')
```

## Exploratory Data Analysis (EDA)

The initial exploration of the dataset involves understanding its structure, checking for missing values, and summarizing the data.

```python
# Exploring the first five rows of the dataset
covid_lat_long.head()

# Exploring the last five rows of the dataset
covid_lat_long.tail()

# Checking the info of the data
covid_lat_long.info()

# Summary statistics of the dataset
covid_lat_long.describe(include='all').transpose()

# Counting the number of unique values in each column
covid_lat_long.nunique()
```

## Handling Missing Values

Missing values in the dataset are handled by filling them with zero, ensuring that the analysis is not skewed by incomplete data.

```python
# Filling all missing values with zero(0)
covid_lat_long_filled = covid_lat_long.fillna(0)

# Verifying that there are no missing values left
covid_lat_long_filled.isnull().sum()
```

## Data Visualization

### Countplot Visualization

A countplot is used to visualize the distribution of COVID-19 cases across different regions.

```python
def countplot(x):
    plt.rcParams['figure.figsize'] = (17, 10)
    sns.countplot(data=covid_lat_long, x=x)
    plt.xticks(rotation=90)
    return

# Plotting the countplot for regions
countplot(covid_lat_long['region'])
```

### Geographical Visualization

Geographical scatter plots and choropleth maps are used to visualize the spread of COVID-19 cases, deaths, and recoveries across the globe.

```python
def covid(hue=None):
    plt.rcParams['figure.figsize'] = (17, 10)
    sns.scatterplot(data=covid_lat_long, x='longitude', y='latitude', hue=hue, size=hue, sizes=(10, 100))
    return

# Scatter plot of all the locations (lat-long)
covid()

# Grouping the data by country and summing the cases, deaths, and recoveries
covid_lat_long_geo = covid_lat_long.groupby(['country', 'country_code']).sum()
covid_lat_long_geo_reset = covid_lat_long_geo.reset_index()

# Visualizing the geographical distribution of confirmed cases
fig = go.Figure(data=go.Choropleth(
    locations=covid_lat_long_geo_reset['country_code'],
    z=covid_lat_long_geo_reset['confirmed_cases'],
    text=covid_lat_long_geo_reset['country'],
    colorscale='Blues',
    autocolorscale=True,
    reversescale=False,
    marker_line_color='darkgray',
    marker_line_width=0.5,
    colorbar_tickprefix='',
    colorbar_title='Covid_19<br>confirmed_cases count',
))

fig.update_layout(
    title_text='Covid_19 Lat_Long_demograph',
    geo=dict(
        showframe=False,
        showcoastlines=False,
        projection_type='equirectangular'
    )
)
fig.show()
```

## Conclusion

This project provides a comprehensive analysis of COVID-19 demographic data, highlighting the geographical distribution of cases, deaths, and recoveries. The visualizations offer valuable insights into the impact of the pandemic across different regions.

## Installation

To run this project locally, you need to have Python installed along with the required libraries. You can install the necessary libraries using pip:

```bash
pip install pandas numpy matplotlib seaborn plotly
```

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/Covid_World_Demographics.git
   ```
2. Navigate to the project directory:
   ```bash
   cd Covid_World_Demographics
   ```
3. Run the Jupyter notebook or Python script:
   ```bash
   jupyter notebook
   ```
   or
   ```bash
   python covid_analysis.py
   ```

## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

For any questions or feedback, please open an issue or contact the repository owner.
