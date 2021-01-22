# 目录

- [团队介绍](#团队介绍)
- [《历史书》](#历史书)
- [DVM和JVM虚拟机](#虚拟机)
- [游戏性能和报错](#游戏性能)
- [目前情况和未来方向](#情况)

### <span id="团队介绍">团队介绍</span>

AOF其实是 After Our Friday 的缩写，翻译过来的意思是“放假后”开发团队（然而不幸的是，after friday并不一定是放假😂）

[ **团队主页** ](https://github.com/AOF-Dev)

 **团队成员有：  **

- 龙俊宇（发起人，MCinabox的主要作者，贴吧ID天使程序员）  
- JVM（Boat的主要作者）  
- 風
- lambda  
- MCredbear
- ShirosakiMio（澪）
- tsaltedfishking
- （开发群里几位没什么存在感的修改过Boat或者MCinabox的开发者）  

还有几位github上的国外老哥（有从我们的“竞争对手” [khanhduytran0](https://github.com/khanhduytran0)的[pojav](https://github.com/PojavLauncherTeam)团队那找来的人）

### <span id="历史书">《历史书》</span>

&emsp;&emsp;时间可以追溯到2014年，当时，美籍华裔的张卓为做出了应该是第一个在安卓手机上运行JAVA版MC的软件——BoardWalk。  
&emsp;&emsp;卓伟的BoardWalk有两个版本，一个是岷叔曾经做过视频的用[DVM](#虚拟机)虚拟机的版本，一个是用[JVM](#虚拟机)虚拟机的版本（版本号为1.9）。 [(两者的区别)](#虚拟机)  
&emsp;&emsp;当时的BoardWalk并不稳定，而且不开源，原作者张卓为也在互联网上销声匿迹了好几年，为了获得更好的游戏体验，贴吧里掀起了逆向魔改BoardWalk热潮。正是这个契机，我们的龙俊宇同学激发了学习编程的念头。  
&emsp;&emsp;龙俊宇一边学习JAVA、Smali，一边逆向BoardWalk和外网的khanhduytran0制作的另一个修改版——MinecraftLauncher。在张卓伟开源BoardWalk前，龙俊宇做出了用于安卓6及以下的BoardWalk ice，和用于安卓6以上的MinecraftLauncher icesty（在这之前还有零零碎碎的很多版本）。因为是逆向工程的产品，出于版权原因，这两个版本都只在BoardWalk吧的qq群里发布过（不过还是被人外传了）。  
&emsp;&emsp;2019年，BoardWalk开源了。JVM修改其源码并自行编译JVM，做出了Boat。由于JVM的手机是32位处理器的，他只编译了32位的JVM。之后，龙俊宇和JVM还有其他几名开发者开始共同开发MCinabox。由于JVM的手机是32位以及他的电脑磁盘空间不足（最主要还是因为他想摸鱼），64位的JVM拖了很久才在JVM的帮助下由龙俊宇编译出来。  

### <span id="虚拟机">DVM和JVM虚拟机</span>

 **JVM** 虚拟机就是我们在电脑上常用的那个JAVA虚拟机。这个虚拟机理论上能让我们完整、正常地体验Minecraft（可以使用Forge，Fabric之类的），但编译一个用于安卓系统的JVM是一件非常麻烦的事情。  


 **DVM** 虚拟机是谷歌专门为安卓系统做的JAVA虚拟机，几乎所有的安卓应用都要用它来运行。它不能直接运行.jar，它能运行的是经过dx处理的.dex。Minecraft的.jar文件除了要经过dx的处理还要再经过jar2jar的处理才能够运行。另外还因为它加载class文件的方式，如果想用DVM来运行带Mod的Minecraft，不仅Minecraft的.jar要改，Mod的.jar也要修改，这些修改步骤并没有自动脚本能完成。早期的BoardWalk（不是1.9版本的）使用的就是DVM虚拟机，它之所以安装一个版本的Minecraft非常慢不仅是因为下载速度慢，还因为执行dx的计算量是非常大的，在手机上要执行很长时间。  


尽管Minecraft是持续更新的，dx却早早地停更了。就我自己的测试，1.9及以上版本的Minecraft都无法被dx成功处理了，这也是BoardWalk无法运行更新版本的Minecraft的原因之一。  

### <span id="游戏性能">游戏性能和报错</span>

#### 图形库

首先要提到的是Minecraft的图形库—— **OpenGL**   


而使用Arm处理器的安卓手机所支持的图形库是它的近亲—— **OpenGL ES**  

尽管是近亲，这两者完全不能互相兼容。得把OpenGL转化为OpenGL ES，这就要用到[gl4es](https://github.com/ptitSeb/gl4es)
了。  

但gl4es不是完美的，一是它只支持转换版本很旧的OpenGL 2.0和1.5，大大限制了Minecraft的帧数；二是它还有很多bug，比如Forge在启动游戏时那个的敲铁砧的动画会导致游戏崩溃、调整minmap会把游戏画面渲染得奇奇怪怪……有人说什么黑屏之类的基本上也是拜它所赐。  

#### JAVA虚拟机

不管是DVM还是JVM虚拟机，都是版本越高性能越好。（当然DVM虚拟机的版本取决于你的安卓系统版本）  


横向比较的话，我个人认为DVM的性能更好，因为我曾经用khanhduytran0的pojavlauncher在骁龙855上以一百多帧玩Minecraft。但现在据说khanhduytran0的团队经过讨论认为还是JVM虚拟机的性能更好，他们将改为使用openjdk9mobile。  

#### 报错提交  

我们当然希望大家提交报错是在github的issue里提交，并且附带上日志文件。但现实是大部分人都不会用github而且在qq群里瞎叫唤。  

### <span id="情况">目前情况和未来方向</span>  

- 我们正在想办法让MCinabox能够自动安装Fabric
- 当然同时还要尽可能适配某些Mod（不过浏览器Mod是不可能了，在安卓系统下这个东西完全不可能运行）
- 我们的“竞争对手” khanhduytran0和他的团队现在的开发重心在[pojavlauncher_iOS](https://github.com/PojavLauncherTeam/PojavLauncher_iOS)
  上，是的你没听错，在iOS上玩JAVA版Minecraft

