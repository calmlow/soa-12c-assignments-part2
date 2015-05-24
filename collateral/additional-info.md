# Additional Information

## 1 SOAP UI - setup

* 1.1 Start Soap UI and add the WSDL so that you can easily call the to operations of the service

* 1.2 Some valid card-numbers stored in the database in the BANKACCOUNTS table

ROW_ID | CARDNUMBER | BALANCE   |  STATUS   
-------|------------|-----------|-----------
1      |  rta-456   |  1000     | valid
2      |  str-780   |  900      | valid
3      |  ers-156   |  25000    | valid
4      |  rbw-132   |  300      | valid
5      |  zxv-333   |  1000     | valid
6      |  pib-998   |  100      | valid



* 1.3 Supply your own unique *CorrelationID* and use the same for both calls, *RegisterAmount* and *EnterCardNumber*

* 1.4  The CashRegisterID field needs at least 7 characters
