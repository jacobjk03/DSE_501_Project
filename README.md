# NYC Yellow Cab Taxi Data Analysis Project

## Overview
This project analyzes the New York City Yellow Cab taxi dataset for January 2024, containing 2,964,624 individual taxi trips. The analysis focuses on understanding relationships between temporal patterns, trip characteristics, and fare determinants.

## Dataset
- **File**: `yellow_tripdata_2024-01.parquet`
- **Size**: ~2.96 million trips
- **Variables**: 19 variables including temporal, geographic, financial, and operational dimensions
- **Source**: NYC Taxi and Limousine Commission (TLC)

## Setup Instructions

### 1. Clone the Repository
```bash
git clone [your-repository-url]
cd DSE_Project
```

### 2. Create Virtual Environment (Recommended)
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the Analysis
```bash
jupyter notebook data_exploration.ipynb
```

## Project Structure
```
DSE_Project/
├── yellow_tripdata_2024-01.parquet    # Main dataset
├── data_exploration.ipynb             # Data analysis notebook
├── project_proposal_2pages.docx       # Project proposal
├── requirements.txt                   # Python dependencies
└── README.md                         # This file
```

## Research Objectives
1. Analyze temporal demand patterns and pricing relationships
2. Examine trip distance/duration impact on fare structure
3. Investigate vendor performance differences
4. Study geographic patterns in trip demand and fare variations

## Key Findings
- Dataset contains 2,964,624 trips with 84,704 trips/day average
- Peak hour: 6:00 PM (212,788 trips), Quiet hour: 4:00 AM (16,742 trips)
- Fare range: -$899 to $5,000 (mean: $18.18, median: $12.80)
- Tips provided in 76% of trips (mean: $3.34)
- Three main vendors with Vendor 2 handling 75.4% of trips

## Requirements
- Python 3.8+
- Jupyter Notebook

## Note
The dataset uses location IDs (PULocationID, DOLocationID) instead of latitude/longitude coordinates. Some data quality issues exist (4.73% missing values) and require preprocessing.
