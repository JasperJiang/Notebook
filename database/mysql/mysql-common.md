# Mysql Common

Docker command

```bash
docker run --name mysql -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=test123 -d -p 3306:3306 mysql
```

login to mysql

```bash
mysql -hlocalhost -uroot -p
-h数据库主机
-u用户
-p密码
-P端口号（大写P）
```

Create User and change privilege 

```sql
use mysql;
CREATE USER 'username'@'%' IDENTIFIED BY 'password'; 
ALTER USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' WITH GRANT OPTION;
FLUSH   PRIVILEGES; 
```

Check User

```sql
use mysql;
select host, user from user; 
```



