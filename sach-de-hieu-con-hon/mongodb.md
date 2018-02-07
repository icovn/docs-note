\#docs

https://docs.mongodb.com/?\_ga=1.197440680.665327511.1472453965



\#presentation

https://www.mongodb.com/presentations?\_ga=1.197440680.665327511.1472453965



Cài đặt mongodb ở: /u01/applications/mongodb-3.0.6/

Data để ở: /u01/mongodb/data/

Cấu hình Sharded Cluster:

- Tạo config db folder:

mkdir /u01/mongodb/configdb

- Start config server

/u01/applications/mongodb-3.0.6/bin/mongod --fork --configsvr --dbpath /u01/mongodb/configdb --port 27019 --logpath /u01/mongodb/mongodb.log

- Start mongos

/u01/applications/mongodb-3.0.6/bin/mongos --fork --configdb 10.58.41.80:27019,10.58.41.81:27019,10.58.41.82:27019 --logpath /u01/mongodb/mongos.log



/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongo --port 27017

use admin

db.shutdownServer\(\)



/u01/applications/mongodb-3.0.6/bin/mongod --fork --dbpath /u01/mongodb-backup/ --logpath /u01/mongodb/mongodb.log

db.collection.find\(\)



/u01/applications/mongodb-test/bin/mongod --fork --dbpath /u01/mongodb-test/data --logpath /u01/mongodb-test/logs/mongodb.log --port 17017



\#create user:

use football\_bet;

db.createUser\( { user: "football",

                 pwd: "eOQ&lWB^aTG9",

                 customData: { site: "choieuro" },

                 roles: \[ { role: "readAnyDatabase", db: "admin" },

                          "readWrite"\] },

               { w: "majority" , wtimeout: 5000 } \);



\#grant role

use admin;

db.createUser\( { 

	user: "icovn",

	pwd: "iFY9\#SRuo%5W",

	customData: { site: "choieuro" },

	roles: \[ { role: "readAnyDatabase", db: "admin" }, "readWrite"\] 

	},{ w: "majority" , wtimeout: 5000 } \);

db.grantRolesToUser\(

   "icovn",

   \[ { role: "dbOwner", db: "admin" }, { role: "dbOwner", db: "football\_bet" }, { role: "dbOwner", db: "test" }\],

   { w: "majority" , wtimeout: 4000 }

\);

db.grantRolesToUser\(

   "football",

   \[ { role: "dbOwner", db: "football\_bet" }\],

   { w: "majority" , wtimeout: 4000 }

\);



\#\#run authen

https://docs.mongodb.com/manual/tutorial/transparent-huge-pages/

https://www.kernel.org/doc/Documentation/vm/transhuge.txt

echo never &gt; /sys/kernel/mm/transparent\_hugepage/enabled

echo never &gt; /sys/kernel/mm/transparent\_hugepage/defrag

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongod --fork --dbpath /u01/mongodb/data/ --logpath /u01/mongodb/logs/mongodb.log --auth 



/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongo admin --username icovn --password

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongo football\_bet --username football --password



db.getCollection\('system.users'\).find\({}\).limit\(10\);



\#\#roles - https://docs.mongodb.com/manual/reference/built-in-roles/

\#\#actions - https://docs.mongodb.com/manual/reference/privilege-actions/

\#\#backup-script------------------------------------------------------------------------------------------------------

\#!/bin/bash

MONGODUMP\_PATH="/usr/bin/mongodump"

MONGO\_HOST="prod.example.com"

TIMESTAMP=\`date +%F-%H%M\`

S3\_BUCKET\_NAME="bucketname"

S3\_BUCKET\_PATH="mongodb-backups"



\# Create backup

$MONGODUMP\_PATH --host $HOST



\# Add timestamp to backup

mv dump mongodb-$HOSTNAME-$TIMESTAMP

tar cf mongodb-$HOSTNAME-$TIMESTAMP.tar mongodb-$HOSTNAME-$TIMESTAMP



\# Upload to S3

s3cmd put mongodb-$HOSTNAME-$TIMESTAMP.tar s3://$S3\_BUCKET\_NAME/$S3\_BUCKET\_PATH/mongodb-$HOSTNAME-$TIMESTAMP.tar

\#\#---------------------------------------------------------------------------------------------------------------------

\#create db & collection

use football\_bet

db.getCollectionNames\(\)

db.runCommand\( {

    create: "memberBet"

} \)

db.runCommand\( {

    create: "memberMoney"

} \)

db.getCollection\('memberBet'\).find\({}\).limit\(30\);

db.getCollection\('memberBet'\).count\({ member\_id : 105}\).limit\(30\);

{ count: "memberBet", query: { member\_id: 105 } }

db.getCollection\('memberBet'\).find\({ match\_id : 185}\).limit\(30\);

db.getCollection\('memberBet'\).find\({ match\_id : NumberLong\("43"\)}\).limit\(30\);

db.getCollection\('memberBet'\).find\({ member\_id : 97}\).sort\( { created\_at: 1 } \)

db.getCollection\('member\_money'\).find\({ member\_id : 10}\).limit\(30\);

db.getCollection\('payBySmsResult'\).find\({ client\_mobile : { $ne: "0983825860" }}\).limit\(30\);

db.getCollection\('payBySmsResult'\).find\({ client\_mobile : { $nin : \["0983825860", "0989080185"\] }}\).limit\(30\);



db.getCollection\('payByMobileCardResult'\).find\({ member\_id : 1755}\).limit\(30\);



db.getCollection\('payBySmsResult'\).find\({}\).limit\(300\);

db.getCollection\('payByMobileCardResult'\).find\({}\).limit\(300\);



db.getCollection\('memberBet'\).find\(\).snapshot\(\).forEach\(

    function \(elem\) {

        db.getCollection\('memberBet'\).update\(

            {

                \_id: elem.\_id

            },

            {

                $set: {

                    matchId: elem.match\_id

                }

            }

        \);

    }

\);



\#pagging

db.getCollection\('comment'\).find\({}\).limit\(3\);

db.getCollection\('comment'\).find\({\_id: { $gt: ObjectId\("548e911745ce265fc7274361"\) }}\). limit\(1\);

\#create hash index

db.collection.createIndex\( { \_id: "hashed" } \)

\#create index in background

db.collection.createIndex\( { a: 1 }, { background: true } \)

\#get index

db.collection.getIndexes\(\)



\#force to use particular index then explain

db.people.find\(

   { name: "John Doe", zipcode: { $gt: "63000" } }

\).hint\( { zipcode: 1 } \).explain\("executionStats"\)



\#check size of index in bytes

db.collection.totalIndexSize\(\)



\#count item

db.collection.count\(\)

\#437960

db.getCollection\('vaaalog'\).count\({}\)

\#396522

db.getCollection\('vaaalog'\).count\({ isRecognized: true}\)

db.getCollection\('vaaalog'\).find\({ isRecognized: true}\)

\#16680

db.getCollection\('vaaalog'\).count\({ isRecognized: false, is3g: true}\)

db.getCollection\('vaaalog'\).find\({ isRecognized: false, is3g: true}\)



db.getCollection\('160128-ga-report'\).count\({action: 'access', msisdn: { $ne: '' }}\)

db.getCollection\('160128-ga-report'\).aggregate\( \[

    {	 

        $match: 

        { 

            $and: 

            \[ 

                { action: "access" }, 

                { msisdn: { $ne: '' } },

                { gaUrl: { $ne: 'http://m.keeng.vn/hga.php/cancel2.html' } },

                { gaUrl: { $ne: 'http://m.keeng.vn/hga.php/cancel1.html' } },

                { gaUrl: { $ne: 'http://m.keeng.vn/hga.php/first.html' } },

            \] 

        } 

    },

    { 

        $group: 

        { 

            \_id: "$gaUrl", count: { $sum: 1 } 

        } 

    }

\] \);

db.getCollection\('160128-ga-report'\).aggregate\( \[

    {	 

        $match: 

        { 

            $and: 

            \[ 

                { action: "access" }, 

                { msisdn: { $eq: '' } },

                { gaUrl: { $ne: 'http://m.keeng.vn/hga.php/cancel2.html' } },

                { gaUrl: { $ne: 'http://m.keeng.vn/hga.php/cancel1.html' } },

                { gaUrl: { $ne: 'http://m.keeng.vn/hga.php/first.html' } },

            \] 

        } 

    },

    { 

        $group: 

        { 

            \_id: "$gaUrl", count: { $sum: 1 } 

        } 

    }

\] \);

\#\#---------------------------------------------------------------------------------------------------------------------

\#\#replicate

\#primary server

/u01/applications/mongodb-3.0.6/bin/mongod --fork --dbpath /u01/mongodb-backup/ --logpath /u01/mongodb/mongodb.log --replSet keeng

rs.initiate\({\_id:"keeng", members: \[{"\_id":1, "host":"10.58.41.81:27017"}\]}\)

\#secondary server: delete all files and folders of db folder

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongod --fork --dbpath /u01/mongodb/data/ --logpath /u01/mongodb/logs/mongodb.log --replSet keeng

numactl --interleave=all /u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongod --fork --dbpath /u01/mongodb/data/ --logpath /u01/mongodb/logs/mongodb.log --replSet keeng

\#primary server

rs.add\("10.30.142.44:27017"\)

rs.add\("10.30.142.45:27017"\)

rs.add\("10.30.142.46:27017"\)

\#secondary server after sync:

rs.slaveOk\(\)

\#remove:

rs.remove\("10.58.41.81:27017"\)

\#\#---------------------------------------------------------------------------------------------------------------------

\#\#command

show dbs

use mydb

db.dropDatabase\(\)

\#\#---------------------------------------------------------------------------------------------------------------------

\#dump

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongodump --db Keeng --archive=/u02/keeng.20161005.archive



\#\#restore

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongorestore --archive=/u02/keeng.20161005.archive --db Keeng



\#\#export data

./mongoexport -d keeng-log -c 1601140830vaaa -q '{ isRecognized: false, is3g: true }' --out /u01/applications/mongodb-3.0.6/1601140830vaaa.csv --type=csv --fields ip,msisdn,dateCreate,msisdn

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongoexport -d keeng-log-action -c userActivity --out /u02/backup\_database/userActivity.json

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongoexport -d keeng-log-action -c logBanner --out /u02/backup\_database/logBanner.json

/u01/applications/mongodb-linux-x86\_64-3.2.4/bin/mongoexport -d keeng-log -c keeng\_alert\_subscriber --out /u02/backup\_database/keeng\_alert\_subscriber.json





use football\_bet

db.getProfilingLevel\(\)

db.setProfilingLevel\(2\)

db.getProfilingLevel\(\)

db.system.profile.find\(\).limit\(10\).sort\( { ts : -1 } \)



http://www.mkyong.com/mongodb/spring-data-mongodb-query-document/

