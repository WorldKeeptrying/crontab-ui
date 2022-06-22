基於 Node.Js 裝配的 Crontab UI
==========

[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=U8328Q7VFZMTS)
[![npm](https://img.shields.io/npm/v/crontab-ui.svg?style=flat-square)](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)
[![npm](https://img.shields.io/npm/dt/crontab-ui.svg?style=flat-square)](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)
[![npm](https://img.shields.io/npm/dm/crontab-ui.svg?style=flat-square)](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)
[![npm](https://img.shields.io/docker/pulls/alseambusher/crontab-ui.svg?style=flat-square)](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)
[![npm](https://img.shields.io/npm/l/crontab-ui.svg?style=flat-square)](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)

Editing the plain text crontab is error prone for managing jobs, e.g., adding jobs, deleting jobs, or pausing jobs. A small mistake can easily bring down all the jobs and might cost you a lot of time. With Crontab UI, it is very easy to manage crontab. Here are the key features of Crontab UI.

![flow](https://github.com/alseambusher/crontab-ui/raw/gh-pages/screenshots/flow.gif)

1. 簡單地裝配並導入你的 Cron 定時器 Easy setup. You can even import from existing crontab.  
2. 
3. 確保資安穩固的同時可以輕易制定任務 Safe adding, deleting or pausing jobs. Easy to maintain hundreds of jobs.
4. 存檔你的定時任務清單並以防意外 Backup your crontabs.
5. 導出你的定時器並讓另一部機器使用 Export crontab and deploy on other machines without much hassle.
6. 可以顯示錯誤日誌 Error log support.
7. 額外支持 Mailing and hooks support.

Read [this](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html) to see more details.
查閱這個文檔 [瞭解更多信息](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)

## Setup
## 開始裝配

Get latest `node` from [here](https://nodejs.org/en/download/current/). Then,
確認機器已經安裝 nodejs ，或者[瞭解如何安裝](https://nodejs.org/en/download/current/)

    npm install -g crontab-ui
    crontab-ui


如果你打算 讓公網用戶也可以使用，需要用以下命令啓動:（端口設定爲9000，可以自定）
If you need to set/use an alternative host, port OR base url, you may do so by setting an environment variable before starting the process:

    HOST=0.0.0.0 PORT=9000 BASE_URL=/alse crontab-ui

默認裝配模式下，數據會存儲位於默認位置，如果需要將數據存儲到其他地方，使用以下命令啓動。
By default, db, backups and logs are stored in the installation directory. It is **recommended** that it be overriden using env variable `CRON_DB_PATH`. This is particularly helpful in case you **update** crontab-ui.

    CRON_DB_PATH=/path/to/folder crontab-ui
    
默認無密碼，如果需要設定登錄帳號名與密碼，用以下命令啓動
If you need to apply basic HTTP authentication, you can set user name and password through environment variables:

    BASIC_AUTH_USER=user BASIC_AUTH_PWD=SecretPassword
    
如果 node模塊 出現問題， 可以查閱這個  [文檔](https://docs.npmjs.com/getting-started/fixing-npm-permissions).
Also, you may have to **set permissions** for your `node_modules` folder. Refer [this](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

默認不啓用 SSL ，需要SSL 功能之前必須先要創建私鑰與證書，然後使用以下命令啓動
If you need to use SSL, you can pass the private key and certificate through environment variables:

    SSL_CERT=/path/to/ssl_certificate SSL_KEY=/path/to/ssl_private_key

Make sure node has the correct **permissions** to read the certificate and the key.

如果需要啓用 自動保存功能，執行以下
If you need to autosave your changes to crontab directly:

    crontab-ui --autosave

### List of ennvironment variables supported
### 已經兼容的功能
- HOST
- PORT
- BASE_URL
- CRON_DB_PATH
- CRON_PATH
- BASIC_AUTH_USER, BASIC_AUTH_PWD
- SSL_CERT, SSL_KEY 
- ENABLE_AUTOSAVE


## Docker
通過 Docker 裝配並啓動
You can use crontab-ui with docker. You can use the prebuilt images in the [dockerhub](https://hub.docker.com/r/alseambusher/crontab-ui/tags)
```bash
docker run -d -p 8000:8000 alseambusher/crontab-ui
```

獲取源碼並用docker 裝配 You can also build it yourself if you want to customize, like this:
```bash
git clone https://github.com/alseambusher/crontab-ui.git
cd crontab-ui
docker build -t alseambusher/crontab-ui .
docker run -d -p 8000:8000 alseambusher/crontab-ui
```

用docker 啓動時使用帳號與密碼功能。
If you want to use it with authentication, You can pass `BASIC_AUTH_USER` and `BASIC_AUTH_PWD` as env variables

```bash
docker run -e BASIC_AUTH_USER=user -e BASIC_AUTH_PWD=SecretPassword -d -p 8000:8000 alseambusher/crontab-ui 
```

你可以把 裝配的文件設定爲其他位置 
You can also mount a folder to store the db and logs.

```bash
mkdir -p crontabs/logs
docker run --mount type=bind,source="$(pwd)"/crontabs/,target=/crontab-ui/crontabs/ -d -p 8000:8000 alseambusher/crontab-ui
```

If you are looking to modify the host's crontab, you would have to mount the crontab folder of your host to that of the container. 
```bash
# On Ubuntu, it can look something like this and /etc/cron.d/root is used
docker run -d -p 8000:8000 -v /etc/cron.d:/etc/crontabs alseambusher/crontab-ui
```

    
## Resources
## 更多幫助訊息
* [Full usage details](https://lifepluslinux.blogspot.com/2015/06/crontab-ui-easy-and-safe-way-to-manage.html)
* [Issues](https://github.com/alseambusher/crontab-ui/blob/master/README/issues.md)
* [Setup Mailing after execution](https://lifepluslinux.blogspot.com/2017/03/introducing-mailing-in-crontab-ui.html)
* [Integration with nginx and authentication](https://github.com/alseambusher/crontab-ui/blob/master/README/nginx.md)
* [Setup on Raspberry pi](https://lifepluslinux.blogspot.com/2017/03/setting-up-crontab-ui-on-raspberry-pi.html)

### Adding, deleting, pausing and resuming jobs.
### 添加定時，刪除定時，查閱定時 示例圖
Once setup Crontab UI provides you with a web interface using which you can manage all the jobs without much hassle.

![basic](https://github.com/alseambusher/crontab-ui/raw/gh-pages/screenshots/main.png)

### Import from existing crontab
### 導入定時器列表 示例圖

Import from existing crontab file automatically.
![import](https://github.com/alseambusher/crontab-ui/raw/gh-pages/screenshots/import.gif)

### Backup and restore crontab
### 存檔定時器列表 示例圖

Keep backups of your crontab in case you mess up.
![backup](https://github.com/alseambusher/crontab-ui/raw/gh-pages/screenshots/backup.png)

### Export and import crontab on multiple instances of Crontab UI.
### 導出並讓其他機器使用 示例圖

If you want to run the same jobs on multiple machines simply export from one instance and import the same on the other. No SSH, No copy paste!

![export](https://github.com/alseambusher/crontab-ui/raw/gh-pages/screenshots/import_db.png)

But make sure to take a backup before importing.

### Separate error log support for every job
![logs](https://github.com/alseambusher/crontab-ui/raw/gh-pages/screenshots/log.gif)

### Donate
Like the project? [Buy me a coffee](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=U8328Q7VFZMTS)!

### Contribute
Fork Crontab UI and contribute to it. Pull requests are encouraged.

### License
[MIT](LICENSE.md)
