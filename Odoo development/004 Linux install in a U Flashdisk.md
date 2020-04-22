# Linux install in a U Flashdisk
### 1、准备材料：
+ 虚拟机VMware Workstation 15.5PRO
+ ubuntu ISO源文件：[Ubuntu 18.04.4 LTS ISO](https://ubuntu.com/download/desktop)
+ 20G以上U盘（本项目使用128G U盘 建议选用支持USB3.0）

### 2、创建虚拟机并安装
1. 打开虚拟机，--> __*创建新的虚拟机*__:  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step1.png)

2. --> __*自定义（高级）*__:

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step2.png)

3. --> 默认：Workstation 15.x ,--> __*下一步*__:

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step3.png)

4. --> __*稍后安装操作系统*__:

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step4.png)

5. --> 客户机操作系统：__*Linux*__ ， 版本：__*Ubuntu*__:

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step5.png)

6. --> 虚拟机名字：默认值（Ubuntu)， 位置：默认值 或 浏览(__*选择自定义的位置*__):

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step6.png)

7. --> 处理器数量：2，每个处理器核心：2 :(可根据自己机器核心填写)

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step7.png)

8. --> 此虚拟机的内存：2G：

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step8.png)

9. --> 网络类型：默认（__*使用网络地址转换（NAT)*__）:

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step9.png)

10. --> SCSI控制器：默认（__*LSI Logic*__)：

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step10.png)

11. --> 虚拟磁盘类型：默认（__*SCSI*__）：

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step10.png)

12. --> 磁盘：__*创建新虚拟磁盘*__：

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step12.png)

13. --> 最大磁盘大小：__*20G，将虚拟磁盘存储为单个文件*__ ：

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step13.png)

14. --> 指定磁盘文件：默认:

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step14.png)

15. --> __*自定义硬件*__：

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step15.png)

16. --> 新CD/DVD(SATA)-->链接：使用ISO印象文件  
--> USB控制器 --> USB兼容性：__*USB.30*__， 勾选：显示所有USB输入设备，与虚拟机共享蓝牙设备  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step16.png)  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step17.png)

--> 关闭 返回到 15的页面，点击完成

17. --> 启动虚拟机，按住 Crtl 键选择：Try Ubuntu
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step18.png)  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step19.png)

18. --> 进入Ubuntu PE系统安装Ubuntu，点击：__*Install Ubuntu 18.04.4 LTS*__ 图标  
打开安装页面 --->  默认：English __*Continue*__ , Keyboard layout:English --> __*Continue*__:
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step20.png)  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step21.png)  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step22.png)


19. --> 把 Other options :Download updates while installing Ubuntu 去掉：
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step23.png)

20. --> 最后一步：选中__*Something else*__   
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step24-1.png)

<font color=blue>**注意**</font>：这时候把U盘插入，会显示弹窗信息如下，选择：链接到虚拟机 Ubuntu2
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step24.png)

![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step25.png)
然后点击下一步 --> __*Continue*__

21. --> 显示两个硬盘，然后给/dev/sdb (U盘)分区，参考链接：[CSDN 安装Ubuntu Linux系统时硬盘分区最合理的方法](https://blog.csdn.net/u012052268/article/details/77145427)  
如下图所示：  
![avatar](https://github.com/orca-info/OdooTutorials/blob/master/Odoo%20development/ubuntu%20virtual/step26.png)  
<font color=blue>**注意**</font>：分好区后，Device for boot loader installation:一定要选择自己的U盘，如本项目的是: __*/dev/sdb Samsung Flash Drive*__

22. --> 最后一步： __*Install Now*__，接下来正常安装流程。

### <font color=blue>**提示：**</font>
> 第20步 一定要试多几次插拔U盘！才会弹出硬件信息。
# 大功告成！！！
