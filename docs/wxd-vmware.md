# IBM watsonx.data VMware Image

The IBM watsonx.data lab can be run in a virtual machine environment using VMWare Workstation, VMWare Fusion, or Oracle VirtualBox. The location of the OVA file (a compressed OS image format) is provided in the TechZone page for the lab:

[https://techzone.ibm.com/collection/ibm-watsonxdata-developer-base-image](https://techzone.ibm.com/collection/ibm-watsonxdata-developer-base-image)

Select the resources tab to get details on how to download the file.

![Browser](wxd-images/techzone-resources.png)

Download the `watsonxdata.ova` file onto your local machine and then use the import function of VMware or VirtualBox to register it with the system. 

**Note**: This virtual machine was created using X64 (Intel) hardware, so this will not work in an OSX environment using M1/M2 chips. Once the machine is imported you can delete the OVA file.

Before starting the machine, you may want to adjust the hardware requirements.

   * vCPUs – 4 VPCs minimum
   * Memory – 16Gb minimum (You can try 12Gb but tight!)
   * Disk – 30Gb initial size, but the image will grow in size
   * Disable side channel mitigation ON (VMware only)

### VMware URLs 
All the URLs in the lab use `192.168.252.2` as the host. When running in the VMware image, you must use `localhost` for the addresses. You must substitute `localhost` for the `192.168.252.2` address when you come across it in the documentation.

The following URLs and Ports are used to access the watsonx.data services.
The ports that are used in the lab are listed below.

   * <a href="https://localhost:9443" target="_blank">https://localhost:9443</a> - watsonx.data management console
   * <a href="http://localhost:8080" target="_blank">http://localhost:8080</a> - Presto console
   * <a href="http://localhost:9001" target="_blank">http://localhost:9001</a> - MinIO console (S3 buckets)
   * <a href="https://localhost:6443" target="_blank">https://localhost:6443</a> - Portainer (Docker container management)
   * <a href="http://localhost:8088" target="_blank">http://localhost:8088</a> - Apache Superset (Query and Graphing)
   * <code style="color:blue;font-size:medium;">8443</code> - Presto External Port
   * <code style="color:blue;font-size:medium;">5432</code> - Postgres External Port
   * <code style="color:blue;font-size:medium;">50000</code> - Db2 Database Port

**The Apache Superset link will not be active until started as part of the lab.**

These links have been placed into the Firefox browser for your convenience.

![Browser](wxd-images/vmware-browser.png)


## Starting the VMware Image

When the machine starts, you will be prompted with the logon screen.

![Browser](wxd-images/wxd-logon.png)
 
There are two userids that we will be using in the VMware image:

   * root – password `watsonx.data`
   * watsonx – password `watsonx.data`

When successfully logged in you should see the following screen.

![Browser](wxd-images/wxd-main.png)
 
Next, check that your network connection is up and running. You will be able to see if the network is connected when the network icon appears on the top row.

![Browser](wxd-images/wxd-internet.png)
 
If it shows Wired Off, make sure to turn it on by clicking on the arrow and choosing "Connect".

![Browser](wxd-images/wxd-interneton.png)

If you are using something other than an English keyboard, click on the en1 symbol on the top bar to switch to a different layout. If your keyboard is not listed, you will need to go into Settings and add your keyboard layout.

![Browser](wxd-images/wxd-options.png)

You may also want to consider making the screen size larger. Use the drop-down menu at the top of the screen to select System Tools -> Settings. 

![Browser](wxd-images/wxd-resolution.png)

In the Devices section of the Setting menu, select Displays and choose a resolution that is suitable for your environment.
 
## Using External Ports with VMware/Virtual Box
The labs assume that you are using a browser "within" your virtual machine console. However, both VMware and VirtualBox provide a method for accessing the ports on the virtual machine in your local environment. 

### VMware

For VMware, the easiest way to connect to the virtual machine from your host machine is to use the ifconfig command to determine your virtual machine IP address.
```
ifconfig
```

![Browser](wxd-images/wxd-ipaddress.png)
 
Search for an `ensxx**` value in the output from the command. There you should see the inet address of your virtual machine (`172.16.210.237`). To access the Portainer application from your local browser, you would use this address followed by the Portainer PORT number: `https://172.16.210.237:6443`.

Remember that inside your virtual machine, you will be using `https://localhost:6443`. The following PORT numbers are open in the machine:

   * `9443` - IBM watsonx.data management console
   * `8080` - Presto console
   * `9001` - MinIO console (S3 buckets)
   * `6443` - Portainer (Docker container management)
   * `8088` - Apache Superset (Query and Graphing)
   * `5901` - VNC Access (Access to GUI in the machine)
   * `7681` - SSH (Terminal access) via Browser
   * `22` - SSH (Terminal access) via local terminal program
   * `8443` - Presto External Port (dBeaver connection)
   * `5432` - Postgres External Port (dBeaver connection)

### VirtualBox

VirtualBox does not externalize the IP address of the virtual machine. The `ifconfig` command will provide an IP address of the machine, but it will not be reachable from your host browser. To open the ports, you must use the network option on the virtual machine. This step can be done while the machine is running. From the VirtualBox console, choose Settings for the machine and then click on the Network option.

![Browser](wxd-images/vbox-network.png)
 
Press the Advanced option near the bottom of the dialog.

![Browser](wxd-images/vbox-network-1.png)
 
Select the Port Forwarding button. This will display the port forwarding menu.

![Browser](wxd-images/vbox-network-2.png)
 
You must place an entry for each port that we want to externalize to the host machine. If the value for Host IP is empty (blank), it defaults to localhost. In the example above, the 5901 port in the Guest machine (watsonxdata) is mapped to the host machines 5901 port. To access VNC, you would use `localhost:5901`. If the guest machine port conflicts with the host machine port number, you can use a different port number. 

## Terminal Command Window

All the commands in the lab will require you execute commands in a terminal window. In addition, the labs require access to the root userid, and this can be accomplished in two ways that are described below.

### Local Terminal Shell

Use a local terminal shell (iterm, Hyper, terminal) and use the SSH command to shell into the machine. For the VMware image, you need to know the IP address of the image and the port number that has been exposed for SSH command (default is 22). Assuming that your VMware machine has an IP address of `172.16.210.237`, the command to SSH into the machine would be:
```
ssh watsonx@172.16.210.237
```
![Browser](wxd-images/ssh-local.png)

You will need to accept the unknown host warning and then provide the password for the watsonx userid: `watsonx.data`.

At this point you are connected as the watsonx user. To become the root user, you must enter the following command in the terminal window.
```
sudo su -
```
Now as the root user you will be ready to run the commands found in the lab.

### Terminal Window in Virtual Machine

You can use the Terminal application in the virtual machine to issue commands. 

![Browser](wxd-images/terminal-vmware-command.png)

This will open up the terminal window.

![Browser](wxd-images/terminal-vmware.png)

At this point you are connected as the watsonx user. You can ignore any lab instructions that ask you to `ssh` into the watsonx server. To become the root user, you must enter the following command in the terminal window.
```
sudo su -
```

Now as the root user you will be ready to run the commands found in the lab.