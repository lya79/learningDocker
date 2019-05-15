# 製作 Docker Image

主要分為兩步驟
1. 製作 Dockerfile
2. 產生 docker image

# Dockerfile撰寫說明
Dockerfile都以 KEY-VALUE格式進行撰寫. 因此需要瞭解各個標籤(ex: ```FROM```、```ENV```...)的用途與使用方法.

#### ```FROM``` 
* 用途: 設定基礎映像
* 說明: 以某一個 Docker image為基底.
* 命令格式:
        
        FROM <image>
        or
        FROM <image>:<tag>

#### ```ENV``` 
* 用途: 設定環境變數
* 說明: 容器內可使用設定好的環境變數, Dockerfile在 build時也可以使用此環境變數.
* 命令格式:

        ENV <key>=<value> …


#### ```WORKDIR```
* 用途: 設定 Dockerfile在執行特定命令時所在的目錄
* 說明: 會受到影響的命令 RUN、CMD、ENTRYPOINT、COPY、ADD. 設定時可以是絕對路徑與相對路徑.
* 命令格式:

        WORKDIR <資料夾絕對路徑或相對路徑>

#### ```COPY``` 
* 用途: 將指定目錄下的檔案和目錄複製到容器內指定位置
* 說明: 指令的來源位置可以多個, 如果目的位置是目錄的話,記得最後要以/結尾. 目的位置可以是絕對路徑或者相對於WORKDIR定義值的相對路徑, 若目的位置不存在，會自動建立.
* 命令格式:

        COPY <src>… <dst>

#### ```ADD``` 
* 用途: 與 ```COPY```相同
* 說明: 與 ```COPY```差異在於來源位置支援遠端位置(ex: HTTP URL), 並且檔案複製到 iamge時, 如果檔案是壓縮檔, 會自動解開.
* 命令格式:

        ADD <來源網址> <dst>

#### ```RUN``` 
* 用途: 在 build image時指定執行的命令
* 說明: 
* 命令格式:

        RUN <command>
        or
        RUN ["executable", "param1", "param2"]

#### ```EXPOSE```
* 用途: 容器要使用的端口
* 說明: 
* 命令格式:

        EXPOSE <port> [<port>/<protocol>…]

#### ```CMD ```
* 用途: 容器啟動後, 容器內要執行的命令
* 說明: 不能有多個 ```CMD```.
* 命令格式:

        CMD [“executable”,”param1″,”param2″]

#### ```ENTRYPOINT``` 
* 用途: 相同於 ```CMD ```
* 說明: 不能有多個 ```ENTRYPOINT```, 與 ```CMD ```差異在於, ```ENTRYPOINT```無法被 ```docker run```覆蓋, 但是 ```CMD```可以被 ```docker run```覆蓋.
* 命令格式:

        ENTRYPOINT [“executable”, “param1”, “param2”]

# 打包 Docker iamge命令說明

製作完 Dockerfile後，就可以進行產生 image. 使用 ```docker build```命令.

#### 參數:

-t 用來指定 image名稱與 tag

#### 範例: 

myimage:v1為 image名稱與 tag名稱, :則是用來區隔 image名稱與 tag名稱. tag為選項.

而 . 則是指 Dockerfile所在位置.
```shell
$ docker build -t myimage:v1 .
```

> 備註: ```docker build```的來源也可以是個網路位置.

# 附錄. Golang官方提供的 Dockerfile寫法
參考: https://hub.docker.com/_/golang

Dockerfile
```Dockerfile
FROM golang:1.8         

WORKDIR /go/src/app     
COPY . .                

RUN go get -d -v ./...  
RUN go install -v ./...  

CMD ["app"]             
```

#### 打包 image
```shell 
$ docker build -t my-golang-app . 
```

## 說明 ```ENTRYPOINT```與 ```CMD ```的使用時機

如標題, 這兩命令看起來相同作用. 差異在於使用的時機不相同. 當要啟動一個 image時, 會使用 docker run，通常 docker run也可以帶入要執行的命令, 因此會有下列三種執行命令方式. 

1. docker run的命令
2. ENTRYPOINT的命令
3. CMD的命令

#### 比較好的使用方式:

CMD可以為 ENTRYPOINT提供參數, ENTRYPOINT本身也可以包含參數, 但是可以把那些可能需要變動的參數寫到 CMD裏而把那些不需要變動的參數寫到 ENTRYPOINT.
     
#### 範例:

Dockerfile    
```Dockerfile
ENTRYPOINT ["/bin/echo", "Hello"]
CMD ["World"]
 ```

```shell
$ docker run -it <image>
```
輸出結果為 Hello World
```shell
$ docker run -it <image> Docker
```
輸出結果為 Hello Docker

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

#### 打包 image
```shell 
$ docker build -t hellodocker .
```

# 參考
- [Docker实战 - 将golang工程部署到docker](https://www.jianshu.com/p/eb799d77563f)
- [go语言工程制作dockerfile，并部署到docker](https://blog.csdn.net/niyuelin1990/article/details/79035728)
- [Docker – Dockerfile 指令教學，含範例解說](https://www.jinnsblog.com/2018/12/docker-dockerfile-guide.html)
- [CMD 和 ENTRYPOINT 在 Dockerfile 的意義](http://blog.gary-lai.com/understand-how-cmd-and-entrypoint-interact/)
- [CMD 與 ENTRYPOINT 的差別](http://jenkins.readbook.tw/docker/basic/104-dockerfile/entrypoint/)
- [Dockerfile文件中的CMD和ENTRYPOINT指令差異對比](https://www.twblogs.net/a/5b9c00912b71773ebace1f21)