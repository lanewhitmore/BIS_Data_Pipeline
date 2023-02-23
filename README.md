# BIS Pipeline - Bank for International Settlements (BIS)
__Lane Whitmore and Dave Friesen__<br>
__lwhitmore@sandiego.edu, dfriesen@sandiego.edu__<br>
__GitHub link: https://github.com/lanewhitmore/BIS_Data_Pipeline__<br>

## Context and Project

The Bank for International Settlements (BIS) is an international “bank for central banks”
supporting monetary and financial cooperation among its central bank owners around the
globe (BIS, 2023). Among its roles, the BIS compiles and publicly publishes statistics that
inform analysis of global financial stability and liquidy.

*BIS Pipeline* is a production-ready, automated data pipeline that extracts, loads, and transforms select BIS datasets, "surfacing" these for advanced analytics.

## How to

### Deployment

### Monitoring


### [architecture - including architectural diagram]

## Data

*BIS Pipeline* data includes US dollar exchange rates (monthly, quarterly and annual), consumer prices, and policy rates (monthly). These datasets are sourced from BIS’ statistics download page located at https://www.bis.org/statistics/full_data_sets.htm, baseline-summarized in Table 1 as follows:

__Table 1__<br>
*BIS Datasets*
| Dataset Name | File Name and Format | Size (dimensional) |
| ------------ | -------------------- | ------------------ |
| US Dollar exchange rates<br>(monthly, quarterly, annual) | WS_XRU_csv_col.csv      | 1,150 rows (less header)<br>3,960 columns |
| Consumer prices                                          | WS_LONG_CPI_csv_col.csv | 240 rows (less header)<br>1,696 columns   |
| Policy rates (monthly)                                   | WS_CBPOL_M_csv_col.csv  | 39 rows (less header)<br>937 columns      |

__Figure 1__<br>
*BIS Entity Relationship Diagram*
<img src="https://github.com/lanewhitmore/BIS_Data_Pipeline/blob/main/data/bis_id_ERD.png" width=130% height=130%>

## Pipeline Functional Overview

### [output/utility]

## Pipeline Non-Functional Overview

### [process]

### [data integrity controls and logging]

### [security]

### [scalability]

### [extensibility]

## Gaps and Opportunities
