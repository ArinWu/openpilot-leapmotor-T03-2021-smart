
###下载这个word文档观看
——————————————————————————————————————————————————————————

# openpilot-leapmotor-T03-2021-smart
持续更新openpilot 适配 零跑T03 2021 豪华版 opendbc开发 cabana使用 directly open cabana on vmstation
同步滚动：
本文涵盖从小白到实现openpilot适配新车所有过程，不是教人，是分享哈，很多后面写的所以角度不一样
风格类似草稿
目前做了的事：ubuntu虚拟机、安装openpilot、panda、cabana、线束连接、维修手册、can消息分析、建了个群 arinwu1 +

先了解自己的车能不能适配？有没有ACC自适应巡航和车道保持？
更多的了解是是不是can协议？canfd协议和flexray协议似乎更难。

所有流程流程都可以参考这个适配，本文也是根据这个适配去做的！！！
https://www.you乱码去除tube.com/watch?v=XxPS5TpTUnI&t=886s

如果你觉得你的车能适配的话，你想自试试的话，那就先要有车车辆的dbc文件
这里要用到cabana去读取车辆的dbc数据
你要有cabana这个工具 连接到你的车辆、然后你也要有车辆的说明书，才好知道需要那几个口的数据
车辆维修说明书和电路图这两个东西，获取渠道的话搜得到问的到更好，问不到就某yu买
目前只买维修的说明书
![image](https://github.com/user-attachments/assets/818e9a7d-4473-4dac-9371-2bb09eb8904f)



![image](https://github.com/user-attachments/assets/0f6fb7c2-c7ae-4ff3-a22f-d80cb7daed9e)


我的车大概是这样，我这样就能读取到信息了（他们从摄像头我问了似乎都可以，我目前不是很了解，我现在只想读取到数据）

然后购买一根odb的线束，导线及一些端子


![image](https://github.com/user-attachments/assets/13c44d18-9cf9-4e83-9061-032131aaedb9)



随便买的线、没考虑信号问题

端子压线的话我都是用老虎钳，专业的钳子搞不好，买了闲置了
线的话一米左右，没考虑那么多

然后有一个读取的硬件叫做panda，我也看了其他的can消息读取的，基本上都不太能用来做op开发
单买panda也要800，所以我买了一套二手的c2套件 850元，



![image](https://github.com/user-attachments/assets/f8bdb192-315d-4a1e-9bd2-68ccf6fe647d)



然后咱们把panda连上汽车，需要自己再做一个接口
电路图如图，这个是panda的接口图
我们只需要连接can高和can低，我的车有四个口，所以我四个都连接了，目前


![image](https://github.com/user-attachments/assets/51d08a9a-62d0-4113-afdb-3f2f94b997d8)


![image](https://github.com/user-attachments/assets/b60d448f-0691-40df-a1d6-976c00cfb236)




所有的电路图都从这个方向看哈


![image](https://github.com/user-attachments/assets/726b5717-1e0c-4b93-bd51-49990123d4b9)


不然会插错

压线什么的自己研究哦

车连接panda，我都是用那个端子活插的，用电胶布绑好了
如图所示


![image](https://github.com/user-attachments/assets/15a9ef4d-5b8a-472b-85c8-c933e675c02e)


接下来panda需要链接您的电脑
用的一根双公头usb线


![image](https://github.com/user-attachments/assets/b13d4524-e4c8-4c33-b8cb-cf53c44c441b)



另一头连到电脑笔记本。
接下来我们需要打开cabana
这个过程比较复杂
需要用linux配置openpilot 配置方法参考官网，
我用vmware虚拟机安装我试了十次，成功概率很小
总需要安装各种包，而且就算你有梯子，也有可能中断，
linux系统+op大概是10个G
自己要配置很麻烦，需要的直接私信给我就好w + arinwu1 或者1943751373@qq.com
我直接给nas链接
打开nas后下载后 解压后 有个vm安装包 15.多的版本，自己找激活码
然后直接打开虚拟机就行
打开系统后，连接好panda
虚拟机会自动检测到usb panda，没检测到就重复拔插，实在不行，就再加个usb供电（我的是这样），要好几次才能连接
没有弹出就自己找下，硬件列表里面，linux使用反正是

使用cabana 可以直接命令行（要在openpilot目录） op cabana
然后选择这个串口 （没有就是没连到），创建一个dbc文件
关闭cabana会让你保存这个dbc文件
这个以后要重复使用哦
以后打开cabana就打开这个dbc然后，打开连接串口
如果看不懂，不知道怎么破解
可以先看下上面那个youtube适配，或者看下demo
op cabana --demo
可以打开一个全部解析好的demo
具体解析方法参考这个

车载测试之CAN总线通信：CAN报文中的信号解析_哔哩哔哩_bilibili
车载测试之CAN总线通信：CAN报文中的信号解析, 视频播放量 36270、弹幕量 40、点赞数 916、投硬币枚数 512、收藏人数 1858、转发人数 278, 视频作者 老贾聊车载测试, 作者简介 绿泡泡：LAOjiatest…www.bilibili.com

我目前门窗转向，油门基本ok
还差蛮多，规范化和其他的需要边开边解析，我没空，
或者用neo，记录can消息和视频同一时间，具体可以看cabana的文档哈

dbc文件半成品有需要邮箱联系
还在做，没时间，可能好久更新
做好后更新dbc教程
读取消息

下面是一些我翻译的值
-----------------------------------------------------------------------------------------
CarState 描述
wheelSpeeds vEgoRaw、vEgo、aEgo由这些值派生
yawRate 可选，目前未使用
gas 驾驶员加速踏板的百分比
gasPressed 如果加速踏板非零值
brake 驾驶员刹车踏板的压力
brakePressed 如果刹车踏板非零值
parkingBrake 手刹或电子驻车已设置
steeringAngleDeg 方向盘角度
steeringRateDeg 方向盘角度变化率
steeringTorque 驾驶员施加在方向盘上的扭矩
steerFaultTemporary 电动助力转向系统(EPS)有可恢复的故障
steerFaultPermanent 电动助力转向系统(EPS)有需要重启的故障
CarState 描述
steeringPressed 驾驶员施加的扭矩超过阈值
espDisabled 稳定控制被禁用
cruiseState 自适应巡航控制(ACC)状态，主开关已启用
gearShifter 档位状态(P-R-N-D-L-S)
leftBlinker 左转向灯
rightBlinker 右转向灯
doorOpen 一扇或多扇门打开
seatbeltUnlatched 驾驶员安全带未扣
leftBlindspot 车辆盲点监测(BSM)检测到左侧盲区有车辆
rightBlindspot 车辆盲点监测(BSM)检测到右侧盲区有车辆
accFaulted 车辆纵向控制发生故障
buttonEvents 列表包含ACC控制按钮状态






![image](https://github.com/user-attachments/assets/47a80778-5cb9-495f-a1aa-a79f4aa7067d)


以下作废
——————————————————————————————————————————————————————————————————————————————————
open cabana directly on vmstation
i tried so many times, wanna save time？ u can contact my email to get this vmstation file: 19434751373@qq.com, contact me if u need this.
![image](https://github.com/user-attachments/assets/5afc9581-fb4f-4785-b937-26a60045f225)
![15fe2df611aa86e056a57239e101859](https://github.com/user-attachments/assets/898a0ebd-5a48-47a3-a232-2b5e1b141311)

```
root：sushi
password: sushi
cd ./openpilot
op cabana // run cabana
or 
op cabana --demo // see a demo
```

contact me ON EMAIL if u have problems about PANDA \ HARNESS \ NEO \ OPEN DBC

————————————————————————————————————————————————————————————————————————————————————————————————————
