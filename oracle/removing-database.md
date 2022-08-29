# Removing Oracle Database 11g

You can remove the Oracle database by using the uninstall script provided with the database, or remove Oracle program files, users, and user groups by executing commands as root.

To manually remove Oracle Database 11g:

1. Log in to Linux as root.

2. Stop the Oracle service.

```
cd $ORACLE_HOME/bin
sqlplus sys/imc123 as sysdba
SQL>shutdown
```
In the command, **imc123** is the password of the ***sys*** user.

3. Stop the listener.
```
cd $ORACLE_HOME/bin
./lsnrctl stop
```

4. Delete the Oracle installation directory **/u01** in the root directory.

```
cd /
rm –rf /u01
```

5. Delete the directory **/usr/local/bin/oracle** (the default is /**usr/local/bin**). This is the directory configured using the configuration scripts during Oracle database installation. The asterisks (*) in the command represents the name of the file you want to delete.
```
rm –rf **
```

6. Delete files **oratab** and **oraInst.loc** in the **/etc** directory.

```
rm –rf oratab
rm –rf oraInst.loc
```
7. Delete the **oracle** user.
```
userdel oracle
```

8. Delete the **oinstall** user group.
```
groupdel oinstall
```
9. Delete the **dba** user group.
```
groupdel dba
```

10. Delete the **/home/oracle** directory.
```
cd /home
rm –rf oracle/
```
1. Delete the startup service.
```
chkconfig -–del dbora
```
12. Reboot Linux.


# Referencias

- [techhub](https://techhub.hpe.com/eginfolib/networking/docs/IMC/v7_3/5200-2694/content/466972550.htm)