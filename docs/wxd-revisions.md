# Revisions

### February 29, 2024 (1.1.2)

* SSL connection for data sources
    
    You can now enable SSL connection for the following data sources by using the Add database user interface to secure and encrypt the database connection :

    * Db2
    * PostgreSQL
    * IBM Data Virtualization Manager for z/OS

    For IBM Data Virtualization Manager for z/OS and PostgreSQL, select Validate certificate to validate whether the SSL certificate that is returned by the host is trusted.

    For the IBM Data Virtualization Manager for z/OS data source, you can choose to provide the hostname in the SSL certificate.

* Secure ingestion job history

    Now users can view only their own ingestion job history. Administrators can view the ingestion job history for all users.

* New data types BLOB and CLOB for SAPHANA and Teradata data sources
    
    New data types BLOB and CLOB are available for SAPHANA and Teradata data sources. You can use these data types only with SELECT statements in the Query workspace to build and run queries against your data. 

* Use more SQL statements
    
    You can now use the following SQL statements in the Query workspace to build and run queries against your data:

    Apache Iceberg data sources:

    * CREATE VIEW
    * DROP VIEW

    MongoDB data sources:

    * DELETE

* Create a new table during data ingestion

    Previously, you had to have a target table in watsonx.data for ingesting data. Now, you can create a new table directly from the source data file (available in parquet or CSV format) by using data ingestion through the watsonx.data user interface. You can create the table by using the following methods of ingestion:

    * Ingesting data by using Iceberg copy loader
    * Ingesting data by using Spark

* Perform ALTER TABLE operations on a column

    With an Iceberg data source, you can now perform ALTER TABLE operations on a column for the following data type conversions:

    * int to bigint
    * float to double
    *  decimal (num1, dec_digits) to decimal (num2, dec_digits), where num2>num1.

* Better query performance by using sorted files
    
    With an Iceberg data source, you can generate sorted files, which reduce the query result latency and improve the performance of Presto.

    Data in the Apache Iceberg table is sorted during the writing process within each file. You can configure the order to sort the data by using the sorted_by table property. When you create the table, specify the array of columns involved in sorting.

* Exposing Hive metastore port details (Developer edition)
    
    You can now expose the Hive metastore port details outside the watsonx.data developer edition's host to facilitate connection from external applications (services outside of docker or Podman), such as the integration with Db2, and Spark to watsonx.data.

### January 25, 2024 (1.1.1)

* Updated Lab Documentation

    - [Instructions for using a Workshop environment](wxd-reference-workshop.md)
    - [New section on user administration and creating policies](wxd-useradmin.md)
    - [Update to running terminal commands uses Jupyter notebook shell](wxd-reference-ssh.md#jupyter-notebook-terminal)

### January 8, 2024 (1.1.1)

* Updated the lab to GA watsonx.data 1.1.1 code

    - [What's new in watsonx.data version 1.1.1 Reference](https://www.ibm.com/docs/en/watsonxdata/1.1.x?topic=watsonxdata-whats-new-in)

    - Audit logging
    
        IBM watsonx.data now integrates with the Cloud Pak for Data audit logging service. Auditable events for watsonx.data are forwarded to the security information and event management (SIEM) solution that you integrate with.
    
    - Use self-signed certificates and CA certificates to connect to object stores
    
        Previously, watsonx.data could connect to HTTPS endpoints that used certificates signed by well-known certificate authorities, such as IBM Cloud® Object Storage and Amazon S3. Now, you can connect to object stores that use self-signed certificates or certificates that are signed by other certificate authorities.
    
    - Integration with Db2® and Netezza®
    
        You can now register Db2 or Netezza engines with valid console URL. You can use the metastore URL shown in Engine detail page to sync the respective engines with appropriate bucket catalog-based table.
    
    - IBM Data Virtualization Manager for z/OS® connector
    
        You can use the new IBM Data Virtualization Manager for z/OS® connector to read and write IBM Z® without having to move, replicate, or transform the data. For more information, see Connecting to an IBM Data Virtualization Manager (DVM) data source.
    
    - Better memory management
    
        Metastore caching and metadata caching (header and footer caching) are now enabled by default to optimize the memory usage. Also, now you can create a local staging directory to optimize the use of resources during data operations. For more information, see Enhancing the query performance through caching and Configuring a local staging directory.
    
    - Presto case-sensitive behavior
    
        The Presto behavior is changed from case-insensitive to case-sensitive. Now you can provide the object names in original case format as in the database. You can also create Schemas, Tables and Columns in mixed case that is, uppercase and lowercase through Presto if the database supports it.
    
    - Teradata connector is enabled for multiple ALTER TABLE statements
    
        Teradata connector now supports the ALTER TABLE RENAME TO, ALTER TABLE DROP COLUMN, ALTER TABLE RENAME COLUMN column_name TO new_column_name statements.
    
    - Removal of development (*-devel) packages
    
        For security reasons, the *-devel packages are removed from watsonx.data. If you are already using the development packages, the programs that use the development packages cannot be compiled . For any queries, contact IBM Support.
    
    - SSL is enabled for PostgreSQL
    
        Now ingestion can use mounted certificates when connecting to PostgreSQL.

### January 3, 2024 (1.1.0)

* Added two open ports to the image

    Sometimes there is a requirement to add another service to the watsonx.data image. For instance, you may want to add MongoDB or MSSQL to the system in order to demonstrate federations with these data source. Since we do not know what your requirements are, we have opened up two ports that can be assigned to any service. The documentation has been updated to describe what steps are needed to use these open up and use these ports.

### December 6, 2023 (1.1.0)

* Updated the lab to GA 1.1.0 code

    - [What's new in watsonx.data version 1.1.0 Reference](https://www.ibm.com/docs/en/watsonxdata/1.1.x?topic=watsonxdata-whats-new-in)

    - Time-travel and roll-back queries

        You can now run the following time-travel queries to access historical data in Apache Iceberg tables:

        <pre style="font-size: small; color: darkgreen; overflow: auto">
        SELECT &lt;columns&gt; FROM &lt;iceberg-table&gt; FOR TIMESTAMP AS OF TIMESTAMP &lt;timestamp&gt;
        SELECT &lt;columns&gt; FROM &lt;iceberg-table&gt; FOR VERSION AS OF &lt;snapshotId&gt;
        </pre>

        You can use time-travel queries to query and restore data that was updated or deleted in the past.

        You can also roll back an Apache Iceberg table to any existing snapshot.

    - Capture historical data about Presto queries
      
        The Query History Monitoring and Management (QHMM) service captures historical data about Presto queries and events. The historical data is stored in a MinIO bucket and you can use the data to understand the queries that were run and to debug the Presto engine.

    - Improved query performance with Metastore, File list, and File metadata caching
      
    - You can now capture and track the DDL changes in watsonx.data by using    an event listener.

    - Ingest data by using Spark
      
        You can now use the IBM Analytics Engine powered by Apache Spark to run ingestion jobs in watsonx.data.

    - Integration with Db2 and Netezza Performance Server
      
        You can now register Db2 or Netezza Performance Server engines in watsonx.data console.

    - New connectors
      
        You can now use connectors in watsonx.data to establish connections to the following types of databases:

        -  Teradata
        -  Delta Lake
        -  Elasticsearch
        -  SAP HANA
        -  SingleStoreDB
        -  Snowflake
        -  Teradata

- Db2 Upgraded to 11.5.9

    - [What's new in Db2 11.5.9 Reference](https://www.ibm.com/docs/en/db2/11.5?topic=new-1159)

    

### October 6, 2023 (1.0.3)

* Updated the lab to GA 1.0.3 code

    - [What's new in watsonx.data version 1.1.0](https://www.ibm.com/docs/en/watsonxdata/1.0.x?topic=watsonxdata-version-103)

* Image now available in 10 data centers with simpler provisioning

    - [Requesting an Image](wxd-reference-techzone.md)

* Removed VPN Requirement
* External URLs and Ports for all UI Services

    - [watsonx.data Ports](wxd-reference-ports.md)

* Added PostgreSQL and MySQL databases

    - [Postgres Connection](wxd-connections.md#postgresql-access)
    - [MySQL Connection](wxd-connections.md#mysql-access)

* Added Jupyter notebook examples

    - [Jupyter Notebook support](wxd-jupyter.md)

* Fixed Presto certificate to support TechZone addresses without updating`/etc/hosts`

    - [Watsonx.data Connection Certificate](wxd-connections.md#watsonxdata-connection-certificate)

* Added standalone Spark server to show connectivity to the Presto database

    - [Accessing watsonx.data with Spark](wxd-jupyter.md#accessing-watsonxdata-with-spark)

* Added watsonx.data Client code

    - [Watsonx.data client utilities](https://www.ibm.com/docs/en/watsonxdata/1.0.x?topic=utilities-lh-client-commands-usage)

* Added MinIO CLI interface

    - [MinIO CLI](wxd-minio.md)

* Exposed external ports for MinIO, Db2, MySQL, PostgreSQL, Hive, PrestoDB

    - [watsonx.data Ports](wxd-reference-ports.md)

* VNC Interface disabled by default

    - [Enabling VNC Access](wxd-reference-vnc#enabling-vnc-access.md)

* Added Ingesting data chapter

    - [Ingesting Data](wxd-ingest.md)

### July 25, 2023 (1.0.1)

* Updated the lab to GA 1.0.1 code
* Automated start of watsonx.data and simplification of many of the sections
* Removed the Ingest section until a new version is available
* Added Db2 and PostgreSQL connection details

### June 12, 2023 (1.0.0)

Clarified some commands and added an Appendix on common issues.

### June 6, 2023 (1.0.0)

Updated instructions for new TechZone image and added Ingest lab instructions.

### May 25th, 2023 (1.0.0)

Initial publication.





