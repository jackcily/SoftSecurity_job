1. 代码内容如下  保存为1.c文件直接编译链接   `cl  /c  1.c `  ` link  /ENTRY:main  /ALIGN:16 User32.lib 1.obj  `生成1.exe 使用 flexhex软件直接打开1.exe  找到 hello world 修改成 hello cuc

   ![1](https://github.com/jackcily/imge/raw/master/1.PNG)

2. 使用dumpbin反汇编1.exe  `dumpbin  /disasm 1.exe `然后找到 hello world字符串对应的地址 。使用flexhex打开 1.exe文件修改hello world的对应地址到文件末尾，使用flexhex直接在1.exe文件末尾添加字符串到2kb即可。

   ![2](https://github.com/jackcily/imge/raw/master/3.PNG))