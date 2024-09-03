# 传递参数

[Command line](https://teratermproject.github.io/manual/5/en/macro/commandline.html)
## xmodesend.bat
```bat
REM @echo off
set script_dir=%~dp0
set CURRENT_DIR=%script_dir:~0,-1%

cd "%CURRENT_DIR%"
ttpmacro.exe xmodesend.ttl 5
```

## xmodesend.ttl
```ttl
com_port ='/C='
strconcat com_port param2
connect com_port 

; 显示当前工作目录
dispstr "串口号："
dispstr com_port
dispstr "。"

pause 10
disconnect
closett
```

# 获取当前目录
```ttl
; 获取当前工作目录
getdir current_dir

; 显示当前工作目录
; dispstr "当前工作目录是："
; dispstr current_dir
; dispstr "。"

; 构建文件的完整路径
makepath file_path current_dir 'efuse_image\efuse_flash\tiboot3.bin'

; 显示完整文件路径
; dispstr "文件路径是："
; dispstr file_path
; dispstr "。"

; 发送文件
xmodemsend file_path 1

; 断开连接
disconnect

; 关闭Teraterm窗口
closett
```

# 综合应用
## config.txt
```txt
uart1:7
uart2:4
uart3:6
uart4:5
```

## efuse_check.bat
```bat
REM @echo off
set script_dir=%~dp0
set CURRENT_DIR=%script_dir:~0,-1%

@echo off
echo script_dir is: %script_dir%
echo CURRENT_DIR: %CURRENT_DIR%
echo The first parameter is: %1

start /b call "%CURRENT_DIR%\teraterm\xmodesend.bat" %1
```


## xmodesend.bat
```bat
REM @echo off
set script_dir=%~dp0
set CURRENT_DIR=%script_dir:~0,-1%

echo The first parameter1 is: %1



@echo off
setlocal enabledelayedexpansion

:: 将第一个参数与文件名拼接作为config.txt的完整路径
set CONFIG_PATH="%1\config.txt"

echo CONFIG_PATH is  %CONFIG_PATH%

:: 定义变量存储结果
set uart_number=

:: 使用设置的路径逐行读取config.txt文件
for /f "tokens=1,2* delims=:" %%a in ('type "%CONFIG_PATH%"') do (
    if /i "%%a"=="uart2" (
        :: 提取并存储序号到变量
        set uart_number="%%b"
        goto printresult
    )
)

:printresult
:: 打印结果
echo The number for uart2 is: %uart_number%


cd "%CURRENT_DIR%"
del "socid.txt"
ttpmacro.exe xmodesend.ttl %uart_number% %1 
python oddhex.py
parse_uart_boot_socid.exe socid.txt
```

## xmodesend.ttl
```ttl
com_port ='/C='
strconcat com_port param2
connect com_port 


; 显示当前工作目录
dispstr "串口号："
dispstr param2
dispstr "。"


; 显示当前工作目录
dispstr "工作目录："
dispstr param3
dispstr "。"



getdir curdir
changedir curdir
logopen "socid.txt" 0 0 1
wait '0'
mpause 100
logclose
disconnect
closett
```

# 参考
[MACRO Help Index](https://teratermproject.github.io/manual/5/en/macro/)
[TTL command reference](https://teratermproject.github.io/manual/5/en/macro/command/index.html)
