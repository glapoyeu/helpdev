##  Solución Mysql Workbench en Ubuntu 20.04 An AppArmor policy prevents this sender

Al actualizar mi ubuntu a la versión 20.04, me resultó este error al querer conectarme a Mysql por medio de Workbench. La explicación de esta situación es que Workbench usa conexiones ssh y Password Manager para funcionar correctamente. Por lo tanto, es necesario otorgar este permiso explícita mente.  

Para proporcionar los permisos es necesario colocar en la terminal estos dos comandos:   
snap connect mysql-workbench-community:password-manager-service  
snap connect mysql-workbench-community:ssh-keys


## ERROR 2068 (HY000): LOAD DATA LOCAL INFILE file request rejected due to restrictions on access
comment I'm trying ,

```
mysql> 

LOAD DATA LOCAL INFILE '/var/tmp/countries.csv' 
INTO TABLE countries 
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"' LINES 
TERMINATED BY '\n' 
IGNORE 1 LINES 
(CountryId,CountryCode,CountryDescription,CountryRegion,LastUpdatedDate,created_by,created_on)
SET created_by = 'DH_INITIAL_LOAD', created_on = current_timestamp();
```

ERROR 2068 (HY000): LOAD DATA LOCAL INFILE file request rejected due to restrictions on access.`

It was working fine, I downloaded pymysql and mysql connector for the python script. I uninstalled and checked still it is not working. The verion and infile is ON,

```
 select version() -| 8.0.17



mysql> SHOW GLOBAL VARIABLES LIKE 'local_infile';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| local_infile  | ON    |
+---------------+-------+
1 row in set (0.00 sec)
```

**Answer**

    This restriction can be removed from MySQL Workbench 8.0 in the following way. Edit the connection, on the Connection tab, go to the 'Advanced' sub-tab, and in the 'Others:' box add the line 'OPT_LOCAL_INFILE=1'.

    This should allow a client using the Workbench to run LOAD DATA INFILE as usual.



Quoted from this link: https://bugs.mysql.com/bug.php?id=91872


## Error al intentar abrir o crear un modelo 
```could not store document data locale::facet::S_create_c_locale name not valid workbench mysql error```
### Solución

After long days and no soluction for this problem... 

I attempt to change my default language to english and.... He is live!

Go to the works fine for me:

1 - edit: /etc/locale.gen 
Find and uncoment line like this: 
#en_US.UTF-8 UTF-8

Save end close file

2 - run: sudo locale-gen
Generating locales...
  en_US.UTF-8... done
  pt_BR.UTF-8... done
Generation complete.

3 - nope, is only this!!

Mysql Workbench running without errors! I feel good!!

