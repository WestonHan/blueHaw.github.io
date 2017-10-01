---
layout: post
title: Linux下任务调度crond笔记
comment: true
date: 2017-10-01 22:44:03
updated:
tags:
---

## 安装
```
yum install -y vixie-cron crond
```
## 添加调度计划
### 调度表达式介绍
```bash
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```
栗子：
```bash
 *  *  *  *  *  /bin/bash /home/crontab.sh    #每分钟执行/home/crontab.sh
 0  *  *  *  *  /bin/bash /home/crontab.sh    #每小时执行/home/crontab.sh
 0  0  *  *  *  /bin/bash /home/crontab.sh    #每天凌晨执行/home/crontab.sh
 */2  *  *  *  *  /bin/bash /home/crontab.sh  #每两分钟执行/home/crontab.sh
```
表达式最小支持分钟，如果想按照秒执行呢？
```bash
step=5 #间隔的秒数
for (( i = 0; i < 60; i=(i+step) )); do  
    restartShutdownTomcat 
    sleep $step  
done
```
脚本里通过for循环，能够按秒执行。
>如果60不能整除间隔的秒数，则需要调整执行的时间。例如需要每7秒执行一次，就需要找到7与60的最小公倍数，7与60的最小公倍数是420（即7分钟）。
则`crontab.sh`中`step`的值为7，循环结束条件`i<420`， `crontab -e`可以输入以下语句来实现
```bash
*/7 * * * * /home/crontab.sh
```
### 添加调度计划
* 一种是直接编辑，执行`crontab -e`，然后输入调度表达式
* 另一种是编辑文件，然后执行`crontab absolutePath`，我使用是这种
>注意：**使用编辑文件方式添加会覆盖原来的crontab任务**
## crond不执行原因分析：
### 1. 没有使用绝对路径
包括**crond命令**和**脚本**
### 2. crontab不提供所执行用户的环境变量
解决方法：在脚本中加入下面这一行：
```bash
. /etc/profile
```
或者导入环境变量(例如Java环境)：
```
export  JAVA_HOME=/usr/local/java
```
### 3. 脚本没有可执行权限
在crontab中建议使用 sh 或 bash 来执行shell脚本，避免因脚本文件的执行权限丢失导致任务失败。
### 4. 放大招：查看日志
```bash
tail /var/log/cron
```
