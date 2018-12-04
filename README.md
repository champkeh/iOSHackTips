# iOSHackTips
记录iOS越狱开发的一些技巧

---

## 设备
1. MacOS
2. 越狱手机

## 软件
1. Reveal
2. usbmud工具包 (主要用到里面的tcprelay脚本进行端口转发)
3. cycript (在越狱手机上安装)
4. openssh (在越狱手机上安装)

## 步骤
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

4. cycript -p 应用程序名称/pid

5. 在电脑上打开Reveal软件，定位目标控件的地址

6. 在cycript中执行
```
var vc = #地址
```

7. 执行对应的方法
