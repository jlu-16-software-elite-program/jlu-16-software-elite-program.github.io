---
title: conda创建虚拟环境
date: 2020-02-22 11:39:33
tags:
 - conda
 - Python
---

# Conda 创建虚拟环境

1. 查看目前都有哪些环境

   `conda env list`

2. 建立一个新环境

   `conda create -n xxxx python=3.5`

3. 激活这个环境

   `conda avtivate xxxx`
4. 删除某个环境
   
   `conda remove -n xxxx`
5. 把某个环境设置为默认
   
   修改环境变量

   `C:\ProgramData\Anaconda3\Scripts` -> `C:\ProgramData\Anaconda3\envs\xxxx\Scripts`
   
   `C:\ProgramData\Anaconda3` -> `C:\ProgramData\Anaconda3\envs\xxxx`

6. 清理无用包
   ```
   conda clean -p      //删除没有用的包
   conda clean -t      //tar打包
   ```


