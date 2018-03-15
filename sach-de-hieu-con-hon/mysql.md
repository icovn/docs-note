/\*------------------------------------------------------------------------------------------------------------------------------------\*/

/\*1. GET INFORMATION\*/

---

\#\#run without install

mkdir /u01/applications/mysql-5.6.28/logs

/u01/applications/mysql-5.6.28/bin/mysqld --basedir=/u01/applications/mysql-5.6.28 --datadir=/u01/mysql-data --plugin-dir=/u01/applications/mysql-5.6.28/lib/plugin --log-error=/u01/applications/mysql-5.6.28/logs/db.err --pid-file=/u01/applications/mysql-5.6.28/db.pid --socket=/tmp/mysql.sock --port=3306 --old\_passwords=0 --skip-host-cache --skip-name-resolve

./scripts/mysql\_install\_db --datadir=/u01/mysql-data/ --user=mysql

\#\#for &gt;= v5.6, set password mode for native authentication

SET old\_passwords = 2;

\#\#grant permission

GRANT ALL PRIVILEGES ON magento.\* TO 'magentodbu'@'%' IDENTIFIED BY 'abc@123' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON mycity.\* TO 'mycitydbu'@'localhost' IDENTIFIED BY 'abc@123' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON \*.\* TO 'admin'@'%' IDENTIFIED BY '7DBn0Emo8GIa' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON \*.\* TO 'admin'@'localhost' IDENTIFIED BY '7DBn0Emo8GIa' WITH GRANT OPTION;

\#\#grant permission with sha256\_password

CREATE USER 'huynq'@'%' IDENTIFIED WITH sha256\_password;

SET PASSWORD FOR 'huynq'@'%' = PASSWORD\('eOQ&lWB^aTG9'\);

GRANT ALL PRIVILEGES ON \*.\* TO 'huynq'@'%' WITH GRANT OPTION;

CREATE USER 'huynq'@'localhost' IDENTIFIED WITH sha256\_password;

SET PASSWORD FOR 'huynq'@'localhost' = PASSWORD\('eOQ&lWB^aTG9'\);

GRANT ALL PRIVILEGES ON \*.\* TO 'huynq'@'localhost' WITH GRANT OPTION;

\#\#change password

UPDATE mysql.user SET Password=PASSWORD\('otCK7aUP3jhR'\) WHERE User='root';

ALTER USER 'root'@'localhost' IDENTIFIED BY 'otCK7aUP3jhR' PASSWORD EXPIRE INTERVAL 180 DAY;

/\*------------------------------------------------------------------------------------------------------------------------------------\*/

/\*2. RESET ROOT PASSWORD\*/

---

mysqld\_safe --skip-grant-tables

mysql --user=root mysql

update user set Password=PASSWORD\('new-password'\) where user='root';

update user set authentication\_string=password\('password'\) where user='root';

flush privileges;

/\*------------------------------------------------------------------------------------------------------------------------------------\*/

/\*3. EXPORT IMPORT DATABASE\*/

---

\#export

mysqldump -u USER -p DATABASE &gt; FILENAME.sql

\#import

mysql -h localhost -u imuzikdbu -p imuzik &lt; imuzik.sql

\#copy to other host

mysqldump -u mediadbu -p'Application@123!@\#' charging\_log \| mysql -h 10.58.62.214 -u mediadbu -p'Application@123!@\#' charging\_log

\#copy to other host with ssh

mysqldump -u mediadbu -p'Application@123!@\#' charging\_log \| ssh ptnd@10.58.62.214 mysql -h 10.58.62.214 -u mediadbu -p'Application@123!@\#' charging\_log

mysqldump -u mediadbu -p'Application@123!@\#' charging\_log \| ssh ptnd@10.58.62.214 'cat - \| mysql -h 10.58.62.214 -u mediadbu -p'Application@123!@\#' charging\_log'

\#dump to file

mysqldump -u mediadbu -p'Application@123!@\#' charging\_log kc\_sms\_mt\_history\_bk\_092015 \| gzip &gt; kc\_sms\_mt\_history\_bk\_092015.sql.gz

mysqldump db\_name table\_name \| gzip &gt; table\_name.sql.gz

gunzip &lt; table\_name.sql.gz \| mysql -u username -p db\_name

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_092015.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_092015"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_102015.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_102015"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_112015.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_112015"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_122015.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_122015"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_012016.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_012016"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_022016.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_022016"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_032016.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_032016"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_042016.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_042016"

mysql --socket=/u01/mysql-5.6.26/mysql.sock --port=3306 -u mediadbu -p'Application@123!@\#' charging\_log -e "SELECT \* INTO OUTFILE '/u01/mysql-backup/kc\_sms\_mt\_history\_bk\_052016.csv' FIELDS TERMINATED BY '\|' LINES TERMINATED BY '\n' FROM kc\_sms\_mt\_history\_bk\_052016"

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_092015.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_102015.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_112015.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_122015.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_012016.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_022016.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_032016.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_042016.csv

gzip /u01/mysql-backup/kc\_sms\_mt\_history\_bk\_052016.csv

/\*------------------------------------------------------------------------------------------------------------------------------------\*/

/\*4. OTHERS\*/

---

\#\#clone/copy table

\#To copy with indexes and triggers

CREATE TABLE newtable LIKE oldtable;

INSERT newtable SELECT \* FROM oldtable;

\#To copy just structure and data

CREATE TABLE tbl\_new AS SELECT \* FROM tbl\_old;

\#\#disable foreign check

SET FOREIGN\_KEY\_CHECKS=0;

\#\#clear binary log

PURGE BINARY LOGS TO 'binlog.000670';

\#\#drop user

DROP USER 'mycitydbu'@'localhost';

DROP USER 'mycitydbu'@'%';

\#\#create user

CREATE USER 'mycitydbu'@'localhost' IDENTIFIED BY 'abc@123';

CREATE USER 'mycitydbu'@'%' IDENTIFIED BY 'abc@123';

\#\#show grants

SHOW GRANTS FOR 'mycitydbu'@'%';

SHOW GRANTS FOR 'mycitydbu'@'localhost';

\#\#revoke grants

REVOKE ALL PRIVILEGES ON \`mycity-ea\`.\* FROM 'mycitydbu'@'localhost';

REVOKE GRANT OPTION ON \`mycity-ea\`.\* FROM 'mycitydbu'@'localhost';

\#\#grant

GRANT SELECT ON mycity.\* TO 'tester'@'%' IDENTIFIED BY 'abc@123' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON \`mycity-ea\`.\* TO 'mycitydbu'@'%' IDENTIFIED BY 'abc@123' WITH GRANT OPTION;

GRANT SELECT, INSERT, UPDATE, DELETE ON mycity.\* TO 'mycitydbu'@'localhost' IDENTIFIED BY 'abc@123' WITH GRANT OPTION;

\#\#set sql mode

SET SQL\_MODE = 'NO\_UNSIGNED\_SUBTRACTION';

SET GLOBAL SQL\_MODE= 'NO\_UNSIGNED\_SUBTRACTION';

\#\#get sql mode

SELECT @@GLOBAL.sql\_mode;

SELECT @@SESSION.sql\_mode;

\#\#set max\_allowed\_package \(16MB\)

SET GLOBAL max\_allowed\_packet=16\*1024\*1024;

\#for big db

SET GLOBAL net\_buffer\_length=1000000;

SET GLOBAL max\_allowed\_packet=1000000000;

\#\#log query

show global variables like'log\_output';

SET GLOBAL general\_log=ON;

SET GLOBAL log\_output='TABLE';

\#\#log slow query

show global variables like'slow\_query\_log';

SET GLOBAL slow\_query\_log=ON;

SET GLOBAL log\_output='TABLE';

SET GLOBAL long\_query\_time=3;

\#\#check plugin

SELECT

```
PLUGIN\_NAME AS NAME, 

PLUGIN\_VERSION AS VERSION, 

PLUGIN\_STATUS AS STATUS 
```

FROM INFORMATION\_SCHEMA.PLUGINS

WHERE PLUGIN\_TYPE='STORAGE ENGINE';

---

\#\#Start up MySQL in rescue mode

\#find the size of your Innodb logfiles

ls -l

-rw-rw—- 1 mysql mysql 5242880 Jun 25 11:30 ib\_logfile0

-rw-rw—- 1 mysql mysql 5242880 Jun 25 11:30 ib\_logfile1

\#Start up your mysqld process with the logfile size and innodb\_force\_recovery as parameters

/usr/sbin/mysqld –innodb\_log\_file\_size=5242880 –innodb\_force\_recovery=6

InnoDB: The user has set SRV\_FORCE\_NO\_LOG\_REDO on

InnoDB: Skipping log redo

070625 11:59:36 InnoDB: Started; log sequence number 0 0

InnoDB: !!! innodb\_force\_recovery is set to 6 !!!

070625 11:59:36 \[Note\] /usr/sbin/mysqld: ready for connections.

Version: ‘5.0.18’ socket: ‘/var/lib/mysql/mysql.sock’ port: 3306 SUSE MySQL

---

CREATE TABLE \`vt\_ga\_log\` \(

    \`created\_at\` VARCHAR\(255\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`action\` VARCHAR\(12\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`ip\` VARCHAR\(20\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`url\` VARCHAR\(255\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`msisdn\` VARCHAR\(12\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`result\` VARCHAR\(2\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`ga\_url\` VARCHAR\(255\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`query\` VARCHAR\(255\) COLLATE latin1\_general\_ci DEFAULT NULL,

    \`inserted\_at\` DATETIME DEFAULT NULL,

    \`is\_3g\` TINYINT\(1\) DEFAULT NULL,

    KEY \`url\` \(\`url\`\),

    KEY \`action\` \(\`action\`\),

    KEY \`msisdn\` \(\`msisdn\`\)

\) ENGINE=INNODB DEFAULT CHARSET=latin1 COLLATE=latin1\_general\_ci

PARTITION BY RANGE\( MONTH\(inserted\_at\) \)

SUBPARTITION BY HASH\( TO\_DAYS\(inserted\_at\) \)

SUBPARTITIONS 2 \(

```
PARTITION p1 VALUES LESS THAN \(2\),

PARTITION p2 VALUES LESS THAN \(3\),

PARTITION p3 VALUES LESS THAN \(4\),

PARTITION p4 VALUES LESS THAN \(5\),

PARTITION p5 VALUES LESS THAN \(6\),

PARTITION p6 VALUES LESS THAN \(7\),

PARTITION p7 VALUES LESS THAN \(8\),

PARTITION p8 VALUES LESS THAN \(9\),

PARTITION p9 VALUES LESS THAN \(10\),

PARTITION p10 VALUES LESS THAN \(11\),

PARTITION p11 VALUES LESS THAN \(12\),

PARTITION p12 VALUES LESS THAN MAXVALUE
```

\);

---

-- find not UNICODE \(ASCII\)

select name, identify, path, wap\_path, image\_path from vt\_video where is\_active = 1

and name = CONVERT\(name USING ASCII\)

order by id desc;

---

-- random

SELECT FLOOR\(\(RAND\(\) \* \(max-min+1\)\)+min\)

/\*------------------------------------------------------------------------------------------------------------------------------------\*/

