# BIS Pipeline - Bank for International Settlements (BIS)
__Lane Whitmore and Dave Friesen__<br>
__lwhitmore@sandiego.edu, dfriesen@sandiego.edu__<br>
__GitHub link: https://github.com/lanewhitmore/BIS_Data_Pipeline__<br>

<link rel="stylesheet" href="github-markdown.css">


## Context and Project

The Bank for International Settlements (BIS) is an international “bank for central banks”
supporting monetary and financial cooperation among its central bank owners around the
globe (BIS, 2023). Among its roles, the BIS compiles and publicly publishes statistics that
inform analysis of global financial stability and liquidy.

*BIS Pipeline* is a production-ready, automated data pipeline that extracts, loads, and transforms select BIS datasets, "surfacing" these for advanced analytics.


## How to


### Pipeline Deployment

1. Go to [BIS ELT Pipeline](https://github.com/lanewhitmore/BIS_Data_Pipeline).

2. Download repository by selecting the green 'Code' button and then 'Download Zip.'

3. Extract zip file into preferred path.

4. Building the schema:
    
    A. Open final_workbench.mwb in MySQL Workbench 8.0 CE.
    
    B. With the model open, click the 'Database' tab and select 'Forward Engineer'.
    
    C. Click next through every option, leaving the default options checked.
    
    D. Refresh local databases; 'BIS_ID' should now be available.

5. Set Operating System Environment Variables (Directions for Windows OS Users):
    
    A. In the windows search bar, type 'Edit the system environment variables' and select it.
    
    B. In the advanced tab, select 'Environment Variables' button in the bottom right just above 'ok', 'cancel', and 'apply'.
    
    C. Once in, you will want to select 'New...' for the top panel 'User Variables'.
    
    D. Create the following User Variables:
       * variable name: DATA_PATH variable value: local path to BIS_Data_Pipeline/Data/. Tip: No quotation marks.
        
       * variable name: DB_NAME variable value: BIS_ID.
        
       * variable name: HOST variable value: your local host name for MySQL server.
        
       * variable name: PASSWORD variable value: your local password for MySQL server.
        
       * variable name: PORT variable value: your local port number for MySQL server.
        
       * variable name: USER variable value: your local user name for MySQL server.
    
    E. Alternatively, these values can be manually typed into the top of the bis_pipe.py file (just below package imports) where environment variables are saved.

6. Open bis_pipe.py in IDE of choice and run the file.

7. Monitoring:
    
    A. Check that display points (i.e., file tracking) within the script to confirm all went through.
    
    B. Open pipeline.log in your local repository on NotePad to see tracking of populated database.
    
    C. If log shows population, refresh BIS_ID in MySQL Workbench 8.0 CE to see freshly populated tables and views.


### Automation (Windows Users Only)

1. Edit the bis_pipe_automation.bat file in NotePad to replace the local files from whitm's machine with:
    
    A. Anaconda environment where the necessary packages have been installed. For me, this was in: "C:/Users/whitm/anaconda3/Scripts/activate.bat" named 'bisenv'
    
    B. Change the windows command prompt directory on the second line to wherever the 'src' file from the repository has been stored.
    
    C. On line three, paste your Anaconda python.exe file location. Then, in the quotation marks, replace my bis_pipe.py file path with your local one.
    
2. Open Window's Task Scheduler and follow these steps:
    
    A. In the upper right of the page in the 'Actions' bar select 'Create Task'
    
    B. Name the task whatever you feel appropriate, I named mine run_bis_pipeline.
    
    C. Add a description of what the task is doing.
    
    D. Go to 'Triggers':
       * Select 'New'
        
       * Set 'Begin the task' to 'On a schedule' in the drop down.
        
       * In the Settings, select 'Monthly' and set the start for the 2nd of the next month at 10 AM PST. In my case it will be March 2nd. 
        
       * In the 'Months' drop down select all months, and in the 'Days' drop down select '2' for the second of every month.
        
       * Additionally, in settings make sure 'Allow Task to Run on Command' to test your automation. 
        
    E. Save Task.
    
3. Test your automation by either doubling clicking your bis_pipe_automation.bat file in File Explorer or right clicking then running your task in task schedule. 

4. Monitoring:

    A. Monitoring functions in the same way as listed above. So long as your cd has been set to 'src' folder, your pipeline logs will appear there.
    
    B. 'pause' has been added to bis_pipe_automation.bat to show the prints within the Window's command prompt until a key is pressed to exit.
    
    C. Task Scheduler will now populate BIS_ID with monthly CSV updates on the 2nd of every month at 10AM PST. Logs should be used to ensure the process has been     completed. 


## Architecture


### [architectural diagram]


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


### Process


### Data Integrity Controls and Logging


### Security

There are multiple steps of security that can be taken when using MySQL as the host for a relational database. Currently, there is no sensitive information that may need to have authorization to access within the database, but the pipeline and database is highly scalable. Due to this, it is within best practice to give privileges to users only when required. In this case, we have stored the data in 6 tables and made 10 views for those tables. Within the structure of the company, permissions can be given to access the connected database allowing the team concerned with tracking long term exchange rate trends to evaluate the strength of the U.S. Dollar in comparison to other countries. Doing so could add context to evaluating the economic strength for the United States, for example. Views have been created for just this purpose. In Figure above, the group of USD views define the long term relationships between the U.S. Dollar and various other currencies in potential countries of interest. Permission could be added for certain users working on such a project to access only these views rather than the entirety of the database to protect any future sensitive information that may be added to the database in addition to protecting the integrity of the database from any mistaken queries that may add false data to the structure. 

In addition to having views to protect the database from security or structural risks, steps have been taken within the pipeline itself to hide credentials. Credentials in this case are stored locally within the machine as environment variables. Another step of security we have taken into consideration is backing up the storage. In the case of this pipeline, CSV backups are stored within the “data” folder of the repository. In the event that the database is attacked and wiped in addition to the website being taken down or attacked in some way, the most recent version of the CSV files hosted on the website are stored as backups. 


### Scalability

  Given that the nature of the data and ETL pipeline is storing the data as a structured relational database within MySQL, the database will be highly scalable. To cement this scalable construct, as the CSV files from BIS comes wide, with dates as columns rather than rows, each CSV is stored as two tables within the database with matching keys to call back. Doing so allows one table to be smaller, in the thousands or hundreds in rows, with more computationally expensive information like descriptions, country, and title. Meanwhile, the larger table, in the hundreds of thousands of rows, stores only row key, data, and value. Establishing the schema in this way allows for sub-querying to be more optimized as the smaller table, with more expensive information, can be filtered then the keys can be matched to inner join the much larger table containing dates and values. This process will make the database more scalable as it grows each month. For example, WS_LONG_CPI_csv_col.csv becomes two tables, Figure 1 above highlights this more clearly. Table one is the smaller table with the columns; consumer_prices_id, frequency, reference_area, unit_of_measure, and series. Table two is the longer table with consumer_prices_values_id, consumer_prices_id, date, and values. An example of the sub-query filtering is the view united_states_cp that grabs the IDs associated with United States reference_area, which, are then used to pull just under 1000 rows of dates and values in table 2. This greatly reduces the computation time to grab potentially thousands of rows.


### Extensibility


## Gaps and Opportunities
