# Monitインストール方法(CentOS7)
Monitとは、Linuxで使用できる監視プログラムの名称.  
Licence: AGPL  
用途: サーバ内監視、外部監視(小規模向け)  

# インストール
## 環境
CentOS7(x86_64)

## epel登録
```
$ sudo yum -y install epel-release.noarch
$ sudo yum -y update
```

## Monitインストール
```
$ sudo yum -y install monit
$ rpm -qa monit
monit-5.14-1.el7.x86_64
```

## 設定
### 基本設定
```
‘/etc/monitrc’ -> ‘/etc/monitrc.bak’
$ sudo vi /etc/monitrc
-----
set daemon  30                 # Monitデーモンによる、サービス監視周期

# Monit Webインターフェイスの設定  ※ Monit Webインターフェイスのがないと 「# monit status」コマンドが実行できない。
set httpd port 2812 and             # Monit WebUIのPort番号
    allow admin:monit               # Monit WebUIのID/PW (admin/monit)
    allow 192.168.0.0/255.255.255.0 # Monit WebUIを閲覧できる範囲(CIDR)
-----
```

### 設定確認
Syntaxチェックができる.
```
$ sudo monit -t
Cannot translate 'monit01.localdomain' to FQDN name -- Name or service not known
Generated unique Monit id d3ffacff9a2b067b54bf2e5fb10f9b0a and stored to '/root/.monit.id'
Control file syntax OK
```

### サービス起動
```
$ sudo systemctl enable monit
$ sudo systemctl start monit
$ sudo systemctl status monit
● monit.service - Pro-active monitoring utility for unix systems
   Loaded: loaded (/usr/lib/systemd/system/monit.service; disabled; vendor preset: disabled)
   Active: active (running) since Sat 2016-06-11 15:45:13 UTC; 3s ago
 Main PID: 2047 (monit)
   CGroup: /system.slice/monit.service
           └─2047 /usr/bin/monit -I
```

## 設定内容
Configファイルを参照.
