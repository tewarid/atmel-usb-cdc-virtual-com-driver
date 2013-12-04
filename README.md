Instructions to self-sign
-------------------------

Windows Driver Kit 7.1 all command located under \bin\amd64\ except if indicated otherwise. Execute:

makecert.exe -r -pe  -sv Atmel(Test).pvk -n CN=Atmel(Test) Atmel(Test).cer

pvk2pfx.exe -pvk Atmel(Test).pvk -spc Atmel(Test).cer -pfx Atmel(Test).pfx

Inf2cat.exe /driver:. /os:7_x64,7_X86
(Windows Driver Kit 7.1 located under bin\selfsign\)

Signtool.exe sign /v /f Atmel(Test).pfx /n Atmel(Test) /t http://timestamp.verisign.com/scripts/timstamp.dll atmel_devices_cdc.cat

certmgr.exe /add Atmel(Test).cer /s /r localMachine root
(Need to run this with administrator privilege)


Instructions to Install
-----------------------

Add Atmel(Test).cer to local machine using certmgr.exe (last command above) or Windows Explorer. In Device Manager choose CDC Virtual Com under Other devices. Right click, choose Update Driver, choose to install driver manually from known location. Specify path to atmel_devices_cdc.inf file.   
