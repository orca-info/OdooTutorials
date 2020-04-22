

# Ubuntu 安装odoo开发环境

---

### 强烈建议在Ubuntu环境下做odoo开发！！！windows的各种奇葩你懂的...



## 准备材料

1. vscode for Unbutu
2. odoo source code
3. python3.6.8 
4. docker
5. postgreSQL10.0
6. wkhtmltopdf

---

### 一、安装docekr

> [参考docker docs文档：Install on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

**1. 检查环境，如把当前系统中的有旧版本，将它们删除，如当前是新系统可忽略这步：**

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

**2. 设置存储仓库：**

2.1 更新apt源，并允许apt通过HTTPS使用库来安装包：

```shell
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

2.2 添加Docker的官方GPGkey：

```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

验证是否带有指纹密钥 `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`，通过搜索密钥的后8位字符。

```shell
$ sudo apt-key fingerprint 0EBFCD88
// 打印如下信息则正常

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

2.3 使用下面命令来设置稳定版的仓库，其中`$(lsb_release -cs)`子命令返回您的Ubuntu的发行版本的名称如：xenial、bionic，你也可以填写你自己的版本，这里我们默认照抄：

```shell
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

**3.安装Docker Engine**

更新apt源索引，并安装Docker Engine，默认安装最新社区版本（CE)：

```shell
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

运行docker --version，查看是否安装成功：

```shell
$ docker --version
```



---



### 二、运行postgreSQL10.0数据库

> [参考odoo官方的dockerhub](https://hub.docker.com/_/odoo)

直接从dockerhub把数据库镜像pull下来运行容器即可（此处我们增加映射宿主机端口）：

```shell
$ docker pull postgrest:10
$ $ docker run -d -p 5430:5432 -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:10
```

然后查看容器db是否运行成功：

```shell
$ docker ps
```



---

### 三、安装python3.6.8

> 熟悉源码安装的小伙伴自行到官网下载[python3.6.8的tar.gz包](https://www.python.org/downloads/release/python-368/).
>
> 这里用最简单的apt安装，参考：
>
> [CSDN Ubuntu16.04安装python3.6 安装pip](https://blog.csdn.net/lee_x_lee/article/details/90204500)
>
> [Ubuntu上安装python3.6以及设置为系统默认](https://www.qingnansun.de/ubuntu-python36-and-set-as-default/)
>
> [Unbutu 安装Python3.6](https://tding.top/archives/3868725a.html)

1. Ubuntu系统自带python2.7，可以查看：

   ```shell
   $ python --version
   ```

2. 更新apt源，并安装：

   ```shell
   $ sudo add-apt-repository ppa:jonathonf/python-3.6
   $ sudo apt-get update
   $ sudo apt-get install python3.6
   ```

3. 安装好后系统中会有三个版本的python：2.7、3.5、3.6，避免每次使用3.6的版本都输入`python3.6`命令，要把python3.6设置为系统默认：

   ```shell
   $ ln -s /usr/local/python3.6/bin/python3.6 /usr/bin/python3.6 ##前面换成你的安装路径
   ```

   检查一下版本：

   ```shell
   $ python3 --version
   ```



---

### 四、安装VScode for Ubuntu

> 到微软官网下载**deb**包，直接点击安装即可：[Visul Studio Code](https://code.visualstudio.com/)



---

### 五、下载odoo最新源码

> 到odoo的nightly网站下载最新版源码，解压倒目录`~/`下，修改目录名为：odoo13：
>
> [odoo13CE社区版源码](https://nightly.odoo.com/)



---

### 六、安装 wkhtmltopdf 
下载对应的amd64版本，直接安装 [下载地址](https://wkhtmltopdf.org/downloads.html)

---

### 七、配置odoo项目的python env环境

> 可参考廖雪峰老师的教程：[virtualenv](https://www.liaoxuefeng.com/wiki/1016959663602400/1019273143120480)

1. 安装virtualenv：

   ```shell
   $ sudo apt-get install python3-venv
   ```

2. 进入odoo项目目录下，并创建env环境目录odooenv，激活环境：

   ```shell
   $ cd ~/odoo13
   $ python3 -m venv odooenv
   $ source odooenv/bin/activate
   ```

3. env环境激活后，会在shell命令行开头显示环境名称（odooenv），代表已激活；安装odoo项目依赖：

   > 注意:一定要在odoo13的目录下操作，默认odooenv也是装在此目录下，此目录下有requirements.txt依赖文件，安装过程中可能有ERROR信息，详情google或百度解决。如：<font color=red>ERROR：** make sure the development packages of libxml2 and libxslt are installed ** </font>,执行：
```sudo apt-get install python3 python-dev python3-dev \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip
```
> 如：python-ldap安装失败，执行：`sudo apt-get install libsasl2-dev python-dev libldap2-dev libssl-dev`

然后再重新执行：
   ```shell
   (odooenv) $ pip3 install -i https://pypi.douban.com/simple -r requirements.txt
   ```


---

### 八、配置VScode

1. 打开vscode

2. __*File*__ --> __*Open Folder*__ 打开odoo13文件夹（目录）

3. 打开__*EXTENSIONS*__，搜索：__*python*__ ---> __*install*__ 安装

4. 配置settings.json、launch.json文件

   ```json
   //launch.json文件
   {
       // Use IntelliSense to learn about possible attributes.
       // Hover to view descriptions of existing attributes.
       // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
       "version": "0.2.0",
       "configurations": [
        
           {
               "name": "odoo",
               "type": "python",
               "request": "launch",
               "program": "~/odoo13/odoo-bin",
               "pythonPath":"~/odoo13/odooenv/bin/python3",
               "args":[
                   "-i",
                   "base",
                   "-c",
                   "~/odoo13/odoo.conf"
               ],
               "console": "integratedTerminal"
           },
       ]
   }
   ```

   ```json
   //settings.json 文件
   {
       "python.pythonPath": "odooenv/Scripts/python",
       "python.venvPath": "~/odoo13/odooenv"
   }
   ```

   5. 创建odoo.conf文件，这里的db信息都是上面运行postgres10容器时配置的端口、用户和密码：

   ```conf
   [options]
   addons_path = ~/odoo13/odoo/addons
   db_host = localhost
   db_password = odoo
   db_port = 5432
   db_user = odoo
   ```

6.  进入setup目录下，复制odoo文件到odoo13目录，并修改名字为：odoo-bin

   

---

### 九、debug项目

方法一：项目配置好后我们之间运行debug，按__*F5*__

> 提示：VScode左下角会显示选择解释器（interpreter），也可以在这里选择运行项目的解释器

方法二：打开VScode 的terminal，然后cd到odoo13目录下，激活当前env环境，根据下面参数之间运行项目odoo-bin。

> 1、-i base：第一次启动初始化数据库，以后启动后可忽略。
>
> 2、`docke ps `检查一下确保你的数据库容器（db）是在运行中，并且是新的容器还未安装有其他odoo版本的数据库

```shell
$ cd ~/odoo13
$ source odooenv/bin/activate

(odooenv)$python3 odoo-bin -i base -c ~/odoo13/odoo.conf
```
## 最后在浏览器打开：http://localhost:8069 

---

## 大功告成

如有疑问可联系564715739@qq.com

### 项目实施

&

### 项目合作
