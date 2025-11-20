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
Systematic data cleaning process aligned with team standards:

1. **Date Filtering**: 
   - Retain trips from January 2024
   - Include New Year transition trips (Dec 31, 2023 23:00+ to Jan 1, 2024 00:00)
   - Remove abnormal year entries (2002, 2009) due to taximeter malfunctions

2. **Duration Filtering**: Remove trips with invalid durations (≤ 0 or > 180 minutes)

3. **Refund/Reversal Pair Removal**:
   - Identify negative `total_amount` records (35,504+ records)
   - Find matching positive records of same amount
   - Remove BOTH negative and positive pairs to avoid double-counting
   - Ensures analysis reflects only real transactions

4. **Financial Filtering**: 
   - Remove remaining negative fares, tips, and total amounts
   - Cap unrealistic fares (> $500)

5. **Distance Filtering**: Keep only trips with distance > 0 and ≤ 100 miles

6. **Sophisticated Passenger Count Handling** (4-group approach):
   - **Group 1**: `total_amount=0` AND `trip_distance=0` → **Remove** (system/human errors)
   - **Group 2**: `total_amount=0` AND `trip_distance≠0` → **Remove** (system/human errors)
   - **Group 3**: `total_amount≠0` AND `trip_distance=0` → **Keep** (E-Hail/Flex Fare trips)
   - **Group 4**: `total_amount≠0` AND `trip_distance≠0` → **Keep** (E-Hail/Flex Fare trips)
   - Fill remaining missing values with median, keep only 1-6 passengers
   - Rationale: E-Hail apps may not transmit passenger_count to TLC system

7. **RatecodeID**: Fill missing values with `99` (Null/Unknown per TLC standard)

8. **Store_and_fwd_flag**: Fill with `N` (technical field, no impact on analysis)

9. **Congestion_surcharge**: Fill with `0` (trips outside congestion zone/time)

10. **Airport_fee** (Location-based imputation):
    - Identify pickups at LaGuardia (zone 138) or JFK (zone 132)
    - Fill with `$1.70` for airport pickups
    - Fill with `0` for all other locations

**Result**: Clean dataset with ~95% of original records retained, following TLC data standards

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

### Issues Identified and Addressed:
1. ✓ **Abnormal timestamps**: Removed 2002/2009 entries, kept New Year transition trips
2. ✓ **Negative trip durations**: Removed invalid temporal data
3. ✓ **Refund/reversal pairs**: Identified and removed 35,504+ negative records + matching positives
4. ✓ **Unrealistic fares**: Capped at $500, removed negative amounts
5. ✓ **Zero/unrealistic distances**: Filtered appropriately, kept valid E-Hail trips
6. ✓ **Missing passenger_count** (~140,162 records): 
   - Sophisticated 4-group handling
   - E-Hail/Flex Fare trip recognition
   - Domain-aware imputation
7. ✓ **Missing RatecodeID**: Filled with 99 (TLC standard for Null/Unknown)
8. ✓ **Missing Airport_fee**: Location-based imputation using pickup zones
9. ✓ **Missing congestion_surcharge**: Filled with 0 (outside zone/time)
10. ✓ **Invalid passenger counts**: Kept only 1-6 passengers

### Team Alignment:
- Follows approach documented in "Initial Data Cleaning.docx"
- Implements TLC data dictionary standards
- Recognizes E-Hail/Flex Fare trip characteristics
- Uses domain knowledge for intelligent imputation
- Ensures data integrity for statistical analysis

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
