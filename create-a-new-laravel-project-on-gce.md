常常在練習了很多東西之後一陣子沒有看就直接忘光光，大概是即將不再是青年的警訊······所以決定要乖乖開始記錄學習的步驟，給幾個月後失去記憶的自己看。

這邊紀錄的是如何在 Google compute engine 上面建立一個新的 laravel 專案，以及過程中遇到的問題。

### 一、 Google Compute Engine 設定

* 開啟新的 project

* 開啟新的 gce instance 

* 透過 google cloud shell 進入機器

---

### 二、 linux 設定步驟

\# 更新並且新增軟體資料庫

```
sudo apt-get update
sudo apt-get install software-properties-common
```

\# 加入 PHP 7 相關的套件資源庫
```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```
\# 安裝 PHP 7.0 相關套件
```
sudo apt-get install php7.0 php7.0-cli php7.0-fpm php7.0-mbstring php7.0-mcrypt php7.0-xml php7.0-zip php7.0-common php7.0-opcache php7.0-soap php7.0-gd php7.0-imap php7.0-curl php7.0-tidy php7.0-mysql php7.0-bcmath php7.0-intl
```
參考自 Laravel 官方替 Homestead 安裝的套件， 可參考

[http://php.net/manual/zh/funcref.php](http://php.net/manual/zh/funcref.php)

\# 安裝 nginx
```
sudo apt-get install nginx
```
\# 安裝 MySQL

安裝 Server MySQL，會跳出互動介面，同時必須設定 root 密碼
```
sudo apt-get install mysql-server
```
\# 安裝 Git
```
sudo apt-get install git
```
\# 安裝 redis-server
```
sudo apt-get install redis-server
```
如果會在 php 中使用的話也要裝 php5-redis 套件
```
sudo apt-get install php7.0-redis
```
\# 安裝 Composer
```
curl -sS https://getcomposer.org/installer | sudo php ——install-dir=/usr/local/bin—filename=composer
```
\# 直接安裝 Laravel
```
sudo composer create -project laravel/laravel /var/www/laravel
```
以上指令會安裝一個全新的 laravel 專案在 /var/www/laravel

\# 修改權限為 
```
sudo chown -R /var/www/laravel
```
\# 設定根目錄資料夾權限
目前假設你把 /var/www/laravel當你的根目錄
修改 Storage 的資料夾的權限為 775
```
sudo chmod -R 775 /var/www/laravel/storage
```

\# 設定 .env 檔
```
cd /var/www/laravel
```
把 .env.example 把裡面的內容全部複製過來
```
sudo mv .env.example .env
```
記得因為你現在是放在 Server，要把裡面的
```
APP_ENV=local
APP_DEBUG=true
```
改成
```
APP_ENV=production // 表示此為 product 環境
APP_DEBUG=false // 避免跑出偵錯訊息
```
\# 讓 Composer 下載相依套件
```
composer install — no-dev
```
以上都已經順利進行之後，應該就快要大功告成了， /var/www 底下目前有

/html
/laravel

兩個資料夾，所以下一步是把 web root指定到新的位置並且讓 laravel 開始服務。這裡恐怖的事情發生了，上面我跟著別人的文章一步一步安裝了 nginx ，但是透過 command line 查半天之後發現 nginx 沒有在動，而是 apache2 正在工作。居然是因為不小心兩個都裝了，所以導致衝突······於是馬上改找如何在 apache2 中更改其 document root 設定，查到方法如下：
```
sudo vim /etc/apache2/sites-enabled/000-default.conf
```
在文件中把 DocumentRoot 這行後面設定的位置改成應該要去的地方，例如 larval 中的 public 資料夾 /var/www/laravel/public

\)。接著， ssl 部分的設定也要一併完成，所以

```
sudo vim /etc/apache2/sites-available/ssl-default.conf
```

同樣把 DocumentRoot 這行後面設定的位置改成應該要去的地方，為了讓設定生效，重新啟動 apache2
```
sudo /etc/init.d/apache2 restart
```
終於成功從外部 IP 看到 laravel 的歡迎頁面了····（累

### 三、參考資料

* [Laravel5 實戰技巧](https://abel_lee.gitbooks.io/laravel-5-in-real/content/Deploy/Laravel_Setup.html)

* [安裝composer](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-14-04)

* [How To Move an Apache Web Root to a New Location on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-move-an-apache-web-root-to-a-new-location-on-ubuntu-16-04)

