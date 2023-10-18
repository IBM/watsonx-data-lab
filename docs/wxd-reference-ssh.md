# SSH Access

Your TechZone reservation will include the server name and port number to use when connecting using ssh. The port number is referred to as <tt style="font-size: large; color: darkgreen;">port</tt> in the command below, while the server will be referred to as <tt style="font-size: large; color: darkgreen;">region.techzone-server.com</tt>. Replace these values with those found in your reservation.

You have the choice of using the VM Remote console and logging in as the watsonx user to issues commands, or using a local terminal shell (iTerm, Hyper, terminal) to run commands against the watsonx.data server. You can have multiple connections into the machine at any one time. 

You may find it is easier to cut-and-paste commands into a local terminal shell rather than using the VM Remote Console because the console does not support cut-and-paste operation from your workstation to the remote desktop.

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

## Copying Files

If you need to move files into or out of the virtual machine, you can use the following commands.

To copy a file into the virtual machine use the following syntax:

```
scp -P port myfile.txt watsonx@region.techzone-server.com:/tmp/myfile.txt
```

The filename `myfile.txt` will be copied to the `/tmp` directory. The temporary directory is useful since you can copy the file to multiple places from within the Linux environment.

Multiple files can be moved by using wildcard characters using the following syntax:

```
scp -P port myfile.* watsonx@region.techzone-server.com:/tmp
```

To move files from the image back to your local system requires you reverse the file specification.

```
scp -P port watsonx@region.techzone-server.com:/tmp/myfile.txt /Downloads/myfile.txt
```

You can also use wildcards to select more than one file.