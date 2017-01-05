# Atmel USB CDC Virtual COM driver for Windows 8

These instructions explain how to self-sign Atmel USB CDC Virtual COM driver for use on Windows 8. Windows 10 comes with built-in support for virtual serial ports - no driver installation is required.

## Instructions to self-sign

In Windows Driver Kit 7.1, all commands are located under bin\amd64, except if indicated otherwise below. In Windows Driver Kit 8.1, all commands are located under bin\x64.

Execute:

```powershell
makecert.exe -r -pe  -sv Atmel(Test).pvk -n CN=Atmel(Test) Atmel(Test).cer

pvk2pfx.exe -pvk Atmel(Test).pvk -spc Atmel(Test).cer -pfx Atmel(Test).pfx

# In Windows Driver Kit 7.1 located under bin\selfsign, and in Windows Driver Kit 8.1 located under bin\x86
Inf2cat.exe /driver:. /os:7_x64,7_X86

Signtool.exe sign /v /f Atmel(Test).pfx /n Atmel(Test) /t http://timestamp.verisign.com/scripts/timstamp.dll atmel_devices_cdc.cat

# Need to run this with administrator privilege
certmgr.exe /add Atmel(Test).cer /s /r localMachine root
```

## Instructions to install

Add `Atmel(Test).cer` to local machine using `certmgr.exe` (last command above) or Windows Explorer. In Device Manager, choose `CDC Virtual Com` under `Other devices`. Right click, choose `Update Driver`, choose to install driver manually from known location. Specify path to `atmel_devices_cdc.inf` file.
