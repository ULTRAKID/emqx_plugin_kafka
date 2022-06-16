# 适配emqx v4.3版本

## 更新说明

*v4.3.12.6*: 

`2022.06.16` 将原来的kafka依赖从`ekaf`替换成[kafka4beam](https://github.com/kafka4beam)/**[brod](https://github.com/kafka4beam/brod)**

emqx-plugin-template
====================

[emqx/emqx-plugin-template at emqx-v4 (github.com)](https://github.com/emqx/emqx-plugin-template/tree/emqx-v4) This is a template plugin for the EMQ X broker. 

Plugin Config
-------------

配置文件模板及说明见`etc/emqx_plugin_kafka.config`

Authentication and ACL
----------------------

```
emqx:hook('client.authenticate', fun ?MODULE:on_client_authenticate/3, [Env]).
emqx:hook('client.check_acl', fun ?MODULE:on_client_check_acl/5, [Env]).
```

Build the EMQX broker
-----------------
###### 1. 基于CentOS7.5环境下编译，先安装相关插件

```
  yum -y install make gcc gcc-c++ glibc-devel glibc-headers kernel-devel kernel-headers m4 ncurses ncurses-devel openssl openssl-devel openssl-libs zlib zlib-devel libselinux-devel xmlto perl git wget
  
  注意：openssl的版本不是1.1.1k，则需要通过源码openssl-1.1.1k.tar.gz来安装openssl
```
###### 2. 准备Erlang/OTP 22及以上环境

参照[Erlang and Elixir Packages Download - Erlang Solutions (erlang-solutions.com)](https://www.erlang-solutions.com/downloads/) 官网安装方式。

###### 3. 下载EMQX源码

官方源码仓库地址为[emqx/emqx: An Open-Source, Cloud-Native, Distributed MQTT Message Broker for IoT. (github.com)](https://github.com/emqx/emqx) ，分支为`main-v4.3`

本人修改了官方的编译脚本，并且在插件目录里添加了该kafka插件的信息，仓库地址为[ULTRAKID/emqx at main-v4.3 (github.com)](https://github.com/ULTRAKID/emqx/tree/main-v4.3) ，分支为`main-v4.3`。

###### 4. 修改EMQX文件，增加kafka插件
参照[emqx/README.md at main-v4.3 · ULTRAKID/emqx (github.com)](https://github.com/ULTRAKID/emqx/blob/main-v4.3/lib-extra/README.md) 。

注：[ULTRAKID/emqx at main-v4.3 (github.com)](https://github.com/ULTRAKID/emqx/tree/main-v4.3) 仓库内已进行此项修改。

###### 5. 编译EMQX，并且启动EMQX

进入emqx目录，执行make命令，需要保持外网通畅，有条件建议科学上网。

二进制编译命令：`make`

docker镜像打包：`make emqx-docker`


License
-------

Apache License Version 2.0

Author
------

Ultrakid.

EMQ X Team.
