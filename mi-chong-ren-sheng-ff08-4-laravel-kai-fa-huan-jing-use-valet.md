![](https://cdn-images-1.medium.com/max/640/1*fuyYleHiuYVejJSFrLh9tQ.png)

開始進行之前需要預先準備的東西：
* Homebrew
* Composer

準備會進行的安裝菜單：
* PHP
* Laravel/valet
* Mysql
* Redis
* NVM

以下是安裝指令順序：

安裝 php 7.1，並且將它啟動、綁定版本。

```bash
brew install homebrew/php/php71
brew services start php@7.1
brew link php@7.1 --force
```

安裝 valet

```bash
composer global require laravel/valet
```

確認 `~/.composer/vendor/bin` 在 `$PATH`全域變數中，
若用 `echo $PATH` 叫出來看之後發現沒有，就用`PATH=$PATH:~/.composer/vendor/bin`新增進去。
接著：

```bash
valet install
```

裝好之後可以輸入 `valet -v` 看現有版本，有出現就代表安裝成功了。
此外我喜歡讓專案跑起來的網址以 .app作為 TLD，所以我會多做

```bash
valet domain app
```

註：如果本地端已經有專案資料夾的話，可以順便移動進去然後做 `valet link` 把專案綁到 valet中、亦可以再透過 `valet secure PROJECT_NAME` 來幫專案加上 SSL。完成後再透過 `valet links` 指令查閱目前 valet中有綁定的專案們。

安裝 mySQL，並且把服務開起來

```bash
brew install mysql
brew services start mysql
```

預設的 user name是 root，密碼是一個空字串。可以透過 `mysql -u root -p` 指令進去看看資料庫結構。
註：可以透過以下指令知道現在本地端有哪些 port在監聽中

```bash
netstat -ap tcp | grep -i “listen"
```

安裝 redis，並且啟動服務

```bash
brew install redis
brew services start redis
```

安裝 npm (by NVM)

NVM是一套 npm版本的多重安裝工具，讓我們可以在不同專案中選擇不同的 NPM版本使用，推薦使用～！

```bash
brew install nvm
```

並且將環境變數也加入綁定

```bash
export NVM_DIR="$HOME/.nvm"
nvm install v9.11.1
nvm use 9.11.1
```

做完 NVM安裝並且綁定完版本之後，可用 `nvm ls` 確認現在綁定的 npm版本。

  


  


