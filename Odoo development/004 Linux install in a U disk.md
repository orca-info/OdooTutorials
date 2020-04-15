# Linux install in a U disk
### 1、准备材料：   
+ 虚拟机VMware Workstation 15.5PRO
+ ubuntu ISO源文件：[Ubuntu 18.04.4 LTS ISO](https://ubuntu.com/download/desktop)
+ 20G以上U盘（本项目使用128G U盘）

### 2、创建虚拟机并安装
1. 打开虚拟机，--> __*创建新的虚拟机*__:  
![avatar](../../ubuntu virtual/step1.png)

2. --> __*自定义（高级）*__:
![avatar]()

3. --> 默认：Workstation 15.x ,--> __*下一步*__:
![avatar]()

4. --> __*稍后安装操作系统*__:
![avatar]()

5. --> 客户机操作系统：__*Linux*__ ， 版本：__*Ubuntu*__:
![avatar]()

6. --> 虚拟机名字：默认值（Ubuntu)， 位置：默认值 或 浏览(__*选择自定义的位置*__):
![avatar]()

7. --> 处理器数量：2，每个处理器核心：2 :(可根据自己机器核心填写)
![avatar]()

8. --> 此虚拟机的内存：2G：
![avatar]()

9. --> 网络类型：默认（__*使用网络地址转换（NAT)*__）:
![avatar]()

10. --> SCSI控制器：默认（__*LSI Logic*__)：
![avatar]()

11. --> 虚拟磁盘类型：默认（__*SCSI*__）：
![avatar]()

12. --> 磁盘：__*创建新虚拟磁盘*__：
![avatar]()

13. --> 最大磁盘大小：__*20G，将虚拟磁盘存储为单个文件*__ ：
![avatar]()

14. --> 指定磁盘文件：默认:
![avatar]()

15. --> __*自定义硬件*__：
![avatar]()

16. --> 新CD/DVD(SATA)-->链接：使用ISO印象文件  
--> USB控制器 --> USB兼容性：__*USB.30*__， 勾选：显示所有USB输入设备，与虚拟机共享蓝牙设备  
--> 关闭 返回到 15的页面，点击完成
![avatar]()

17. --> 启动虚拟机，按住 Crtl 键选择：Try Ubuntu
![avatar]()

18. --> 默认：English __*Continue*__ , Keyboard layout:English --> __*Continue*__:
![avatar]()

19. --> 把 Other options :Download updates while installing Ubuntu 去掉：
![avatar]()

20. --> 最后一步：选中__*Something else*__   
![avatar]()
注意：这时候把U盘插入，会显示弹窗信息如下，选择：链接到虚拟机 Ubuntu2
![avatar]()
然后点击下一步 --> __*Continue*__

21. --> 显示两个硬盘，然后给/dev/sdb (U盘)分区，参考链接：![avatar]()()如下图所示：
![avatar]()
注意：分好区后，Device for boot loader installation:一定要选择自己的U盘，如本项目的是/dev/sdb Samsung Flash Drive

22. --> 最后一步： __*Install Now*__
# 大功告成！！！
