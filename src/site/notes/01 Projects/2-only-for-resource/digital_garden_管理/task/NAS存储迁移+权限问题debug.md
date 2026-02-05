---
{"dg-publish":true,"dg-path":"NAS/","permalink":"/NAS//","title":"NAS存储迁移+权限问题debug","tags":["task"],"noteIcon":"","created":"2026-02-05T16:42:19.765+08:00","updated":"2026-02-05T17:08:57.135+08:00"}
---

# docker 存储迁移+权限问题debug


## 问题背景

- 默认的docker 容器把数据都存在HDD 硬盘里面，如果有**很频繁的访问的话**，nas 一直有机械硬盘运行咯咯的声音

- HDD 的高频读写会严重影响其寿命，所以后续的目标是把数据“冷热隔离”，把常用的数据存放在SSD 中，HDD 中放一些读写频率较低的数据。



## 操作方法

1. 把绿联nas docker 容器的配置进行迁移，
	- 这里迁移的是绿联对docker 的一些配置，例如镜像的下载位置
	- ![NAS存储迁移+权限问题debug-20260205165254.png|400](/img/user/04%20Archives/11%20attachment/NAS%E5%AD%98%E5%82%A8%E8%BF%81%E7%A7%BB+%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98debug-20260205165254.png)


2. 关闭docker 中的所有容器
	- 不要在docker 容器运行的时候进行复制，会导致一些莫名其妙的错误


3. 查看每一个容器他存储空间映射的位置
	- ![NAS存储迁移+权限问题debug-20260205165806.png|442](/img/user/04%20Archives/11%20attachment/NAS%E5%AD%98%E5%82%A8%E8%BF%81%E7%A7%BB+%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98debug-20260205165806.png)
	- 如果是默认的配置，基本上所有的数据都在docker 文件夹📂下，但是要进行确定，如果有存储在其他位置的要单独处理


4. 在SSD 上建立一个新的文件夹，然后直接把docker 文件夹📂下的所有文件都复制到新建的存储位置
	- ![NAS存储迁移+权限问题debug-20260205170038.png|213](/img/user/04%20Archives/11%20attachment/NAS%E5%AD%98%E5%82%A8%E8%BF%81%E7%A7%BB+%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98debug-20260205170038.png)
	- 如果有单独存放在其他位置的，建议统一存储到此文件夹下


5. 修改每一个docker 容器存储空间的映射路径
	- ![NAS存储迁移+权限问题debug-20260205170657.png|600](/img/user/04%20Archives/11%20attachment/NAS%E5%AD%98%E5%82%A8%E8%BF%81%E7%A7%BB+%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98debug-20260205170657.png)


6. 启动你的容器！！！





# 内部link

[[03 Resource/Nas小功能开发/docker 容器运行权限报错\|docker 容器运行权限报错]]
[[03 Resource/Nas小功能开发/绿联NAS 存储空间优化\|绿联NAS 存储空间优化]]