# 软件包安装

```Python
python -m pip install --upgrade pip -i https://pypi.zlgmcu.com/root/zlg/+simple/
pip install -i  https://pypi.zlgmcu.com/root/zlg/+simple/ colorama
pip install -i  https://pypi.zlgmcu.com/root/zlg/+simple/ PyQt5==5.12.1
pip install -i  https://pypi.zlgmcu.com/root/zlg/+simple/ pyinstaller
pip list
```

# python脚本打包成exe

```Bash
将python脚本放置到C:\Users\Administrator目录，
假设python脚本的名字为：efuse.py、parse_uart_boot_socid.py
打开cmd窗口

# 显示cmd窗口
pyinstaller --onefile efuse.py

# 不显示cmd窗口
pyinstaller --onefile --noconsole efuse.py

pyinstaller --onefile parse_uart_boot_socid.py


phython文件打包完成后的exe放在这个目录:
C:\Users\Administrator\dist\efuse.exe
C:\Users\Administrator\dist\parse_uart_boot_socid.exe
```

![[Pasted image 20240830110708.png]]
![[Pasted image 20240830110802.png]]
[python 3.x - Can't get .exe to execute on other machine - "No module named serial" error - Stack Overflow](https://stackoverflow.com/questions/54910208/cant-get-exe-to-execute-on-other-machine-no-module-named-serial-error/73846888#73846888?newreg=52bd2df66aca44c8a32d463aa57a1a88)

```bash
cp efuse.py C:\Users\leefly\AppData\Roaming\Python\Python312\site-packages
cd C:\Users\leefly\AppData\Roaming\Python\Python312\site-packages
pyinstaller --onefile efuse.py --additional-hooks-dir serial\__init__.py
```

# 单文件制作
![[Pasted image 20240823132413.png]]
```HTML
https://mp.weixin.qq.com/s/Y17usEY99GeYvRmTAyVK5g
https://url27.ctfile.com/f/43907027-1015033222-fd71d3?p=4261
http://bbs.wuyou.net/forum.php?mod=viewthread&tid=419412&extra=&page=1
```

![[Pasted image 20240910150809.png]]
# Inno Setup 配置文件

```Bash
C:\Program Files (x86)\Inno Setup 6\Examples\64Bit.iss
用Inno打开上面配置文件后，点击"run"按钮
生成的安装程序文件：
C:\Program Files (x86)\Inno Setup 6\Examples\am62x_efuse_setup.exe
双击“am62x_efuse_setup.exe”进行安装，安装完成后，桌面出现“am62x_efuse”快捷方式
```

```TOML
; C:\Program Files (x86)\Inno Setup 6\Examples\64Bit.iss
[Setup]
AppName=am62x_efuse
AppVersion=1.0
DefaultDirName={pf}\am62x_efuse
DefaultGroupName=am62x_efuse
OutputDir=.
OutputBaseFilename=am62x_efuse_setup
Compression=lzma
SolidCompression=yes
DisableDirPage=yes
PrivilegesRequired=admin

[Files]
Source: "D:\am62x_efuse\*"; DestDir: "{app}"; Flags: recursesubdirs ignoreversion

[Icons]
Name: "{group}\am62x_efuse"; Filename: "{app}\efuse.exe"
Name: "{commondesktop}\am62x_efuse"; Filename: "{app}\efuse.exe"
```

```toml
; 脚本由 Inno Setup 脚本向导生成。
; 有关创建 INNO SETUP 脚本文件的详细信息请查阅帮助文档！

#define MyAppName "overwork"
#define MyAppVersion "M62xx-T@2.0.0"
#define MyAppPublisher "zlg,Inc."
#define MyAppURL "https://www.zlg.cn"
#define MyAppExeName "overwork.exe"

[Setup]
; 注: AppId的值为单独标识该应用程序。
; 不要为其他安装程序使用相同的AppId值。
; (若要生成新的 GUID，可在菜单中点击 "工具|生成 GUID"。)
AppId={{99479638-C9DE-4064-AB67-D4C17BF2F468}}
AppName={#MyAppName}
AppVersion={#MyAppVersion}
;AppVerName={#MyAppName} {#MyAppVersion}
AppPublisher={#MyAppPublisher}
AppPublisherURL={#MyAppURL}
AppSupportURL={#MyAppURL}
AppUpdatesURL={#MyAppURL}
DefaultDirName={autopf}\{#MyAppName}
DisableProgramGroupPage=yes
; 以下行取消注释，以在非管理安装模式下运行（仅为当前用户安装）。
PrivilegesRequired=admin
OutputDir=Output
OutputBaseFilename={#MyAppName}
Compression=lzma
SolidCompression=yes
WizardStyle=modern

[Languages]
Name: "english"; MessagesFile: "compiler:Default.isl"

[Tasks]
Name: "desktopicon"; Description: "{cm:CreateDesktopIcon}"; GroupDescription: "{cm:AdditionalIcons}"

[Files]
Source: "D:\加班\overwork\*"; DestDir: "{app}"; Flags: ignoreversion recursesubdirs createallsubdirs
; 注意: 不要在任何共享系统文件上使用"Flags: ignoreversion"

[Icons]
Name: "{autoprograms}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"
Name: "{autodesktop}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"; Tasks: desktopicon

[Run]
Filename: "{app}\{#MyAppExeName}"; Description: "{cm:LaunchProgram,{#StringChange(MyAppName, '&', '&&')}}"; Flags: nowait postinstall skipifsilent

```

![[Pasted image 20240910141151.png]]
![[Pasted image 20240910141204.png]]
![[Pasted image 20240910141225.png]]
![[Pasted image 20240910141037.png]]

![[Pasted image 20240910141128.png]]

# 问题汇总
## 加密
1.加密完成后，蜂鸣器没有响

## 验证
1.没有显示最后的“Trying to boot from UART”，芯片无法正常启动
![[Pasted image 20240830171940.png]]
正常应该是下面这样
![[图片1.png]]
## 检测
1.芯片没有发送数据到上位机


## 调试方法
### main uart1 的log
![[Pasted image 20241022150554.png]]
![[Pasted image 20241022145628.png]]

##### 正常的log
```txt
=~=~=~=~=~=~=~=~=~=~=~= MobaXterm log 2024.10.22 14:29:16 =~=~=~=~=~=~=~=~=~=~=~=
0x409031
0x800023
#
# Decrypting extensions..
#
MPK Options:  0x0
MEK Options:  0x0
MPK Opt P1:  0x0
MPK Opt P2:  0x0
MEK Opt   :  0x0
* SMPKH Part 1 BCH code: 0070dd04

* SMPKH Part 2 BCH code: 00085431

* SMPK Hash (part-1,2):

65a50670992a96c271bb6bc7b7d22f7670dea228abbb24e769a9c5a95f78e55c00

092e0e7e87ea84701ef5ff858f9473f82592e1c38c36690b4615f79c1183706400

* SMEK BCH code: 60d5636e

* SMEK Hash: f8667150107ce648c4e5c9ccee87e6cd6a7ead15c90de585275f4a16e1408d204d4383479dc20d17ea18f6078f2e22cee07253f18a138d5d36d859d8b443444e

EXT OTP extension disabled
* BCH code & MSV: fe0fac8b

* KEY CNT: 01010000

* KEY REV: 01010000

SWREV extension disabled

FW CFG REV extension disabled

* KEYWR VERSION:  0x20000

#
# Programming Keys..
#

* MSV: 
[u32] bch + msv:  0x0
Programmed 2/2 rows successfully
[u32] bch + msv:  0x8BAC0FFE

* SWREV: 
[u32] SWREV-SBL:  0x1
[u32] SWREV-SYSFW  :  0x1
SWREV extension disabled
[u32] SWREV-SBL:  0x1
[u32] SWREV-SYSFW  :  0x1

* FW CFG REV: 
[u32] SWREV-FW-CFG-REV:  0x1
SWREV SEC BCFG extension disabled
[u32] SWREV-FW-CFG-REV:  0x1

* EXT OTP: 
EXT OTP extension disabled

* BMPKH, BMEK: 
BMPKH extension disabled
BMEK extension disabled

* SMPKH, SMEK: 
Programmed 11/11 rows successfully
Programmed 2/2 rows successfully
Programmed 11/11 rows successfully
Programmed 2/2 rows successfully
Programmed 11/11 rows successfully
Programmed 2/2 rows successfully

* KEYCNT: 
[u32] keycnt:  0x0
Programmed 2/2 rows successfully
[u32] keycnt:  0x1

* KEYREV: 
[u32] keyrev:  0x0
Programmed 2/2 rows successfully
[u32] keyrev:  0x1

```


##### 不正常的log
```txt
=~=~=~=~=~=~=~=~=~=~=~= MobaXterm log 2024.10.22 14:14:27 =~=~=~=~=~=~=~=~=~=~=~=
0x409031
0x800023
#
# Decrypting extensions..
#
MPK Options:  0x0
MEK Options:  0x0
MPK Opt P1:  0x0
MPK Opt P2:  0x0
MEK Opt   :  0x0
* SMPKH Part 1 BCH code: 0070dd04

* SMPKH Part 2 BCH code: 00085431

* SMPK Hash (part-1,2):

65a50670992a96c271bb6bc7b7d22f7670dea228abbb24e769a9c5a95f78e55c00

092e0e7e87ea84701ef5ff858f9473f82592e1c38c36690b4615f79c1183706400

* SMEK BCH code: 60d5636e

* SMEK Hash: f8667150107ce648c4e5c9ccee87e6cd6a7ead15c90de585275f4a16e1408d204d4383479dc20d17ea18f6078f2e22cee07253f18a138d5d36d859d8b443444e

EXT OTP extension disabled
* BCH code & MSV: fe0fac8b

* KEY CNT: 01010000

* KEY REV: 01010000

SWREV extension disabled

FW CFG REV extension disabled

* KEYWR VERSION:  0x20000

#
# Programming Keys..
#

* MSV: 
[u32] bch + msv:  0x0
Error in programming MSV
debug_response:  0x2000000
[u32] bch + msv:  0x0

* SWREV: 
[u32] SWREV-SBL:  0x1
[u32] SWREV-SYSFW  :  0x1
SWREV extension disabled
[u32] SWREV-SBL:  0x1
[u32] SWREV-SYSFW  :  0x1

* FW CFG REV: 
[u32] SWREV-FW-CFG-REV:  0x1
SWREV SEC BCFG extension disabled
[u32] SWREV-FW-CFG-REV:  0x1

* EXT OTP: 
EXT OTP extension disabled

* BMPKH, BMEK: 
BMPKH extension disabled
BMEK extension disabled

* SMPKH, SMEK: 
Error in programming SMPKH part 1
debug_response:  0x2010000
```