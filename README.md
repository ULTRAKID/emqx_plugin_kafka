
emqx-plugin-template
====================

This is a template plugin for the EMQ X broker. And you can see [Plugin Development Guide](https://developer.emqx.io/docs/emq/v3/en/plugins.html#plugin-development-guide) to learning how to use it.

Plugin Config
-------------

Each plugin should have a 'etc/{plugin_name}.conf|config' file to store application config.

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
###### 2. 准备Erlang/OTP 22环境 (推荐使用erlang22.3版本)
```
  (1).根据服务器CPU架构不同选择不同安装包，如果是X86架构，使用esl-erlang_22.3.4.2-1_centos_7_amd64.rpm安装；如果是arm架构，使用源码包安装。
  
   esl-erlang_22.3.4.2-1_centos_7_amd64.rpm (或 otp_src_22.3.tar.gz ) 
  
  (2).如果通过源码otp_src_22.3.tar.gz安装方式，如果中间有报错，如下错误：
    jinterface     : No Java compiler found
    odbc           : ODBC library - link check failed  

    wx             : No OpenGL headers found, wx will NOT be usable
    
  (3).错误的解决方法，一个一个来安装相关依赖包，如下：
   (a).安装：yum install java-devel
   (b).安装：yum install unixODBC-devel
   (c).通过：yum list mesa-libGLU-devel*  然后安装 yum install mesa-libGLU-devel-9.0.1-1.ky10.aarch64
```
###### 3. 下载EMQX源码
通过git下载EMQX源码，执行此命令
```
  git clone -b v4.1.0 https://github.com/emqx/emqx-rel.git emqx-rel4.1
```
###### 4. 修改EMQX文件，增加kafka插件
下载成功后完成后进入emqx-rel4.1目录，编辑emqx-rel4.1目录下的rebar.config文件，如下：
````
(a). 在 {deps,
 [emqx,
  emqx_retainer,
  emqx_management,
  emqx_dashboard,
   ...]}
 增加
  {emqx_plugin_kafka, {git, "https://github.com/jameycai/emqx_plugin_kafka.git", {tag, "master"}}}
 修改后变成
 {deps,
 [emqx,
  emqx_retainer,
  emqx_management,
  emqx_dashboard,
   ..., 
 {emqx_plugin_kafka, {git, "https://github.com/jameycai/emqx_plugin_kafka.git", {tag, "master"}}}
]}

(b). 增加 {emqx_plugin_kafka, load}
````

###### 5. 编译EMQX，并且启动EMQX
进入emqx-rel4.1目录，执行make命令，此过程会因为网络问题，多次出现错误导致停止，只需要不断地make直到成功。
````
  编译成功后，会出现_build目录，然后进入_build/emqx/rel/emqx/bin目录，启动emqx，如下：
  ./emqx start  
````

 
License
-------

Apache License Version 2.0

Author
------

EMQ X Team.
