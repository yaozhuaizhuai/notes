mongodb数据备份
=======================
## 备份数据库文件
```
/usr/local/mongodb/mongodb-3.4.2/bin/mongodump -h ip:port -u username -p password -d dbname /tmp/mongodb/dbname.dmp
```

## 备份文件导入迁移服务器
```
scp /tmp/mongodb/dbname.dmp/* -r root@desIP:/tmp/mongodb/dbname.dmp
```

## 恢复备份数据
```
/usr/local/mongodb/mongodb-3.4.2/bin/mongorestore -h ip:port -u username -p password -d dbname /tmp/mongodb/dbname.dmp/dbname
```