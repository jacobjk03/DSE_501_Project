# Data Preprocessing and EDA - Documentation

## Overview
This document summarizes the data preprocessing and exploratory data analysis performed on the NYC Yellow Cab taxi dataset (January 2024) as part of the project management plan.

## File Created
**Notebook**: `data_preprocessing_eda.ipynb`

This comprehensive Jupyter notebook implements all data preprocessing and exploratory data analysis steps required for the project.

## Contents

### 1. Setup and Data Loading
- Import required libraries (pandas, numpy, matplotlib, seaborn)
- Configure visualization settings
- Load the parquet dataset
- Display basic dataset information

### 2. Data Quality Assessment
- **Missing Values Analysis**: Identifies columns with missing data and calculates percentages
- **Duplicate Detection**: Checks for duplicate records
- **Statistical Summary**: Provides descriptive statistics for all numerical columns

### 3. Anomaly Detection
Identifies and quantifies data quality issues:
- **Temporal Anomalies**:
  - Trips outside January 2024 date range
  - Negative or zero trip durations
  - Unrealistic trip durations (> 3 hours)
  
- **Financial Anomalies**:
  - Negative fare amounts
  - Zero fares
  - Extremely high fares (> $100, > $500)
  
- **Trip Characteristic Anomalies**:
  - Zero distance trips
  - Unrealistic distances (> 50 miles, > 100 miles)
  - Invalid passenger counts

### 4. Data Cleaning and Preprocessing
Systematic data cleaning process:

1. **Date Filtering**: Retain only trips from January 2024
2. **Duration Filtering**: Remove trips with invalid durations (≤ 0 or > 180 minutes)
3. **Financial Filtering**: Remove negative fares, tips, and total amounts
4. **Fare Capping**: Remove unrealistic fares (> $500)
5. **Distance Filtering**: Keep only trips with distance > 0 and ≤ 100 miles
6. **Passenger Count**: Fill missing values with median, keep only 1-6 passengers
7. **Missing Value Imputation**: Fill remaining missing values appropriately

**Result**: Clean dataset with ~95% of original records retained

### 5. Feature Engineering
Creates derived features for analysis:

**Temporal Features**:
- `pickup_date`: Date of pickup
- `pickup_hour`: Hour of day (0-23)
- `pickup_day_of_week`: Day name (Monday-Sunday)
- `pickup_day_num`: Day number (0-6)
- `is_weekend`: Binary indicator for weekend trips
- `is_peak_hour`: Binary indicator for peak hours (6-8 PM)

**Financial Features**:
- `fare_per_mile`: Fare amount divided by distance
- `fare_per_minute`: Fare amount divided by duration
- `tip_percentage`: Tip as percentage of fare
- `has_tip`: Binary indicator for trips with tips

**Trip Characteristics**:
- `speed_mph`: Average speed in miles per hour
- `payment_method`: Human-readable payment type labels

### 6. Exploratory Data Analysis

#### 6.1 Temporal Patterns
- **Daily Trends**: Line plot showing trip volume across January 2024
- **Hourly Patterns**: Bar chart of trips by hour + line plot of average fare by hour
- Identifies busiest/quietest hours and days

#### 6.2 Financial Analysis
- **Fare Distribution**: Histogram and box plot of fare amounts
- **Tip Analysis**: 
  - Distribution of tip amounts
  - Distribution of tip percentages
  - Percentage of trips with tips

#### 6.3 Trip Characteristics
- **Distance Distribution**: Histogram of trip distances
- **Duration Distribution**: Histogram of trip durations
- **Scatter Plots**:
  - Distance vs Fare
  - Duration vs Fare

#### 6.4 Categorical Analysis
- **Payment Methods**: Distribution and average fare by payment type
- **Vendor Analysis**: Distribution and performance metrics by vendor

### 7. Correlation Analysis
- Heatmap showing correlations between numerical variables
- Identifies strongest predictors of fare amount

### 8. Key Insights Summary
Comprehensive summary statistics including:
- Dataset overview (total trips, date range, daily averages)
- Temporal patterns (busiest hours, peak vs off-peak, weekday vs weekend)
- Financial metrics (average/median fare, tip statistics)
- Trip characteristics (distance, duration, speed, passengers)
- Payment and vendor distributions

### 9. Save Cleaned Dataset
- Exports cleaned data to `yellow_tripdata_2024-01_cleaned.parquet`
- Ready for hypothesis testing and statistical analysis

## Data Quality Improvements

### Issues Identified (from original data_exploration.ipynb):
1. ✓ Trips outside January 2024 date range
2. ✓ Negative trip durations
3. ✓ Negative fare amounts (~37,448 trips)
4. ✓ Unrealistic fare amounts (> $500)
5. ✓ Zero or unrealistic distances
6. ✓ Missing values in passenger_count, RatecodeID, etc. (~4.73%)
7. ✓ Invalid passenger counts (0 or > 6)

### All Issues Addressed:
- Filtered to valid January 2024 trips only
- Removed all invalid financial values
- Capped extreme outliers
- Imputed missing values appropriately
- Created clean, analysis-ready dataset

## Next Steps
The cleaned dataset is now ready for:
1. **Hypothesis Testing** (as outlined in project proposal)
   - H1: Temporal demand and pricing relationship
   - H2: Trip distance and fare elasticity
   - H3: Customer satisfaction and tip behavior
   - H4: Vendor performance differences
   - H5: Geographic fare variation

2. **Statistical Analysis**
   - Regression modeling
   - ANOVA/t-tests
   - Correlation analysis
   - Geographic clustering

## Usage Instructions

1. **Open the notebook**:
   ```bash
   jupyter notebook data_preprocessing_eda.ipynb
   ```

2. **Run all cells** to:
   - Load and clean the data
   - Generate all visualizations
   - Create the cleaned dataset file

3. **Review outputs**:
   - All data quality metrics
   - Comprehensive visualizations
   - Key insights summary

4. **Use cleaned data**:
   - Load `yellow_tripdata_2024-01_cleaned.parquet` for further analysis
   - All preprocessing steps documented and reproducible

## Dependencies
Required Python packages (see `requirements.txt`):
- pandas
- numpy
- matplotlib
- seaborn
- pyarrow (for parquet files)

## Notes
- The notebook corrects issues found in the original `data_exploration.ipynb`
- All preprocessing steps are clearly documented
- Visualizations use consistent styling
- Code is well-commented and reproducible
- Aligns with project proposal objectives and methodology
