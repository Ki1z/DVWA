# DVWA 学习笔记

`更新时间：2023-11-10`

注释解释：

- `<>`必填项，必须在当前位置填写相应数据

- `{}`必选项，必须在当前位置选择一个给出的选项

- `[]`可选项，可以选择填写或忽略

# 暴力破解 Brute Force

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

# 命令注入 Command Injection

## Low

提示输入一个IP地址，输入127.0.0.1进行尝试，发现返回格式与cmd中ping一致

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/8(`2QC47JFACRBY7_F%]]FT.png?raw=true">

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/S4}T{[2U7CWOKW~58@K2P%P.png?raw=true ">

查看源码，发现是对输入的ip执行ping命令

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/}TWYPW]BJJHFJNVO{I(0@JN.png?raw=true">

使用命令连接符进行尝试，注入成功

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/SW@`F@P5[%XD%LHU024@80I.png?raw=true">

命令连接符：

- `a & b` ：a与b都会执行，无制约关系

- `a && b` ：a为真，才会执行b

- `a | b` ：无论a是否为真，都会执行b

- `a || b` ：若a为真不执行b，若a为假才执行b

- `a ; b ; c` ：执行多个命令，命令之间无制约

## Medium

查看源码，发现将 `&&` 和 `;` 加入了黑名单

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/AF$9@YBB}%}M@~$4T`V]TF2.png?raw=true">

但是可以使用黑名单之外的连接符

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/R2I%R05MGE(6NB226}IWBE2.png?raw=true">

## High

查看源码，发现黑名单中包含了所有可用的命令连接符，但是仔细查看，发现 `'| '`后多了个空格，此时 `|` 后若不带空格直接跟命令不受影响

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/)5AUY4W_5DX@80UN6_[843E.png?raw=true">

填入 `127.0.0.1 |dir` 注入成功

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/5F3SX3EOH3USOQ1E%WXZ7Q2.png?raw=true">

# 跨站请求伪造 CSRF

## Low

查看网页，提示更改密码，输入pass进行尝试，密码更改成功

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/K8))9D`XGER97YL2A@7`9L4.png?raw=true">

进行登录验证，密码已经被更改为pass

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/)VX(HL09VFZ4OWK{53_QP`5.png?raw=true">

回到刚才的页面，仔细观察，发现url栏中显示了更改的密码

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/_TCQEI2V1XR0YUV0B)4(D13.png?raw=true">

观察url栏：
http://127.0.0.1/dvwa/vulnerabilities/csrf/?password_new=pass&password_conf=pass&Change=Change#
password_new是新密码，password_conf是确认密码，说明输入的信息可能会在url栏进行传输执行。进行抓包，发现请求信息与url栏信息一致

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/H1$HALEPGPF_6LG9S%XU~DX.png?raw=true">

尝试对url栏内容进行修改，新建标签页进行提交，这里我将密码改为root

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/87OHW4C853[5C6$~(NEE~]6.png?raw=true">

进行登录验证，密码更改成功

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/{1TLUD8{3)1_R_O9}TY9C6O.png?raw=true">

分析源码，只对传入的两个密码进行比较，没有任何过滤，可以轻易执行CSRF攻击

> <img src="https://github.com/Ki1z/DVWA/blob/main/IMG/~XRAAE9~IHD_GK19L1(X$DJ.png?raw=true">

## Medium