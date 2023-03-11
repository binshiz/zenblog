# Docker基础操作

## 准备好你的工作目录
- 在项目的根目录下
- 准备文件

## 制作Dockerfile
输入命令：
```bash
touch Dockerfile
```
请注意，***Dockerfile***这个文件是没有后缀的
- 选择你要运行的应用程序，和系统，特别是确定好版本
- 准备好工作目录
- 将原项目资源拷贝到工作目录中
- 运行命令，安装程序需要的依赖
- 设置暴露的端口号
- 运行需要执行的cmd命令
一个Dockerfile的文件格式如下，这里用`node.js`的配置为列：
```dockerfile
FROM node:19-alipine3.16
WORKDIR /binshiz
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```
同时，要注意项目中有一些文件是会安装依赖时自动添加的文件，或者是git的配置文件，那么要在项目中制作`.dockerignore`，输入指令：
```bash
touch .dockerignore
```
然后在.dockerignore中编写:
```Dockerfile
node_modules
Dockerfile
.dockerignore
.gitignore
```

## 编译docker镜像
接下来就很简单了，在终端中下面指令编译项目：
```bash
docker build .
```
或者使用下面这个命令，编译成功，同时制定这个项目的名字：
```bash
docker build -t binshiz-nodejs:v1.0 .
```
编译成功后，可以输入：
```bash
docker images
```
查看你制作好的镜像。

## 运行docker镜像
下面在终端中使用这个命令来运行镜像：
```bash
docker run -d -p 3000:3000 --name nodejs-container binshiz-nodejs:v1.0
```
`-d`表示在后台运行，`-p`表示制定的端口号。
若要*删除docker镜像*，可以使用下面的指令：
```bash
docker rmi -f e5b
```
`-f`代表是强制删除，在运行的镜像也能被删除。


## 查看docker容器

可以在终端使用下面这行指令查看正在运行的docker容器
```bash
 docker ps
```
可以使用下面查看所有运行过的docker容器
```bash
 docker ps -a
```
可以使用下面指令停止正在运行的docker容器
```bash
docker stop e5b
```
可以使用下面指令来删除容器
```bash
docker rm -f e5b
```

## docker容器和volume
要进入到容器内部进行交互，在终端中输入下面的命令：
```bash
docker exec -it nodejs-container /bin/sh
```
`-i`表示***interactive***=交互，`-t`表示***pseudo-TTY***=伪终端，`/bin/sh`表示执行一个新的shell。
如果要把本地的文件夹映射到容器的工作目录，可以在终端中输入下面的指令：
```bash
 docker run -d -v /Users/phil/WebProjects/binshiz-docker/nodeweb-test/:/binshiz -p 3000:3000 --name nodejs-container binshiz-nodejs:v1.0
```
`-v`表示volume，通过本地文件夹:容器文件夹来映射。

