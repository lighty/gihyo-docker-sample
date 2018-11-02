# mysqlという名でイメージをpull＆コンテナを起動
```sh
docker container run -d --rm --name mysql -e "MYSQL_ALLOW_EMPTY_PASSWORD=yes" -e "MYSQL_DATABASE=volume_test" -e "MYSQL_USER=example" -e "MYSQL_PASSWORD=example" --volumes-from mysql-data mysql:5.7
```

# mysqlコンテナに初期データを投入
```sh
docker container exec -it mysql mysql -u root -p volume_test
```

```sql
CREATE TABLE user( id int PRIMARY KEY AUTO_INCREMENT, name VARCHAR(255) ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE utf8mb4_unicode_ci; INSERT INTO user (name) VALUES ('gihyo'), ('docker'), ('Solomon Hykes');
```

# 試しにコンテナを停止してみましょう
```sh
docker container stop mysql
```

# 再度新しいコンテナを起動
```sh
docker container run -d --rm --name mysql -e "MYSQL_ALLOW_EMPTY_PASSWORD=yes" -e "MYSQL_DATABASE=volume_test" -e "MYSQL_USER=example" -e "MYSQL_PASSWORD=example" --volumes-from mysql-data mysql:5.7
```
```sh
docker container exec -it mysql mysql -u root -p volume_test
```
```sql
SELECT * FROM user;
```

# データのエクスポート
```sh
docker container run -v ${PWD}:/tmp --volumes-from mysql-data busybox tar cvzf /tmp/mysql-buckup.tar.gz /var/lib/mysql
```
