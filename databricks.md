**CREATING AZURE  DATABRICKS WORKSHOP**

Step1: Search AZURE databricks-> Click 'Create' 

Choose your Resource Group -> Give a name to your workspace -> Add a region -> Choose your ADLS Storage Account and Container

Pricing Tier: standard

Click 'Review and Create'

After Deployment Click 'Go to resource' -> lunch the databricks'

step 2: Create compute

Go to Compute on the left menu --> Create compute

Use these exact specs to keep it cheap:

Name: (No spaces!)

Policy: Set to Unrestricted

Runtime: Stick to 16.4 LTS (Scala 2.12, Spark 3.5.2)

Photon Acceleration: Check this 

Node Type: Pick Standard_D4ds_v5 (16GB Mem, 4 Cores)

Cluster Mode: Check Single node 

Termination: Change 120 to 15 mins of inactivity

Access Mode: Set to Single user 

step 3:Upload file

Open the DBFS file browser (via Catalog or File menu)

Click Upload.

Drop your file.. Don't touch the advanced settings

**Code for retriving the json file from storage account to databricks notebook**

storage account -> containers -> file (eq.csv,json) -> three dots -> copy url of the file path

**Set the configuration to use the Access Key**
```
spark.conf.set(
    "fs.azure.account.key.<your_storage_account_name>.dfs.core.windows.net",
    # eg:"fs.azure.account.key.asa001pr.dfs.core.windows.net"
    "<storageaccount_access key>"
)
abfss_path = "Json dataset url"
df = spark.read.format("json").option("multiline", "true").load(abfss_path)
display(df)
df.createOrReplaceTempView("json_dataset")
result = spark.sql("SELECT * FROM json_dataset LIMIT 10")
display(result)
```


***Databricks Notebook Code for mounting the date from the adls account Mount ADLS Gen2**
```
storage_account_name = "mystorageacct"
container_name = "data"
storage_key = "YOUR_STORAGE_KEY"

mount_point = "/mnt/data"

dbutils.fs.mount(
    source=f"wasbs://{container_name}@{storage_account_name}.blob.core.windows.net",
    mount_point=mount_point,
    extra_configs={f"fs.azure.account.key.{storage_account_name}.blob.core.windows.net": storage_key}
)

df = spark.read.json("/mnt/data/customers.json")
df.show()
```
