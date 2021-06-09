
# Launching Hadoop Cluster Using EC2 Instances From Scratch



In this tutorial, we will cover how to set up a fully-distributed Hadoop cluster on Amazon EC2 from scratch.

1. Creating a fresh EC2 instance

Log into the AWS Management Console, go to EC2, and click on Launch instances.
The technical set up of the fresh instance are as follows:

- Ubuntu Server 20.04 LTS (HVM), SSD Volume Type

- **t2.micro** – free tier
- The default security rule allows only SSH connections to the instance.


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

    Now, switch to the new account:

    ```shell
    $ sudo su - hadoop
    ```

    Then we can find the prefix of the command line change to *hadoop*, indicating that the active user is *hadoop*. And the working directory becomes */home/hadoop*. From now on, we'll refer to this directory as the home directory (or just home). We have full access rights (read/write/execute, or **rwx**) to the home directory, so we won't have to fiddle with *sudo* every time we need to make a change.


    1.2 Install JDK

    Because this is a fresh virtual machine with only Ubuntu OS installed on it, we need to configure our computing environment by installing necessary software packages. The following commands install the <a href="https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions">OpenJDK 8</a> on this machine:

    ```shell
    $ sudo apt-get update
    $ sudo apt install openjdk-8-jdk

    ```

    We can observe the installation starts. We will also be able to monitor the progress of the installation and read a success message in the end.

    Now typing `java -version` can tell us if Java is successfully installed.

    1.3 Download the latest stable release of Hadoop (version 3.2.1)

    We'll use *wget* to download files from the Hadoop website. Download the binary distribution for installation by typing:

    ```shell
    $ cd ~
    $ wget http://apache.mirrors.tds.net/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
    $ ls
    ```

    Unpack the downloaded tar file using this command:

  ```shell
  $ tar vxzf hadoop-3.2.1.tar.gz
  $ ls
  ```

Several flags are used with the tar command.  Among them, the *x* flag tells *tar* we are extracting files, and the *f* flag lets us specify the name of the file we're going to be working with.

Rename the extracted directory for easier reference. Because we are going to set Hadoop configurations by editing files in this directory, we need to give the active user the permission to make such changes in the “hadoop” directory. Give the ownership of the directory to the active user. Lastly, delete the compressed file to save the space:

```shell
$ mv hadoop-3.2.1 hadoop
$ sudo chown -R hadoop hadoop
$ rm hadoop-3.2.1.tar.gz
```




  <sup>[1](#footnote1)</sup> If you use a password other than *bigdata*, you must change the affected part of *sshconf.sh* accordingly for configuring SSH connections.
