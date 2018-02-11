\#common

\#\#install from .deb

dpkg -i DEB\_FILE

\#\#get process listen on port

lsof -i :8000

ps -fp 1289

\#\#get ubuntu version

lsb\_release -a

\#\#get application location

dpkg -L subli&lt;tab&gt;

\#ibus-unikey

\#\#install

sudo apt-get install ibus-unikey

ibus restart

\#\#then config "Text Entry Settings"

\#sublime-text

\#\#install

wget -qO - [https://download.sublimetext.com/sublimehq-pub.gpg](https://download.sublimetext.com/sublimehq-pub.gpg) \| sudo apt-key add -

echo "deb [https://download.sublimetext.com/](https://download.sublimetext.com/) apt/stable/" \| sudo tee /etc/apt/sources.list.d/sublime-text.list

sudo apt-get update

sudo apt-get install sublime-text

\#\#uninstall

sudo apt-get remove sublime-text && sudo apt-get autoremove

\#keepass

sudo apt-get install keepass2

\#flash player

\#\#install

sudo add-apt-repository "deb [http://archive.canonical.com/](http://archive.canonical.com/) $\(lsb\_release -sc\) partner"

sudo apt update

sudo apt install adobe-flashplugin browser-plugin-freshplayer-pepperflash

\#docker

\#\#install

curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) \| sudo apt-key add -

sudo add-apt-repository "deb \[arch=amd64\] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $\(lsb\_release -cs\) stable"

sudo apt-get update

sudo apt-get install -y docker-ce

\#\#status

sudo systemctl status docker

\#\#run without sudo

sudo usermod -aG docker ${USER}

su - ${USER}

id -nG

\#\#add without login

sudo usermod -aG docker huynq12

\#\#run

docker run -e "SPRING\_PROFILES\_ACTIVE=dev" -p 8080:8080 -t topica/gp600

\#filezilla

\#\#install

sudo sh -c 'echo "deb [http://archive.getdeb.net/ubuntu](http://archive.getdeb.net/ubuntu) xenial-getdeb apps" &gt;&gt; /etc/apt/sources.list.d/getdeb.list'

wget -q -O - [http://archive.getdeb.net/getdeb-archive.key](http://archive.getdeb.net/getdeb-archive.key) \| sudo apt-key add -

sudo apt update

sudo apt install filezilla

\#openvpn client

\#\#install

apt-get install openvpn

\#\#run

openvpn --config client.ovpn --http-proxy-override 127.0.0.1 3128

\#unzip

\#\#this command will not preserve original archive file

bzip2 -d filename.bz2

\#\#To preserve the original archive, add the -k option

bzip2 -dk filename.bz2

\#\#

tar xf name.tar

\#\#

tar -zxvf name.tar.xz

\#install jdk8

sudo add-apt-repository ppa:webupd8team/java

sudo apt update; sudo apt install oracle-java8-installer

\#enable mode proxy

a2enmod proxy\_http

bbb-conf --stop

/etc/init.d/jitsi-videobridge stop

/etc/init.d/jicofo stop

systemctl stop bluetooth

systemctl stop elasticsearch

systemctl stop elasticsearch5

systemctl stop kibana

systemctl stop kibana4

systemctl stop libreoffice

systemctl stop memcached

systemctl stop mongod

systemctl stop mysql

systemctl stop nginx

systemctl stop php7.0-fpm

systemctl stop prometheus

systemctl stop prosody

systemctl stop redis-server

systemctl stop teamviewerd

systemctl stop tomcat7

\# Service location

/usr/lib/systemd/system/

/etc/systemd/system/

\# Sample service

\# vim /etc/systemd/system/bbb-log.service

\[Unit\]

Description=bbb-log

After=syslog.target

\[Service\]

User=huynq12

PIDFile=/u01/applications/bbb-log/logs/app.pid

WorkingDirectory=/u01/applications/bbb-log/

ExecStart=/u01/applications/bbb-log/bin/app start

ExecStop=/u01/applications/bbb-log/bin/app stop

Restart=on-failure

RestartSec=5

\[Install\]

WantedBy=multi-user.target

\#\#reload

systemctl daemon-reload

\# Stop MySQL

sudo service mysql stop

\# Make MySQL service directory.

sudo mkdir /var/run/mysqld

\# Give MySQL user permission to write to the service directory.

sudo chown mysql: /var/run/mysqld

\# Start MySQL manually, without permission checks or networking.

sudo mysqld\_safe --skip-grant-tables --skip-networking &

\# Log in without a password.

mysql -uroot mysql

\# Update the password for the root user.

UPDATE mysql.user SET authentication\_string=PASSWORD\('YOURNEWPASSWORD'\), plugin='mysql\_native\_password' WHERE User='root' AND Host='%';

EXIT;

\#\#

GRANT ALL PRIVILEGES ON pypromgen.\* TO 'promgen'@'%' IDENTIFIED BY 'promgen' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON pypromgen.\* TO 'promgen'@'localhost' IDENTIFIED BY 'promgen' WITH GRANT OPTION;

\#\#

UPDATE mysql.user SET authentication\_string=PASSWORD\('promgen'\), plugin='mysql\_native\_password' WHERE User='root' AND Host='%';

EXIT;

\#\#

UPDATE mysql.user SET authentication\_string=PASSWORD\('promgen'\), plugin='mysql\_native\_password' WHERE User='root' AND Host='localhost';

EXIT;

\#\#

\# Turn off MySQL.

sudo mysqladmin -S /var/run/mysqld/mysqld.sock shutdown

\# Start the MySQL service normally.

sudo service mysql start

huynq12pass

[https://hn25.topicanative.edu.vn/bigbluebutton/api/configXML?a=1508838986286](https://hn25.topicanative.edu.vn/bigbluebutton/api/configXML?a=1508838986286)

[https://hn25.topicanative.edu.vn/client/conf/locales.xml?a=1508838986289](https://hn25.topicanative.edu.vn/client/conf/locales.xml?a=1508838986289)

[https://hn25.topicanative.edu.vn/client/locale/en\_US\_resources.swf?a=1508838986729](https://hn25.topicanative.edu.vn/client/locale/en_US_resources.swf?a=1508838986729)

[https://hn25.topicanative.edu.vn/bigbluebutton/api/enter](https://hn25.topicanative.edu.vn/bigbluebutton/api/enter)

[https://hn25.topicanative.edu.vn/test.html](https://hn25.topicanative.edu.vn/test.html)

[https://hn25.topicanative.edu.vn/client/BigBlueButton.html](https://hn25.topicanative.edu.vn/client/BigBlueButton.html)

* ly do dung HTTPS thay vi HTTP cho VCR

* bang thong tai thoi diem bi tam giac den \(cao tai\) --&gt; bang thong server, bang thong switch

* khi loi vao server grep log theo ip tim ERROR

* khi loi gia lap throttle vao VCR

* giang vien vao lop ntn? den gio la vao o at?

wget "[http://lms.topicanative.edu.vn/webservice/rest/server.php?wstoken=1a075920cfdbd0a1b4ccbb4c14741a3f&wsfunction=local\_addlogschat&roomid=2900938&content=e+co+sb+a&fromuser=102520&touser=public\_chat\_userid&type=PUBLIC\_CHAT](http://lms.topicanative.edu.vn/webservice/rest/server.php?wstoken=1a075920cfdbd0a1b4ccbb4c14741a3f&wsfunction=local_addlogschat&roomid=2900938&content=e+co+sb+a&fromuser=102520&touser=public_chat_userid&type=PUBLIC_CHAT)"

admin:Topica2016

48CBA01128ED75BAD8B23CBA7A1F1C95

\#create room

[http://kvip2.ivynative.com:4000/api/create](http://kvip2.ivynative.com:4000/api/create)

{"success":true,"boardId":"gux5m12kpah4","displayBoard":"gux5m12kpah4"}

support:

[https://kvip2.ivynative.com/trial/boards/gux5m12kpah4?fName=Giang&uId=4550&uRole=role\_support&lName=Ng%F4 %D0%ECnh&email=ngogiang0710@gmail.com&appId=test\_lms&modId=3767&fromId=21&auto=1](https://kvip2.ivynative.com/trial/boards/gux5m12kpah4?fName=Giang&uId=4550&uRole=role_support&lName=Ng%F4 %D0%ECnh&email=ngogiang0710@gmail.com&appId=test_lms&modId=3767&fromId=21&auto=1)

teacher:

[https://kvip2.ivynative.com/trial/boards/gux5m12kpah4?fName=Giang&uId=4551&uRole=role\_teacher&lName=Ng%F4 %D0%ECnh&email=ngogiang0710@gmail.com&appId=test\_lms&modId=3767&fromId=21&auto=1](https://kvip2.ivynative.com/trial/boards/gux5m12kpah4?fName=Giang&uId=4551&uRole=role_teacher&lName=Ng%F4 %D0%ECnh&email=ngogiang0710@gmail.com&appId=test_lms&modId=3767&fromId=21&auto=1)

student:

[https://kvip2.ivynative.com/trial/boards/gux5m12kpah4?fName=Giang&uId=4552&uRole=role\_student&lName=Ng%F4 %D0%ECnh&email=ngogiang0710@gmail.com&appId=test\_lms&modId=3767&fromId=21&auto=1](https://kvip2.ivynative.com/trial/boards/gux5m12kpah4?fName=Giang&uId=4552&uRole=role_student&lName=Ng%F4 %D0%ECnh&email=ngogiang0710@gmail.com&appId=test_lms&modId=3767&fromId=21&auto=1)

media group call server: 1.55.145.201:3000

kurento media server: ws://1.55.145.201:8888/kurento

ThuyLB3@123

1dc@123@12345

fvvta1b66tug

sudo docker run -e CATTLE\_AGENT\_IP="42.115.221.16"  -e CATTLE\_HOST\_LABELS='name=dev1'  --rm --privileged -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/rancher:/var/lib/rancher rancher/agent:v1.2.7 [http://52.221.92.104:8080/v1/scripts/98608FF2732182BDBBD4:1514678400000:wgqVwqN06fU3fcQEMXpaI9IDmo](http://52.221.92.104:8080/v1/scripts/98608FF2732182BDBBD4:1514678400000:wgqVwqN06fU3fcQEMXpaI9IDmo)

