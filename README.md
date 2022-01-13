# esp8266接入blinker

---

使用`esp8266`通过blinker平台实现点灯，通过小爱同学控制灯的状态颜色，也可以通过blinker提供的app进行控制。

## 环境搭建

---

 [Arduino IDE](https://www.arduino.cc/en/software) 是由 Arduino 官方提供的支持 **C 语言**的集成开发环境，主要是针对 Arduino 系列的开发板进行编程。

#### 一、添加 ESP8266 支持

首先从 [Arduino 官网](https://www.arduino.cc/en/main/software) 下载最新版本的 **Arduino IDE** 软件并安装。
安装完成以后，进入**首选项**（Preferences），找到**附加开发板管理器地址**（Additional Board Manager URLs），并在其后添加如下信息：
`http://arduino.esp8266.com/stable/package_esp8266com_index.json`

![](https://s3.bmp.ovh/imgs/2022/01/8ae79900a7f8f07d.png)

之后点击**工具** - **开发板** - **开发板管理器**，进入开发板管理器界面：

找到 `esp8266`并安装：

![](https://s3.bmp.ovh/imgs/2022/01/ad82a26a9fbe430d.png)

安装完成后，重启 Arduino IDE 软件。在**工具** - **开发板**选项中即会看到 ESP8266 开发板的选项：

![](https://upload-images.jianshu.io/upload_images/6875152-8e43f9590922701a.png?imageMogr2/auto-orient/strip|imageView2/2/w/570/format/webp)



使用前您需要做好如下准备:

- 按照 [安装说明](https://github.com/espressif/arduino-esp32#installation-instructions) 安装 [espressif/arduino-esp32](https://github.com/espressif/arduino-esp32)

> 注意：blinker AT固件至少需要1MB以上的Flash空间，请自行在**IDE>工具>Flash size**中设置

#### 下载并安装blinker Arduino库

1. [点击下载](https://diandeng.tech/dev)
2. 通过Arduino IDE **菜单>项目>加载库>添加.ZIP库** 导入到库，如图：
   ![img](https://diandeng.tech/docs/img/001/import-lib.png)

## 材料

* `esp8266` 开发板（`nodemcu`）
* 杜邦线
* 数据线
* RGB灯珠（本人使用的是共阴极）
* 数据线一条



## 实现

##### 一键配网：

开机上电时候就自动进入等待配网环节

进入手机app-开发者- smartconfig 进行一键配网  

值得注意的是：

 			esp8266仅支持2.4Ghz的wifi,搜不到5Ghz的信号

```c++
#define BLINKER_ESP_SMARTCONFIG    //智能配网
#define BLINKER_WIFI
#include <Blinker.h>
#include <EEPROM.h>

char auth[] = "5f4e37b8b10***";

void setup(){
     Blinker.begin(auth);
	
}
```



##### 一键擦出：

当app触发按钮“btn2”时，执行回调函数，擦除一键配网信息。

```c++
//在手机app上创建一个名为btn2的按键
BlinkerButton Button2("btn2");

//创建一个点击处理函数
void button2_callback(const String & state) {
       BLINKER_LOG("get button state: ", state);
       Blinker.reset();   //调用系统定义的擦数函数
}

void setup(){
    
    //注册回调函数
     Button2.attach(button2_callback);

}


```



## 演示

[bilibili演示](https://www.bilibili.com/video/BV1VZ4y1f7Wp)

