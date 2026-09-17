Use this as the README content and save it as `README.md` in the notebooks folder.

````markdown
# Road Accident Severity Analysis for U.S. Data with a Kenya Application Context

## Overview

This project analyzes road accident severity using a working sample of the U.S. Accidents dataset. The goal is to understand which accident characteristics are associated with more severe outcomes, assess whether severity can be predicted from information likely available at the time of an incident, and then frame the findings in a way that is realistic about transferability to Kenya.

The full U.S. Accidents dataset contains roughly 7.7 million records. Because of local memory and processing constraints, this project uses the first 50,000 rows as the working dataset. All analysis, feature engineering, modeling, and evaluation in this notebook are based on that sample.

The Kenya context is used to discuss feasibility, data requirements, and transferability. It is not used to claim that patterns observed in the U.S. sample are Kenyan patterns.

## Contribution Statement

This project was completed in stages.

- Alvin Maina completed Steps 1 to 4:
  1. Business Understanding
  2. Data Understanding
  3. Data Preparation
:
  4. Exploratory Data Analysis
  5. Modeling
  6. Evaluation
  7. Kenya Application / Business Evaluation
  8. Final conclusion and documentation

## Project Objective

The notebook answers the following questions:

- Which conditions are associated with more severe accidents?
- Are there identifiable temporal patterns?
- What environmental and road characteristics appear alongside severe accidents?
- Can accident severity be predicted from information available at the time of an incident?
- What information would a Kenyan road-safety system need?
- Which U.S. variables would require Kenyan equivalents, and what additional Kenyan data would be needed before local deployment?

## Business Problem

Road-safety agencies and transport planners need to identify where severe crashes are more common and whether early reporting features can help prioritize investigation and response. This project examines whether accident severity can be understood from reporting-time information such as timestamps, weather, visibility, and road context.

## Data Source

- Source dataset: U.S. Accidents (2016-2023)
- Working dataset: first 50,000 rows from the source CSV
- File used in the notebook: US_Accidents_March23.csv
- Scope: U.S. data only
- Application framing: Kenya used as a policy and feasibility context, not as a validation target

## Analytical Workflow

The project follows the CRISP-DM approach:

1. Business Understanding
   - Define the decision problem
   - Clarify the operational use of the model
   - Identify the main analytical questions

2. Data Understanding
   - Inspect the schema
   - Check for missing values and invalid fields
   - Review severity coding and timestamp coverage

3. Data Preparation
   - Remove duplicates and invalid records
   - Drop records with missing Severity or invalid Start_Time
   - Restrict the target to valid severity levels
   - Create time-based features
   - Define a binary high-severity target

4. Exploratory Data Analysis
   - Review severity distribution
   - Inspect temporal patterns
   - Examine weather and visibility associations
   - Interpret descriptive relationships carefully

5. Modeling
   - Build a time-aware train/test split
   - Use variables plausibly available at incident reporting time
   - Train baseline and comparison models

6. Evaluation
   - Use recall as the primary metric for high-severity cases
   - Review precision, F1, ROC-AUC, and PR-AUC
   - Check confusion matrices and classification reports

7. Kenya Application / Business Evaluation
   - Separate U.S.-specific findings from transferability claims
   - Define what would be needed for Kenyan deployment

8. Conclusion
   - Summarize findings
   - State limitations
   - Clarify the distinction between evidence and assumptions

## Data Preparation Details

The notebook uses a conservative workflow to reduce leakage and improve realism.

Key decisions:

- Remove exact duplicate records
- Drop rows with missing Severity or invalid Start_Time
- Keep only valid severity values from the observed scale
- Create the following derived features from Start_Time:
  - hour
  - month
  - day_of_week
  - is_weekend
- Group rare Weather_Condition values into Other to avoid unstable sparse categories
- Create a binary target called high_severity where Severity >= 3
- Use a temporal train/test split instead of a random split
- Fit preprocessing only on the training data

## Feature Set

The model uses variables that are plausibly available when an accident is reported, including:

- Numeric features:
  - Temperature(F)
  - Humidity(%)
  - Pressure(in)
  - Visibility(mi)
  - Wind_Speed(mph)
  - hour
  - month
  - is_weekend

- Categorical features:
  - State
  - Weather_Condition
  - Sunrise_Sunset
  - day_of_week
  - Amenity
  - Bump
  - Crossing
  - Give_Way
  - Junction
  - No_Exit
  - Railway
  - Roundabout
  - Station
  - Stop
  - Traffic_Calming
  - Traffic_Signal
  - Turning_Loop

These variables are used for a decision-support analysis and are not intended to represent a fully causal explanation of crashes.

## Modeling Strategy

The notebook builds a binary classification model to predict high severity.

Target:
- high_severity = 1 if Severity is 3 or 4
- high_severity = 0 otherwise

Train/test setup:
- Sort by time
- Use 80% for training
- Use the final 20% as a holdout set

Models evaluated:
- DummyClassifier baseline
- Logistic Regression
- Random Forest
- XGBoost

The main evaluation focus is recall for the high-severity class, because missing a severe crash is operationally more costly than creating a false alarm.

## Evaluation

The notebook reports:

- recall
- precision
- F1-score
- ROC-AUC
- PR-AUC
- classification report
- confusion matrix

The selected model is the one with the best recall on the time-ordered holdout set. This fits the project objective, but it does not imply the model is valid for Kenya.

## Key Findings from the U.S. Sample

The notebook finds that:

- severity is not evenly distributed across the working sample;
- severity patterns vary across time of day;
- weekend versus weekday patterns differ in the observed sample;
- weather and visibility are associated with different high-severity rates;
- road context variables carry predictive signal;
- a model can learn useful patterns in the U.S. sample.

These are sample-specific observations. They are not evidence that the same patterns exist in Kenya.

## Kenya Application Context

This project is not written to claim that U.S. results are Kenyan results. Instead, it shows how the same workflow could be adapted for Kenya.

What may transfer:
- CRISP-DM structure
- time-aware validation
- transparent model evaluation
- disciplined cleaning and investigation
- use of operationally relevant road and weather features

What does not automatically transfer:
- road design and traffic mix
- reporting practices and severity definitions
- weather and seasonal variation
- traffic exposure and trip volume
- emergency-response coverage and reporting lag
- infrastructure and land-use patterns

## Data Required for a Kenyan Deployment

A Kenyan system would need local records that match the same general concepts but reflect Kenyan conditions. The data would need to include:

- crash timing
- crash location
- severity coding
- road and traffic-control features
- weather and visibility
- vehicle and road-user details
- traffic exposure or volume
- emergency-response outcomes
- a local validation period for retraining and evaluation

## Technical Requirements

This project requires Python and the following libraries:

- pandas
- numpy
- matplotlib
- seaborn
- scipy
- scikit-learn
- xgboost

Install dependencies with:

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn xgboost
```

## Files in the Project

The project contains the analysis notebook and the working dataset file.

Typical structure:

- notebooks/
  - us_accidents_crisp_dm_project.ipynb
  - README.md
- US_Accidents_March23.csv

## Limitations

This project is intentionally limited in scope:

- It uses a 50,000-row working sample, not the full 7.7-million-row dataset
- It describes the U.S. sample only
- It does not claim causality
- It does not estimate risk per trip or per road segment because exposure data are missing
- It does not validate the model for Kenya

## Final Statement

This notebook demonstrates a disciplined data-science workflow for accident analysis using U.S. data and places that workflow into a Kenyan road-safety context without overstating what the evidence supports. The value is in the method, the honest separation of evidence from assumptions, and the requirement for local Kenyan validation before any deployment decision.
````