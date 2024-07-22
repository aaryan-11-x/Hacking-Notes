Compile exploits for Windows in Linux.
```sh
sudo apt install mingw-w64
# 32-bit compilation
i686-w64-mingw32-gcc 42341.c -o syncbreeze_exploit.exe
# 64-bit compilation
x86_64-w64-mingw32-gcc adduser.c -o adduser.exe

i686-w64-mingw32-gcc 42341.c -o syncbreeze_exploit.exe -lws2_32
```