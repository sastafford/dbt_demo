# dbt on Databricks

## Start using data build tool (dbt) on Databricks

First, make sure that [pipenv](https://pipenv.pypa.io/en/latest/installation.html) is installed in your environment.  This will help set up a virtual Python environment.  

Next, you need to install dbt and the adapter for Databricks.  The following command will install the dbt-databricks library from PyPi and required dependencies.  
 
```
pipenv install
pipenv shell
```

Now, you need to configure the [connection profile to Databricks](https://docs.getdbt.com/reference/resource-configs/databricks-configs).  Make a copy of template_profiles.yml and rename profile.yml.  

### profile.yml 

| name | value | 
| ---- | ----- |
| host | If using an All-Purpose cluster, enter the Server Hostname value from the Advanced Options, JDBC/ODBC tab for your Databricks cluster. If using a SQL warehouse, enter the Server Hostname value from the Connection Details tab for your SQL warehouse. |
| http_path | For an existing All-Purpose cluster, enter the HTTP Path value from the Advanced Options, JDBC/ODBC tab for your Databricks cluster. For a SQL warehouse, enter the HTTP Path value from the Connection Details tab for your SQL warehouse. |
| schema | Enter the value for an existing schema |
| token  | Enter a personal access token |

Start up your cluster referenced by your profile.yml.  Run the following dbt command to validate your connections.  Run the command from the project home directory.  
 ```
 dbt debug --profiles-dir . --project-dir dbt_pvc_demo/ 
 ```

If everything is setup, then you should see a message indicating **All Checks Passed**

The first thing is to populate a seed table for the purpose of this example.  The following command will take the csv file in the dbt_pvc_demo/seeds directory and create a table in the schema referenced in profile.yml.  

```
dbt seed --profiles-dir . --project-dir dbt_pvc_demo/
```

You should see a table named diamonds in your schema.  

The following command will execute the sql declarations under the dbt_pvc_demo/models/diamonds directory.
```
dbt run --profiles-dir . --project-dir dbt_pvc_demo/
```

