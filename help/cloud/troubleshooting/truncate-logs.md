---
title: 截断日志
description: 了解如何通过截断大型日志文件来分类因硬盘已满而失败的部署。
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
source-git-commit: b90aa9eb8759391a16dfb29ca25b0d2d271956ed
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# 截断日志

了解如何分类以及由于硬盘已满导致的部署失败。 了解如何查找以及可以运行哪些命令来释放Adobe Commerce云环境中的空间。

如果您认为可能需要这些日志文件，则可以`rsync`它们，或者使用其他方法在截断它们之前从服务器获取可用的副本。

## 此视频面向谁

- 开发人员和IT专业人员
- 系统管理员

## 视频内容

- 诊断和解决失败的部署
- 在其中找到一些常见的大型日志文件
- 截断日志文件的快速方法

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## 视频中使用的命令

检查硬盘空间`df -h`。 请注意行dev/mapper/xxxx

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


使用命令`ls -lah`以人类可读的格式（如kb、mb和gb）显示文件及其大小

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## 截断日志的示例

ssh进入正确的项目和环境后，转到`var/log`目录。 然后，您可以截断包含类似`> some-log-file.log`内容的文件

```BASH
> support_report.log 
> cron.log 
```

## 相关文档

- [运行状况通知](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}
