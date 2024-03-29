# Using SQL*Plus to Unlock Accounts and Reset Passwords

Use this SQL*Plus procedure to unlock and reset user account passwords.

1. Log in as the Oracle Database software owner user.
2. Set the ORACLE_HOME and ORACLE_SID environment variables.
3. Start SQL*Plus and log in as the SYS user, connecting as SYSDBA:

```
$ $ORACLE_HOME/bin/sqlplus
SQL> CONNECT SYS as SYSDBA
Enter password: sys_password 
```

4. To unlock an account:
```
ALTER USER account ACCOUNT UNLOCK;
```

5. To reset the password:
```
ALTER USER user_name IDENTIFIED BY new_password; 
```

    Note:If you unlock an account but do not reset the password, then the password remains expired. The first time someone connects as that user, they must change the user's password. 


One of the reasons of your problem could be the password policy you are using.

And if there is no such policy of yours then check your settings for the password properties in the DEFAULT profile with the following query:

```
SELECT resource_name, limit
FROM dba_profiles 
WHERE profile = 'DEFAULT'
AND resource_type = 'PASSWORD';
```

And If required, you just need to change the PASSWORD_LIFE_TIME to unlimited with the following query:

```
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;

```
