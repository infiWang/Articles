---
title: 彩虹六号围攻 CGI与声音解包教程
date: 2020-03-17 20:15:46
categories: 
  - [游戏]
  - [彩虹六号]
  - [逆向]
tags: [游戏, 彩虹六号, 逆向, 教程, RAD Video Tools, QuickBMS]
---

**包本身和解包后的文件已上传至我的下载站**

[infiDownloadSite/R6SFiles](https://dl.infi.wang/R6SFiles/)

**本教程需要您有一定计算机基础.**

<!-- more -->

下载站也会对一些赛季限定内容进行归档, 让Siege的历史变得Unbreakable[^1]! 

[^1]: R6S立项之初的内部代号

**注意, 为了规避版权风险并减少无谓的破解，我永远不会归档二进制文件.**

# 围攻 -> 突围

## 缘起

*Rainbow Six Siege Operation Shifting Tides

*正在连接到服务器

![R2SI2020 Main Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/Y4S4.R2SI.MainMenu.Week1.jpg)

*通行证加载

!["Welcome to the program. "](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/Y4S4.R2SI.CGI.TheProgram.jpg)

*通行证购买, 通行证关闭

<i class="fas fa-history"></i> 通往国际邀请赛之路 58:32

<blue> <i class="fas fa-arrow-right"> 对战已开始</i> </blue>

![Stadium Match Start](https://1734811051.rsc.cdn77.org/data/images/full/360859/rainbow-six-siege-road-to-si-2020-everything-you-need-to-know.jpg)

{% meting "000zIdVW2duNL8" "tencent" "song" "autoplay:true" %}

~~当时听到这段BGM的时候差点没Ban对干员, 实在太ao听了~~

活动开始几天后发生了什么大家都知道, 后来电脑显卡爆炸没有BGM听我要死了

所以我就开始研究怎么解包力

## 解包

R6S的exe(PE)也许会有部分逻辑, 但太大了没必要. ~~而且通过观察黑盒判断黑盒内部逻辑不挺好玩的吗~~

R6S使用了Ubi内部的AnvilNext2.0引擎, forge文件还和其它Anvil游戏不太一样(好像FH也这个德性?) 所以材质、模型之类的东西我现在就不尝试了. (也许高考完会试试)

那剩下的只有CGI和声音了. 

### CGI

**Ubi没有把声音打在bik里, 要么准备解pck声音包要么就享受无声世界罢.**

![Situation m21 Intro](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Player.m21_intro.png)

![R2SI2020 CGI](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Player.y4s4_overview.png)

![Bik Info](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Menu.Fileinfo.png)

只要你曾经看过拆迁的文件就会发现所有CGI其实都装在RAD Video Tools打包的.bik视频文件里, 没有加密, 具体编码封装工具据RAD报告称是Bink 2. 这样的bik文件去[RAD官网](http://www.radgametools.com)下一套[工具](http://www.radgametools.com/bnkdown.htm)就能打开. Outbreak TTS时我就是靠这个工具提前12小时看到了不同任务的CG. 

![RAD Main Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Menu.Main.png)

RAD Video Tools用来解包其实相当简单, 一般来说打开Tools后选中想解的CG点"Convert to……"按钮, 选择输出格式就好了. 视频只能输出avi, 编码选未压缩. 不过未压缩会相当大, 如果要分享最好输出exe, 选项在大小和源文件几乎一致. (我猜是把一个player和bik打一起了)

![RAD Convert Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Menu.Convert.png)

![RAD Convert Select Codec](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Menu.SetCodec.png)

![RAD Converting](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Convert.Processing.png)

![RAD Converted](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Convert.Done.png)

![RAD Advanced Play Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Menu.AdvancedPlay.png)

什么, 你问其它avi编码器? 在, 看看关于? 上世纪的编码器您敢用吗? 即便用了输出也会有一堆问题, 是FFmpeg重新编码不香吗? 但是这个我就不赘述了. 

![RAD Grandpa Codec](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.GrandpaCodec.png)

不过R2SI 2020的Intro CGI是个例外, 即便输出raw avi也会出现诡异的时间轴和采样问题. 但输出图片没有问题……

![Weird Ouput](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/Y4S4.R2SI.CGI.RadTransGlitch.jpg)

因此我认为需要一个备用方案, 所以又该FFmpeg出场了. 

#### 让RAD输出图片并使用FFmpeg将图片打包为视频

(我实在没有空间和时间再搞一次R2SI 2020的CGI了, 找了个Y5干员bundle解锁CGI权当示例. 反正操作过程一样的

首先设置RAD的输出为png, 选好输出目录与输出名字. 文件将自动按帧输出, 并在扩展名前带上帧数号. 

![RAD Pic Output List](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RAD.Output.Pics.png)

假定你已经配置好了FFmpeg, 可以使用终端调用. 无需配置到PATH. 

下面便是是成品. 注意, 为了能塞进CDN, 我把成品转成了gif并做了裁剪. 

![y5s1_opsunlock_bundle_dynamic.part](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/y5s1_opsunlock_bundle_dynamic.part.gif)