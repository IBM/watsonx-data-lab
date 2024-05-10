# Milvus

Milvus is a vector database that stores, indexes, and manages massive embedding vectors that are developed by deep neural networks and other machine learning (ML) models. It is developed to empower embedding similarity search and AI applications. Milvus makes unstructured data search more accessible and consistent across various environments.

## Local Milvus Connection

A local connection assumes that you are running your Python code or Jupyter notebook inside the same server that is running watsonx.data and the Milvus server. The connection user is the default watsonx.data userid (ibmlhadmin). 

### Generate the Connection Certificate 

You need to generate the certificate that will be used by the connection.

!!! abstract "Generate the Connection Certificate"
      ```bash
      echo QUIT | openssl s_client -showcerts -connect watsonxdata:8443 | \
            awk '/-----BEGIN CERTIFICATE-----/ {p=1}; p; /-----END CERTIFICATE-----/ {p=0}' > /tmp/presto.crt 
      ```

### Local Connection Parameters

The application or notebook connecting to the Milvus server will use the following settings.

| Setting        | Value
|:---------------|:---------
| host           | watsonxdata
| port           | 19530
| apiuser        | ibmlhadmin
| apikey         | password
| server_pem_path| /tmp/presto.crt

## Remote Connection

A remote connection requires information from your TechZone reservation details. The reservation will contain a URL for the Milvus service:
```
Milvus Endpoint - Server: useast.techzone-services.com Port:99999
```
The server and port number will need to be substituted into the connection details below.

### Generate the Connection Certificate 

In addition to the server and port number, you will need to get the Certificate for the watsonx.server. You will need to download the contents of this file and place it into a file that is local to your Python application or Jupyter notebook server. The following command will retrieve the contents of the cert file and print the values. Copy all the text between `BEGIN CERTIFICATE` and `END CERTIFICATE` and place it into a local file with a name of `presto.crt`. Make sure to update the `server_pem_path` with your local file name.

!!! abstract "Generate the Connection Certificate"
      ```
      echo QUIT | openssl s_client -showcerts -connect watsonxdata:8443 | \
            awk '/-----BEGIN CERTIFICATE-----/ {p=1}; p; /-----END CERTIFICATE-----/ {p=0}' > /tmp/presto.crt 
      cat /tmp/presto.crt
      ```

### Remote connection Settings

The application or notebook connecting to the Milvus server will use the following settings.

| Setting        | Value
|:---------------|:---------
| host           | useast.services.cloud.techzone.ibm.com (your server)
| port           | 99999 (Open port 1 or 2)
| apiuser        | ibmlhadmin
| apikey         | password
| server_pem_path| /tmp/presto.crt


## Milvus Connection

The following section of Python code will connect to the Milvus service. You must load the `pymilvus` library onto your system in order for this to work.

!!! abstract "Milvus Connection Code"
      ```
      from pymilvus import (
         connections,
         FieldSchema,
         CollectionSchema,
         DataType,
         Collection,
      )

      connections.connect(alias="root", 
                        host=host, 
                        port=port, 
                        user=apiuser, 
                        password=apikey,
                        server_pem_path=server_pem_path,
                        server_name='watsonxdata',
                        secure=True) 
      ```

### Check Connection Status

To check whether or not you are connect to Milvus, use the following code.

!!! abstract "Milvus Connection Test"
      ```
      print(f"\nList connections:")
      print(connections.list_connections())
      ```

To see an example of an application connecting to Milvus, check the Milvus Example notebook found in the Jupyter notebook lab.