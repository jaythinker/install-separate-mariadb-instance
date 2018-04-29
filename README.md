# install-separate-mariadb-instance
How to install a separate isolated MariaDB instance(s) if there is another MySQL/MariaDB running already

$NEW_MYSQL_HOME - directory for the new mysql install
$NEW_PORT - port different from where mysql is currently running (probably 3306)


mkdir $NEW_MYSQL_HOME
mkdir $NEW_MYSQL_HOME/conf
mkdir $NEW_MYSQL_HOME/log (log dir can be anywhere else)
mkdir $NEW_MYSQL_HOME/data (does not have to be in $NEW_MYSQL_HOME)
mkdir $NEW_MYSQL_HOME/etc


Download preferred version of MariaDB: mariadb-xx.x.xx.tar.gz

tar xvfz mariadb-xx.x.xx.tar.gz

cd mariadb-xx.x.xx

Configure:
----------

cmake -D MYSQL_DATADIR=/app/MySQL/data -D SYSCONFDIR=$NEW_MYSQL_HOME/conf -D 
              CMAKE_INSTALL_PREFIX=$NEW_MYSQL_HOME 
              -DCMAKE_BUILD_TYPE=Release 
              -DMYSQL_TCP_PORT=$NEW_PORT 
              -DMYSQL_UNIX_ADDR=$NEW_MYSQL_HOME/mysql.sock .
              

Compile and install:
--------------------
              
make install


Initialize:
-----------

$NEW_MYSQL_HOME/scripts/mysql_install_db --user=julias --basedir=$NEW_MYSQL_HOME --datadir=$NEW_MYSQL_HOME/data


Set root password:
------------------

$NEW_MYSQL_HOME/bin/mysql_secure_installation


Start:
-------

$NEW_MYSQL_HOMEL/bin/mysqld_safe --datadir='$NEW_MYSQL_HOME/data' 
          --pid-file=$NEW_MYSQL_HOME/mysqld.pid  
          --log-error=$NEW_MYSQL_HOME/mysql.log 
          --port=$NEW_PORT 
          --socket=$NEW_MYSQL_HOME/mysql.sock & 

Stop:
-----

$NEW_MYSQL_HOME/bin/mysqladmin -u root -p shutdown





