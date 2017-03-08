---
title: Linux 安装supervisor (CentOs or RedHat)
date: 2016-11-09 18:38:07
tags:
categories: Linux
---

-----------------------

#### supervisor是linux进程管理利器。如果是一个服务程序，要可靠地在后台运行，我们就需要把它做成daemon，最好还能监控进程状态，在意外结束时能自动重启。

#### supervisor就是用Python开发的一套通用的进程管理程序，能将一个普通的命令行进程变为后台daemon，并监控进程状态，异常退出时能自动重启。

#### 安装supervisor

```bash
$ yum install python-setuptools
```
![Alt text](/img/QQ截图20161108151312.png)

```bash
$ easy_install supervisor
```
![Alt text](/img/QQ截图20161108151521.png)

#### 测试是否安装成功

```bash
$ echo_supervisord_conf
```
![Alt text](/img/QQ截图20161108151708.png)

#### 创建配置文件

##### 创建主配置文件

```bash
$ mkdir -m 755 -p /etc/supervisor/
$ echo_supervisord_conf > /etc/supervisor/supervisord.conf
```

##### 创建项目配置目录

```bash
$ mkdir -m 755 conf.d
```

![Alt text](/img/QQ截图20161108152106.png)

#### 创建测试

##### 在/home/tengyunlong/supervisor_simple目录下创建test.c

```cpp
#include<stdio.h>
#include<string.h>
#include <unistd.h>
int main() {
	FILE *fp = fopen("/home/slightech/supervisor_simple/a.txt", "a+");
	if (fp == 0) {
		printf("Can not open file \n");
		return 0;
	}
	int ix = 0;
	for (;; ix++) {
		fseek(fp, 0, SEEK_END);
		char s_add_arr[10];
		memset(s_add_arr, '\0', 10);
		sprintf(s_add_arr, "%i\n", ix);
		fwrite(s_add_arr, strlen(s_add_arr), 1, fp);
		sleep(1);
	}
	fclose(fp);
	return 0;
}
```

##### 编译为test

```bash
$ gcc -o test test.c
```

#### 更改主配置文件，去掉最后两行的分号(;)，我习惯设置项目配置文件为.conf 格式

![Alt text](/img/QQ图片20161108153215.png)

#### 在/etc/supervisor/conf.d 创建test.conf文件

```bash
[program:test]
command=/home/slightech/supervisor_simple/test
;directory= ;directory to cwd to before exec (def no cwd)
autostart=true ; start at supervisord start (default: true)
autorestart=unexpected ; whether/when to restart (default: unexpected)
startsecs=1 ; number of secs prog must stay running (def. 1)
redirect_stderr=true ; redirect proc stderr to stdout (default false) 错误重定向
stdout_logfile=/var/log/supervisor/test.log ; stout log path, NONE of none ;default AUTO,log输出
```

#### 启动supervisor服务

```bash
$ supervisord -c /etc/supervisor/supervisord.conf
```

#### 使用pstree查看进程

```bash
$ pstree | grep supervisor
```
![Alt text](/img/QQ截图20161108153954.png)

#### 查看监控的进程，发现test running

```bash
$ supervisorctl -c /etc/supervisor/supervisord.conf
```

![Alt text](/img/QQ截图20161108154215.png)

#### 命令

```bash
$ supervisorctl [-c /etc/supervisor/supervisor.conf ] stop|start|restart all #停止|启动|重启 所有进程
$ supervisorctl [-c /etc/supervisor/supervisor.conf ] #登录控制台
```

#### 参见[supervisor初体验](http://www.jianshu.com/p/9abffc905645)
