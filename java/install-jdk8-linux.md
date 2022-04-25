# Install Oracle JDK 8 on Linux

Oracle Java is the proprietary, reference implementation for Java. This article shows you the way to manually install Oracle Java Development Kit 8 (Oracle JDK) on Linux. 

Note: This article is using JDK8_Update_321 to demonstrate the installation. In the provided commands, replace the version specific paths and file names to your downloaded version.

[Video tutorial](https://www.youtube.com/watch?v=f-oBpWOZFQE)

- Step 1:
Download the latest JDK(jdk-8u321-linux-x64.tar.gz) from this official site. 

    This article assumes that you have a x64 bit Linux distribution as most of the famous Linux distributions have dropped the support for x86 architecture. However, if you are using an x86 bit Linux distribution, download the jdk-8u321-linux-i586.tar.gz from the official site and change the following commands according to the filename 


If you want to download to a remote server or if you simply prefer wget, use the command given in this Stackoverflow answer: [Downloading JDK](http://stackoverflow.com/a/10959815/4382663)

- Step 2:
Open the terminal (**Ctrl + Alt + T**) and enter the following command. 

```
sudo mkdir /usr/lib/jvm
```

If the /usr/lib/jvm folder does not exist, this command will create the directory. If you already have this folder, you can ignore this step and move to next step.

- Step 3:
Enter the following command to change the directory.
```
cd /usr/lib/jvm
```

- Step 4:
Extract the jdk-8u321-linux-x64.tar.gz file in that directory using this command.
```
sudo tar -xvzf ~/Downloads/jdk-8u321-linux-x64.tar.gz
```

According to this command, the JDK filename is jdk-8u321-linux-x64.tar.gz and which is located in the ~/Downloads folder. If your downloaded file is in any other location, change the command according to your path.

- Step 5:
Enter the following command to open the environment variables file.
```
sudo gedit /etc/environment
```

- Step 6:
In the opened file, add the following bin folders to the existing PATH variable.
```
/usr/lib/jvm/jdk1.8.0_321/bin
/usr/lib/jvm/jdk1.8.0_321/db/bin
/usr/lib/jvm/jdk1.8.0_321/jre/bin
```
The PATH variables have to be separated by a colon.
Notice that the installed JDK version is 1.8 update 321. Depending on your JDK version, the paths can be different.
Add the following environment variables at the end of the file.
```
J2SDKDIR="/usr/lib/jvm/jdk1.8.0_321"
J2REDIR="/usr/lib/jvm/jdk1.8.0_321/jre"
JAVA_HOME="/usr/lib/jvm/jdk1.8.0_321"
DERBY_HOME="/usr/lib/jvm/jdk1.8.0_321/db"
```


The environment file before the modification:
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
```

The environment file after the modification:
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk1.8.0_321/bin:/usr/lib/jvm/jdk1.8.0_321/db/bin:/usr/lib/jvm/jdk1.8.0_321/jre/bin"
J2SDKDIR="/usr/lib/jvm/jdk1.8.0_321"
J2REDIR="/usr/lib/jvm/jdk1.8.0_321/jre"
JAVA_HOME="/usr/lib/jvm/jdk1.8.0_321"
DERBY_HOME="/usr/lib/jvm/jdk1.8.0_321/db"
```


Save the changes and close the gedit. To learn more about setting environment variables and/or to set the environment variable without root privilege, check How to Set Environment Variables in Linux?.

- Step 7:
Enter the following commands to inform the system about the Java's location. Depending on your JDK version, the paths can be different.

```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_321/bin/java" 0
```
```
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_321/bin/javac" 0
```
```
sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_321/bin/java
```
```
sudo update-alternatives --set javac /usr/lib/jvm/jdk1.8.0_321/bin/javac
```

- Step 8:
To verify the setup enter the following commands and make sure that they print the location of java and javac as you have provided in the previous step.
```
update-alternatives --list java
```
```
update-alternatives --list javac
```

- Step 9:
Restart the computer (or just log-out and login) and open the terminal again.

Step 10:
Enter the following command.
```
java -version
```

If you get the installed Java version as the output, you have successfully installed the Oracle JDK in your system.

## References
[Java Helps](https://www.javahelps.com/2015/03/install-oracle-jdk-in-ubuntu.html)