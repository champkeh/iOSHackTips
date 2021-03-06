# iOSHackTips
记录iOS越狱开发的学习笔记

---

## 设备
1. MacOS
2. 越狱手机

## 软件
1. Reveal (查看UI以及运行时地址(包括控件和ViewController))
2. usbmud工具包 (主要用到里面的tcprelay脚本进行端口转发)
3. cycript (在越狱手机上安装)
4. openssh (在越狱手机上安装)
5. PP助手 (用于安装越狱app，避免自己手动脱壳)
6. Hopper Disassembler v4
7. Xcode
8. PostMan
9. Calculator (计算16进制地址转换)


## 使用cycript进行动态调试
1. 配置mac的ssh
```shell
file: .ssh/config

Host 5s
Hostname localhost
User root
Port 8888
```

2. 使用tcprelay进行端口转发
```
tcprelay.py -t 22:8888
```

3. ssh登录手机
```
ssh 5s
```

4. Cycript绑定目标进程
```
cycript -p 应用程序名称/pid
```

5. 在电脑上打开Reveal软件，定位目标控件的地址

6. 在cycript中执行
```
var vc = #地址
```

7. 执行对应的方法


## 使用lldb动态调试app
1. 在Mac上使用tcprelay进行端口转发
```
tcprelay.py -t 22:8888 (用于ssh登录)
tcprelay.py -t 1234:1234 (用于lldb调试)
```

2. ssh登录到手机
```
ssh 5s
```

3. 在手机上启动debugserver
```
ps -e | grep xxx
debugserver *:1234 -a pid
```

4. 在Mac上开启lldb进行调试
```
lldb
(lldb)process connect connect://localhost:1234
```

5. 获取应用的ASLR偏移
在lldb调试器中，执行
```
image list -o -f
```
找出对应程序的偏移
![ASLR偏移](asserts/images/aslr偏移.png)

6. 修改hopper中的 base address,这样就不用手动计算aslr偏移了
![](asserts/images/changebaseaddress.png)

7. lldb设置断点
```
br s -a 0x1234xxx
```

## 获取目标app的头文件及汇编代码
1. 从PP助手下载对应app的越狱版本
![](asserts/images/download_app.png)

2. 将下载的.ipa文件后缀名改为.zip，然后解压出二进制文件
![](asserts/images/renameipa.png)

3. 导出头文件
```
class-dump -A -a --arch arm64 -H -o header -S <上一步得到的二进制文件>
```

4. 使用Hopper打开上面的二进制文件，会得到汇编代码


## 调试小技巧
1.如果断点打在方法开头，则可以使用 po $arg3,$arg4,$arg5...查看方法的参数
$arg1是该方法的实例, $arg2暂时不知

2. 如何查看方法的返回值呢？
