# docker_compose 建立 gitlab 

Compose 是一個工具

用來定義與執行多個 container 組成的 Docker Applications。

你可以使用 Compose 檔案來組態設定你的應用服務。

然後使用單一命令，透過你的組態設定來建立與啟動你的服務。

Compose 適合用來開發、測試、與建立 staging 環境，如同 CI workflows。

###  Compose 有基本的三個處理步驟

1.使用 Dockerfile 定義你的 app 環境，讓它可以在任何地方都能複製(reproduced)。

2.使用 docker-compose.yml 定義你的服務，讓他們可以在獨立環境內一起執行。

3.最後，執行 docker-compose up，Compose 將會開始與執行你所有的 app。

### 

>docker create -v /var/www/html --name dataContainer busybox

>docker create -v /config --name volume_test busybox

![](https://github.com/a121514191/docker_volume/blob/master/create_volume.PNG)

### 查看

>docker ps -a 

![](https://github.com/a121514191/docker_volume/blob/master/ps_create_volume.PNG)

### 將你要的文件cp(複製)到剛剛建立的容器裡面

!這邊卡了很久，原因只出在於範例多了空白

docker cp path/file volume_test:/config/ (O)
>docker cp test.txt volume_test:/config/ 

![](https://github.com/a121514191/docker_volume/blob/master/O.PNG)

docker cp path/file volume_test:/ config / (X)
>docker cp test.txt volume_test:/ config / 

![](https://github.com/a121514191/docker_volume/blob/master/X.PNG)



### 之後將我們要的檔案附加進去容器

可以看到原本放在我們容器的config在這個容器也有了
再往裡面找能找到剛剛所加入的test.txt

>docker run -it --volumes-from volume_test ubuntu 
>ls
>cd config
>ls

![](https://github.com/a121514191/docker_volume/blob/master/volume-from.PNG)

### 最後是導入和導出容器數據

>docker export  volume_test> dataContainer.tar

>docker import  volume_test.tar

!!!不知為何導出無數據

# 實做

### 準備一個空的Data Volume Container

>docker create -v /var/www/html --name Practice busybox

### 複製本機資料夾到container裡

>docker cp test01 Practice:/var/www/html

>docker cp test02 Practice:/var/www/html

>docker cp test03 Practice:/var/www/html

### 啟動container 

>docker run -p 80:80 --volumes-from Practice -d --privileged=true test01 /usr/sbin/init

### 進入查看

>docker exec -it name /bin/bash


