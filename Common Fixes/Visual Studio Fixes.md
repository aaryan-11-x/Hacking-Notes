1) Nuget Package Re-Targeting
https://stackoverflow.com/questions/36023982/nuget-re-targeting-after-upgrading-from-net-framework-4-5-to-4-6-1


2) NuGet.targets error : '.', hexadecimal value 0x00, is an invalid character after upgrade to .Net Core 2.1.1
```
In Visual Studio, click: TOOLS > NUGET PACKAGE MANAGER > PACKAGE MANAGER SETTINGS
Select Option NUGET PACKAGE MANAGER > GENERAL
In General, click on button CLEAR ALL NUGET CACHE(S) await a few seconds
After this , close all windows and go to TOOLS > NUGET PACKAGE MANAGER > PACKAGE MANAGER CONSOLE
type dotnet restore
done =)
```
