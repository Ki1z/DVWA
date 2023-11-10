# DVWA 学习笔记

`更新时间：2023-11-10`

注释解释：

- `<>`必填项，必须在当前位置填写相应数据

- `{}`必选项，必须在当前位置选择一个给出的选项

- `[]`可选项，可以选择填写或忽略

# Brute Force

## Low

先输入任意用户名与密码尝试登录

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/EAG00}XF)J3K]XTETW_I9FB.png?raw=true">

打开BurpSuite进行抓包

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/RY64R[5KNYKG(@_M[PHMMS4.png?raw=true">

可以直接获取用户名及密码，右键发送到Intruder

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/$H}12M9CRR]AA~25]1)P3A4.png?raw=true">

点击右边的 `清除Payload位置` ，然后再双击用户名，点击 `添加Payload位置` ，密码相同操作，再把攻击类型设置为 `集束炸弹`

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/9@5[SC3TJPKVM9X86OM]QR3.png?raw=true">

切换到 `Payload` 分页，设置 `Payload集` 为1（即用户名），在 `Payload settings` 栏下导入字典或手动添加可能出现的用户名，然后设置 `Payload集` 为2（即密码），进行相同操作

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/S3R5AYXN10GNIWZB]P{ECTF.png?raw=true">

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/}I~VJBB62X{LKVS0%GG93NQ.png?raw=true">

点击 `开始攻击` ，攻击完成后，找到长度与其他不同的一项并点击，选择下方 `响应 > 页面渲染` ，发现破解成功，用户名为admin，密码为password

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/Z1K`I68R9)1{{1RMO$MF_~A.png?raw=true">

## Medium

Medium操作和Low一致，从源码得知仅仅是在登录失败后添加了2秒的系统休眠，延长了攻击时间

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/D6URFSHRJEC%2X33~)NDUVF.png?raw=true">

破解成功

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/A~VC%2N4@A61LZ8)JGUS0(2.png?raw=true">

## High

通过代码分析，High难度将休眠时间设置为了0到3秒随机数，并增加了token验证。token的作用是为每个登录成功的IP增加一个密匙，每次该IP登录服务器时不需要通过验证就能直接登入，减小服务器负担

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/1.png?raw=true">

如果登陆失败token会刷新，所以我们使用 `Pitchfork` 攻击。在使用 `Pitchfork` 攻击时，只能设置两个 `Payload` ，所以我们设定已知用户名admin，爆破密码和token

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/}CQKYTIDW(VJ$_TWRVN(4{D.png?raw=true">

与密码不同，token需要每次爆破时重新获取，先设置获取token。选择设置分页，下滑找到 `检索-提取` ，勾上 `从响应包中提取以下项目` ，点击 `添加`

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/R@M`(CNU1_$8LZP84Y~4JVE.png?raw=true">

点击 `重新获取响应` ，下滑找到token，双击，并按复制下来，后面会使用，然后点击 `确定` 返回

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/{ZZCO~VG%$E)7OXQJO8$8ED.png?raw=true">

回到 `Payload` 分页，先同上文一样填入密码，然后选择 `Payload集` 为2，设置 `Payload类型` 为递归提取，并在下方 `Payload Setting` 下的 `首次请求初始payload` 栏里填入刚才复制的token

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/WDQKZ}]QYUNE5XHXW)I61KL.png?raw=true">

然后转到 `资源池` 分页，下滑选择 `新建资源池` ， `最大并发请求数` 设置为2， `请求间隔` 设置为300，然后点击 `开始攻击`

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/KN[M2S{E_KP`C`K(BX]E}G8.png?raw=true">

等待攻击完成，会有两个长度不同的项，点击下方的 `响应 > 页面渲染` 找到正确项即可

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/NC_]C9$J0CI~2(}J2{V6YPW.png?raw=true">