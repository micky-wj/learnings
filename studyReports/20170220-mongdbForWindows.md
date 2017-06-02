## 2017-2-20 Windows环境下的MongoDB安装配置以及常用指令


# 一、安装
* **【下载】** 到[Mongodb官网](http://www.mongodb.org/downloads)下载合适的安装包
* **【解压】** 解压到你存放安装软件的文件夹下，如 `D:\soft\`
* **【配置环境变量】** 将mongodb配置成系统命令，即在Path变量值的后面加上mongodb的bin文件夹路径，如 `D:\soft\mongodb\bin`
* **【新建数据库文件】**
	* 在安装文件夹下，创建文件夹data，存放数据相关文件，比如 `D:\soft\mongodb\data`
	* 在data文件夹下，创建文件夹db，存放数据库数据，比如 `D:\soft\mongodb\data\db`
	* 在data文件夹下，创建文件夹log，存放数据库日志，比如 `D:\soft\mongodb\data\log`，并且在其中新建文件mongodb.log


# 二、启动服务
## 方法一：命令行
打开命令行shell， 输入下列命令，--dbpath是用来指定数据库存放目录.
```
mongod --dbpath D:\mongodb\data\db
```
## 方法二：将mongodb存为系统服务【强烈建议】
* **【新建配置文件】**：在 `D:\soft\mongodb`下新建文件mongodb.config，用记事本打开输入以下配置
```
dbpath=D:\soft\mongodb\data #数据库路径
logpath=D:\soft\mongodb\log\mongodb.log #日志输出文件路径
logappend=true #错误日志采用追加模式，配置这个选项后mongodb的日志会追加到现有的日志文件，而不是从新创建一个新文件
journal=true #启用日志文件，默认启用
quiet=true #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
port=27017 #端口号 默认为27017
```
* **【注册服务】**：命令行输入以下命令
```
sc create MongoDB binPath= "D:\soft\mongod\bin\mongod.exe --service --config=D:\soft\mongod\mongodb.conf"
```
* **【检测服务】**：打开cmd输入`services.msc`查看MongoDB服务，点击可以启动，也可以设置为自动服务。
* **【检测mongodb的启动情况】**：浏览器输入[http://localhost:27017/](http://localhost:27017/) ，如果打印以下信息表示启动成功。
```
It looks like you are trying to access MongoDB over HTTP on the native driver port.
```
# 三、常用指令


## 操作数据库
> 命令行输入`mongo`,就可以进行数据库的一系列操作


* show dbs：显示数据库列表 
* show collections：显示当前数据库中的表
* show users：显示用户
* use <db name>：切换/新建数据库
* db.createUser(): 创建用户，如`db.createUser({user: "admin",pwd: "wuliu123",roles: [ "readWrite", "dbAdmin" ]})`
* db.help()：显示数据库操作命令
* db.foo.help()：显示当前数据库中的foo表的操作命令
* db.foo.find()：当前数据库中的foo表数据查找


## 数据库备份和恢复
* 数据备份：mongodump -d [数据库名] -o [存放目录]
* 数据恢复：mongorestore -d [数据库名] --drop [备份文件目录]
