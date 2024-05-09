# Using the watsonx.data Command Line
Connectivity to watsonx.data can be done using the following methods:

   * Command line interface (CLI)
   * JDBC drivers
   * watsonx.data UI 

In this lab you will learn how to use the Command line interface (CLI) to interact with the Presto engine.

### Connecting to watsonx.data

The examples in the section require that you use either a terminal SSH shell, or the SSH browser to issue commands to the Presto database engine.

Open a terminal window and use the following syntax to connect as the <code style="font-size: medium;color:blue;">watsonx</code> userid.

!!! abstract "Connect to the watsonx server using this command in a terminal window"
    ```bash
    ssh -p port watsonx@region.services.cloud.techzone.ibm.com
    ```

The port number and server name are provided as part of the TechZone reservation details.

To become the root user, issue the following command.

!!! abstract "Become the root user"
    ```bash
    sudo su -
    ```

Password for both users is <code style="color:blue;font-size:medium;">watsonx.data</code>.

!!! abstract "Change to the development directory."
    ```bash
    cd /root/ibm-lh-dev/bin
    ```
!!! abstract "Start the Presto Command Line interface."
    ```
    ./presto-cli
    ```

The output on your screen will look similar to the following:

![Browser](wxd-images/presto-output.png)

The arrows on the far right side indicate that there is more output to view. Press the right and left arrows on your keyboard to scroll the display.

![Browser](wxd-images/presto-scroll.png)

If the result set is small, all of the results will display on the screen and no scrolling will be available unless the results are wider than the screen size. 

When the display shows <code style="color:blue;font-size:medium;">(END)</code> you have reached the bottom of the output. If the display shows a colon (<code style="color:blue;font-size:medium;">:</code>) at the bottom of the screen, you can use the up and down arrow keys to scroll a record at a time, or the Page Up and Page Down keys to scroll a page at a time. To quit viewing the output, press the Q key.

!!! abstract "Quit the Presto CLI. The Presto quit command can be used with or without a semicolon."
    ```
    quit;
    ```

We are going to inspect the available catalogs in the watsonx.data system. A watsonx.data catalog contains schemas and references a data source via a connector. A connector is like a driver for a database. Watsonx.data connectors are an implementation of Presto’s SPI which allows Presto to interact with a resource. There are several built-in connectors for JMX, Hive, TPCH etc., some of which you will use as part of the labs.

!!! abstract "Reconnect to Presto"
    ```
    ./presto-cli
    ```

!!! abstract "Display the catalogs in the system."
    ```
    show catalogs;
    ```

<pre style="font-size: small; color: darkgreen; overflow: auto">
    Catalog    
---------------
 hive_data     
 iceberg_data 
 jmx           
 system        
 tpcds         
 tpch          
(6 rows)
</pre>

Let's look up what schemas are available with any given catalog. We will use the TPCH catalog which is an internal PrestoDB auto-generated catalog and look at the available schemas.

!!! abstract "Show schemas"
    ```
    show schemas in tpch;
    ```

<pre style="font-size: small; color: darkgreen; overflow: auto">
       Schema       
--------------------
 information_schema 
 sf1                
 sf100              
 sf1000             
 sf10000            
 sf100000           
 sf300              
 sf3000             
 sf30000            
 tiny               
(10 rows)
</pre>

!!! abstract "Quit the Presto CLI"
    ```
    quit;
    ```

You can connect to a specific catalog and schema and look at the tables etc.

!!! abstract "Connect to a specific schema"
    ```
    ./presto-cli --catalog tpch --schema tiny
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
presto:tiny>
</pre>
You will notice that the Presto prompt includes the name of the schema we are currently connected to.

!!! abstract "Look at the available tables in the TPCH catalog under the `tiny` schema."
    ```
    show tables;
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
  Table   
----------
 customer 
 lineitem 
 nation   
 orders   
 part     
 partsupp 
 region   
 supplier 
(8 rows)
</pre>

!!! abstract "Describe the customer table. Note that no catalog or schema name is required."
    ```
    describe customer;
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
   Column   |     Type     | Extra | Comment 
------------+--------------+-------+---------
 custkey    | bigint       |       |         
 name       | varchar(25)  |       |         
 address    | varchar(40)  |       |         
 nationkey  | bigint       |       |         
 phone      | varchar(15)  |       |         
 acctbal    | double       |       |         
 mktsegment | varchar(10)  |       |         
 comment    | varchar(117) |       |         
(8 rows)
</pre>

!!! abstract "You could also use the syntax below to achieve the same result."
    ```
    show columns from customer;
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
Column     |     Type     | Extra | Comment
-----------+--------------+-------+---------
custkey    | bigint       |       |
name       | varchar(25)  |       |
address    | varchar(40)  |       |
nationkey  | bigint       |       |
phone      | varchar(15)  |       |
acctbal    | double       |       |
mktsegment | varchar(10)  |       |
comment    | varchar(117) |       |
(8 rows)
</pre>
 
!!! abstract "List the functions that are available in the system."
    ```
    show functions like 'date%';
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
  Function   |       Return Type        |                         Argument Types                         | Function Type | Deterministic |                         Description                         | Variable Arity | Built In | Temporary | Language 
-------------+--------------------------+----------------------------------------------------------------+---------------+---------------+-------------------------------------------------------------+----------------+----------+-----------+----------
 date        | date                     | timestamp                                                      | scalar        | true          |                                                             | false          | true     | false     |          
 date        | date                     | timestamp with time zone                                       | scalar        | true          |                                                             | false          | true     | false     |          
 date        | date                     | varchar(x)                                                     | scalar        | true          |                                                             | false          | true     | false     |          
 date_add    | date                     | varchar(x), bigint, date                                       | scalar        | true          | add the specified amount of date to the given date          | false          | true     | false     |          
 date_add    | time                     | varchar(x), bigint, time                                       | scalar        | true          | add the specified amount of time to the given time          | false          | true     | false     |          
 date_add    | time with time zone      | varchar(x), bigint, time with time zone                        | scalar        | true          | add the specified amount of time to the given time          | false          | true     | false     |          
 date_add    | timestamp                | varchar(x), bigint, timestamp                                  | scalar        | true          | add the specified amount of time to the given timestamp     | false          | true     | false     |          
 date_add    | timestamp with time zone | varchar(x), bigint, timestamp with time zone                   | scalar        | true          | add the specified amount of time to the given timestamp     | false          | true     | false     |          
 date_diff   | bigint                   | varchar(x), date, date                                         | scalar        | true          | difference of the given dates in the given unit             | false          | true     | false     |          
 date_diff   | bigint                   | varchar(x), time with time zone, time with time zone           | scalar        | true          | difference of the given times in the given unit             | false          | true     | false     |          
 date_diff   | bigint                   | varchar(x), time, time                                         | scalar        | true          | difference of the given times in the given unit             | false          | true     | false     |          
 date_diff   | bigint                   | varchar(x), timestamp with time zone, timestamp with time zone | scalar        | true          | difference of the given times in the given unit             | false          | true     | false     |          
 date_diff   | bigint                   | varchar(x), timestamp, timestamp                               | scalar        | true          | difference of the given times in the given unit             | false          | true     | false     |          
 date_format | varchar                  | timestamp with time zone, varchar(x)                           | scalar        | true          |                                                             | false          | true     | false     |          
 date_format | varchar                  | timestamp, varchar(x)                                          | scalar        | true          |                                                             | false          | true     | false     |          
 date_parse  | timestamp                | varchar(x), varchar(y)                                         | scalar        | true          |                                                             | false          | true     | false     |          
 date_trunc  | date                     | varchar(x), date                                               | scalar        | true          | truncate to the specified precision in the session timezone | false          | true     | false     |          
 date_trunc  | time                     | varchar(x), time                                               | scalar        | true          | truncate to the specified precision in the session timezone | false          | true     | false     |          
 date_trunc  | time with time zone      | varchar(x), time with time zone                                | scalar        | true          | truncate to the specified precision                         | false          | true     | false     |          
 date_trunc  | timestamp                | varchar(x), timestamp                                          | scalar        | true          | truncate to the specified precision in the session timezone | false          | true     | false     |          
 date_trunc  | timestamp with time zone | varchar(x), timestamp with time zone                           | scalar        | true          | truncate to the specified precision                         | false          | true     | false     |          
(21 rows)

</pre>

!!! abstract "Switch to a different schema."
  ```
  use sf1;
  ```

!!! abstract "Display the Tables in the schema."
    ```
    show tables;
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
  Table   
----------
 customer 
 lineitem 
 nation   
 orders   
 part     
 partsupp 
 region   
 supplier 
(8 rows)
</pre>
 
!!! abstract "Query data from customer table."
    ```
    select * from customer limit 5;
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
 custkey |        name        |                 address                  | nationkey |      phone      | acctbal | mktsegment |                                                comment                                                
---------+--------------------+------------------------------------------+-----------+-----------------+---------+------------+-------------------------------------------------------------------------------------------------------
   37501 | Customer#000037501 | Ftb6T5ImHuJ                              |         2 | 12-397-688-6719 | -324.85 | HOUSEHOLD  | pending ideas use carefully. express, ironic platelets use among the furiously regular instructions.  
   37502 | Customer#000037502 | ppCVXCFV,4JJ97IibbcMB5,aPByjYL07vmOLO 3m |        18 | 28-515-931-4624 |  5179.2 | BUILDING   | express deposits. pending, regular deposits wake furiously bold deposits. regular                     
   37503 | Customer#000037503 | Cg60cN3LGIUpLpXn0vRffQl8                 |        13 | 23-977-571-7365 | 1862.32 | BUILDING   | ular deposits. furiously ironic deposits integrate carefully among the iron                           
   37504 | Customer#000037504 | E1 IiMlCfW7I4 1b9wfDZR                   |        21 | 31-460-590-3623 | 2955.33 | HOUSEHOLD  | s believe slyly final foxes. furiously e                                                              
   37505 | Customer#000037505 | Ad,XVdA6XAa0h aukZHUo5Mxh,ZRwVR3k7b7     |         3 | 13-521-760-7263 | 3243.15 | FURNITURE  | ites according to the quickly bold instru                                                             
(5 rows)

</pre>

!!! abstract "Gather statistics on a given table."
    ```
    show stats for customer;
    ```
<pre style="font-size: small; color: darkgreen; overflow: auto">
 column_name |  data_size  | distinct_values_count | nulls_fraction | row_count | low_value | high_value 
-------------+-------------+-----------------------+----------------+-----------+-----------+------------
 custkey     | NULL        |              150039.0 |            0.0 | NULL      | 1         | 150000     
 name        |   2700000.0 |              149980.0 |            0.0 | NULL      | NULL      | NULL       
 address     |   3758056.0 |              150043.0 |            0.0 | NULL      | NULL      | NULL       
 nationkey   | NULL        |                  25.0 |            0.0 | NULL      | 0         | 24         
 phone       |   2250000.0 |              150018.0 |            0.0 | NULL      | NULL      | NULL       
 acctbal     | NULL        |              140166.0 |            0.0 | NULL      | -999.99   | 9999.99    
 mktsegment  |   1349610.0 |                   5.0 |            0.0 | NULL      | NULL      | NULL       
 comment     | 1.0876099E7 |              149987.0 |            0.0 | NULL      | NULL      | NULL       
 NULL        | NULL        | NULL                  | NULL           |  150000.0 | NULL      | NULL       
(9 rows)

</pre>

!!! abstract "Quit Presto."
    ```
    quit;
    ```