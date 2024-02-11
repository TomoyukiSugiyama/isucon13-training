# bench を実行

```bash
./bench run --enable-ssl --webapp [WEBAPPサーバIP] --nameserver [NAMEサーバIP]
```

# mysql の log 解析
## pt-query-digest をインストール

```
sudo apt update
sudo apt upgrade
sudo apt install percona-toolkit
```

## mysql の slow log を有効化

`/etc/mysql/mysql.conf.d/mysqld.cnf` の設定を有効化する

```
# Here you can see queries with especially long duration
slow_query_log		= 1
slow_query_log_file	= /var/log/mysql/mysql-slow.log
long_query_time = 2
log-queries-not-using-indexes
```

## mysql を再起動

```
sudo systemctl restart mysql
```

## log の分析

```
pt-query-digest mysql-slow.log
```

