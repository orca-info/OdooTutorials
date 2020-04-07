## 开发
### __*001*__  安装Odoo到Linux
> 参考链接：[dockerhub.com](https://hub.docker.com/_/odoo)
  
步骤：  
1-安装docker
> 参考链接：[docker.com](https://docs.docker.com/engine/install/ubuntu/)  
  
2-拉取镜像  

```shell  
docker pull odoo:13  
docker pull postgres:10  
```
3-运行容器  
先运行postgreSQL10数据库容器
```shell
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:10
```  
再运行odoo容器  
```shell
docker run -p 8069:8069 --name odoo12 -v /var/odoo12/addons:/mnt/extra-addons --link db:db -d odoo:13
```
> docker 命令说明  
-d：后台运行容器，并返回容器ID；  
-e：设置环境变量；  
-p：指定映射端口，格式为：宿主机端口：容器端口；  
--name：容器名字；  
-v：映射的目录，格式为：宿主机目录：容器目录（默认是/mnt/extra-addons）  
postgres:10(odoo:13)：刚才pull下来的镜像  
如需配置Nginx，请参考博文：[CSDN:odoo系统的Nginx配置https证书及微信域名业务认证](https://orca-coooooo.blog.csdn.net/article/details/99691202)

4-打开浏览器，登录：http://localhost:8069 ,即可打开odoo配置账套的页面