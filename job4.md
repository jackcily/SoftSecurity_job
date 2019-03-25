> 4、通过调试器监控计算器程序的运行，每当运行结果为666时，就改为999。
> 提示：找到运行结果在内存中保存的地址。监控 “=” 按键消息等。

##### 实验环境

- virtualbox + win7 （32位）+windbg（32位）+calc.exe(32位)

##### 操作步骤

- 首先配置windbg的符号表

  首先打开windbg，`File - symbol file path`,配置内容为 `.sympath cache*c:\MySymbols;srv*https://msdl.microsoft.com/download/symbols`，勾选reload框，保存。

  然后在命令行输入指令`.load /f /i` ，强制加载符号表。

- 首先在虚拟机中打开windbg，然后在windbg中 `File - Open Execuatble` ，打开calc.exe

- 新建一个文本文件,,命名为`1.txt`，并存入以下内容

  ```bash
  #新定义一个别名为name 将 poi（esp+0x8）处的 unicode字符串取出
  as /mu ${/v:name} poi(esp+0x8)  
  
  #进行字符串比较 如果name和666相同 就改为 999 否则输出name
  .if($scmp(@"${name}","666")==0){ezu poi(esp+0x8) "999";}.else{.echo ${name};}
  
  #程序继续运行
  g
  ```

  

- 在windbg中输入指令 `bp SetWindowTextW "$<C:\\1.txt"`,执行1.txt文件中的脚本。当计算器中显示为666时，被修改为999。

  结果如下

  ![1](https://github.com/jackcily/SoftSecurity_job/raw/master/img/1.gif)

  

##### 问题

- 刚开始使用eu修改字符串，出现了栈溢出的情况。

  eu存入的是不以0结尾的uniode字符串，改为ezu。

- 直接使用一串指令不行

  修改成脚本

- 还未实现监控等号

- [ ] 参考资料

- [e, ea, eb, ed, eD, ef, ep, eq, eu, ew, eza (Enter Values)](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/e--ea--eb--ed--ed--ef--ep--eq--eu--ew--eza--ezu--enter-values-)

- [WinDBG conditional breakpoint with a unicode string ](https://social.msdn.microsoft.com/Forums/en-US/f998bf93-026d-4803-a067-49f98dbc4e03/windbg-conditional-breakpoint-with-a-unicode-string?forum=windbg)

- [windbg 脚本简单入门](https://bbs.pediy.com/thread-180879.htm)

- [as, aS (Set Alias](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/as--as--set-alias-)

- [Conditional breakpoints in WinDbg and other Windows debuggers](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-a-conditional-breakpoint)

- [MASM Numbers and Operators](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/masm-numbers-and-operators)





