# Reporting/Dashboarding using Apache Superset

Apache Superset is not a part of watsonx.data and is only used to demonstrate the capability to connect to watsonx.data from other BI/Reporting tools. You will need to install Apache Superset as part of this lab. The Superset repository needs to be in sync with the image being downloaded, so these libraries cannot be preloaded into this development image.

Open a terminal window and connect via SSH as the `watsonx` user. Do not connect as the root user.

Clone the Apache Superset repository with the git command. This command typically takes less than 1 minute to download the code.

```
git clone https://github.com/apache/superset.git
```

The `docker-compose-non-dev.yml` file needs to be updated so that Apache Superset can access the same network that watsonx.data is using. 

```
cd ./superset
cp docker-compose-non-dev.yml docker-compose-non-dev-backup.yml

sed '/version: "3.7"/q' docker-compose-non-dev.yml > yamlfix.txt
cat <<EOF >> yamlfix.txt
networks:
  default:
    external: True
    name: ibm-lh-network
EOF
sed -e '1,/version: "3.7"/ d' docker-compose-non-dev.yml  >> yamlfix.txt
```

We update the Apache Superset code to version `2.1.0`.
```
sed 's/\${TAG:-latest-dev}/2.1.0/' yamlfix.txt > docker-compose-non-dev.yml
```

Use docker-compose to start Apache Superset.
```
docker compose -f docker-compose-non-dev.yml up
```

The `docker compose` command will download the necessary code for Apache Superset and start the service. The terminal session will contain the logging information for the service. The process is running in the foreground so you will see all the messages being produced by the program. If you want to stop the service at any time you will need to press CTRL-C. If you close this terminal window at any time, the process will stop.

When you see "Init Step 4/4", the service is ready for connections.

![Browser](wxd-images/superset-1.png)

If you have already installed Apache Superset and you stopped it, there is no need to reinstall the program. Go back to the `/home/watsonx/superset` directory and run the `docker compose` program again:

Once the service is running, open your browser and navigate to the URL and port that were provided in your TechZone reservation.

The credentials for Apache Superset are userid `admin`, Password `admin`.

![Browser](wxd-images/superset-2.png)
  
### Setup a Database Connection to watsonx.data
Open another terminal window for this next step. Once Apache Superset has started loading examples, you can issue the following command as `watsonx` or `root`.
```
docker cp /certs/lh-ssl-ts.crt superset_app:/tmp/lh-ssl-ts.crt
```
In the Apache Superset console,  press the Settings button on the far right and select Database connections.

![Browser](wxd-images/superset-3.png) 

Then select the [+ DATABASE] option on the far-right side of the panel.
 
![Browser](wxd-images/superset-4.png) 

A connection dialog will display.

![Browser](wxd-images/superset-5.png)
 
Select Presto as the database connection type.
 
![Browser](wxd-images/superset-6.png)

In the SQLALCHEMY URI field, enter the following information to connect to the hive_data catalog which contains the GOSALES, ONTIME, and TAXI data.
```
presto://ibmlhadmin:password@ibm-lh-presto-svc:8443/hive_data
```

Enter the following information to connect to the iceberg_data catalog which will contain any tables you created when running the examples in the lab.
```
presto://ibmlhadmin:password@ibm-lh-presto-svc:8443/iceberg_data
```

Select the Advanced tab.

![Browser](wxd-images/superset-7.png)
 
Copy the following information into the security box.
```
{"connect_args":{"protocol":"https","requests_kwargs":{"verify":"/tmp/lh-ssl-ts.crt"}}}
```

![Browser](wxd-images/superset-8.png)
 
Press the Connect button to create the connection.

### Create reports/charts/dashboards
Once the connection has been tested and created for watsonx.data, we can click on Dataset and create a new dataset based on the customer table in the tiny schema. Reports/dashboards can then be created using the very intuitive Superset interface. 

**Note**: The Apache Superset team removes, inserts and updates charts on a frequent basis with no advance notification. The example you see below may not be exactly the same when you run the code. This is not something that we can control in the demonstration environment.

Select Datasets at the top of the Apache Superset window.

![Browser](wxd-images/superset-9.png)
 
Press [+ DATASET]. 

![Browser](wxd-images/superset-10.png)
 
In the Database field, select Presto.

![Browser](wxd-images/superset-11.png)
 
The schemas will take a few seconds to load. Select the `workshop` schema.

![Browser](wxd-images/superset-12.png)
 
Select customer from the list.

![Browser](wxd-images/superset-13.png)
 
The display will show the columns associated with this table. On the bottom right-hand corner is a button named CREATE DATASET AND CREATE CHART. Press that to display the following panel.

![Browser](wxd-images/superset-14.png)
 
To create a simple Bar Chart, we start by selecting the Bar Chart icon. If you click it once it displays information about the chart type. If you double-click it, the chart builder screen will display. 

![Browser](wxd-images/superset-15.png)
 
Click on the `mktsegment` field and drag it into the DIMENSIONS field. Then drag the `acctbal` field into the METRICS field. The program will ask how the field is to be computed. Select AVG from the list and SAVE.

![Browser](wxd-images/superset-16.png)
 
Now press the CREATE CHART button found at the bottom of the screen.

![Browser](wxd-images/superset-17.png)

Try to create different charts/dashboards if you have time.

**Note**: When you are finished using Apache Superset, press CTRL-C (Control-C) in the terminal window that you used to start it. This will stop the program and release the resources it is using. If you press CTRL-C twice, it immediately kills the program, but it may lose some of the work that you may have done.
 
 ![Browser](wxd-images/superset-18.png)
