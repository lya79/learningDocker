# 製作 Docker Image

主要分為兩步驟
1. 製作 Dockerfile
2. 產生 docker image

# Dockerfile寫法

1. FROM 

    設定基礎映像

2. WORKDIR 

    要被打包的工作目錄

3. COPY

    將指定目錄下檔案/目錄複製到 docker image

4. RUN 

    要執行的命令

5. EXPOSE 
    
    容器要使用的端口

6. ENTRYPOINT 

7. CMD

# 附錄. Golang官方提供的 Dockerfile寫法
參考: https://hub.docker.com/_/golang

Dockerfile
```Dockerfile
FROM golang:1.8         # golang基礎 image

WORKDIR /go/src/app     # 設定工作目錄
COPY . .                # 將工作目錄資料複製到 image

RUN go get -d -v ./...  # 
RUN go install -v ./... # 

CMD ["app"]             # 
```

打包 image
```shell 
$ docker build -t my-golang-app .   #
```

# 附錄. Dockerfile其他寫法
參考: https://www.jianshu.com/p/eb799d77563f

Dockerfile
```Dockerfile
FROM golang:latest

WORKDIR $GOPATH/src/hellodocker
ADD . $GOPATH/src/hellodocker
RUN go build .

EXPOSE 8080

ENTRYPOINT ["./hellodocker"]
```

打包 image
```shell 
$ docker build -t hellodocker .
```

# 參考
- [Docker实战 - 将golang工程部署到docker](https://www.jianshu.com/p/eb799d77563f)
- [go语言工程制作dockerfile，并部署到docker](https://blog.csdn.net/niyuelin1990/article/details/79035728)