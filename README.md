# homebridge-mi-ir-remote
[![npm version](https://badge.fury.io/js/homebridge-mi-ir-remote.svg)](https://badge.fury.io/js/homebridge-mi-ir-remote)

ChuangMi IR Remote plugin for HomeBridge.   

欢迎加入我们的QQ群 545171648 讨论  
QQ Group:545171648  
Telegram Group: https://t.me/joinchat/EujYfA-JKSwpRlXURD1t6g

## Installation
1. Install HomeBridge, please follow it's [README](https://github.com/nfarina/homebridge/blob/master/README.md).   
If you are using Raspberry Pi, please read [Running-HomeBridge-on-a-Raspberry-Pi](https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi).   
如果你能阅读中文,你可以阅读 [homebridge非官方安装指南](https://homekit.loli.ren).
2. Make sure you can see HomeBridge in your iOS devices, if not, please go back to step 1.   
3. Install packages.   
4. Install Plugin.

## Supported Types
1.Switch  
2.LightBulb  
3.Projector  
4.Airconditioner  
5.Custom  
6.MomentarySwitch  
7.Fan

##
U should active MiLearn from Home app then try to learn each command manually.  
At the meantime, you are supposed to see the command string come out in the HomeBridge console.  
Just like:   
[10/27/2017, 2:39:35 AM] [ChuangmiIRPlatform] [MiIRRemote][irLearn]Learned Code: Z6WDAC8CAAAIBQAAAggAALsiAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAjAAAAAAAQABAQAAAAAAAAAAAAABAQAAAAEAAAABAAAAAQEAAAEBAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAEBAQAAA=   
Just grab the string then fill it to config file, everything should be working after u restart HomeBridge.  

### Custom is used for multi-commands. when you want to add a lot of commands in one switch, just use custom. Custom's data is just like a switch.add your commands in on/off like the sample-config. Just like:  
 "0": "interval|code"
* Because of the limit of JSON, you have to add "number" as the key of the data.Use it just as the sample below.
* interval means the time to send the command after you press the on/off switch.Use 0 to send immediately.
* interval's unit is second. you can use number like 0.5 if you want to send commands within a second.(I don't suggest you set too much commands in a short time.)

### MomentarySwitch is a switch which will automaticly turn off after you turn on it (0.3s). It is used for some situation like you want to launch a channel on button.

### Fan settings explanation:
Only `on` and `off` settings are mandatory. In case you use only these, you will have basic fan with only ON and OFF functions.   
If your fan supports swing mode, you can set `swing`'s `on` and `off` codes - functionality will appear in Home app.   
`speed` setting is an array in which you define codes for setting speed (order: ASC). Home app can display speed only in percentage. Therefore if you set 3 codes in here ("xxxxxxx", "yyyyyyy", "zzzzzzz"), then "xxxxxxx" code will be sent at 0-33%, "yyyyyyy" at 34-66% and "zzzzzzz" at 67-100% speed.

## Installation
1. Install HomeBridge, please follow it's [README](https://github.com/nfarina/homebridge/blob/master/README.md).   
If you are using Raspberry Pi, please read [Running-HomeBridge-on-a-Raspberry-Pi](https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi).   
2. Make sure you can see HomeBridge in your iOS devices, if not, please go back to step 1.   
3. Install packages.   
```
npm install -g miio homebridge-mi-ir-remote
```
## Configuration
```
"platforms": [
    {
        "platform": "ChuangmiIRPlatform",
        "hidelearn": false,
        "learnconfig":{
            "ip": "192.168.31.xx",
            "token": "xxxxxxx"
        },
        "deviceCfgs": [{
            "type": "Switch",
            "ip": "192.168.31.xx",
            "token": "xxxxxxx",
            "Name": "IR Switch",
            "data": {
                "on" : "xxxxxxx",
                "off": "xxxxxxx"
            }
        },{
            "type": "Projector",
            "ip": "192.168.31.xx",
            "token": "xxxxxxxx",
            "Name": "IR Projector",
            "interval": 1,
            "data": {
                "on" : "xxxxxxxxxxxxx",
                "off": "xxxxxxxxxxxxx"
            }
        },{
            "type": "Light",
            "ip": "192.168.31.xx",
            "token": "xxx",
            "Name": "IR LightBulb",
            "data": {
                "100" : "xxxx",
                "75" : "xxxxx",
                "50" : "xxxxx",
                "25" : "xxxxx",
                "off" : "xxxx"
            }
         },{
            "type": "AirConditioner",
            "ip": "192.168.31.xx",
            "token": "xxx",
            "Name": "IR AC",
            "DefaultTemperature": 25,
            "MinTemperature": 16,
            "MaxTemperature": 30,
            "data": {
                "Cool":{
                    "30" : "xxx",
                    "25" : "xxx",
                    "20" : "xxx",
                    "16" : "xxx"
                },
                "Heat":{
                    "30" : "xxx",
                    "25" : "xxx",
                    "20" : "xxx",
                    "16" : "xxx"
                },
                "off" : "xxxx"
            }
        },{
            "type": "Fan",
            "ip": "192.168.31.xx",
            "token": "xxx",
            "Name": "IR Fan",
            "data": {
                "on" : "xxxx",
                "off" : "xxxx",
                "keepstate" : false,
                "speeds": [
                    "xxx",
                    "xxx",
                    "xxx"
                ],
                "swing":{
                    "on" : "xxx",
                    "off" : "xxx"
                }
            }
        },{
            "type": "Custom",
            "ip": "192.168.31.xx",
            "token": "xxx",
            "Name": "Custom",
            "data": {
                "on": {
                    "0": "0|xxx",
                    "1": "2|xxx",
                    "2": "5|xxx"
                },
                "off": {
                    "0": "1|xxx"
                }
            }
        },{
                "type": "MomentarySwitch",
                "ip": "192.168.31.xx",
                "token": "xxxx",
                "Name": "Momentary Switch",
                "data": "xxxxxx"
        }]
    }]
```
You can also set IP and token globally so that you won't need to set it for each button.
```
"platforms": [
    {
        "platform": "ChuangmiIRPlatform",
        "ip": "192.168.31.xx",
        "token": "xxxxxxx",
        "hidelearn": false,
        "deviceCfgs": [
            {
                "type": "Switch",
                "Name": "IR Switch",
                "data": {
                    "on" : "xxxxxxx",
                    "off": "xxxxxxx"
                }
            },
            {
                "type": "Projector",
                "Name": "IR Projector",
                "interval": 1,
                "data": {
                    "on" : "xxxxxxxxxxxxx",
                    "off": "xxxxxxxxxxxxx"
                }
            }
        ]
    }
]
```
In case you need specific commands to go via different IR Remote Control device, you can customize IP and token inside needed command configuration. In following example IR Projector will use global IP `192.168.31.xx` and token `xxxxxxx` while IR Switch will use IP `192.168.31.zz` and token `zzzzzzz`.
```
"platforms": [
    {
        "platform": "ChuangmiIRPlatform",
        "ip": "192.168.31.xx",
        "token": "xxxxxxx",
        "hidelearn": false,
        "deviceCfgs": [
            {
                "type": "Switch",
                "ip": "192.168.31.zz",
                "token": "zzzzzzz",
                "Name": "IR Switch",
                "data": {
                    "on" : "xxxxxxx",
                    "off": "xxxxxxx"
                }
            },
            {
                "type": "Projector",
                "Name": "IR Projector",
                "interval": 1,
                "data": {
                    "on" : "xxxxxxxxxxxxx",
                    "off": "xxxxxxxxxxxxx"
                }
            }
        ]
    }
]
```
## Get token
Open command prompt or terminal. Run following command:
```
miio --discover
```
Wait until you get output similar to this:
```
Device ID: xxxxxxxx   
Model info: Unknown   
Address: 192.168.88.xx   
Token: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx via auto-token   
Support: Unknown   
```
"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" is token.   
If token is "???", then reset device and connect device created Wi-Fi hotspot.   
Run following command:   
```
miio --discover --sync
```
Wait until you get output.  
Or you can try to get Your token from any rootrd android device.  
Details in https://github.com/jghaanstra/com.xiaomi-miio/blob/master/docs/obtain_token.md

## Version Logs  
### 0.2.0
1. Implement Fan with speed settings (optional) and swing mode (optional)
### 0.1.1
1. Implement global ip and token settings in config
### 0.1.0
1. Rewrite the plugin and added support for both miio.Device and miio.device.
### 0.0.10
1. Add QQ and Telegram group link
### 0.0.7  
1. Support for MomentarySwitch  
### 0.0.6  
1. Support for custom
### 0.0.1
1. support for Switch and IRlearn.  
