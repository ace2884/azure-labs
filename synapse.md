**CREATING AZURE SYNAPSE WORKSPACE**

STEP 1 : Search AZURE SYNAPSE ANALYTICS -> Click 'Create' 

STEP 2 : Choose your Resource Group -> Give a name to your workspace -> Add a region -> Choose your ADLS Storage Account and Container

STEP 3 : Click 'Review and Create'

STEP 4 : After Deployment Click 'Go to resource' -> Open the 'Workspace web URL'

STEP 5 : In the Web Page Select the 2nd icon(Data) in the left panel

STEP 6 : Linked -> Azure Data Lake Storage Gen2 -> Open your container

STEP 7 : Upload a File

STEP 8 : Click 'New SQL script' -> Write your query -> Publish -> Run
 
 Open Synapse Studio
 
 Go to Database → Workspace
 
 Click + → Lake database
 
 Name it → Create
 
 Click + → Lake table
 
 Give table name
 
 Select lowest leaf ADLS folder
 
 Click Create
 
 Click Publish All
 
 Go to Develop → + → SQL script
 
 Change Use database: master → Database1
 
 Run SQL queries



 Step 1: Create master key (only once per DB)
 
```CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'StrongP@ssw0rd123!';```

Step 2: Create scoped credential

```

CREATE DATABASE SCOPED CREDENTIAL <storage account>
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'sv=2024-11-04&ss=bfqt&srt=sco&sp=rwdlacupyx&se=2025-10-15T21:31:57Z&st=2025-10-15T13:16:57Z&spr=https&sig=YvmWVu71HokS3Wf6nJjG4oOVNzISS%2F%2BaDjA2RoykC%2Bo%3D'
```
Step 3: Create external data source

```
CREATE EXTERNAL DATA SOURCE MyADLS2DataSource3
WITH (
    TYPE = HADOOP,
    LOCATION = 'abfss://inputdatacontainer@azadls13253876.dfs.core.windows.net/',
    CREDENTIAL = MyADLS2Credential3);
```

Step 4: Create file format

```
CREATE EXTERNAL FILE FORMAT CSVFormat3
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',', STRING_DELIMITER = '"')
);
```

Step 5: Create external table

```
CREATE EXTERNAL TABLE dbo.Employeetable
(
    Id INT,
    Names VARCHAR(50),
    Email varchar(30),
    DOJ date,
    Project VARCHAR(50),
    Salary VARCHAR(20)
)
WITH (
    LOCATION = '/emp.csv',
    DATA_SOURCE = MyADLS2DataSource3,
    FILE_FORMAT = CSVFormat3
);
```
