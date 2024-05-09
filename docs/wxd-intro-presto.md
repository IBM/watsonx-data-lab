# Using the Presto console UI
Your TechZone reservation will include the server name and port number to use when connecting to the Presto UI. 

!!! abstract "Find the URL in your reservation that reads Presto console - https://useast.services.cloud.techzone.ibm.com:xxxx and click on it"

You will be presented with a logon screen.

![Browser](wxd-images/wxd-intro-presto-login.png)

!!! abstract "Enter the following credentials"

    * Username: ibmlhadmin
    * Password: password

The Presto console allows you to do the following:

   * Monitor state of the cluster
   * Queries being executed
   * Queries in queue
   * Data throughput 
   * Query details (text and plan)

**Note**: The Presto console is very valuable when it comes to diagnosing problems with any queries you run in the watsonx.data environment. If a query fails you can find more details in the Presto console using the instructions below.

When initially connecting to the Presto console, you will be requested to enter a userid and password.

![Browser](wxd-images/watsonx-presto-login.png)
   
![Browser](wxd-images/presto-main.png)
 
On the main Presto screen, click the Finished Button (middle of the screen).

![Browser](wxd-images/presto-finished.png)
 
A list of finished queries will display below the tab bar. You can scroll through the list of queries and get details of the execution plans. If you scroll through the list, you should see the test query "select * from customer limit 5". If you had a query that failed, look for the SQL in this list and continue on with the next step.

![Browser](wxd-images/presto-limit-5.png)
 
Click on the query ID to see details of the execution plan that Presto produced.

![Browser](wxd-images/presto-query-details.png)
 
You can get more information about the query by clicking on any of the tabs that are on this screen. For instance, the Live Plan tab will show a visual explain of the stages that the query went through during execution. Scrolling to the bottom of this screen will also display any error messages that may have been produced by the SQL.

![Browser](wxd-images/presto-live-plan.png)

Take time to check out the other information that is available for the query including the stage performance.