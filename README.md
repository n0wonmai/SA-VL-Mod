# Run with bat script

```
@echo off
color 17
set scriptversion=v1.1
Title Windows 8/8.1/10 ENBSeries 0.313 compatibility script %scriptversion% by Marty McFly, Craank_AG, SacsDeveloper

echo.
echo  .:[ Windows 8/8.1/10 ENBSeries 0.313 startup utility %scriptversion% ]:.
echo           .:[ Authors: Marty McFly, Craank_AG, edit sacs ]:.
echo                 .:[ Exclusive for all enb's ]:.
echo.
REM Gets the SID of the current logged in user, needed for wiping the registry values which cause issues with ENB 0.248

for /f "delims=" %%i in ('wmic useraccount where "name='%UserName%'" get sid /value') do (
  for /f "delims=" %%j in ("%%i") do set "%%j"
)

REM Sets batch file dir as working directory, since batch files ran as admin use system32 by default

pushd %~dp0 >nul 2>&1

REM Checks existance of problematic registry keys and deletes them thereafter, plus giving out proper message.

REG Query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" >nul 2>&1
if %errorlevel%==0 ( 
	REG Delete "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" /f
	echo. [startup util] Registry entry successfully deleted!
) else (
	echo. [startup util] Registry entry not found, skipping. 
) 

REG Query "HKEY_USERS\%SID%\Software\Microsoft\Internet Explorer\LowRegistry\Audio\PolicyConfig\PropertyStore" >nul 2>&1
if %errorlevel%==0 ( 
	REG Delete "HKEY_USERS\%SID%\Software\Microsoft\Internet Explorer\LowRegistry\Audio\PolicyConfig\PropertyStore" /f
	echo. [startup util] Registry entry successfully deleted!
) else (
	echo. [startup util] Registry entry not found, skipping. 
) 

REG Query "HKEY_USERS\%SID%\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" >nul 2>&1
if %errorlevel%==0 ( 
	REG Delete "HKEY_USERS\%SID%\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" /f
	echo. [startup util] Registry entry successfully deleted!
) else (
	echo. [startup util] Registry entry not found, skipping. 
) 
```
