# DVWA 学习笔记

`更新时间：2023-11-8`

注释解释：

- `<>`必填项，必须在当前位置填写相应数据

- `{}`必选项，必须在当前位置选择一个给出的选项

- `[]`可选项，可以选择填写或忽略

# Brute Force

## Low

先输入任意用户名与密码尝试登录

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/EAG00}XF)J3K]XTETW_I9FB.png?raw=true">

打开BurpSuite进行抓包

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/RY64R[5KNYKG(@_M[PHMMS4.png?raw=true">

可以直接获取用户名及密码，右键发送到Intruder

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/$H}12M9CRR]AA~25]1)P3A4.png?raw=true">

点击右边的清除playload位置，然后再双击用户名，点击添加playload位置，密码相同操作，再把攻击类型设置为集束炸弹

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/9@5[SC3TJPKVM9X86OM]QR3.png?raw=true">

切换到playload分页，设置playload集为1（即用户名），在playload settings栏下导入字典或手动添加可能出现的用户名，然后设置playload集为2（即密码），进行相同操作

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/S3R5AYXN10GNIWZB]P{ECTF.png?raw=true">

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/}I~VJBB62X{LKVS0%GG93NQ.png?raw=true">

点击开始攻击，攻击完成后，找到长度与其他不同的一项并点击，选择下方 `响应 > 页面渲染` ，发现破解成功，用户名为admin，密码为password

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/Z1K`I68R9)1{{1RMO$MF_~A.png?raw=true">

## Medium

Medium操作和Low一致，从源码得知仅仅是在登录失败后添加了2秒的系统休眠，增加了攻击事件

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/D6URFSHRJEC%2X33~)NDUVF.png?raw=true">

破解成功

> <img src="https://github.com/Ki1z/DVWA/blob/main/Image/A~VC%2N4@A61LZ8)JGUS0(2.png?raw=true">

## High

