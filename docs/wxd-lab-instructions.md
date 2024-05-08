# Lab Instructions

Lab instructions may contain three types of information:

* Screen (UI) interactions 
* Text commands
* Dialog Close

#### Keyboard or Mouse Action

When a keyboard or mouse action is required, the text will include a box with the instructions.

!!! abstract "Select the Infrastructure Icon in the watsonx.data UI"

#### Command Text

Any text that needs to be typed into the system will be outlined in a grey box. 

!!! abstract "Enter this text into the SQL window and Run the code"

    ```
    SELECT
      *
    FROM
      "hive_data"."ontime"."ontime"
    LIMIT
      10;
    ``` 

A copy icon is usually found on the far right-hand side of the command box. Use this to copy the text and paste it into your dialog or command window. You can also select the text and copy it that way. Once you have copied the text, paste the value into the appropriate dialog using the paste command or menu.

!!! warning "Cut and Paste Considerations"

      * Some commands may span multiple lines, so make sure you copy everything in the box if you are not using the copy button.
      * Commands pasted into a terminal window will require that you hit the `Return` or `Enter` key for the command to be executed.
      * Commands pasted into a Presto CLI window will execute automatically.

#### Dialog Close 

There are instructions that will tell you to close the current dialog.

!!! abstract "Close the dialog by pressing the [x] in the corner"

In many cases you will also be able to use the Escape key to close the current window.

### Icon Reference

There are certain menu icons that are referred to throughout the lab that have specific names:

* Hamburger menu <span style="font-style:bold; color:blue;">&equiv;</span>
  
     This icon is used to display menu items that a user would select from.

* Kebab menu <span style="font-style:bold; color:blue;">&vellip;</span>

     This icon is usually used to indicate that there are specific actions that can be performed against an object.

* Twisty <span style="font-style:bold; color:blue;">&#9658;</span> and <span style="font-style:bold; color:blue;">&#9660;</span>

     Used to expand and collapse lists.
  

### URL Conventions

Your TechZone reservation contains a number of URLs for the services provided in the watsonx.data server. The URL will contain the name of the server and the corresponding port number for the service. Throughout the documentation, the server name will be referred to as <tt style="font-size: large; color: darkgreen;">region.services.cloud.techzone.ibm.com</tt> and port number is referred to as <tt style="font-size: large; color: darkgreen;">port</tt>. Where you see these URLs, replace them with the values found in your reservation.

## System Status

The watsonx.data server is started as part of the lab. To make sure that the services are running, open a terminal session and use the following command to connect to the watsonx.data server.

!!! abstract "SSH into the watsonx.data server"
      ```bash
      ssh -p port watsonx@region.services.cloud.techzone.ibm.com
      ```

Password is <code style="color:blue;font-size:medium;">watsonx.data</code>.
Next switch to the root userid and change to the watsonx.data bin directory.

!!! abstract "Switch to the root user"
      ```bash
      sudo su -
      ```

!!! abstract "Change to the bin directory"
      ```bash
      cd /root/ibm-lh-dev/bin
      ```

You can check the watsonx.data system status with the following command.

!!! abstract "Watsonx.data status"
      ```bash
      ./status.sh --all
      ```

Output will look similar to:
<pre style="font-size: small; color: darkgreen; overflow: scroll">
using /root/ibm-lh-dev/localstorage/volumes as data root directory for user: root/1001 
infra config location is /root/ibm-lh-dev/localstorage/volumes/infra
lhconsole-ui				running			0.0.0.0:9443->8443/tcp, :::9443->8443/tcp
lhconsole-nodeclient-svc		running			3001/tcp
lhconsole-javaapi-svc			running			8090/tcp
lhconsole-api				running			3333/tcp, 8081/tcp
ibm-lh-presto				running			0.0.0.0:8443->8443/tcp, :::8443->8443/tcp
ibm-lh-hive-metastore			running			
ibm-lh-postgres				running			5432/tcp
ibm-lh-minio				running			
</pre>

Continue on to the [Presto Engine Test](#presto-engine-test) section.

## System Utilities

There are several commands that have been added to the watsonx.data developer edition to make it easier to manage the system. These commands are not part of the original installation, but are provided as a convenience for managing the system.

All commands require that you connect to the server as the root user. Use the following commands to connect to the watsonx.data system.

!!! abstract "SSH into the watsonx.data server"
      ```bash
      ssh -p port watsonx@region.services.cloud.techzone.ibm.com
      ```

Password is <code style="color:blue;font-size:medium;">watsonx.data</code>.
Next switch to the root userid.

!!! abstract "Switch to the root user"
      ```bash
      sudo su -
      ```

As the root user, switch to the bin directory.

!!! abstract "Switch to the bin directory"
      ```bash
      cd /root/ibm-lh-dev/bin
      ```

### Watsonx.data Shutdown

Use the `stop-watsonx` command to stop the watsonx.data system.

!!! abstract "Watsonx.data shutdown"
      ```bash
      stop-watsonx
      ```

The `stop-watsonx` command will stop the Milvus service before stopping the watsonx.data services. If you stop watsonx.data prior to Milvus, the watsonx.data catalog will not be properly updated with the Milvus status, and error messages will be generated about invalid SQL insert statements. This is caused by the catalog service (hosted in PostgreSQL) becoming unavailable because of it being shutdown.

You can manually stop the service using either of these commands:

!!! abstract "Stop watsonx.data service"
      ```bash
      systemctl stop watsonx.service
      ```

Alternatively, you can use the following commands. You must be in the `/root/ibm-lh-dev/bin` directory to issue these commands.

!!! abstract "Stop watsonx.data using direct commands"
      ```bash
      cd /root/ibm-lh-dev/bin
      ./stop-milvus
      ./stop
      ```

Make sure you follow the exact sequence of commands or else you will get a shutdown error with Milvus.

### Watsonx.data Startup

To start the watsonx.data system, use the `start-watsonx` command.

!!! abstract "Watsonx.data startup"
      ```bash
      start-watsonx
      ```

The `start-watsonx` command will start watsonx.data service before the Milvus service. If you start Milvus prior to starting watsonx.data, the watsonx.data catalog will not be properly updated with the Milvus status, and error messages will be generated about invalid SQL insert statements. 

You can manually start the service using either of these commands:

!!! abstract "Start watsonx.data service"
      ```bash
      systemctl start watsonx.service
      ```

Alternatively, you can use the following commands. You must be in the `/root/ibm-lh-dev/bin` directory to issue these commands.

!!! abstract "Stop watsonx.data using direct commands"
      ```bash
      cd /root/ibm-lh-dev/bin
      export LH_RUN_MODE=diag
      ./start 
      ./start-milvus
      ```

!!! note "Startup time"
      Using the `systemctl` or `start` command will take a few minutes to complete. There will not be any status messages displayed when using the `systemctl` command, so if you want to watch the startup sequence, use the `start` command instead. You must set `LH_RUN_MODE` to `diag` to open up the ports in the various watsonx.data containers. If you do not set this environment variable, you will not be able to access several services externally.

### System Status

You can check the status with the following command.

!!! abstract "System Status"
      ```bash
      ./status.sh --all
      ```

Output will look similar to:
<pre style="font-size: small; color: darkgreen; overflow: scroll">
using /root/ibm-lh-dev/localstorage/volumes as data root directory for user: root/1001 
infra config location is /root/ibm-lh-dev/localstorage/volumes/infra
lhconsole-ui				running			0.0.0.0:9443->8443/tcp, :::9443->8443/tcp
lhconsole-nodeclient-svc		running			3001/tcp
lhconsole-javaapi-svc			running			8090/tcp
lhconsole-api				running			3333/tcp, 8081/tcp
ibm-lh-presto				running			0.0.0.0:8443->8443/tcp, :::8443->8443/tcp
ibm-lh-hive-metastore			running			
ibm-lh-postgres				running			5432/tcp
ibm-lh-minio				running			
</pre>

### Stop and Start Presto

If you need to start or stop the Presto engine, use the following commands.

!!! abstract "Stop Presto"
      ```bash
      stop-presto
      ```

Alternatively, you can use the watsonx.data commands.

!!! abstract "Stop Presto using watsonx.data commmands"
      ```bash
      cd /root/ibm-lh-dev/bin
      ./stop_service ibm-lh-presto
      ```

To start Presto, use the following commands.

!!! abstract "Start Presto"
      ```bash
      start-presto
      ```

Alternatively, you can use the watsonx.data commands.

!!! abstract "Start Presto using watsonx.data commmands"
      ```bash
      cd /root/ibm-lh-dev/bin
      export LH_RUN_MODE=diag
      ./start_service ibm-lh-presto
      ```

To check on the current status of the Presto engine, use the `check-presto` command.

!!! abstract "Check Presto status"
      ```bash
      check-presto
      ```

The command will print out dots while it waits for the service to become available.
<pre style="font-size: small; color: darkgreen">
Waiting for Presto to start.
...........................
Ready
</pre>

## Presto Engine Test
Check the Presto engine by connecting to a schema. 

!!! note "Java Error Messages"
      If the Presto engine has not yet started (you didn't run the check-presto command), the next command may result in a useless Java error message. You may need to wait for a minute for attempting to run the statement again.

Check the record count of the customer table. 

!!! abstract "Get record count from customer table"

      ```bash
      ./presto-run  --catalog=tpch <<< "select * from tiny.customer limit 10;"
      ```

All Presto commands end with a semi-colon. The result set should include the a number of rows (the results will be random).

<pre style="font-size: small; color: darkgreen; overflow: auto">
presto> select * from tiny.customer limit 10;
 custkey |        name        |                address                 | nationkey |      phone      | acctbal | mktsegment |                                                  comment                                                   
---------+--------------------+----------------------------------------+-----------+-----------------+---------+------------+------------------------------------------------------------------------------------------------------------
    1126 | Customer#000001126 | 8J bzLWboPqySAWPgHrl4IK4roBvb          |         8 | 18-898-994-6389 | 3905.97 | AUTOMOBILE | se carefully asymptotes. unusual accounts use slyly deposits; slyly regular pi                             
    1127 | Customer#000001127 | nq1w3VhKie4I3ZquEIZuz1 5CWn            |        10 | 20-830-875-6204 | 8631.35 | AUTOMOBILE | endencies. express instructions wake about th                                                              
    1128 | Customer#000001128 | 72XUL0qb4,NLmfyrtzyJlR0eP              |         0 | 10-392-200-8982 | 8123.99 | BUILDING   | odolites according to the regular courts detect quickly furiously pending foxes? unusual theodolites use p 
    1129 | Customer#000001129 | OMEqYv,hhyBAObDjIkoPL03BvuSRw02AuDPVoe |         8 | 18-313-585-9420 | 6020.02 | HOUSEHOLD  | pades affix realms. pending courts haggle slowly fluffily final requests. quickly silent deposits are. iro 
    1130 | Customer#000001130 | 60zzrBpFXjvHzyv0WObH3h8LhYbOaRID58e    |        22 | 32-503-721-8203 | 9519.36 | HOUSEHOLD  | s requests nag silently carefully special warhorses. special accounts hinder slyly. fluffily enticing      
    1131 | Customer#000001131 | KVAvB1lwuN qHWDDPNckenmRGULDFduxYRSBXv |        20 | 30-644-540-9044 |  6019.1 | MACHINERY  | er the carefully dogged courts m                                                                           
    1132 | Customer#000001132 | 6dcMOh60XVGcGYyEP                      |        22 | 32-953-419-6880 | 4962.12 | AUTOMOBILE | ges. final, special requests nag carefully carefully bold deposits. ironic requests boost slyly through th 
    1133 | Customer#000001133 | FfA0o cMP02Ylzxtmbq8DCOq               |        14 | 24-858-762-2348 | 5335.36 | MACHINERY  | g to the pending, ironic pinto beans. furiously blithe packages are fina                                   
    1134 | Customer#000001134 | 79TYt94ty a                            |         9 | 19-832-924-7391 | 8458.26 | HOUSEHOLD  | riously across the bold instructions. quickly                                                              
    1135 | Customer#000001135 | cONv9cxslXOefPzhUQbGnMeRNKL1x,m2zlVOj  |        11 | 21-517-852-3282 | 3061.78 | FURNITURE  | regular frays about the bold, regular requests use quickly even pin                                        
(10 rows)

Query 20240508_171644_00060_egvws, FINISHED, 1 node
Splits: 21 total, 21 done (100.00%)
[Latency: client-side: 0:06, server-side: 0:06] [1.5K rows, 0B] [260 rows/s, 0B/s]

presto> 
</pre>

Congratulations, your system is now up and running!