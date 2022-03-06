# ORM diesel

## note: LINK : fatal error LNK1181: cannot open input file 'libpq.lib'

```text
Adding the folder to the PATH variable didn't help, at least in my case, as by some reason it is not used in the /LIBPATH parameter passed to link.exe. In my case it was C:\Users\<username>\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\x86_64-pc-windows-msvc\lib You can see it in the beginning of the error message. Copy libpq.lib in there and it will be used from there.

After installation diesel would require some other assemblies. Copy libcrypto-1_1-x64.dll, libiconv-2.dll and libssl-1_1-x64.dll into the folder showed after where diesel command execution
```
