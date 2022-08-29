# DROP USER

## Purpose

Use the DROP USER statement to remove a database user and optionally remove the user's objects.

When you drop a user, Oracle Database also purges all of that user's schema objects from the recycle bin.
    
    Caution:
        Do not attempt to drop the users SYS or     SYSTEM. Doing so will corrupt your database.


## Prerequisites

You must have the DROP USER system privilege.

## Semantics

### user

Specify the user to be dropped. Oracle Database does not drop users whose schemas contain objects unless you specify CASCADE or unless you first explicitly drop the user's objects.

### CASCADE

Specify CASCADE to drop all objects in the user's schema before dropping the user. You must specify this clause to drop a user whose schema contains any objects.

    - If the user's schema contains tables, then Oracle Database drops the tables and automatically drops any referential integrity constraints on tables in other schemas that refer to primary and unique keys on these tables.

    - If this clause results in tables being dropped, then the database also drops all domain indexes created on columns of those tables and invokes appropriate drop routines.
    
    - Oracle Database invalidates, but does not drop, the following objects in other schemas:

        - Views or synonyms for objects in the dropped user's schema

        - Stored procedures, functions, or packages that query objects in the dropped user's schema

    - Oracle Database does not drop materialized views in other schemas that are based on tables in the dropped user's schema. However, because the base tables no longer exist, the materialized views in the other schemas can no longer be refreshed.
    
    - Oracle Database drops all triggers in the user's schema. 
    
    - Oracle Database does not drop roles created by the user.

    Caution:
    Oracle Database also drops with FORCE all types owned by the user. See the FORCE keyword of DROP TYPE.


## Examples

### Dropping a Database User: 
Example If user Sidney's schema contains no objects, then you can drop sidney by issuing the statement:

```
DROP USER sidney; 
```

If Sidney's schema contains objects, then you must use the CASCADE clause to drop sidney and the objects:

```
DROP USER sidney CASCADE;
```