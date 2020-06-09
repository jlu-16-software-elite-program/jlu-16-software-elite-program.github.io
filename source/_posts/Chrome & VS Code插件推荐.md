---
title: Chrome & VS Code插件推荐
date: 2020-02-18 23:33:51
tags:
- Chrome
- VS Code
---

# Chrome & VS Code插件推荐

> 推荐几个自己用的插件
>
> 获取地址：如果能访问外网，Chrome Web Store直接搜；如果不能，好像有个[Chrome中文插件网](https://chromecj.com/)，在这装谷歌访问助手。
>
> [插件英雄榜](https://github.com/zhaoolee/ChromeAppHeroes)github上13k的一个项目，推荐了一些好的插件
>
> VS Code插件主要是根据自己写什么语言的code来配

---



## Chrome插件

![image-20200609182552382](https://raw.githubusercontent.com/JLUtangchuan/picBed/dev/image-20200609182552382.png)

### 1. Tampermonkey

> 油猴号称最强的浏览器插件绝非浪得虚名，一个油猴抵得上数十个一般插件也并不是在开玩笑.

#### 油猴上推荐的插件

> 安装完油猴后，点击油猴图标，点获取脚本，会显示几个获取脚本的途径，本人常用**GreasyFork**；由于目前自己电脑重装了一次，插件不多了

1. **懒人专用，全网VIP视频免费破解去广告、全网音乐直接下载、百度网盘直接下载、知乎视频下载等多合一版**（看着下载量最多就下了）

   

2. 百度网盘直接下载插件（自己搜，看哪个最近更新了装哪个）

3. 百度网盘提取码插件（同上）

### 2. Adblock

> 防一刀999，但是知乎好像能检测出来，可以单独禁掉

### 3. SmallPDF

> Small PDF 还有一个 ConvertIO，格式转换的在线工具

### 4. 单词发现者

> 今天刚在github上发现的，能把网页上的**低频词汇高亮**，感觉对学习英语有帮助。但是由于其本身的查词是页面跳转的查词，不是很方便，所以我关掉了它的弹出菜单，而是配合下面的沙拉查词。

### 5. 沙拉查词

> 能显示多个词典的词义
>
> **相应非常快**，比有道的相应还要快
>
> 界面自定义丰富
>
> 可以设置使本地pdf文件也能在Chrome上浏览查词
>
> 能加入**生词本**，好像可以配合扇贝单词以及anki（之前一直想搞这个anki+kindle的生词本，但是苦于电脑端的生词不知道怎么一起同步）

### 6. 远方New tab

> 启动页，美观一些
>
> 但是原始的启动页其实看久了也还行

### 7. IDM for Chrome

> IDM感觉下载速度比普通下载快，可能是IDM多线程多任务做得好。
>
> 由于IDM是付费软件，不想付钱就得破解
>
> 装IDM应该哔哩哔哩上有教学视频

---

## VS Code插件

### 1. Chinese

### 2. Windows opacity

可以让VS Code变得透明（透明度在setting.json中可调）

### 3. Markdown All in One

Ctrl + Shift + V  切换预览\编辑

但目前我写MD用Typora，一个本地程序，优点是可以设置自动将复制的图片保存在指定位置，缺点是本地，如果发布博客插图有点麻烦。

### 4. Python

目前的Python支持Notebook，直接在VS Code中写ipynb挺方便的

### 5. autoDocstring

培养个人的写注释习惯，Tab切下一个[  ] 。但好像只针对Python

```python
def make_data(data1,data2,dicts):
    """[summary]
    
    Arguments:
        data1 {[type]} -- [description]
        data2 {[type]} -- [description]
        dicts {[type]} -- [description]
    
    Returns:
        [type] -- [description]
    """
```

### 6.  GitLens

强烈推荐！！！对于自己这个git小白，记git上的所有命令的代码很难，这个插件可以在VScode中帮助我们图形化管理我们的项目，而且能看到我们的每行代码是什么时候提交的，谁写的，也方便比较不同版本的code

### 7. vscode-icons

美化不同后缀的文件图标

### 8. LeetCode

在VS Code中直接做题，Submit Test直接点就可以，很方便；而且这里面对题按照难度和数据结构都分好类。

### 9. indent-rainbow

美化编辑器

### 10. background

如果觉得VS Code界面太单调，你可以用这个插件在VS Code中插张png图，透明度，位置大小均可调。

