# 安裝

docker安裝: https://docs.docker.com/install/linux/docker-ce/ubuntu/

docker compose安裝: https://docs.docker.com/compose/install/

# 重點

- docker是 container(容器)的管理工具

- 以往應用程式在不同機器下運作, 需要有不同的設定. 容器內的應用程式不管在哪台機器下運作, 只要藉由 docker就能運作, 不需要有額外設定.

- container共用 linux核心提供的功能

- docker分為 ee與(企業版) ce版本(社區版、免費版)

- docker compose用來同時管理多個 container, 例如同時啟動多個 container

- docker可以打包應用程式, 打包出來的檔案叫做 docker image, docker image可以拿到別台電腦藉由 docker執行

- docker registry是用來存放 docker image, 實現的有官方的 docker hub, 可以找想要的 docker image並且下載回來使用

- 檔案 Dockerfile用來描述與產生 docker image

- docker image用來啟用 container