---
title: 彩虹六号围攻 CGI与声音解包教程
date: 2020-03-17 20:15:46
categories: 
  - [游戏]
  - [彩虹六号]
  - [逆向]
tags: [游戏, 彩虹六号, 逆向, 教程, RAD Video Tools, QuickBMS]
---

![y5s1_opsunlock_bundle_dynamic](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/R6S.CGI.y5s1_opsunlock_bundle_dynamic.360P10FPS.gif)

**包本身和部分解包后的文件已上传至我的下载站, 持续更新**

[infiDownloadSite/R6SFiles](https://dl.infi.wang/R6SFiles/)

**本教程需要您有一定计算机基础**

<!-- more -->

下载站也会对一些赛季限定内容进行归档, 让Siege的历史变得Unbreakable[^1]! 

[^1]: R6S立项之初的内部代号

**注意, 为了规避版权风险并减少无谓的破解，我永远不会归档二进制文件.**

# Operation Breach Out

## 缘起

*Rainbow Six Siege Operation Shifting Tides

*正在连接到服务器

![R2SI2020 Main Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/R6S.Y4S4.R2SI.MainMenu.Week1.jpg)

*通行证加载

!["Welcome to the program. "](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/R6S.CGI.y4s4_overview.jpg)

*通行证购买, 通行证关闭

<i class="fas fa-history"></i> 通往国际邀请赛之路 58:32

<cyan><i class="fas fa-arrow-right"> 对战已开始</i></cyan>

![Stadium Match Start](https://1734811051.rsc.cdn77.org/data/images/full/360859/rainbow-six-siege-road-to-si-2020-everything-you-need-to-know.jpg)

<video controls="" autoplay="" name="media"><source src="https://dl.infi.wang/R6SFiles/Sounds/Music/Map/Stadium/Theme.ogg" type="audio/ogg"></video>

~~当时听到这段BGM的时候差点没Ban对干员, 实在太ao听了~~

活动开始几天后发生了什么大家都知道. 很不巧, 后来电脑显卡爆炸, 没有BGM听我要死了

所以我就开始研究怎么解包力

## 分析及解包

R6S的exe(PE)也许会有部分逻辑, 但太大了、没必要. ~~而且通过观察黑盒判断黑盒内部逻辑不挺好玩的吗~~

R6S使用了Ubi自研的AnvilNext2.0引擎, 而用于存档的forge文件还和其它Anvil引擎游戏不太一样(好像FH也这个德性?) 所以材质、模型之类的东西我现在就不尝试了. (也许高考完会试试)

那剩下的只有CGI和声音了. 

### CGI

**Ubi没有把声音打在bik里, 要么准备解pck声音包, 要么就享受无声世界罢.**

#### 文件信息

![Situation m21 Intro](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Player.m21_intro.png)

![R2SI2020 CGI](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Player.y4s4_overview.png)

![Bik Info](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Menu.Fileinfo.png)

如果你曾经研究过拆迁的文件结构, 会发现所有CGI其实都存储在RAD Video Tools打包的.bik视频文件里, 而文件本身则位于拆迁目录里的videos文件夹中. 文件没有加密, 具体编码封装工具据RAD VT报告称是Bink 2. 这样的bik文件去[RAD官网](http://www.radgametools.com)下载一套[工具](http://www.radgametools.com/bnkdown.htm)就能打开. 在2018年Outbreak TTS时, 我就是靠这个工具得以提前12小时看到了不同任务的序言CG. 

![RAD Main Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Menu.Main.png)

#### 工具

- RAD Video Tools    由RAD Game Tools开发的专有视频处理工具, 可处理Bink及Smack格式视频    下载地址: [Bink Downloads](http://www.radgametools.com/bnkdown.htm)

#### 使用RAD Video Tools解包

RAD Video Tools用来解包相当简单, 一般来说只需打开RAD VT, 在选中想解包的CG后点击"Convert to……"按钮, 最后选择输出格式、确定输出路径就好了. 如果需要直接转为视频则只能输出avi, 编码也只能选未压缩. 不过众所周知, 未压缩的avi视频相当大, 如果您只是需要分享则最好输出exe, 输出文件大小和源文件几乎一致. (我猜是把一个播放器和bik打包在一起了)

![RAD Convert Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Menu.Convert.png)

![RAD Convert Select Codec](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Menu.SetCodec.png)

![RAD Converting](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Convert.Processing.png)

![RAD Converted](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Convert.Done.png)

![RAD Advanced Play Menu](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Menu.AdvancedPlay.png)

你问能不能用现代的编码器? 醒醒, Bink 1不支持外部编码器, 而现代的Bink 2 SDK是付费、商业用的……

"那其它内置的编码器不能用🐎?" 在, 看看关于? 上世纪的编码器您敢用? 即便使用, 输出的文件也会有一车问题. 是FFmpeg重新编码不香吗? 但这个我就不赘述了. 

![RAD Grandpa Codec](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.GrandpaCodec.png)

不过R2SI 2020的序言CGI是个例外, 即便输出raw avi也会出现诡异的时间轴和采样问题. 但将每一帧输出为图片则没有问题……

![RAD Weird Ouput](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/R6S.CGI.y4s4_overview.RadTransGlitch.png)

鬼知道未来还会不会出现这种问题, 而且方便传播的视频谁不爱呢? 因此我认为需要一个备用方案, 所以该FFmpeg登场了. 

#### 让RAD VT输出每一帧为图片, 并使用FFmpeg将图片编码为视频

(我没有空间和时间再演示一次R2SI 2020的CGI解包打包了, 找了个Y5干员bundle解锁CGI权当示例, 反正操作完全一致

首先通过RAD VT查看.bik视频的帧率(File Info), 要是和整数很接近的小数的话四舍五入到整数并记住它. (这样的帧数其实是个历史包袱, 感兴趣的话可以观看[影视飓风的视频](https://www.bilibili.com/video/BV1kE411c7yZ))

接下来使用RAD VT的转换功能, 设置RAD VT的输出为png以保持无损, 选好输出目录与输出名字, 开始转换. 视频的每一帧将自动输出为png图片, 并在文件名最后自动加上帧数号(帧数号含前缀0). 

![RAD Pic Output List](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/RADVT.Output.Pics.png)

假定你已经配置好了FFmpeg, 可以使用终端/命令行调用. Win下推荐使用静态链接版. 我们只需要ffmpeg/ffmpeg.exe

打开终端/命令行, 执行以下命令: 

```shell

(ffmpeg程序路径) -framerate (源文件帧率) -i (图片位置)/(输出文件名)%(最大帧数位数)d.png -c:v libx264 -preset veryslow -crf 17 -tune film -r (源文件帧率) (输出路径)/(输出文件名, 含扩展名)

```

-framerate指定了输入文件被看待的帧率, -i指定了输入文件, -c:v指定了输出视频的编码器, -crf指定了输出质量, -r指定了输出文件的帧率. 

文件名中的通配符与C/C++中scanf函数的参数一致. 

如有Intel/Nvidia/AMD的可编码视频的GPU, 可将编码器换为h264_qsv/h264_nvenc/h264_amf, 相应地输出文件质量会有所下降. 

-preset 为预设, 赶时间用veryfast/fast, 要质量slower/veryslow. 越快输出文件大小越大质量越差, 反之亦然. 需要其它预设自行查阅FFmpeg手册. 

-crf 取值0-51, 17/18为视觉无损压缩. 如果希望降低输出大小可适当增加, 当然质量会下降. 

-tune 可针对场景调节编码, 优化图像质量. 由于拆迁所有CGI全为写实渲染, 建议选择film. 

默认使用目前最流行的H.264编码, 需要H.265将上述编码器名内264换为265即可. 其它编码如VP9, AV1请自行摸索. 不是所有编码器都支持以上微调! 

建议输出mp4/flv/mkv, 方便后期添加声音. 

根据实际情况替换括号(包括括号本身)的内容. 

以我举的样例为例, 命令为: 

```shell

./ffmpeg.exe -framerate 30 -i ./tutor/y5s1_opsunlock_bundle_dynamic%3d.png -c:v libx264 -crf 17 -r 30 ./y5s1_opsunlock_bundle_dynamic.mp4

```
这里是运行结果: 

![FFmpeg Encoding](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/FFmpeg.Encoding.png)

![FFmpeg Done](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/FFmpeg.Done.png)

只要不出错、不关闭, 成品就会出现在你指定的位置. 

下面便是是成品. 注意, 为了能塞进CDN, 我把成品转为了gif并降低了分辨率和帧率. 

![y5s1_opsunlock_bundle_dynamic](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/R6S.CGI.y5s1_opsunlock_bundle_dynamic.360P10FPS.gif)

如需添加音轨, 再在第一个-i后添加-i并输入音频, 对音频使用copy编码就行了. 其它高级玩法请自行查阅FFmpeg手册. 

### 声音

拆迁的声音使用了Audiokinect Wwise套件, 并被打包在拆迁根目录下的sounddata目录的子目录中, 扩展名为.pck

#### 文件信息

.pck文件有多种命名方式: 

- sound_sfx              存储各种音效, 大多枪声/物件声. 
- sound_sfx_bootstrap    存储赛季/活动音乐(主题曲)、地图音乐与部分UI/氛围音效. 
- sound_sfx_playgo       存储情境、情境/干员CGI音乐与部分UI/氛围音效 
- sound_sfx_maps         存储地图环境音与地图内物件声音. 
- sound_sfx_events       存储活动所使用的特殊音效. 
- sound_sfx_cgi          存储CGI使用的音乐. 
- sound_(语言缩写)        存储对应语言的语音. 
- 及以上格式的组合, 等. 

这里是Y5S1.0文件列表示例: 

![Y5S1.0 Sounddata](https://cdn.infi.wang/pic/blog/R6S.Tutorial-ExtractCgiAndSound/R6S.Y5S1_0.sounddata.png)

将.pck拆开后会发现子文件有四种扩展名. 这里给出对应编码和主要存储内容: 

- .wwise    为Wwise Vorbis RIFF编码的有损声音文件, 几乎什么都存储(例如赛季主题曲, 地图主题音乐, 菜单BGM、按钮音效)
- .lwav     为Wwise IMA ADPCM编码的无损声音文件, 存储部分地图/物件音效
- .at3      为Wwise PCM编码的无损声音文件, 寥寥无几
- .pnk      为包含文件内部名称及部分声音子文件的包  ~~禁止套娃~~

在Y4的某次更新中, .pnk文件不再存储声音的内部名称, 使用现有工具/脚本解当前版本游戏的包, 所得的对应声音的文件名将会变为相应文件在原包中的16位offset地址! 在Y5S3到来前, 这样的情况无法得到改善. 

#### 工具

***以下内容较为硬盒, 不建议普通玩家轻易尝试. 而且由于我时间不足, 以下教程仅为粗略指导, 若有无法复现情况的请认真阅读相应工具的帮助文档, 进行Debug, 并前往相应工具来源论坛求助. 如果未来有时间, 我会对下述内容进行对普通人更友好的完善. ***

Wwise在游戏工业界已经使用多年, 即便官方从未支持解包, 民间工具也已相当完善. 这里列举我所使用的工具. 

下述工具均已上传至我的下载站. [infiDownloadSite/R6SFiles/Tools](https://dl.infi.wang/R6SFiles/Tools/)

##### 手动工具

- QuickBMS  通用文件处理引擎, 为一脚本解释器. 由Luigi Auriemma开发, 开源, 协议未知.  主页: [Luigi Auriemma QuickBMS](https://aluigi.altervista.org/quickbms.htm)
- ww2ogg  Wwise Vorbis RIFF/RIFX编码声音文件转ogg Vorbis声音文件工具, 用于转换.wwise文件.  由hcs(Adam Gashlin)开发, 开源, 使用BSD Clause-3协议.  GitHub仓库: [hcs64/ww2ogg](https://github.com/hcs64/ww2ogg)
- wwise_ima_adpcm  Wwise IMA ADPCM编码声音文件与PCM声音文件互转工具, 用于转换.lwav文件.  由Zwagoth开发, 未找到原始下载源. 
- revorb  ogg Vorbis音频granule_position修复工具, 用于修复ww2ogg输出.  由Yirkha(Jiri Hruska)开发, 开源, 使用MIT协议.  原发布贴: [Can't play vorbis](https://web.archive.org/web/20150619030401/https://hydrogenaud.io/forums/lofiversion/index.php/t64328.html)  源代码: [revorb.cpp](https://web.archive.org/web/20150619030401/http://yirkha.fud.cz/progs/foobar2000/revorb.cpp)
- func_getTYPE.bms  使用启发式算法确定文件类型的QBMS脚本, 是多个QBMS脚本的依赖, 作者为XeNTaX论坛的AlphaTwentyThree. 
- pck_AKPK_extractor.bms  用于解包.pck文件的QBMS脚本, 作者同为XeNTaX论坛的AlphaTwentyThree. 
- bnk_extractor.bms  用于解包.bnk文件的QBMS脚本, 作者依然是XeNTaX论坛的AlphaTwentyThree. 
- wwise_pcm_decoder.bms  Wwise PCM编码声音文件转PCM声音文件QBMS脚本, 用于转换.at3文件.  作者未知. 

##### 自动工具

***One tool to Rule'em all***

- Tom Clancy's Rainbow Six Siege Sound Extractor  R6S声音文件解包工具, 作者为XeNTaX论坛的FatalBulletHit.  开源, 协议未知. 本质为Windows CMD脚本与Powershell脚本, 将上述手动工具解包过程自动化.  原发布贴: [Tom Clancy's Rainbow Six Siege Sound Extractor](http://forum.xentax.com/viewtopic.php?p=140570#p140570)

##### 使用手动工具解包

大多数工具按照相应--help和document进行操作即可. 

##### 使用自动工具解包

从源站下载bat, 执行, 完结撒花(

有以下几个坑: 

- Powershell版本不应低于Powershell 4
- bat脚本其实只是核心Powershell脚本的Bootstrapper, 建议在实际使用时直接从Powershell启动ps1脚本, 否则容易出现像是文件解不出来、解出AudioKinect文件不转换甚至都解好转换完毕了还能删歪来的迷惑行为. 
- 工具默认从注册表读取Uplay版本的安装地址. 如果你用的是Steam / Origin / 其它平台(包括破解版 / 备份), 记得修改脚本中的地址定义变量. 注意符号转义行为! 
- 脚本默认使用Powershell的Test-Connection函数对www.google.com 测试网络连通性, 若不通脚本则会罢工, 而且在国内即便开了小飞机也有可能出现奇妙的问题. 建议全文搜索, 将 www.google.com 替换成国内可ping地址. 我个人使用的是 www.miui.com . 当然, 要是您的网络无法下载脚本所需的相应手动工具, 自然也无法解包. 
- 建议打开-Debug开关, 时刻准备好debug. 
- 只要你在工具内选择解相应格式的包, 所有对应格式的包都会被解包、转换. 对于pck原包来说, 其实工具只是识别并解"游戏文件夹/sounddata/pc"内的所有pck包文件. 如果您只想解某一特定的包, 可以将不想要的文件剪切走; 或者干脆新建个文件结构和游戏目录一致的"假"文件夹, 把想解的包粘贴进去, 再把假游戏文件夹喂给脚本就行了. 
- 原文件内小文件众多, IOPS压力会很大, 解包输出也大的离谱. 建议准备好声音文件大小两倍左右的空间, 放在SSD上跑. 个人解Y4S4时程序在eMMC USB盘上跑了15个小时, 人都等傻了. 

## Happy Hacking! 
