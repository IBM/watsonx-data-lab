# Watsonx.data Certificates

!!! error "Watsonx.data Certificate Failure"

Due to a change in TechZone URLs, the self-signed certificates in the watsonx.data Developer image may be invalid. If you are attempting to connect to the watsonx.data system from outside the virtual machine, you will need to run the following commands to fix the self-signed certificate. 

### Step 1: Connect to the Server

Use the SSH port to connect into the server and make sure that you become the root user.

```bash
sudo su -
```

### Step 2: Update the Certificate

We need to update the certificate by using a utility in the developer toolbox.  Start the toolbox code by switching to the bin directory and issuing the following command.
```bash
cd /root/ibm-lh-dev/bin
./dev-sandbox
```

Once inside the development container, you will need to update the program that generates the certificates. **Note**: The certificate should cover all TechZone locations. If for some reason your TechZone server does not match the pattern `*.services.cloud.techzone.ibm.com`, update it in the command below.

```bash
sed -i '/DNS.14.*/a DNS.15 = watsonxdata' /scripts/gen_certs.sh
sed -i '/DNS.15.*/a DNS.16 = watsonxdata.gym.lan' /scripts/gen_certs.sh
sed -i '/DNS.16.*/a DNS.17 = *.services.cloud.techzone.ibm.com' /scripts/gen_certs.sh
./scripts/gen_certs.sh
```

Once the script completes, exit the toolkit.

```bash
exit
```

### Step 3: Stop and Restart the System

The certificates need to be replaced in all the running containers. You must stop and restart them. You must include the diagnostic flag or else the system will not work properly. The startup will take some time to complete. The Postgres pod will display some warning messages which can be safely ignored.

```bash
./stop.sh
export LH_RUN_MODE=diag
./start.sh 
```

### Step 4: Generate Custom Certificate

The first step is to copy the new certificates to the central `/certs` directory use by this image.

```bash
docker cp ibm-lh-presto:/mnt/infra/tls/lh-ssl-ts.jks /certs/lh-ssl-ts.jks
docker cp ibm-lh-presto:/mnt/infra/tls/cert.crt /certs/lh-ssl-ts.crt
```

Next we need to generate the certificate file that is used by a number of the examples in the lab instructions.

```bash
rm -f presto.crt
echo QUIT | openssl s_client -showcerts -connect 127.0.0.1:8443 | awk '/-----BEGIN CERTIFICATE-----/ {p=1}; p; /-----END CERTIFICATE-----/ {p=0}' > presto.crt
```

You can print the certificate if you need it for connections from CP4D.

```bash
cat presto.crt
```

### Step 5: Generate Java Keystore File

The next step will create the Java Keystore file. When prompted, use a password of `watsonx.data` and say `yes` to accepting the certificate. Make sure that you see your host in the list. For instance, `useast.services.cloud.techzone.ibm.com` should be displayed when you see the results.

```bash
rm -f presto-key.jks
keytool -import -alias presto-crt -file ./presto.crt -keystore ./presto-key.jks
```

The following is an example of the output from the `keytool` command.

<pre style="font-size: small; color: darkgreen; overflow: auto">
Owner: CN=Dummy-Self-signed-Cert, EMAILADDRESS=dummy@example.dum, OU=For-CPD, O=Data and AI, L=Home-Town, ST=XX, C=YY
Issuer: CN=Dummy-Self-signed-Cert, EMAILADDRESS=dummy@example.dum, OU=For-CPD, O=Data and AI, L=Home-Town, ST=XX, C=YY
Serial number: 73f26644ad83ac8cdf9afbda6006d4e52f244fac
Valid from: Tue Mar 05 17:42:56 EST 2024 until: Wed May 23 18:42:56 EDT 2035
Certificate fingerprints:
         SHA1: 3A:6C:52:80:3D:14:CF:D0:E7:AC:14:13:6F:46:FB:B1:8C:BA:E4:37
         SHA256: 28:E7:AD:4E:BA:5F:00:4C:B7:2E:61:3E:3B:96:E5:DF:01:D5:80:CE:1A:B3:EF:B7:86:11:26:4A:B6:7C:90:8A
Signature algorithm name: SHA512withRSA
Subject Public Key Algorithm: 2048-bit RSA key
Version: 3

Extensions: 

#1: ObjectId: 2.5.29.37 Criticality=false
ExtendedKeyUsages [
  serverAuth
]

#2: ObjectId: 2.5.29.17 Criticality=false
SubjectAlternativeName [
  DNSName: ibm-lh-presto-svc
  DNSName: *.svc.cluster.local
  DNSName: api-svc
  DNSName: *.api
  DNSName: localhost
  DNSName: ibm-lh-hive-metastore
  DNSName: ibm-lh-hive-metastore-svc
  DNSName: lhconsole-api-svc
  DNSName: lhconsole-nodeclient-svc
  DNSName: ibm-lh-ranger-svc
  DNSName: ibm-lh-javaapi-svc
  DNSName: ibm-lh-prestissimo-svc
  DNSName: ibm-lh-qhmm
  DNSName: ibm-lh-qhmm-svc
  DNSName: watsonxdata
  DNSName: watsonxdata.gym.lan
  DNSName: *.services.cloud.techzone.ibm.com
]

Trust this certificate? [no]:  yes
Certificate was added to keystore
</pre>

### Step 6: Create Certificate and Keystore Copies

The final step is to copy the certs and keystore values in a central location so they can be used in various scripts and notebooks.

```bash
\cp -f presto-key.jks /certs
\cp -f presto.crt /certs

chmod +r /certs/*.*

\cp -rf /certs /notebooks/
```