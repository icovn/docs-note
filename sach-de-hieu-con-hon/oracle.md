\#\#install

- install requirement libs 

- unzip both files

- create user oracle

- set permission for the folder which has been unzipped

- set permission for the folder which will installed on

\(default for 11gR2 is /u01/app/oracle\)

\#\#config

vi ~/.bash\_profile

\#add at the end those lines:

\#----

export ORACLE\_SID=orcl

export ORACLE\_BASE=/u01/app/oracle

export ORACLE\_HOME=$ORACLE\_BASE/product/11.2.0/dbhome\_1

export PATH=$PATH:$ORACLE\_HOME/bin

\#----

source ~/.bash\_profile

\#\#stop oracle

sqlplus / as SYSDBA

SQL&gt; SHUTDOWN IMMEDIATE

\#This command may take a short while to complete

\#If the command displays no output after a number of minutes, indicating that the shutdown operation is not proceeding, 

\#press CTRL-C to interrupt the command, and then enter the following command:

SQL&gt; SHUTDOWN ABORT

\#The database must go through a recovery process when it starts up after a SHUTDOWN ABORT command. 

\#It is recommended that you enable the recovery process to take place immediately, after which you can shut down the database normally. 

\#To do this, enter the following commands when the SHUTDOWN ABORT completes

SQL&gt; STARTUP

SQL&gt; SHUTDOWN IMMEDIATE

\#To exit the SQL Command Line. enter the following command:

SQL&gt; EXIT

\#\#start oracle

sqlplus / as SYSDBA

SQL&gt; STARTUP

\#\#list of database start when oracle start

vi /etc/oratab

\#\#create profile

CREATE PROFILE app\_user LIMIT 

   SESSIONS\_PER\_USER          UNLIMITED 

   CPU\_PER\_SESSION            UNLIMITED 

   CPU\_PER\_CALL               3000 

   CONNECT\_TIME               45 

   LOGICAL\_READS\_PER\_SESSION  DEFAULT 

   LOGICAL\_READS\_PER\_CALL     1000 

   PRIVATE\_SGA                15K

   COMPOSITE\_LIMIT            5000000; 

\#\#create tablespace

CREATE TABLESPACE vsatablespace DATAFILE '/u02/app/oracle/oradata/vsa01.dbf' SIZE 510M

    EXTENT MANAGEMENT LOCAL AUTOALLOCATE;

\#\#create user

CREATE USER vsa 

    IDENTIFIED BY vsa 

    DEFAULT TABLESPACE vsatablespace 

    QUOTA 500M ON vsatablespace 

    TEMPORARY TABLESPACE temp

    QUOTA 500M ON system 

    PROFILE app\_user 

    PASSWORD EXPIRE;

\#\#grant create session

GRANT CREATE SESSION TO vsa;

\#\#create database

CREATE DATABASE mynewdb

   USER SYS IDENTIFIED BY sys\_password

   USER SYSTEM IDENTIFIED BY system\_password

   LOGFILE GROUP 1 \('/u01/app/oracle/oradata/mynewdb/redo01.log'\) SIZE 100M,

           GROUP 2 \('/u01/app/oracle/oradata/mynewdb/redo02.log'\) SIZE 100M,

           GROUP 3 \('/u01/app/oracle/oradata/mynewdb/redo03.log'\) SIZE 100M

   MAXLOGFILES 5

   MAXLOGMEMBERS 5

   MAXLOGHISTORY 1

   MAXDATAFILES 100

   CHARACTER SET US7ASCII

   NATIONAL CHARACTER SET AL16UTF16

   EXTENT MANAGEMENT LOCAL

   DATAFILE '/u01/app/oracle/oradata/mynewdb/system01.dbf' SIZE 325M REUSE

   SYSAUX DATAFILE '/u01/app/oracle/oradata/mynewdb/sysaux01.dbf' SIZE 325M REUSE

   DEFAULT TABLESPACE users

      DATAFILE '/u01/app/oracle/oradata/mynewdb/users01.dbf'

      SIZE 500M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED

   DEFAULT TEMPORARY TABLESPACE tempts1

      TEMPFILE '/u01/app/oracle/oradata/mynewdb/temp01.dbf'

      SIZE 20M REUSE

   UNDO TABLESPACE undotbs

      DATAFILE '/u01/app/oracle/oradata/mynewdb/undotbs01.dbf'

      SIZE 200M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED;

\#\#connect to db from sqlplus

sqlplus -L "vsa/vsa@\(DESCRIPTION=\(ADDRESS=\(PROTOCOL=TCP\)\(HOST=10.58.71.158\)\(PORT=1521\)\)\(CONNECT\_DATA=\(SID=huynq28\)\)\)"	    

