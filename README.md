# BURST Coin Ninja Pool V3 Source + Instructions; (CentOS)

I didn't write this and its not really a release; Contributions are welcome.

Will be making some changes to this very soon.

```yum -y install epel-release
yum -y group install "Development Tools"
yum -y install gcc gcc-c++ perl java wget net-tools ntp nginx java-devel maven mlocate vim perl-libwww-perl.noarch perl-Time-HiRes
yum -y install http://www.percona.com/downloads/percona-release/redhat/0.1-4/percona-release-0.1-4.noarch.rpm
yum -y install Percona-Server-server-57
yum -y update
systemctl start mysql
systemctl enable mysql
rpm -ivh https://dev.mysql.com/get/Downloads/Connector-C++/mysql-connector-c++-1.1.8-linux-el7-x86-64bit.rpm
wget http://ftp.gnu.org/gnu/libmicrohttpd/libmicrohttpd-0.9.54.tar.gz
tar -xzvf libmicrohttpd-0.9.54.tar.gz.tar.gz
cd libmicrohttpd-0.9.54.tar.gz
./configure && make && make install
useradd burst
mkdir /home/burst
mkdir /home/burst/pool
mkdir /home/burst/wallet
mkdir /home/burst/wallet/burst_db
chown -R burst:burst /home/burst
```



# DATABASE SETUP

BEFORE creating pool database, the server config needs to be changed.

/etc/my.cnf.d/burstpool.cnf (or equivalent mysql/mariadb server config file):
[server]
character-set-server=utf8
collation-server=utf8_unicode_ci
init-connect="SET NAMES utf8"
wait-timeout=600
max-connections=400
skip-networking="ON"

If using /etc/my.cnf.d then check for this in /etc/my.cnf:
!includedir /etc/my.cnf.d

To create actual database, use supplied 'burstpool.sql' or use DORM's emit_object_SQL thusly:

mysql --user=root -p --database=burstpool < burstpool.sql

OR

build/contrib/DORM/bin/emit_object_SQL objects/*.hpp | mysql --user=root -p --database=burstpool

(You will be prompted for mysql's root password in either case)
