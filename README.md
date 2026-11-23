
# Urban Mobility Intelligence
### Transportation Analytics and Anomaly Detection

**Tools:** Python | PySpark | Databricks | Delta Lake | scikit-learn | Pandas | Matplotlib

## 1. Project Overview

Transportation companies generate large amounts of data from daily journeys, including trip distances, travel times, locations, fares, and payment information.

However, collecting this information is only useful when it can support better decisions.

For example:
- Which locations and routes experience the most travel activity?
- When do journeys take longer than expected?
- How do transaction values differ across journeys and services?
- Which trips have unusual characteristics that may require further investigation?
- How can companies identify potential issues without reviewing every transaction manually?

**Urban Mobility Intelligence** explores these questions using 120,000 simulated taxi journeys across a fictional metropolitan area.

The project combines transportation analytics with anomaly detection to demonstrate how trip and transaction data can support operational planning, transaction monitoring, and more informed decision-making.

## 2. Business Problem

Transportation operators must balance service efficiency, customer experience, and financial oversight.

Without a structured way to analyse journey data, companies may struggle to identify patterns across thousands of trips or distinguish unusual transactions from normal operational variation.

For instance, a journey with a high fare may be perfectly reasonable if it covers a long distance. Similarly, a slow journey during peak commuting hours may not indicate a problem.

Flagging such trips without considering their circumstances could lead to unnecessary investigations.

This project addresses two related challenges:

**Operational visibility:** Understanding where, when, and how journeys occur so that companies can investigate service patterns and potential inefficiencies.

**Unusual activity monitoring:** Identifying trips whose pricing or journey characteristics differ significantly from typical behaviour, helping teams prioritize records for further review.

## 3. Project Objectives

The project aims to:

1. Build a structured process for loading, validating, and analysing large volumes of journey records.
2. Examine travel patterns across time periods, locations, routes, and transaction categories.
3. Identify operational and financial patterns that could inform business decisions.
4. Develop contextual measures that account for normal differences in journey behaviour.
5. Compare two approaches for detecting unusual trips.
6. Evaluate the trade-off between identifying more unusual cases and generating additional review alerts.

## 4. Approach

The project follows four stages.

### Stage 1 — Data Preparation

Loaded and validated 120,000 simulated journey records using PySpark and Databricks.

Checks included missing values, duplicate records, trip timestamps, transaction calculations, and geographical references.

Validated data was stored in Delta Lake tables for subsequent analysis.

### Stage 2 — Transportation and Transaction Analysis

Explored travel activity across time periods, pickup locations, routes, service types, payment methods, and booking channels.

The analysis examined journey volumes, travel speeds, durations, and transaction values to understand how different operational conditions relate to trip behaviour.

### Stage 3 — Understanding Journey Context

Developed additional measures that compare each journey with typical behaviour during the same operational time period.

For example, instead of considering a journey's speed in isolation, the analysis compares it with the typical speed for peak or off-peak periods.

This provides a more meaningful way to investigate unusual journeys.

### Stage 4 — Detecting Unusual Activity

Compared two approaches:

**Contextual Rules**

A straightforward method that flags journeys with unusually large differences in speed, duration, or fare per kilometre.

**Isolation Forest**

A machine learning method that examines multiple journey characteristics together to identify records that stand out from typical patterns.

The models were developed without using the dataset's preassigned unusual-trip labels. Those labels were used only to evaluate the results.

## 5. Key Findings and Business Implications

### A. Journey Efficiency Varies by Time of Day

The simulated dataset showed slower journeys and longer travel durations during morning and evening peak periods.

For example, median speeds were approximately:
- Off-peak: 21 km/h
- Morning peak: 16.1 km/h
- Evening peak: 16.1 km/h

**Potential business value:**

Transportation companies could use similar analysis of real journey data to investigate congestion-related delays, improve journey-time estimates, and review scheduling decisions.

The analysis also demonstrates why expected travel conditions should be considered when investigating unusual trips.

### B. Travel Activity Is Concentrated Around Key Locations

The Central Business District and Central Station accounted for approximately 23% of recorded pickups.

The most frequently recorded route was Central Station to the Central Business District, with 2,563 journeys.

The analysis also identified directional differences between incoming and outgoing trips at several locations.

**Potential business value:**

With real operational data, these insights could help companies investigate where vehicles may be needed, understand important travel corridors, and assess opportunities for better fleet positioning.

Trip counts alone do not establish actual vehicle availability, so further operational data would be required before making fleet decisions.

### C. High Transaction Values Require Context

The median transaction value was €16.83, while the maximum was €459.52.

Airport-originating journeys had an average transaction value of €28.98, higher than the overall average of €20.96, alongside longer journey distances and durations.

**Potential business value:**

Companies could use similar analysis to understand how journey characteristics influence transaction values.

This could also help monitoring teams avoid treating every expensive journey as suspicious and instead focus on transactions that appear unusual relative to their circumstances.

## 6. Anomaly Detection Results

### What Is Anomaly Detection?

Anomaly detection is a way of identifying records that differ from what is normally observed.

In this project, an unusual trip might involve:
- A fare that appears high compared with the distance travelled.
- A journey duration that differs substantially from typical conditions.
- An unusual combination of distance, speed, duration, and payment characteristics.

An unusual trip does not automatically indicate fraud or wrongdoing. It is simply a record that may deserve closer examination.

### How Were the Methods Evaluated?

The methods were tested on 24,088 journeys that were kept separate from the training data.

Of these, 629 contained deliberately introduced unusual scenarios.

Two important measures were used:

- **Precision:** Of the trips flagged for review, how many matched the deliberately introduced unusual scenarios?
- **Recall:** Of all the deliberately introduced unusual scenarios, how many were successfully identified?

### Performance Comparison

| Detection Method | Trips Flagged | Precision | Recall |
|---|---:|---:|---:|
| Contextual Rules | 652 | 40.95% | 42.45% |
| Isolation Forest | 634 | 42.90% | 43.24% |
| Either Method | 860 | 37.56% | 51.35% |
| Both Methods Agree | 426 | 50.70% | 34.34% |



Isolation Forest performed slightly better at the selected alert threshold.

However, the contextual rule-based approach achieved better overall ranking performance when evaluated across different thresholds.

Neither method was consistently better at identifying every type of unusual journey.

### What Do These Results Mean for a Business?

Consider a company that has limited staff available to review unusual transactions.

If it wants to identify more potentially problematic journeys, it could prioritize trips flagged by either detection method.

In this experiment, that approach identified 51.35% of the injected unusual cases but generated 860 alerts.

Alternatively, if the company needs a shorter review list, it could prioritize journeys flagged by both methods.

That approach generated 426 alerts and achieved 50.70% precision, but identified fewer of the injected unusual cases.

**The main lesson is that detection systems involve a trade-off between review workload and the risk of missing unusual activity.**

The appropriate approach would depend on a company's priorities, investigation capacity, and the consequences of missed cases.

## 7. Potential Business Impact

Although this project uses synthetic data, the analytical approach illustrates several possible applications for transportation and mobility companies.

| Business Area | Potential Application |
|---|---|
| Fleet operations | Investigate busy routes, directional travel patterns, and possible vehicle positioning needs |
| Service planning | Understand journey durations and travel conditions across different periods |
| Financial monitoring | Identify transactions with unusual pricing or journey characteristics |
| Operational oversight | Prioritize unusual records for human investigation |
| Decision support | Turn large volumes of trip records into interpretable operational insights |

These are potential applications, not measured business outcomes.

The project does not establish cost savings, fraud reduction, improved customer satisfaction, or increased operational efficiency.

## 8. Tools and Technologies

| Technology | Purpose |
|---|---|
| Python | Data analysis and modelling |
| PySpark | Processing and analysing trip records |
| Databricks | Development and analysis environment |
| Delta Lake | Storing validated and prepared datasets |
| Pandas | Preparing model inputs and evaluation results |
| scikit-learn | Building the Isolation Forest model |
| Matplotlib | Creating charts and visual summaries |

## 9. Project Structure

```text
urban-mobility-intelligence/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_Data_Ingestion_and_Inspection.ipynb
│   ├── 02_Exploratory_Data_Analysis.ipynb
│   ├── 03_Feature_Engineering.ipynb
│   └── 04_Anomaly_Detection_and_Evaluation.ipynb
└── visuals/
    ├── model_performance.png
    └── scenario_recall.png
```

### Notebook Guide

| Notebook | Description |
|---|---|
| 01 — Data Ingestion and Inspection | Load, inspect, and validate the source data |
| 02 — Exploratory Data Analysis | Investigate travel patterns, routes, and transaction behaviour |
| 03 — Feature Engineering | Prepare journey and contextual measurements |
| 04 — Anomaly Detection and Evaluation | Compare detection methods and investigate their performance |

## 10. Data Availability and Reproducibility

The dataset was synthetically generated for this project and contains no real passenger or transaction records.

**The source dataset is not publicly available in this repository.**

The notebooks are shared to demonstrate the analytical workflow, methodology, and results.

The original analysis was conducted in Databricks using source files stored in a Unity Catalog Volume and intermediate datasets stored as Delta tables.

Reproducing the numerical results requires access to the original dataset or a compatible dataset and corresponding configuration changes.

## 11. Limitations

- The dataset and locations are fictional.
- The injected unusual scenarios are not confirmed fraudulent activity.
- The results are based on one chronological training and evaluation split.
- Detection thresholds were informed by the known proportion of synthetic review scenarios.
- The models do not account for every possible factor influencing journey behaviour.
- The results cannot be assumed to generalize to real transportation or financial monitoring systems.
- The project demonstrates batch analytics rather than a deployed, real-time monitoring service.

## 12. Conclusion

Urban Mobility Intelligence demonstrates how transportation data can be used for more than reporting journey counts and transaction totals.

By combining operational analysis with contextual anomaly detection, the project explores how organizations could better understand travel activity, investigate unusual behaviour, and make more informed decisions about where to focus their attention.

The most important finding is that effective monitoring is not simply about identifying extreme values or choosing the most complex model.

It requires understanding context, evaluating different approaches, and balancing the need to detect unusual activity with the practical cost of reviewing alerts.

The project demonstrates an analytical approach that could be adapted and further evaluated for real-world transportation operations and other event-monitoring applications.
