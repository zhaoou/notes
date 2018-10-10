# Mysql setup

* Install mysql
```
apt-get update
apt-get install mysql-server
```

* run secure installation script
```
sudo mysql_secure_installation
```
* Edit /etc/mysql/mysql.conf.d/mysqld.cnf to listen on 0.0.0.0 instead of 127.0.0.1

* Sign in into mysql `mysql -u root -p` and run:
```
CREATE USER 'newuser'@'%' IDENTIFIED BY 'password';
```
* Created schema for the application:
```
create schema new_schema;
```

* Grant privileges to this user on this schema:
```
grant all privileges on mydb.* to myuser@'%' identified by 'mypasswd'; // <- for remote connection
grant all privileges on mydb.* to myuser@localhost identified by 'mypasswd'; // <- local connection
```
* exit and enjoy!
