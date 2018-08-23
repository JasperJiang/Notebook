# Database Container

### MySql

Run mysql

```bash
docker run --name mysql -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=test123 -d -p 3306:3306 mysql
```

### Sql Server

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=test123!' -p 1433:1433 -d microsoft/mssql-server-linux
```

```bash
docker exec -i <container_id> bash -c 'cat > /var/opt/mssql/backup.bak' < '/source/path/backup.bak’
```

restore

```bash
RESTORE DATABASE AnalyticAppsUserManagement  
   FROM DISK = '/var/opt/mssql/backup.bak'
   WITH
   MOVE 'test' TO '/var/ops/mssql/test.mdf',
   MOVE 'test1' TO '/var/ops/mssql/test1.mdf',
   MOVE 'test2' TO '/var/ops/mssql/test2.mdf',
   MOVE 'test3' TO '/var/ops/mssql/test3.mdf',
   MOVE 'test_log' TO '/var/ops/mssql/test_log.ldf',
   MOVE 'test_log1' TO '/var/ops/mssql/test_log1.ldf',
   MOVE 'test_log2' TO '/var/ops/mssql/test_log2.ldf',
   MOVE 'test_log3' TO '/var/ops/mssql/test_log3.ldf'
```

