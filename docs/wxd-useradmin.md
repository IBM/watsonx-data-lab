# Watsonx.data User Administration and Roles

Security and access control within watsonx.data are based on roles. A role is a set of privileges that control the actions that users can perform. Authorization is granted by assigning a specific role to a user, or by adding the user to a group that has been assigned one or more roles.

Access control at the infrastructural level allows permissions to be granted on the engines, catalogs, buckets, and databases. Roles for these components include Admin, Manager, User, Writer, and Reader (depending on the component).

Access to the data itself is managed through data control policies. Policies can be created to permit or deny access to schemas, tables, and columns.
User account management and access management varies between the different deployment options for watsonx.data. For instance, in the managed cloud service (SaaS), the service owner would need to invite other users to the environment and give them appropriate service access.

With the standalone software, users can be added within the console’s Access control page. In the Developer Edition, users can be added using a command line tool.

## User Administration

This lab is using the Developer edition of the watsonx.data software, which means that the Access control panel is not available. In order to manage users, the `user-mgmt` command will need to be used. The `user-mgmt` command is found in the `/root/ibm-lh-dev/bin` directory. Examples of using the command are found below.

### Add a User
The syntax for adding a user is:
```bash
./user-mgmt add-user <username> [User | Admin] <password>
```
The values are:

* `username` - The name of the user
* `[User|Admin]` - The type of user. Note that the type of user is case-sensitive!
* `password` - The password for the user.

The following command will add the user `watsonx` with a password of `watsonx.data`. This will be a standard user with no privileges. The first step is to make sure you are connected as the root user in watsonx.data server and have switched to the proper directory.

```bash
sudo su -
```
```bash
cd /root/ibm-lh-dev/bin
```

The next command will add a new user to the system.

```bash
./user-mgmt add-user watsonx User watsonx.data
```

### Change a User's Password
The syntax for changing a password is:
```bash
./user-mgmt change-password <username>
```

This command will require that the user enter the new password. You can issue the command and provide the new password at the prompt. The other way to simulate the enter command is to use the Linux `yes` function which repeats a value multiple times.

The following command will change the password of `watsonx` to `hellowatson`.

```bash
yes hellowatson | ./user-mgmt change-password watsonx
```

### Validate a User's Password

You can validate a password by using the following command:

```bash
./user-mgmt test-user-cred <username>
```

The username is the name of the user that you want to check the password for. This command will require that the user enter the existing password to check it. You can use the `yes` function (as described above) to simulate the enter command.

The following command will check that we have changed the password of watsonx to `hellowatson`.

``` bash
yes hellowatson | ./user-mgmt test-user-cred watsonx
```

### Delete a User
To delete a user, use the following command:

```bash
./user-mgmt delete-user <username>
```

The error messages on group ownership can be safely ignored.

The following command will remove our watsonx user.

```bash
./user-mgmt delete-user watsonx
```