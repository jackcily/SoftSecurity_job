> 实验三 在notepad（32位64位均可）中，输入一段文字。然后使用调试器，在内存中修改这段文字



- 原理

  直接找到notepad字符串在内存中的位置，然后修改字符串

- 首先打开notepad，在文件中输入hhhhhhhh使用notepad依次点击 `File - Attach to a Process` ，使用windbg调试notepad的进程。

  ![0](https://github.com/jackcily/SoftSecurity_job/raw/master/img/30.PNG)

- 使用指令定位在堆中待查找字符串的位置

  ```bash
  #查看所有堆
  !heap -a
  
  #在堆中查找字符串
  s -u 堆起始地址 L000ff000 "待查找的字符串"
  
  #显示某个位置中对应的unicode字符串
  du 0000026f`f608d600
  ```

  ![1](https://github.com/jackcily/SoftSecurity_job/raw/master/img/31.PNG)

  ![2](https://github.com/jackcily/SoftSecurity_job/raw/master/img/32.PNG)

- 对查找到的内存位置进行修改，直到修改成功。

  ```bash
  # 修改字符串
  ezu 0000026f`f608d600 "AAAAAAAA"
  
  # 显示Unicode
  du 0000026f`f608d600
  
  # 继续运行程序
  g
  ```

  修改完成后如下图

  ![3](https://github.com/jackcily/SoftSecurity_job/raw/master/img/33.PNG)

  ![4](https://github.com/jackcily/SoftSecurity_job/raw/master/img/34.PNG)

​    

#### 参阅

- [[jckling](https://github.com/jckling)/**Day-Day-Up**](https://github.com/jckling/Day-Day-Up/tree/sf/%E8%BD%AF%E4%BB%B6%E4%B8%8E%E7%B3%BB%E7%BB%9F%E5%AE%89%E5%85%A8/3)
- [windbg修改notepad内容(!pte/!dd/.process/s命令)](https://blog.csdn.net/lixiangminghate/article/details/53086667)

