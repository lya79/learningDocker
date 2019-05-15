# Docker Compose

根據官方說法 
> [Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.](https://docs.docker.com/compose/overview/) 

主要我們會藉由 Docker Compose來做到同時管理/運作多個容器. 要做到此作用需要下列步驟:
1. 撰寫 docker-compose.yml
2. 執行 docker-compose.yml

# docker-compose.yml撰寫說明
`docker-compose.yml`主要分為三類標籤

標籤     | 用途
---------|---
services | 作用類似於 `docker run`命令, 因此用的命令會相似甚至相同.
volumes  | 定義服務使用的資料儲存
networks | 定義服務使用的網路資訊

#### 範例 `docker-compose.yml`
```docker-compose.yml
version: '3'               # compose版本, 參考: https://docs.docker.com/compose/compose-file/compose-versioning/
services:                  # 定義服務相關的區塊
  web:                     # image名稱
    build: .               # 依據 Dockerfile建立 image並且啟動成 container
    ports:                 # port設定
    - "5000:5000"
    volumes:               # 資料儲存設定
    - .:/code
    - logvolume01:/var/log
    links:                 # 讓服務的網路可以互相通訊的設定
    - redis
  redis:
    image: redis
volumes:                   # 資料儲存設定
  logvolume01: {}
```

# 執行 docker-compose.yml
使用 `docker-compose`命令. 此命令的參數很多樣, 因此列出較基礎的使用方式. 並且命令都是控制**服務名稱**, 並非 **image名稱**、**container id**.

用來驗證 `docker-compose.yml`檔案內容是否有撰寫錯誤.
```shell
$ docker-compose config
```

---

與 docker logs類似.
```shell
$ docker-compose logs
```

---
用來來查詢指定 container內部使用的端口所對應的外部 IP位址與端口.
```shell
$ docker-compose port aaa 1880
```

---
類似 ```docker ps```, 差異在於只有顯示 `docker-compose.yml`內定義的服務資訊.
```shell
$ docker-compose ps
```

---
`--help`用於查詢命令使用方式與參數說明, 下列範例則是查詢 `docker-compose ps`命令的說明. 
```shell
$ docker-compose ps --help 
```

如果 `docker-compose.yml`內有使用到 Dockerfile來產生 image的話, 而這些標籤在使用 `docker-compose build`時便會產生 image.
```shell
$ docker-compose build
```

---

`start`、`stop`、`restart`、`pause`、`unpause`、`kill`用來控制 `docker-compose.yml`所定義的 container狀態.
```shell
$ docker-compose stop # 停止全部 docker-compose.yml所定義的服務
```
or
```shell
$ docker-compose stop aaa bbb # 單獨停止 docker-compose.yml內定義的 aaa和 bbb服務
```
---
刪除暫存的服務的 container
```shell
$ docker-compose rm -v # 刪除全部 docker-compose.yml所定義的服務容器
```
or
```shell
$ docker-compose rm -v aaa # 刪除 docker-compose.yml內定義的aaa服務容器
```

---
`docker-compose up` 用於啟動服務, 依據 `docker-compose.yml`
```shell
$ docker-compose up
```

參數:

    -d 後台運行服務
    --no-recreate 如果容器已經存在, 則不重建
    --force-recreate 強制創建容器

# 參考
- [Compose 命令说明](https://yeasy.gitbooks.io/docker_practice/compose/commands.html)
- [Compose 模板文件](https://yeasy.gitbooks.io/docker_practice/compose/compose_file.html#compose-%E6%A8%A1%E6%9D%BF%E6%96%87%E4%BB%B6)
- [拙言docker-compose搭建golang本地开发环境](https://www.cnblogs.com/yin5th/p/9604573.html)
