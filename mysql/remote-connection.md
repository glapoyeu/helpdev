# How to Allow Remote Connections to MySQL

## Introduction

It is not uncommon to host databases and web servers on the same local machine. However, many organizations are now moving to a more distributed environment.

A separate database server can improve security, hardware performance, and enable you to scale resources quickly. In such use cases, learning how to manage remote resources effectively is a priority.

This tutorial shows you how to enable remote connections to a MySQL database.

## Prerequisites
    - Access to a terminal window/command line
    - Remote MySQL server
    - Sudo or root privileges on local and remote machines



**Note**: If you do not have direct access to your MySQL server, you need to establish a secure SSH connection. In case you need assistance, we have prepared a comprehensive tutorial on how to use SSH to connect to a remote server. This article a must-read for anyone new to the process.

## MySQL Server Remote Connection

Allowing connections to a remote MySQL server is set up in 3 steps:

    1. Edit MySQL config file
    2. Configure firewall
    3. Connect to remote MySQL server

### Step 1: Edit MySQL Config File
#### 1.1 Access mysqld.cnf File

Use your preferred text editor to open the mysqld.cnf file. This example uses the nano text editor in Ubuntu 18.04. Enter the following command in your command-line interface to access the MySQL server configuration file:
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

The location of the file may vary based on the distribution and version in use. If the MySQL configuration file is not it its default location try using the Linux find command to detect it.

#### 1.2 Change Bind-Address IP

You now have access to the MySQL server configuration file. Scroll down to the **bind-address** line and change the IP address. The current default IP is set to **127.0.0.1**. This IP limits MySQL connections to the local machine.

The new IP should match the address of the machine that needs to access the MySQL server remotely. For example, if you bind MySQL to 0.0.0.0, then any machine that reaches the MySQL server can also connect with it.

Once you make the necessary changes, save and exit the configuration file.

**Note**: Remote access is additionally verified by using the correct credentials and user parameters you have defined for your **MySQL users**.


#### 1.3 Restart MySQL Service

Apply the changes made to the MySQL config file by restarting the MySQL service:

```
sudo systemctl restart mysql
```

Next, your current firewall settings need to be adjusted to allow traffic to the default MySQL port.

### Step 2: Set up Firewall to Allow Remote MySQL Connection

While editing the configuration file, you probably observed that the default MySQL port is **3306**.

If you have already configured a firewall on your MySQL server, you need to open traffic for this specific port. Follow the instructions below that correspond to your firewall service in use.

#### Option 1: UFW (Uncomplicated Firewall)

UFW is the default firewall tool in Ubuntu. In a terminal window, type the following command to allow traffic and match the IP and port:

```
sudo ufw allow from remote_ip_address to any port 3306
```

The system confirms that the rules were successfully updated.

#### Option 2: FirewallD
The firewalld management tool in CentOS uses **zones** to dictate what traffic is to be allowed.

Create a new zone to set the rules for the MySQL server traffic. The name of the zone in our example is mysqlrule, and we used the IP address from our previous example 133.155.44.103:

```
sudo firewall-cmd --new-zone=mysqlrule --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --permanent --zone=mysqlrule --add-source=133.155.44.103
sudo firewall-cmd --permanent --zone=mysqlrule --add-port=3306/tcp
sudo firewall-cmd --reload
```

You have successfully opened port **3306** on your firewall.

#### Option 3: Open Port 3306 with iptables

The **iptables** utility is available on most Linux distributions by default. Type the following command to open MySQL port 3306 to unrestricted traffic:

```
sudo iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```

To limit access to a specific IP address, use the following command instead:

```
sudo iptables -A INPUT -p tcp -s 133.155.44.103 --dport 3306 -j ACCEPT
```

This command grants access to 133.155.44.103. You would need to substitute it with the IP for your remote connection.

It is necessary to save the changes made to the iptables rules. In an Ubuntu-based distribution type the following commands:

```
sudo netfilter-persistent save
sudo netfilter-persistent reload
```
Type the ensuing command to save the new iptables rules in CentOS:

```
service iptables save
```

### Step 3: Connect to Remote MySQL Server
Your remote server is now ready to accept connections. Use the following command to establish a connection with your remote MySQL server:

```
mysql -u username -h mysql_server_ip -p
```

The **-u username** in the command represents your MySQL username. The **-h mysql_server_ip** is the IP or the hostname of your MySQL server. The -p option prompts you to enter the password for the MySQL username.

You should see an output similar to the one below:

```
Connection to mysql_server_ip 3306 port [tcp/mysql] succeeded!
```

## How to Grant Remote Access to New MySQL Database?
If you do not have any databases yet, you can easily **create a database** by typing the following command in your MySQL shell:

```
CREATE DATABASE ‘yourDB’;
```

To grant remote user access to a specific database:
```
GRANT ALL PRIVILEGES ON yourDB.* TO user1@’133.155.44.103’ IDENTIFIED BY ‘password1’;
```

The name of the database, the username, remote IP, and password need to match the information you want to use for the remote connection.

## How to Grant Remote Access to Existing MySQL Database
Granting remote access to a user for an existing database requires a set of two commands:

```
update db set Host=’133.155.44.103' where Db='yourDB';
update user set Host=’133.155.44.103' where user='user1';
```

User1 is now able to access yourDB from a remote location identified by the IP 133.155.44.103.



**Note**: Learn everything you need to know about database servers and how they work in our article What Is a Database Server.

**Conclusion**

In this article, you have gained valuable insight into the general principles of a remote MySQL connection.

With the appropriate credentials, a user originating from the specified IP address can now access your MySQL server from a remote machine.
