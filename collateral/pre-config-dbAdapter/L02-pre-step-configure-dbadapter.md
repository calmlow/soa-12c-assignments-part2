# 1: Configure the DB Adapter and Data Sources

> This is a guide to help you setup the wsl console with a new database source

## 1.1 Details

| Name                                     | Value                             
| ---------------------------------------- |---------------------------------
| JNDI Name (in DataSources)               | *jdbc/MySQLExt*                   
| *JNDI Name* (in New Outbound Connection) | *eis/DB/MySQLExt*                 
| XADataSourceName                         | *jdbc/MySQLExt*                   
| Database Name                            | *demodb*                          
| Hostname                                 | _see whiteboard_                  
| Port                                     | *3306*                            
| Database User Name                       | *demodb*                          
| Password                                 | _see whiteboard_                  
| Custom JDBC URL (alternative)            | jdbc:mysql://hostname:3306/demodb 


## 1.2 Instructions

* 1.0 Open the wls console
* 1.1 Consult the image files on how to navigate
* 1.2 Goto: http://127.0.0.1:7101/console or http://<computername>:7101/console
* 1.3 Click the left menu choice DataSources
* 1.4 Add the Data Source. Select the _MySQL_ data type
* 1.5 Add the corresponding values from table
* 1.6 Add the Data Source
* 1.7 Add the outbound connection 
* 1.8 Mark the target *DefaultServer* checkbox and hit finish
* 1.9 Re-deploy the DbAdapter