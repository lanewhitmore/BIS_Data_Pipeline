# BIS Pipeline - Bank for International Settlements (BIS)
__Lane Whitmore and Dave Friesen__<br>
__lwhitmore@sandiego.edu, dfriesen@sandiego.edu__<br>
__GitHub: https://github.com/lanewhitmore/BIS_Data_Pipeline__<br>


<br>

## Opportunity and Solution

The Bank for International Settlements (BIS) is an international “bank for central banks” supporting monetary and financial cooperation among its central bank owners around the globe (BIS, 2023). Among its roles, the BIS compiles and publicly publishes a "gold mine" of data: statistics rates, and metrics that inform analysis of global financial stability and liquidity. This information spans banking, currency, debt, derivatives, credit, property, consumers, and financial market domains. Leverage of this data can be accelerated through automation and structure.

__*BIS Pipeline*__ does just that. It provides a production-ready, automated data pipeline to extract, load, transform, and persist select BIS datasets to a relational database for further analysis and flexible "consumption." Data assets "surfaced" through the baseline version of __*BIS Pipeline*__ include US dollar exchange rates (monthly, quarterly and annual), consumer prices, and policy rates (monthly).

__*BIS Pipeline*__ consumption opportunities range from simple descriptive analytics and visualization to advanced predictive models. For example, the *base* pipeline provided here demonstrates automated output of Consumer Price Index (CPI) vs. the US federal discount rate, North American currency exchange rate comparisons, and exchange rate views across countries of interest. A simple and natural extension to these examples might be a time-series predictive model (e.g., ARIMA-based) to forecast CPI changes from the federal discount rate (as a leading indicator). Further opportunities exist through code and relational database schema extensions to this "open source" code base, and even third-party data augmentation.


<br>

## Repository Contents

1. Archive folder - storage for previous notebooks, operating as backup.<br>
2. Data folder - stores MySQL workbench model for forward engineering schema, backup CSVs after running the pipeline locally, also stores EER diagram that offers a visualization (EER Diagram) of the schema.<br>
3. SRC folder - contains all three visualizations (in .SVG format) created during analytics/consumption portion of pipeline, the pipeline in python and ipython notebook format (python file should be used for deployment), defaults.py makes changes to matplotlib formatting for consumption, and the pipeline log that tracks the actions/errors that occur during the process.<br>


<br>

## How to

The following sections describe steps to deploy and automate __*BIS Pipeline*__.


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

1. Edit the bis_pipe_automation.bat file in NotePad to replace the local files from my machine by changing:
    
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


<br>

## Source Data

As summarized under *Opportunity and Solution*, __*BIS Pipeline*__ data includes US dollar exchange rates (monthly, quarterly and annual), consumer prices, and policy rates (monthly). These datasets are sourced from BIS’ statistics download page located at https://www.bis.org/statistics/full_data_sets.htm, baseline-summarized in Table 1 as follows:

__Table 1__<br>
*BIS Datasets*
| Dataset Name | File Name and Format | Size (dimensional) |
| ------------ | -------------------- | ------------------ |
| US Dollar exchange rates<br>(monthly, quarterly, annual) | WS_XRU_csv_col.csv      | 1,150 rows (less header)<br>3,960 columns |
| Consumer prices                                          | WS_LONG_CPI_csv_col.csv | 240 rows (less header)<br>1,696 columns   |
| Policy rates (monthly)                                   | WS_CBPOL_M_csv_col.csv  | 39 rows (less header)<br>937 columns      |

This "raw" source data is extracted, loaded, transformed, and ultimately persisted into a MySQL relational database for further analysis and "consumption." Reference *Pipeline Functional and Non-Functional Overview* below for further detail.


<br>

## Pipeline Functional and Non-Functional Overview


### Pipeline Architecture and Process

The following Figure 1 overviews __*BIS Pipeline*__'s end-to-end architecture and data flow, followed by a walkthrough of each step and data stage in the process:

__Figure 1__<br>
*BIS Pipeline Architecture and Data Flow*
<img src="https://github.com/lanewhitmore/BIS_Data_Pipeline/blob/main/src/bis_pipe_flow.png" width=130% height=130%>


<br>

__1. Online Data Source__ - Per *Data* section above: https://www.bis.org/statistics/full_data_sets.htm

__2.a. Pipeline Trigger__ -<br>
Automation Directions are available at the top of the README on GitHub in addition to pipeline setup directions in general. The pipeline trigger is unfortunately only available within Window's Operating System. The pipeline has been created by constructing a batch (.bat) file in NotePad that contains four line items; pathing to an Anaconda environment that has been used to construct the pipeline, current working directory pathing to the 'src' folder within the repository, pathing to the Anaconda python.exe file, and, finally, pathing to the python pipeline file. The batch file is then used within Window's Task Scheduler to create a new task that runs on the second of every month at 10am, as the BIS datasets are updated every month on the first at any given time. The task will open Window's Command Prompt at that time and date and run the commands outlined earlier to begin updating the database with the pipeline. This will either populate the database, if the pipeline is running for the first time, or extract only rows that have not been populated within the database to update the tables with. Doing so will show print functions tracking the pipeline's progress in the command prompt. Once the pipeline has completed, within the 'src' folder, that has been set as the working directory, a pipeline log will be populated with recent updates or any expected errors that may have occurred.

__2.b. File Download__ -<br>
[. . .]

__3. Archive Data__ - full_xru_csv.zip, full_cbpol_m_csv.zip, and full_long_cpi_csv.zip as replicated directly from bis.org.

__4. File Extraction and Load__ -<br>
[. . .]

__5. Source Data__ - WS_XRU_csv_col.csv, WS_LONG_CPI_csv_col.csv, WS_CBPOL_M_csv_col.csv as extracted from (3).

__6. Data Transformation and Database Load__ -<br>
Following extraction and load, the pipeline then executes a transformation stage. This part of the process has been constructed by using custom built commands within Python that employ the use of the package PyMySQL and sqlalchemy’s create_engine function. The first formula creates the connection between the python script and the database cursor and closes the connection once the formula has run the SQL script in the cursor. The second function can be used to create tables on the database through the python script, this is mostly for future usability if needed. The third function uses the first connection function to push the data to the database. The formula creates a connection to the database, pulls the table’s columns from the database, uses those column names to extract the important columns from the pandas dataframe, and creates a dataframe column to be filled by the auto incremental IDs within the MySQL schema. The formula also pulls the existing index from the schema and uses the difference function to find indexes that have not yet been posted to the databases to extract then ultimately push to the schema. By comparing and extracting indexes, it removes the possibility of reposting the same data and end up with duplicate data throughout the database. In total, as Figure 1 points out, the transformation portion creates six tables, two for each CSV, and ten views that are to be used for easier access to the data and/or security purposes. 

__7. Entity Relationship Diagram (ERD)__ - The following Figure 2 visualizes the physical model for *BIS_Pipeline*'s relational database:

__Figure 2__<br>
*BIS Entity Relationship Diagram*
<img src="https://github.com/lanewhitmore/BIS_Data_Pipeline/blob/main/data/bis_id_ERD.png" width=130% height=130%>


<br>

__8.a. Consumption Sample One Using Base Schema__ <br>
The consumption sample below has been created at the end of the pipeline by pulling the consumer price index and federal rates from the United States. It acts as a sample of a data analytics dashboard that could automatically be constructed during the pipeline. 

__Figure 3__<br>
*Consumer Price Index vs. Federal Rates - 1980 to Present*
<img src = https://github.com/lanewhitmore/BIS_Data_Pipeline/blob/main/src/CPI_v_FedRate.svg>


<br>

__8.b. Consumption Samples Two and Three Using Built-in Views__ <br>
The consumption samples below are created by pulling the pre-filtered views that have been constructed within the schema. This serves as an example of potential use cases when using views. Views can be used for convienence when a query is used often, but, in addition to that, views can serve as security in the database. More on this topic is explained in the security section. Figures 4 and 5 below can offer insight into how the U.S. economy is performing by evaluating the U.S. Dollar in proportion to various other countries. Figure 4 highlights the exchange rate with two other North American countries Canada and Mexico over the span of 50 years. It points out that the relationship between the American Dollar and the Canadian Dollar have stayed static over the years while the exchange rate has steadily climbed going to Mexican Peso. For instance, this could point out that while Canadian and U.S. economies have stayed strong, or at least stayed relative to one another, the Mexican economy has struggled in comparison. 

__Figure 4__<br>
*U.S. Dollar North American Exchange Rates - 1970 to Present*
<img src = https://github.com/lanewhitmore/BIS_Data_Pipeline/blob/main/src/USD_Exchange_rates.svg>

Figure 5 offers more examples from  the views about potential analytics use cases from the database. The U.K., China, and Japan all have been world powers at some point in economic history. Figure 5 shows that the Japanese economy has become much stronger in comparison to the United States during the 21st Century. This could be due to the technological boom that has occurred as Japan has been at the forefront with multiple tech companies. The Pound and Yuan have stayed static for the last 50 years. These views are just options for how much the database can be expanded depending on potential use case. Although, based on the datasets already hosted within the database, and the way the schema has been setup, the best option moving forward will be evaluating the strength of the U.S. economy using analytical dashboarding and time series projecting. 

__Figure 5__<br>
*U.S. Dollar International Exchange Rates - 1970 to Present*
<img src = https://github.com/lanewhitmore/BIS_Data_Pipeline/blob/main/src/USD_Exchange_rates_int.svg>


<br>

### Security

There are multiple steps of security that can be taken when using MySQL as the host for a relational database. Currently, there is no sensitive information that may need to have authorization to access within the database, but the pipeline and database is highly scalable. Due to this, it is within best practice to give privileges to users only when required. In this case, we have stored the data in 6 tables and made 10 views for those tables. Within the structure of the company, permissions can be given to access the connected database allowing the team concerned with tracking long term exchange rate trends to evaluate the strength of the U.S. Dollar in comparison to other countries. Doing so could add context to evaluating the economic strength for the United States, for example. Views have been created for just this purpose. In Figure above, the group of USD views define the long term relationships between the U.S. Dollar and various other currencies in potential countries of interest. Permission could be added for certain users working on such a project to access only these views rather than the entirety of the database to protect any future sensitive information that may be added to the database in addition to protecting the integrity of the database from any mistaken queries that may add false data to the structure. 

In addition to having views to protect the database from security or structural risks, steps have been taken within the pipeline itself to hide credentials. Credentials in this case are stored locally within the machine as environment variables. Another step of security we have taken into consideration is backing up the storage. In the case of this pipeline, CSV backups are stored within the “data” folder of the repository. In the event that the database is attacked and wiped in addition to the website being taken down or attacked in some way, the most recent version of the CSV files hosted on the website are stored as backups. 


### Data Integrity Controls and Logging

[. . . Dave to add supporting narrative here to summarize logging approach]



### Scalability

Given that the nature of the data and ETL pipeline is storing the data as a structured relational database within MySQL, the database will be highly scalable. To cement this scalable construct, as the CSV files from BIS comes wide, with dates as columns rather than rows, each CSV is stored as two tables within the database with matching keys to call back. Doing so allows one table to be smaller, in the thousands or hundreds in rows, with more computationally expensive information like descriptions, country, and title. Meanwhile, the larger table, in the hundreds of thousands of rows, stores only row key, data, and value. Establishing the schema in this way allows for sub-querying to be more optimized as the smaller table, with more expensive information, can be filtered then the keys can be matched to inner join the much larger table containing dates and values. This process will make the database more scalable as it grows each month. For example, WS_LONG_CPI_csv_col.csv becomes two tables, Figure 1 above highlights this more clearly. Table one is the smaller table with the columns; consumer_prices_id, frequency, reference_area, unit_of_measure, and series. Table two is the longer table with consumer_prices_values_id, consumer_prices_id, date, and values. An example of the sub-query filtering is the view united_states_cp that grabs the IDs associated with United States reference_area, which, are then used to pull just under 1000 rows of dates and values in table 2. This greatly reduces the computation time to grab potentially thousands of rows.


<br>

## Gaps and Opportunities (Extensibility)

As the pipeline is highly scalable moving forward, there are many opportunities moving forward. For instance, as more use cases immerge throughout  the creation of new teams or new ideas at the company, other datasets housed on BIS’s website can be easily implemented into the data pipeline. In addition to this, this creates many opportunities to create more specified views for teams to access their data with ease.<br> 
Any gaps that exist stem from potential machine limitations when it comes to expanding the scope of the database to include most or all the datasets housed on BIS’s website. For instance, that massive increase in data will result in more space to house the data which may lead to further investment in hardware. So, while the pipeline itself will be highly scalable moving forward, the limitations of on-site hardware could potentially put hold on massively expanding the database.

[. . . I added a couple small paragraphs with what I was thinking given the nature of our database and data... Go ahead and adjust or expand as necessary]
[. . . Lane, feel free to weigh in; was going to extend comments at top about e.g., limited starting data set but otherwise opportunities to extend both consumption using current data and/or by adding additional, etc.]
