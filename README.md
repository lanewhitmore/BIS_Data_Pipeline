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

1. Go to [BIS ELT Pipeline](https://github.com/lanewhitmore/BIS_Data_Pipeline).

2. Download repository by selecting the green 'Code' button and then 'Download Zip'.

3. Extract zip file into preferred path. 
  
4. Building the schema:
    A. Open final_workbench.mwb in MySQL Workbench 8.0 CE.
    B. With the model open, click the 'Database' tab and select 'Forward Engineer'.
    C. Click next through every option leaving the default options checked.
    D. Refresh local databases 'BIS_ID' should now be available.

5. Set Operating System Environment Variables (Directions for Windows OS Users):
    A. In the windows search bar type 'Edit the system environment variables'.
    B. 

5. Open bis_pipe.py in IDE of choice and run the file.

6. Monitoring:
    A. Check that prints within IDE tracking where the file is all went through.
    B. Open pipeline.log in your local repository on NotePad to see tracking of populated database.
    C. If log shows population, refresh BIS_ID in MySQL Workbench 8.0 CE to see freshly populated tables and views.

### Automation (Windows Users Only)
1. Edit the bis_pipe_automation.bat file in NotePad to replace the local files from whitm's machine with:
    A. Anaconda environment where the necessary packages have been installed. For me, this was in: "C:/Users/whitm/anaconda3/Scripts/activate.bat" named 'bisenv'
    B. Change the windows command prompt directory on the second line to wherever the 'src' file from the repository has been stored.
    C. On line three, paste your Anaconda python.exe file location. Then, in the quotation marks, replace my bis_pipe.py file path with your local one.
    
2. 



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
