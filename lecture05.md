# 第5回課題
## EC2上にサンプルアプリケーションをデプロイして動作させる
### 1. 組み込みサーバー
* Ruby(3.1.2)をインストール
````
# パッケージアップデート
$ sudo yum update

# パッケージをインストール
$ sudo yum install \
git make gcc-c++ patch \
openssl-devel \
libyaml-devel libffi-devel libicu-devel \
libxml2 libxslt libxml2-devel libxslt-devel \
zlib-devel readline-devel \`
mysql mysql-server mysql-devel \
ImageMagick ImageMagick-devel \
epel-releas


# rbenvをインストールするためにGitをインストール
$ sudo yum install git

# rbenv をインストール
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

#  ~/.bash_profile に bin の位置を登録する
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile

# ~/.bash_profile に rbenv コマンドが使えるように登録する
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

# 先ほど設定したものを再読み込みする
$ exec $SHELL -l

# rbenv のバージョンを確認する
$ rbenv --version

# ruby-build をインストール
$ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

# rehash を行う
$ rbenv rehash

````
````
# rbenv のバージョンのリストを確認する
$ rbenv install --list

# ruby3.1.2をインストール
$ rbenv install 3.1.2 --verbose

# 3.1.2を使用するように設定する
$ rbenv global 3.1.2

# rehash を行う
$ rbenv rehash

# ruby のバージョンを確認する
$ ruby -v
````
* Rails(7.0.4)をインストール
````
$ gem install rails -v 7.0.4
````
* Node.js(17.9.1)をインストール
````
# nvmをインストール
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash

# nvmを有効化
$ . ~/.nvm/nvm.sh

# Node(17.9.1)をインストール
$ nvm install 17.9.1
````
* yarnのインストール
````
$ npm install --global yarn
````
* Bandler(2.3.14)をインストール
````
$ gem install bundler -v 2.3.14
````

* サンプルアプリケーションをクローン
````
$ git clone https://github.com/yuta-ushijima/raisetech-live8-sample-app.git
$ cd raisetech-live8-sample-app
````
* MySQLをインストール
````
# インスタンス作成初期からインストールされているMariaDB用パッケージを削除する。
$ sudo yum remove mariadb-*

# MySQLのリポジトリをyumに追加する。
$ sudo yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm

# MySQLに必要なパッケージ(mysql-community-server)を取得する。
$ sudo yum install --enablerepo=mysql80-community mysql-community-server

# 下記コマンドを実行してMySQLに必要なパッケージ(mysql-community-devel)を取得する
$ sudo yum install --enablerepo=mysql80-community mysql-community-devel

# 下記コマンドを実行してインストールされたMySQLに関係のあるパッケージを出力する。
$ yum list installed | grep mysql

# 先のコマンドの結果が下記の様になれば必要パッケージのインストール完了。
mysql-community-client.x86_64         8.0.28-1.el7                   @mysql80-community
mysql-community-client-plugins.x86_64 8.0.28-1.el7                   @mysql80-community
mysql-community-common.x86_64         8.0.28-1.el7                   @mysql80-community
mysql-community-devel.x86_64          8.0.28-1.el7                   @mysql80-community
mysql-community-icu-data-files.x86_64 8.0.28-1.el7                   @mysql80-community
mysql-community-libs.x86_64           8.0.28-1.el7                   @mysql80-community
mysql-community-server.x86_64         8.0.28-1.el7                   @mysql80-community
mysql80-community-release.noarch      el7-5                          installed  

# 下記コマンドを実行してlogファイルを作成する。
$ sudo touch /var/log/mysqld.log

# 下記コマンドを実行してmysqldを起動する。
$ sudo systemctl start mysqld 
````
````
# 下記コマンドを実行してmusqldの状態を確認する。
$ systemctl status mysqld.service
 
# 下記の様に出力されれば正常にmysqldが起動できている。
         ● mysqld.service - MySQL Server
    Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
    Active: active (running) since Mon 2020-06-08 14:35:25 UTC; 25s ago
      Docs: man:mysqld(8)
            http://dev.mysql.com/doc/refman/en/using-systemd.html
   Process: 11493 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
  Main PID: 11566 (mysqld)
    Status: "Server is operational"
    CGroup: /system.slice/mysqld.service
            └─11566 /usr/sbin/mysqld
 
# 下記コマンドを実行してmysqldがインスタンスの起動と同時に起動するように設定する。
$ sudo systemctl enable mysqld
````
* database.ymlを作成、RDSのエンドポイント/ユーザー名/パスワードを登録
````
# database.yml（config/database.yml.sampleのコピー）を作成
$ cp config/database.yml.sample config/database.yml

# config/database.ymlの中身を編集
$ vim config/database.yml
````
````
# 編集内容
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: RDS作成時に設定したユーザー
  password: RDS作成時に設定したパスワード 
  host:     RDSのエンドポイント 
 
development:
  <<: *default
  database: raisetech_live8_sample_app_development
#  socket: /tmp/mysql.sock
 
test:
  <<: *default
  database: raisetech_live8_sample_app_test
#  socket: /tmp/mysql.sock

````

### 2. 組み込みサーバー(Puma)でのサンプルアプリケーション動作確認
````
$ bin/setup
$ bin/dev
````
````
# PUMAのバージョン確認
$ puma -v
````
* インバウンドルールに3000ポートを追加
* `http://IPアドレス:3000/`でアクセス

  ![3000port](/img/lecture05/puma/port3000.png)







## NginxとUnicornに分けて動作確認
### 1. Nginx
* Nginxをインストール
````
# amazon linux extrasでNginxをインストール
$ sudo amazon-linux-extras install nginx1

# Nginx起動
$ sudo systemctl start nginx

# nginx起動確認
$ sudo systemctl status nginx

````
* EC2のインバウンドルールに80ポートを追加
* `http://パブリックIPv4アドレス/`でアクセス

  ![nginx](/img/lecture05/nginx/welcome.to.nginx.png)


* Nginxの設定ファイルを編集する
````
$ sudo vim /etc/nginx/nginx.conf
````
* 設定内容
````
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user ec2-user;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
worker_connections 1024;
}

http {
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    upstream unicorn {
    server unix:/home/ec2-user/raisetech-live8-sample-app/unicorn.sock;
    }
    server {
        listen       80;
        #listen       [::]:80;
        server_name  ec2-52-198-92-78.ap-northeast-1.compute.amazonaws.com;

        access_log /var/log/nginx/access.log;
        error_log  /var/log/nginx/error.log;

        root         /home/ec2-user/raisetech-live8-sample-app/public;
        #root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        try_files $uri/index.html $uri @unicorn;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

        location @unicorn {
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header Host $http_host;
          proxy_pass http://unicorn;
        }
    }
   ````

* ec2-userにnginxの実行権限を付与
````
$ cd /var/lib
$ sudo chmod -R 775 nginx
````
````
# 設定内容が正しいか確認
$ sudo nginx -t

# 設定を保存し終えたら、Nginxを再起動して新しい設定を適用
 $ sudo systemctl restart nginx

nginxが正常に動作していることを確認
$ sudo systemctl status nginx

# Amazon Linux2 インスタンスが起動したときに自動でnginxを起動
$ sudo systemctl enable nginx
````


### 2. Unicorn
* unicorn.rbを編集
````
# configディレクトリのunicorn.rbを編集
$ cd raisetech-live8-sample-app/config
$ vim unicorn.rb

# ファイル内のlisten と pid のパスは /home/{ユーザ名}/{Railsアプリケーション名}/tmp/unicorn.sock(.pid) とする。
listen '/home/ec2-user/raisetech-live8-sample-app/unicorn.sock'
pid    '/home/ec2-user/raisetech-live8-sample-app/unicorn.pid'
````
* Gemfileを編集
````
$ cd raisetech-live8-sample-app
$ vim Gemfile
````
````
# Use Unicorn as the app server
# gem 'unicorn'

# ↑上記の箇所があるはずなので、ここの # gem 'unicorn' のコメント外し、この1行がなければ gem 'unicorn' をどこかに追加。
````
* 編集したら保存して Unicorn をインストール
````
# Unicorn をインストール
$ bundle install
````
* Unicorn の起動・停止スクリプトを作成する
````
# 以下のコマンドを実行してファイルを生成
$ rails g task unicorn
````

lib/tasks ディレクトリに unicorn.rake というファイルが生成される。  
このファイルを開いて以下を追加。  

````
namespace :unicorn do
 
  # Tasks
  desc "Start unicorn"
  task(:start) {
    config = Rails.root.join('config', 'unicorn.rb')
    sh "unicorn -c #{config} -E development -D"
  }
 
  desc "Stop unicorn"
  task(:stop) {
    unicorn_signal :QUIT
  }
 
  desc "Restart unicorn with USR2"
  task(:restart) {
    unicorn_signal :USR2
  }
 
  desc "Increment number of worker processes"
  task(:increment) {
    unicorn_signal :TTIN
  }
 
  desc "Decrement number of worker processes"
  task(:decrement) {
    unicorn_signal :TTOU
  }
 
  desc "Unicorn pstree (depends on pstree command)"
  task(:pstree) do
    sh "pstree '#{unicorn_pid}'"
  end
 
  # Helpers
  def unicorn_signal signal
    Process.kill signal, unicorn_pid
  end
 
  def unicorn_pid
    begin
      File.read("/home/vagrant/myapp/tmp/unicorn.pid").to_i
    rescue Errno::ENOENT
      raise "Unicorn does not seem to be running"
    end
  end
 
end

# ↑def unicorn_pid の File.read の引数のパスは /home/{ユーザ名}/{Railsアプリケーション名}/tmp/unicorn.pid とする。

````

* Unicornを起動
````
# Unicorn起動
$ rake unicorn:start

# unicorn -c /home/{ユーザ名}/{Railsアプリケーション名}/config/unicorn.rb -E production -D の1行が表示され、ほかにエラーが表示されなければ起動成功。

# Unicorn起動確認
$ ps -ef | grep unicorn | grep -v grep

````
* `http://パブリックIPv4アドレス/`でアクセス
  ![unicorn](/img/lecture05/unicorn/public.ip.access.png)

#### CSSが当てられていない時に対処したこと

* ログを確認すると、NginxがCSSファイルにアクセスするためにアクセス権限を変更する必要があるという内容が出ているので以下を行う。

````
# 権限変更
$ sudo chmod 755 /var/lib/nginx/tmp/proxy/

# nginx再起動
$ sudo systemctl restart nginx 

# 起動確認
$ sudo systemctl status nginx
````
* それでもうまくいかない場合 
  
development.rbの中の  
config.assets.debug = true を  config.assets.debug = falseに変更。  

````
# unicornを再起動
$ rake unicorn:stop
$ rake unicorn:start
````  
  



## ELB(ALB)追加
### 1. ターゲットグループ作成
![alb_target-group](/img/lecture05/alb/rtg.group.png)
### 2. セキュリティグループ作成
![alb_sg-detail](/img/lecture05/alb/security.group.png)
### 3. ロードバランサー作成
* リスナーとルール
![alb_listner&rule](/img/lecture05/alb/role.png)
* ネットワークマッピング
![alb_NetworkMapping](/img/lecture05/alb/alb.netmap.png)
* セキュリティ
![alb_sg](/img/lecture05/alb/alb.security.png)
### 4. DNSでアクセスする
* `development.rb`に追記
````
$ cd raisetech-live8-sample-app/config/environments
$ vim development.rb

config.host << "ALBのDNS名" 
````
* NginxとUnicornを再起動

* DNS名でアクセスし確認
  ![alb_DNS](/img/lecture05/alb/dns.access.png)

## S3追加
### 1. バケットを作成
![s3_backet](/img/lecture05/s3/s3.bucket.png)
### 2.S3にアクセスするためのIAMユーザーを作成
![s3_IAMrole](/img/lecture05/s3/s3.iam.role.png)
### 3. Railsの設定を変更
* config/storage.ymlの設定を変更
````
$ bucket: 作成したバケット名
````
* config/environments/development.rbの設定を変更
````
$ config.active.storage.service:amazon
````
### 4.New Fruitから入力し反映され、バケットに保存されていることを確認
* Nginxとunicornを再起動
*New Fruitから入力し反映されることを確認
![s3_up1](/img/lecture05/s3/s3.app.up1.png)
![s3_up2](/img/lecture05/s3/s3.app.up2.png)

### 5. EC2で確認
* S3バケットへのアクセスを検証
````
# ローカルからs3にオブジェクトをアップロード
$ aws s3 cp [アップロードしたいローカルファイルのパス] s3://バケット名/フォルダ名/
````
![s3_upload-local](/img/lecture05/s3/s3.up.png)
![s3_upload](/img/lecture05/s3/s3.up.kakunin.png)

### 6. S3で確認
````
# s3からローカルにオブジェクトをダウンロード
$ aws s3 cp s3://バケット名/オブジェクトのキー [ダウンロード先のローカルフォルダのパス]
````
![s3_download](/img/lecture05/s3/s3test.file.png)
![s3_download-local](/img/lecture05/s3/s3.dl.png)

* オブジェクトを削除
````
$ aws s3 rm s3://バケット名/オブジェクトのキー
````
![s3_delete-local](/img/lecture05/s3/s3.delete.cm.png)

## 構成図
![lecture05_diagram](/img/lecture05/diagram/lecture05.drawio.png)
