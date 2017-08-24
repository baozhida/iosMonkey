# iosMonkey


现在客户端是使用同步的方式处理发送的事件，效率稍低

经实测：

iPhone 6plus 10.2.1，每分钟可以执行大约40次事件；

iPhone 7 10.2，每分钟可执行大约60次事件；

通过修改代码实现了异步http请求，发送事件的请求数量可以大幅提升，但是很多请求会阻塞，没有执行，实际效果不好，遂放弃。

### 更新

2017.3.17: 原版本呼起app的指令是当成事件，按照一定的频率执行，不管现在是否是在测试APP页面都会执行。现在改为守护进程的模式，只有当前页面跳出测试APP时候才会执行打开APP的指令。

2017.4.5:

1.修复app守护进程的bug，由于时间问题导致的

2.增加返回homescreen的事件，事件概率是2%，返回手机主页5s后重新打开app

# 0、简介

- 1.修改了原版的构建方式，maven bulid（其实没看到之前构建的配置）

- 2.解决了idevicedebug 命令呼起app无效的问题

- 3.根据app实际情况，修改了特殊点的处理（在有些页面，点击固定的位置才能返回，不是通用的右上角）

- 4.如果不修改直接使用，直接使用target下面的jar包，如果实现自己的方法，需要重新编译

# 1、功能
- 1.可以模拟android monkey执行的方式，在IOS APP页面点击 滑动操等作,支持 ios 9和ios10

- 2.支持多台ios一起执行

- 3.计划保存截图（未完成）

- 4.计划自动收集crash日志（未完成）


# 2、准备macaca环境

### 安装usbmuxd

$ brew install usbmuxd

### 安装ios-webkit-debug-proxy

$ brew install ios-webkit-debug-proxy

### 安装ios-deploy

$ npm i ios-deploy -g

### 安装ideviceinstaller

$ brew install ideviceinstaller

### 编译libimobiledevice

libimobiledevice需要自己编译，证实直接安装的不能用

https://github.com/libimobiledevice/libimobiledevice

下载到本地

Before building, try setting:

```
LD_LIBRARY_PATH=/usr/local/opt/openssl/lib:"${LD_LIBRARY_PATH}"
CPATH=/usr/local/opt/openssl/include:"${CPATH}"
PKG_CONFIG_PATH=/usr/local/opt/openssl/lib/pkgconfig:"${PKG_CONFIG_PATH}"
export LD_LIBRARY_PATH CPATH PKG_CONFIG_PATH
You might want to add that to your .bash_profile.
```

CPATH and PKG_CONFIG_PATH are for compiling. LD_LIBRARY_PATH is for runtime. See also https://gist.github.com/samrocketman/70dff6ebb18004fc37dc5e33c259a0fc


Then to compile run:

	./autogen.sh
	make
	sudo make install


### 安装carthage
$ brew install carthage

### 安装macaca套件 包括，macaca-cli macaca-ios

```
npm install macaca-cli -g

npm install macaca-ios -g

```
MAC全局安装的路径分别如下：

```
/usr/local/lib/node_modules/macaca-cli
/usr/local/lib/node_modules/macaca-ios
```

### 检测macaca环境，无报错
$ macaca doctor

# 3、WebDriverAgent项目重签名
按照项目

https://github.com/baozhida/MacacaAutomation

操作项目重签名

# 4、帮助命令
如果需要多个iOS设备一起执行，需要指定 proxyport ，同时macaca使用不同的端口
```
$ java -jar iosMonkey-1.0.jar -h
-u:设备的UDID
-b:测试App的Bundle
-port:macaca服务的端口，默认3456
-proxyport:usb代理端口，默认8900
```

# 5、执行iosMonkey
开一个窗口执行

$ macaca server --verbose

在一个新窗口执行

$ java -jar [iosMonkey.jar Path] -u [设备的UDID] -b [测试App的BundleID] -port [macaca服务端口,可选] -proxyport[usb代理端口,可选]
# 6、修改源码重新打包方法

如果需要源码实现自定义的功能，在项目目录下执行

$ mvn assembly:assembly

最后提示如下，标示打包成功，target下生成iosMonkey-1.0.jar，可以使用最新的包
```
INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 7.350 s
[INFO] Finished at: 2017-03-06T17:01:30+08:00
[INFO] Final Memory: 20M/324M
[INFO] ------------------------------------------------------------------------
```
