
pc_wxapkg_decrypt_python
=====
[![License](https://img.shields.io/github/license/kksanyu/pc_wxapkg_decrypt_python)](https://github.com/kksanyu/pc_wxapkg_decrypt_python)

### 概述

PC微信小程序加密包解密方案 wxapkg

本脚本在PC微信版本 `2.9.5.41` 上测试可用, 不排除后续更新更换相关密钥参数的可能，如无法解密可自行替换。

### 准备工作

找到 `C:\Users\{用户名}\Documents\WeChat Files\Applet` 目录, 找到你要解密的 `wxapkg文件`, 以及目录父级目录的 `微信APPID`

### 使用

完成了准备工作之后, 就可以愉快的使用脚本了

#### 命令

```
usage: main.py [-h] --wxid 微信小程序ID [--iv iv] [--salt salt] -f 加密的小程序包文件路径 -o
               解密后的小程序包文件路径

PC微信小程序wxapkg包解密工具

optional arguments:
  -h, --help            show this help message and exit
  --wxid 微信小程序ID
  --iv iv
  --salt salt
  -f 加密的小程序包文件路径, --file 加密的小程序包文件路径
  -o 解密后的小程序包文件路径, --output 解密后的小程序包文件路径
```

#### 例子

```shell
# python main.py --wxid 微信APPID --file 输入文件 --output 输出文件
$ python main.py --wxid wx1234567890123456 --file __APP__.wxapkg --output dec.wxapkg
```

### 原理

PC版本的微信的加密特征: `V1MMWX`

下面直接引用 `BlackTrace` 大神的解释, 原文链接看下面相关链接里的GO版本代码。

> 首先pbkdf2生成AES的key。利用微信小程序id字符串为pass，salt为saltiest 迭代次数为1000。调用pbkdf2生成一个32位的key
首先取原始的wxapkg的包得前1023个字节通过AES通过1生成的key和iv(the iv: 16 bytes),进行加密
接着利用微信小程序id字符串的倒数第2个字符为xor key，依次异或1023字节后的所有数据，如果微信小程序id小于2位，则xorkey 为 0x66，把AES加密后的数据（1024字节）和xor后的数据一起写入文件，并在文件头部添加V1MMWX标识

#### 相关链接

- [通过frida直接提取未加密的wxapkg包](https://github.com/kksanyu/frida_with_wechat_applet)

- [GO版本的解密工具代码](https://github.com/BlackTrace/pc_wxapkg_decrypt)

### License

The MIT License(http://opensource.org/licenses/MIT)

请自由地享受和参与开源
