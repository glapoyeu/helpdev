# How to Install RPM Packages on Ubuntu

## Introduction
RPM is a package format used by Red Hat-based derivatives like CentOS, RHEL or Fedora. The name comes from the RPM Package Manager (RPM), a free and open-source package management system for installing, uninstalling, and managing software packages in Linux.

Is it possible to install .rpm files on Debian based distributions like Ubuntu? The answer is yes. However, you need to be careful as it could lead to package dependency conflicts.

Follow the steps in this tutorial to learn how to install .rpm packages on Ubuntu.

## Prerequisites

- A user account with sudo privileges
- Access to a terminal/command line
- apt package manager (included by default)

# Steps to Install an RPM Package on Ubuntu

## Install Alien Package

To improve the stability of the installation process, we need to convert the .rpm file to a .deb file format.

**Alien** is a useful tool that facilitates conversions between Red Hat rpm, Debian deb, Stampede slp, Slackware tsz, and Solaris pkg file formats.

To install Alien follow these steps:

1. Check the status of the Universe distribution component:
```
sudo add-apt-repository universe
```

2. Make sure that your repositories are up-to-date:
```
sudo apt-get update
```

3. The following command installs the Alien conversion tool:
```
sudo apt-get install alien
```

## Convert .rpm Files to .deb Format
Now that **Alien** has been installed, it’s time to convert the files to the .deb format to complete the installation. Go to the folder where the .rpm file is located and enter the following command:

```
sudo alien packagename.rpm
```
This command instructs the Alien tool to initiate the conversion process of the .rpm file to a .deb file.

## Install the converted .rpm package on Ubuntu

Once the conversion has run its course, enter the following command to start the installation:

```
sudo dpkg –i packagename.deb
```

You have successfully installed a converted .rpm file on Ubuntu.

# How to Install .rpm Package Directly on Ubuntu

The command we’ll use below installs a .rpm package in Ubuntu without previously converting it to a .deb file format.

This command can lead to serious compatibility issues if you attempt to run it with important system packages. RPM was not developed initially for Debian based distributions. As we have already installed Alien, we can use the tool to install RPM packages without the need to convert them first.

To complete this action, enter this command:
```
sudo alien –i packagename.rpm
```

You have now directly installed an RPM package on Ubuntu. Keep in mind that installing packages in formats that are not native to Ubuntu can pose a significant risk.

## Conclusion
By following the tutorial, you have installed an RPM package on Ubuntu. Understanding the installation processes and the available options significantly reduce the likelihood of something going wrong.

If you were planning to update essential system packages, a better option would be to use Ubuntu repositories and find adequate alternative packages.


## Agradecimiento
- [phoenixnap](https://phoenixnap.com/kb/install-rpm-packages-on-ubuntu)

[Inidce](README.md)