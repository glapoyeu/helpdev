# How to Unzip a Zip File in Linux [Beginner’s Tutorial]

Brief: This quick tip shows you how to unzip a file in Ubuntu and other Linux distributions. Both terminal and GUI methods have been discussed.

Zip is one of the most common and most popular ways to create compressed archive files. It is also one of the older archive file formats that were created in 1989. Since it is widely used, you’ll regularly come across a zip file.

In an earlier tutorial, I showed how to zip a folder in Linux. In this quick tutorial for beginners, I’ll show you how to unzip files in Linux.

**Prerequisite: Verify if you have unzip installed**  
In order to unzip a zip archive file, you must have the unzip package installed in your system. Most modern Linux distributions come with unzip support but there is no harm in verifying it to avoid bad surprises later.

In a terminal, use the following command:
```
unzip --version
```

If it gives you some details, you have unzip installed already. If you see an ‘unzip command not found’ error, you have to install.  

In Ubuntu and Debian based distributions, you can use the command below to install unzip.  

Once you have made sure that your system has unzip support, it’s time to unzip a zip file in Linux.

You can use both the command line and GUI for this purpose and I’ll show you both methods.

    Unzip files in Linux terminal
    Unzip files in Ubuntu via GUI

## Unzip files in Linux command line

Using unzip command in Linux is absolutely simple. In the directory, where you have the zip file, use this command:

```
unzip zipped_file.zip
```

You can also provide the path to the zip file instead of going to the directory. You’ll see extracted files in the output:
```
unzip metallic-container.zip -d my_zip
Archive:  metallic-container.zip
  inflating: my_zip/625993-PNZP34-678.jpg  
  inflating: my_zip/License free.txt  
  inflating: my_zip/License premium.txt
```

There is a slight problem with the above command. It will extract all the contents of the zip file in the current directory. That’s not a pretty thing to do because you’ll have a handful of files leaving the current directory unorganized.

## Unzip to a specific directory

A good practice is to unzip to directory in Linux command line. This way, all the extracted files are stored in the directory you specified. If the directory doesn’t exist, it will create one.

```
unzip zipped_file.zip -d unzipped_directory
```

Now all the contents of the zipped_file.zip will be extracted to unzipped_directory.

Since we are discussing good practices, another tip you can use is to have a look at the content of the zip file without actually extracting it.

## See the content of the zip file without unzipping it

You can check the content of the zip file without even extracting it with the option -l.

```
unzip -l zipped_file.zip
```

Here’s a sample output:
```
unzip -l metallic-container.zip 
Archive:  metallic-container.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
  6576010  2019-03-07 10:30   625993-PNZP34-678.jpg
     1462  2019-03-07 13:39   License free.txt
     1116  2019-03-07 13:39   License premium.txt
---------                     -------
  6578588                     3 files
```

There are many other usage of the unzip command in Linux but I guess now you have enough knowledge to unzip files in Linux.

## Option to force overwrite?
    unzip -o /path/to/archive.zip
Note that -o, like most of unzip's options, has to go before the archive name.
