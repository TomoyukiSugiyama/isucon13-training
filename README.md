# bench を実行

```bash
./bench run --enable-ssl --webapp [WEBAPPサーバIP] --nameserver [NAMEサーバIP]
```

# mysql の ログ解析
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

```bash
sudo systemctl restart mysql
```

## ログをクリア

```
sudo cp /dev/null /var/log/mysql/mysql-slow.log
```

## ログの解析

```bash
pt-query-digest mysql-slow.log
```

# nginx の ログ解析
## alp のインストール

## ログのフォーマットを変更

`/etc/nginx/nginx.conf` にログフォーマットを設定する

```bash
        ##
        # Logging Settings
        ##
        log_format ltsv "time:$time_local"
                "\thost:$remote_addr"
                "\tforwardedfor:$http_x_forwarded_for"
                "\treq:$request"
                "\tstatus:$status"
                "\tmethod:$request_method"
                "\turi:$request_uri"
                "\tbodysize:$body_bytes_sent"
                "\tsize:$bytes_sent"
                "\treqlen:$request_length"
                "\treferer:$http_referer"
                "\tua:$http_user_agent"
                "\treqtime:$request_time"
                "\tcache:$upstream_http_x_cache"
                "\truntime:$upstream_http_x_runtime"
                "\tapptime:$upstream_response_time"
                "\tvhost:$host";

        access_log  /var/log/nginx/access.log ltsv;
```

## nginx を再起動

```bash
sudo systemctl restart nginx.service
```

## ログをクリア

```
sudo cp /dev/null /var/log/nginx/access.log
```