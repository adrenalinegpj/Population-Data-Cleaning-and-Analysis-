# Population Data Cleaning Project

## Project Overview

This project focuses on cleaning and preparing population data for analysis and visualization.

The data cleaning process was performed using Python with Pandas and NumPy. The cleaned dataset was then exported to an Excel file for further analysis and visualization.

## Project Files

* `populationdatacleaning.ipynb`
  Jupyter Notebook containing the complete data cleaning process.

* `population_cleaned_ds.xlsx`
  Excel file containing population data used for the project.

* `campdatapbidb.pbix`
  Power BI project file for data visualization and reporting.

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Jupyter Notebook
* Microsoft Excel
* Power BI

## Data Cleaning Process

The notebook performs the following data preparation steps:

### 1. Import Libraries

The project uses Pandas and NumPy for data manipulation, along with Matplotlib and Seaborn for visualization.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### 2. Load the Dataset

The population dataset is loaded from an Excel file using Pandas.

```python
Population_Clean = pd.read_excel('dataset.xlsx')
```

### 3. Data Type Conversion

The `Date` column is converted to datetime format.

Text-based columns are converted to string format, while population and housing-related columns are converted to nullable integer types.

```python
Population_Clean["Date"] = pd.to_datetime(Population_Clean["Date"])

Population_Clean[["Country", "City", "PlaceName"]] = \
    Population_Clean[["Country", "City", "PlaceName"]].astype(str)

Population_Clean[["Members", "Male", "Female", "Room", "Flat", "House", "Tent"]] = \
    Population_Clean[["Members", "Male", "Female", "Room", "Flat", "House", "Tent"]].astype("Int64")
```

### 4. Remove Unnecessary Spaces

Leading and trailing spaces are removed from the main text columns.

```python
Population_Clean["Country"] = Population_Clean["Country"].str.strip()
Population_Clean["City"] = Population_Clean["City"].str.strip()
Population_Clean["PlaceName"] = Population_Clean["PlaceName"].str.strip()
```

### 5. Handle Missing Housing Values

Missing values in housing-related columns are replaced with zero.

```python
housing_cols = ['Room', 'Flat', 'House', 'Tent']

Population_Clean[housing_cols] = Population_Clean[housing_cols].fillna(0)
```

### 6. Remove Empty Rows and Duplicates

Rows containing no data are removed, followed by removal of duplicate records.

```python
Population_Clean = Population_Clean.dropna(how='all')

Population_Clean = Population_Clean.drop_duplicates()
```

### 7. Gender Data Validation

A validation column is created to check whether the number of members is equal to the sum of male and female members.

```python
Population_Clean["Gender_Validation"] = "Review"

Population_Clean.loc[
    Population_Clean["Members"] == Population_Clean["Male"] + Population_Clean["Female"],
    "Gender_Validation"
] = "OK"
```

Records where the values match are marked as `OK`, while records requiring further checking are marked as `Review`.

### 8. Extract Year and Month

The year and month are extracted from the `Date` column to support time-based analysis.

```python
Population_Clean["Date"] = pd.to_datetime(Population_Clean["Date"])

Population_Clean["Year"] = Population_Clean["Date"].dt.year
Population_Clean["Month"] = Population_Clean["Date"].dt.month
```

### 9. Export the Cleaned Dataset

The processed dataset is exported to an Excel file.

```python
Population_Clean.to_excel("population_cleaned.xlsx", index=False)
```

## Data Quality Checks

The cleaning workflow addresses several common data quality issues:

* Incorrect or inconsistent data types
* Leading and trailing spaces
* Missing housing values
* Completely empty rows
* Duplicate records
* Gender/member consistency
* Date formatting
* Additional time-based fields

## Output

The final cleaned dataset is saved as:

```text
population_cleaned.xlsx
```

The cleaned data can then be used for further analysis and visualization in tools such as Excel and Power BI.

## Project Workflow

```text
Raw Population Data
        |
        v
Load Data with Pandas
        |
        v
Convert Data Types
        |
        v
Clean Text Fields
        |
        v
Handle Missing Values
        |
        v
Remove Empty Rows
        |
        v
Remove Duplicates
        |
        v
Validate Gender Data
        |
        v
Extract Year and Month
        |
        v
Export Cleaned Dataset
        |
        v
Excel / Power BI Analysis
```

## Conclusion

The project demonstrates a complete basic data-cleaning workflow using Python. The resulting dataset is structured and prepared for analysis and visualization, with additional validation and time-based fields to support future data analysis.
