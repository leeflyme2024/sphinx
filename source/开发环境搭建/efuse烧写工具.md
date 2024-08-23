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

pyinstaller --onefile efuse.py
pyinstaller --onefile parse_uart_boot_socid.py

phython文件打包完成后的exe放在这个目录:
C:\Users\Administrator\dist\efuse.exe
C:\Users\Administrator\dist\parse_uart_boot_socid.exe
```

# 单文件制作
![[Pasted image 20240823132413.png]]
```HTML
https://mp.weixin.qq.com/s/Y17usEY99GeYvRmTAyVK5g
https://url27.ctfile.com/f/43907027-1015033222-fd71d3?p=4261
http://bbs.wuyou.net/forum.php?mod=viewthread&tid=419412&extra=&page=1
```


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