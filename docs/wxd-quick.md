# Quick Start

The following sections describe how to get started quickly with the watsonx.data developer system. If you are not familiar with the tools mentioned below, select the details link for more instructions.

* [Requesting an IBM userid](#ibm-userid)
* [Requesting a TechZone image](#requesting-a-techzone-image-individual-use)
* [Accessing the Image](#accessing-the-image-techzone)
* [SSH Access](#ssh-access)
* [Open Ports](#open-ports)
* [Passwords](#passwords)
* [Portainer Console](#portainer)
* [Documentation](#documentation)

## IBM Userid

An IBMid is needed to access IBM Technology Zone. If you do not have an IBMid, click on the following link and request a new IBMid.

<a href="https://techzone.ibm.com target="_blank">https://techzone.ibm.com</a>

More details: [Creating an IBM Userid](wxd-reference-ibmid.md)

## Requesting a TechZone image 

Log into TechZone (<a href="https://techzone.ibm.com" target="_blank">https://techzone.ibm.com</a>) and search for the watsonx.data
Developer Base Image or use the following link.

[https://techzone.ibm.com/collection/ibm-watsonxdata-developer-base-image](https://techzone.ibm.com/collection/ibm-watsonxdata-developer-base-image)

Problem with reservations failing? Check the TechZone status page at <a href="https://techzone.status.io" target="_blank">https://techzone.status.io</a>.

More details: [Reserving a TechZone image](wxd-reference-techzone.md)

## Accessing the Image 

The email from TechZone indicating that the image is ready will contain a link to your reservations. Click on the link and search for the watsonx.data reservation. 

More details: [Accessing a TechZone image](wxd-reference-access.md)

## SSH Access

Your TechZone reservation will include the server name and port number to use when connecting using ssh. The port number is referred to as <tt style="font-size: large; color: darkgreen;">port</tt> in the command below, while the server will be referred to as <tt style="font-size: large; color: darkgreen;">region.techzone-server.com</tt>. Replace these values with those found in your reservation.

Open a terminal window and use the following syntax to connect as the <code style="font-size: medium;color:blue;">watsonx</code> userid.

```
ssh -p port watsonx@region.techzone-server.com
```

The port number and server name are provided as part of the TechZone reservation details.

To become the root user, issue the following command.
```
sudo su -
```
Password for both users is <code style="color:blue;font-size:medium;">watsonx.data</code>.

You can copy files into and out of the server using the following syntax:

```
scp -P port myfile.txt watsonx@region.techzone-server.com:/tmp/myfile.txt
scp -P port watsonx@region.techzone-server.com:/tmp/myfile.txt myfile.txt
```

More details: [SSH Access](wxd-reference-ssh.md)

## Open Ports

The following URLs and Ports are used to access the watsonx.data services. Most browsers will work with these URLs. However, Mac OSX users should be aware that accessing the MinIO console may occasionally cause Firefox and Chrome to lock up. If you find that this occurs, try using Safari which appears to work fine.

The ports that are used in the lab are listed below. Note that the internal port number is always the same when running in the VMware image using the VM Remote Console. When using your workstation's browser, you will need to use the server name and port number supplied in the TechZone reservation. 

|Service|Port|Active|
|-------|------|----|
| watsonx.data management console|9443|Yes
| Presto console|8443|Yes
| MinIO console (S3 buckets)|9001|Yes
| MinIO S3 Endpoint|9000|Yes
| Portainer (Docker container management)|6443|Yes
| Apache Superset (Query and Graphing)|8088|**No**
| Jupyter Notebook|8888|Yes
| Presto External Port|8443|Yes
| Hive metadata Port|9043|Yes
| MySQL External Port|3306|Yes
| Postgres External Port|5432|Yes
| Db2 Database Port|50000|Yes
| VNC Port |5901|**No**

**Note**: The following ports are not active unless the service is started:

* Apache Superset (8088)
* VNC Terminal Display (5901)

More details: [Open Ports](wxd-reference-ports.md)

## Passwords

This table lists the passwords for the services that have "fixed" userids and passwords.

|Service|Userid|Password
|-------|------|--------|
|Virtual Machine|watsonx|watsonx.data
|Virtual Machine|root|watsonx.data
|watsonx.data UI|ibmlhadmin|password
|Jupyter Notebook|none|watsonx.data
|Presto|ibmlhadmin|password
|Minio|Generated|Generated
|Postgres|admin|Generated
|Apache Superset|admin|admin
|Portainer|admin|watsonx.data
|Db2|db2inst1|db2inst1
|MySQL|root|password
|VNC Windows|none|watsonx.
|VNC OSX|none|watsonx.data

Use the following commands to get the generated userid and password for MinIO.
```
export LH_S3_ACCESS_KEY=$(docker exec ibm-lh-presto printenv | grep LH_S3_ACCESS_KEY | sed "s/.*=//")
export LH_S3_SECRET_KEY=$(docker exec ibm-lh-presto printenv | grep LH_S3_SECRET_KEY | sed 's/.*=//') 
echo "MinIO Userid  : " $LH_S3_ACCESS_KEY
echo "MinIO Password: " $LH_S3_SECRET_KEY
```

Use the following command to get the password for Postgres.
```
export POSTGRES_PASSWORD=$(docker exec ibm-lh-postgres printenv | grep POSTGRES_PASSWORD | sed 's/.*=//')
echo "Postgres Userid   : admin"
echo "Postgres Password : " $POSTGRES_PASSWORD
```

You can get all passwords for the system when you are logged by issuing the following command:
```bash
cat /certs/passwords
```

If the passwords do not appear to work, you may need to regenerate them. The following must be run as the `root` user.

```bash
sudo su -
passwords
```

The `passwords` command will refresh the passwords and also display them. If this command is not run as root, an error message will be displayed because the password file cannot be updated as the `watsonx` user.

More details: [Passwords](wxd-reference-passwords.md)

## Portainer

This lab system has Portainer installed. Portainer provides an administrative interface to the Docker images that are running on this system. You can use this console to check that all the containers are running and see what resources they are using. Open your TechZone reservation and select the Portainer link to connect to it.

   * Credentials: userid: <code style="font-size: medium;color:blue;">admin</code> password: <code style="font-size: medium;color:blue;">watsonx.data</code>

More details: [Portainer](wxd-reference-portainer.md)   

## Documentation

The following links provide more information on the components in this lab.

* watsonx.data - <a href="https://www.ibm.com/docs/en/watsonxdata/1.0.x" target="_blank">https://www.ibm.com/docs/en/watsonxdata/1.0.x</a>
* Presto SQL - <a href="https://prestodb.io/docs/current/sql.html" target="_blank">https://prestodb.io/docs/current/sql.html</a>
* Presto Console - <a href="https://prestodb.io/docs/current/admin/web-interface.html" target="_blank">https://prestodb.io/docs/current/admin/web-interface.html</a>
* MinIO - <a href="https://min.io/docs/minio/linux/administration/minio-console.html" target="_blank">https://min.io/docs/minio/linux/administration/minio-console.html</a>
* Apache Superset - <a href="https://superset.apache.org/docs/creating-charts-dashboards/exploring-data" target="_blank">https://superset.apache.org/docs/creating-charts-dashboards/exploring-data</a>
* dBeaver - <a href="https://dbeaver.com/docs/wiki/Application-Window-Overview/" target="_blank">https://dbeaver.com/docs/wiki/Application-Window-Overview/</a>
* Db2 SQL - <a href="https://www.ibm.com/docs/en/db2/11.5?topic=queries-select-statement" target="_blank">https://www.ibm.com/docs/en/db2/11.5?topic=queries-select-statement</a>
* PostgreSQL SQL - <a href="https://www.postgresql.org/docs/current/sql.html" target="_blank">https://www.postgresql.org/docs/current/sql.html</a>
