
# Launching Hadoop Cluster Using EC2 Instances From Scratch



In this tutorial, we will cover how to set up a fully-distributed Hadoop cluster on Amazon EC2 from scratch.

1. Creating a fresh EC2 instance

Log into the AWS Management Console, go to EC2, and click on Launch instances.
The technical set up of the fresh instance are as follows:
•	Ubuntu Server 20.04 LTS (HVM), SSD Volume Type
•	t2.micro – free tier
•	8gb general purpose SSD
•	The default security rule allows only SSH connections to the instance.


1. Install and Configure Hadoop on one EC2 instance

    1.1 Create a *hadoop* group, and add a user with *hadoop* as the username in the group
    When deploying a production environment, it is recommended to create a dedicated user for the express purpose of owning and running Hadoop tasks later. Simply type the following in the terminal:

    ```shell
    $ sudo addgroup hadoop
    $ sudo adduser --ingroup hadoop hadoop
    ```


    Most of the commands here need to be prefaced with the sudo command. This allows for executing commands with privileges elevated to the root-user administrative level on an as-needed basis. It is necessary when working with directories or files not owned by your user account. When using *sudo* you will be prompted for the password of the current user account. Initially, only users with *sudo* (administrative) privileges (in this case ubuntu) will be able to use this command.

    It will prompt you to create the password for the newly-added user. To facilitate our successive configuration, use *bigdata* as the password (IMPORTANT!!!)<sup><a href="#footnote1">1</a></sup>.  


    Then, add the user into group *sudo* to allow root access with the following command


```shell
$ sudo adduser hadoop sudo
```

Now, switch to the new account by issuing the command:

```shell
$ sudo su - hadoop
```


  <sup>[1](#footnote1)</sup> If you use a password other than “bigdata”, you must change the affected part of sshconf.sh accordingly for configuring SSH connections.
