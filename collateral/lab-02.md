# LAB 02 - DB Adapter

> In this lab we will cover 4 topics. DBAdapters, Correlations, SubProcesses and Faults (Extra)

## 1.0 Create a New Application and Project
Create the application and project base. Also add the given wsdl+schema onto composite with a BPEL component (defined interface later option)

### 1.1 Details

Name                  | Value           
--------------------- |----------------
Application Name      | SoaLabApp02
Package Prefix        | _leave blank_
Project Name          | PurchaseToPay
Start from:           | _Empty Composite_
Service Name          | DoPayment
WSDL File             | [PaymentTerminal.wsdl](WSDLs/PaymentTerminal.wsdl)
Schema File           | [PaymentTerminalSchema.xsd](Schemas/PaymentTerminalSchema.xsd)
BPEL Name             | ProcessPayment

### 1.2 Instructions

* 1.0 Create a new application and project
* 1.1 Add the Service, locate the given WSDL+schema, import to project if prompted
* 1.2 Add the BPEL (define interface later option)

### 1.3 Hints

* 1.1 Do not create applications and projects in directory paths that have spaces (for example, c:\Program Files).
* 1.2 [Hints](https://docs.oracle.com/middleware/1213/soasuite/develop-soa/soa-standards-architecture.htm#SOASE84935) for below questions.

### 1.4 Questions

* 1.1 What is the minimum version of the JDK for JDeveloper to run?
* 1.2 What is the maximum number of characters a Composite or component can have?
* 1.3 Can you re-arrange the file structure of the files created as long as they are under the directory SOA for composites?
* 1.4 What does the composite.xml file describe?

## 2 Add Receive, Assign and Reply activities
Make the flow work by making it retrieve you request and assign the reply with a message

### 2.1 Details

Name                  | Value           
--------------------- |----------------
Receive Operation     | RegisterAmount
Transaction           | Required
Input variable        | Receive1_RegisterAmount_InputVariable (default)
Output variable       | Reply1_RegisterAmount_OutputVariable (default)
ResponseCode          | 1
ResponseMessage       | concat('Amount ',$Receive1_RegisterAmount_InputVariable.part1/ns2:Amount  ,' registered.)

### 2.2 Instructions

* 2.0 Wire the Service with the BPEL

* 2.1 In BPEL view. Add a receive activity. Connect to the Partner Link. Choose correct operation. Setup default variable.

* 2.2 Add a Reply activity

* 2.3 Add an assign activity and add the outgoing message to ResponseMessage Field

* 2.3.1 Build (Alt-F9) ( and if ok build) Deploy your composite

### 2.3 Hints

* 2.0 Check the box _Create Instance_ in the Receive Activity
* 2.1 When deploying you composite, make sure to check: "Overwrite any existing composites with the same revision ID". In order to be able to re-deploy easily.

### 2.4 Questions

* 2.0 What is the error message or error messages after building the composite if any?
* 2.1 If above complies, how did you solve the problem?
* 2.2 What is the composite's revision ID?

## 3 Setup correlations and add a Pick Activity

### 3.1 Details

Name                          | Value         
----------------------------- |--------------
Correlation Set Name          | Correlation_Set1
Correlation Property          | MyCorrProperty (_type SimpleType String_)
Initiate (on 1st Receive)     | _Yes_
OnMessage operation           | EnterCardNumber
Input variable                | OnMessage_EnterCardNumber_InputVariable
Initiate (on OnMessage)       | _No_


### 3.2 Instructions

* 3.0 Open the Receive Activity and add a new Correlation set
* 3.1 Make Property Aliases to all the Request and Response Messages for both operations
* 3.2 Add a Pick Activity after the last reply in the BPEL and edit it to use the *EnterCardNumber* operation of the service
* 3.3 Auto-Create the input variable for this OnMessage
* 3.4 Add the newly created correlation set for this Activity. Set initiate to *No*

### 3.4 Hints

* 3.0 *Important* When in the Property Alias dialog, select the *part1* when selecting the type.
* 3.1 Below is how a property alias linking to a field on a message looks like. Inspect source of the file.

```XML
<vprop:propertyAlias propertyName="corr:MyCorrProperty" part="part1" messageType="tns:RegisterAmountRequestMessage">
    <vprop:query>sch:CorrelationID</vprop:query>
</vprop:propertyAlias>
```

### 3.3 Questions

* 3.1 What is the Property Alias for?
* 3.2 To what file is the Correlation Property added to?
* 3.3 To what file does the correlation wizard add the Property Alias information?

    
## 4 Adding the Database Adapter + Point to the new datasource

### 4.1 Details

Name                                 | Value         
------------------------------------ |--------------
DB Connection name                   | MySQLExt      
Database Name                        | demodb                   
Hostname                             | _see whiteboard_   
Port                                 | 3306                            
Database User Name                   | demodb                    
Password                             | _see whiteboard_                  
Custom JDBC URL (alternative)        | jdbc:mysql://hostname:3306/demodb
JNDI Name                            | eis/DB/MySQLExt (default)
Adapter Reference Name               | QuerySaldo
primary key                          | CARDNUMBER    
Table to query                       | BANKACCOUNTS 
Field to select                      | BALANCE       
parameter name (in select statement) | cardnr     

### 4.2 Instructions

* 4.0 In composite view. Drag and drop a Database Adapter into the External References swim lane

* 4.0.1 Give the Reference Name *QuerySaldo* to the DBAdapter

* 4.0.2 Setup the DB Connection for this Application. Use details from above table.

* 4.0.3 Follow the database adapter wizard, use the _SELECT_ operation only from the radio buttons. Select the table BANKACCOUNTS.

* 4.0.4 Step 5 of the wizard lets you override or define the primary key for your table.  In this case, no primary key is defined in the database, so you'll need to specify it.
Check BALANCE and leave the rest unchecked.

* 4.0.5 Add a new *parameter* to the db adapter wizard and call it *cardnr*

* 4.0.6 Build the query with help of the new parameter and make it only select *CARDNUMBER* that equals the parameter

* 4.1 Wire the DbAdapter to the BPEL. Invoke the DB Adapter from BPEL. Add both Input and Output variables with default names.

* 4.2 Query DB with correct field. Assign a value from the OnMessage_EnterCardNumber_InputVariable to the DB input variable before the Invoke to the DBAdapter.


### 4.3 Hints

* 4.1 Uncheck all fields of that is not of interest

* 4.2 Choose your DB connection from before. (hint: double-click to select it) 

* 4.3 Your select statement should look like _SELECT CARDNUMBER, BALANCE FROM BANKACCOUNTS WHERE (CARDNUMBER = #cardnr)_

### 4.4 Questions

* 4.1 What's the name of the swim lane where you placed the DB Adapter?
* 4.2 What's the name of the swim lane where the Mediator/BPEL component lives? 
* 4.3 Does the database adapter require a primary key for when interacting with tables?


## 5 Add a Subprocess

### 5.1 Details

Name                            | Value           
--------------------------------|---------------------
Subprocess component name       | ValidateCashRegisterID
In variable name                | In (simple type String)
Out variable name               | Out  (simple type String)
If-condition                    | string-length($In) > 6)
BPEL subprocess response var    | ValidateCashRegisterIDResponse
Pass Out variable as...         | Uncheck "Copy By Value"
Call Activity name              | Call1 (default)
Variable for storing response   | SubprocessResponseValue

### 5.2 Instructions

* 5.0 Add a Subprocess in the Composite Editor.

* 5.1 Open up the Subprocess. Add 2 variables (X-symbol in the scope pane) and add a IF-activity with a condition, input variable needs to be at least 7 characters long.

* 5.2 Add Assign activity to both IF and ELSE sections. If section assigns "OK" to Out variable. Else section assigns "NOT OK" to Out variable.

* 5.2 From the BPEL add a call activity and call the Subprocess

* 5.2.1 Setup the call with the CashRegisterID as input and *SubprocessResponseValue* for storing the response

### 5.3 Questions

* 5.3.1 Does subprocesses exist in 11g?
* 5.3.2 What are good scenarios to use subprocesses?
* 5.3.3 What is implied when you _not_ check "Copy By Value" checkbox?
* 5.3.4 What is file ending of the file the subprocess created?

## 6 Wrap it up. Add a Reply
Reply from to the second operation on the OnMessage branch, with values from the subprocess and the DB Adapter.

### 6.1 Instructions

* 6.0 Add an Reply activity for the EnterCardNumber
* 6.1 Add an Assign and give the reply variables some nice values
* 6.1.1 Build and deploy composite. Setup SOAP UI according to [this page](https://github.com/calmlow/12clabs-wip/blob/master/collateral/additional-info.md) if needed.
* 6.1.2 Try and call the EnterCardNumber operation before the RegisterAmount operation

### 6.2 Questions

* 6.0 Why does the service timeout when trying to call the second operation before the first? 


## 7.0 (Extra) WriteAccountStatement - Add the FlowN Activity and a DB Write Adapter

### 7.1 Details

Name                              | Value                    
--------------------------------- |--------------------------
DB Write Adapter name             | *WriteAccountStatement*   
DB Connection                     | *MySQLExt*             
Operation Type                    | *Insert only*           
Import Table                      | ACCOUNTSTATEMENTS     
Attribute Filtering               | *deselect tstamp*    
Invoke Name of db writer in BPEL  | InvokeDBWriter     

### 7.2 Instructions

* 7.1 Drag a Flow Activity to the OnMessage Area

* 7.2 In the composite view, Construct DB Write Adapter in wizard

* 7.3 Wire the Write DB Adapter to the BPEL.

* 7.4 In BPEL Invoke the DB Writer from the second sequence in the FlowN activity.

* 7.5 Assign appropriate values to the DB Write call variable. Include the amount from first call & the card number.

* 7.6 Assign the necesary response messages

* 7.7 Use concat to assign some nice response messages along with the variables retrieved from output of db call.

## 8.0 (Extra) Add fault handling

### 8.1 Instructions

* 8.0 Add a catchAll activity to catch a negative response from the subprocess

* 8.1 Deploy your app

