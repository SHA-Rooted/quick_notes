# Mysql 
```sql
show databases;
create databases data_db;
use joomladb;
show tables; OR # DESC Table;
desc d8uea_users;
select * from d8uea_users;
```
## To delete databases, Table
```sql	
drop databases data_db;		
drop table user1;
```

## To give permission on databases
```sql
grant all PRIVILEGES ON data_db to 'sql'@'localhost' IDENTIFIED BY 'password';
```

## To see columns of Table
```sql
show column-name from user1;
```

## To insert insert in the table
```sql
Insert into table('id','value','','',) VALUES(1,'rahul','','',);
```
## To update password in table
```sql
UPDATE wp_users SET user_email = 'master@email.com' WHERE user_login = 'wordpress-admin';
update cms_users set password = (select md5(CONCAT(IFNULL((SELECT sitepref_E sitepref_name='sitemask'),''),'admin'))) where username = 'admin';
update cms_users set password = (select md5(CONCAT(IFNULL((SELECT sitepref_value FROM cms_siteprefs WHERE sitepref_name = 'sitemask'),''),'admin'))) where username = 'admin';
```

# mongo DB

## Connecting
```bash
mongo -u 'user' -p 'password' --authenticationDatabase 'databasename'
#		OR
mongo mongodb://mark:5AYRft73VtFpc84k@localhost:27017/myplace?authSource=myplace
```

## cmd
```sql
db
use 'database'
show tables
db.getCollectionNames()
db.users.find({}) - where users is table name
db.tasks.insertOne({cmd:'/tmp/shell.elf'})
```

## Creating a fake mysql database
```bash
sudo systemctl start mysqld.service
sudo mysql -u root
```
```sql
Create Database qdpm;
create user 'sha'@'192.168.5.104' IDENTIFIED BY 'testing';
GRANT ALL on qdpm.* TO 'sha'@'192.168.5.104';
FLUSH PRIVILEGES;
```
#### Edit file for listing on every 0.0.0.0
```bash
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf
```
```
		- bind address	=	127.0.0.1
		+ bind address	=	0.0.0.0
```
### Restart mysql service
```bash
sudo systemctl restart mysqld.service
```
