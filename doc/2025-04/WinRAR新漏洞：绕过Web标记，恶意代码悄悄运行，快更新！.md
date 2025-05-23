#  WinRAR新漏洞：绕过Web标记，恶意代码悄悄运行，快更新！   
 一个不正经的黑客   2025-04-05 10:29  
  
![CVE-2025-31334](https://mmbiz.qpic.cn/mmbiz_jpg/cxf9lzscpMqTMNAyhBiaUuevLF7Qb5beN7dIUaxwXV6fzTv5AfblQln9rn3kyycqQIky8Fl9kialRTHQ4n5CjI8g/640?wx_fmt=other&from=appmsg "")  
  
WinRAR新漏洞：绕过Web标记，恶意代码悄悄运行，快更新！  
  
你知道吗？你每天都在用的WinRAR——这款全球超过5亿人都在用的文件压缩工具，最近爆出了一个不小的漏洞，可能会让黑客绕过Windows的安全警告，偷偷在你的电脑上运行恶意代码！  
  
简直像是给恶意软件开了一扇后门，心里想想都觉得毛骨悚然。  
  
这个漏洞被命名为 CVE-2025-31334  
，是一种名为“Web标记绕过  
”（Mark of the Web，简称MotW）的漏洞，主要影响WinRAR 7.11之前的版本。  
  
由于这个漏洞可能被恶意利用，它的CVSS（公共漏洞评分）高达6.8，意味着它的威胁不小。所以，如果你还在用旧版本的WinRAR，那可得抓紧时间更新了！  
### 漏洞是怎么回事？  
###   
  
想知道这个漏洞到底是什么原理吗？  
  
其实很简单——这就涉及到符号链接  
（symlink）。符号链接是一个指向文件或文件夹的快捷方式，类似于Windows里的快捷方式。  
  
攻击者可以利用WinRAR处理符号链接时的漏洞，巧妙地将恶意文件藏在压缩包里，诱使你在没有任何警告的情况下，执行恶意代码。  
  
正常情况下，Windows会为从网上下载的文件打上“Web标记”，并在你打开这些文件时给出警告：  
小心，来自网络的文件可能有风险！  
  
但是，这个漏洞就是通过绕过这个Web标记的安全检查，偷偷让恶意代码运行，从而让你毫无察觉。  
###   
### 怎么利用这个漏洞？  
###   
  
设想一下这样的场景：你收到一个看似无害的压缩包，打开一看，里面有各种各样的文件。  
  
可是，你没有注意到，这个压缩包里藏着一个恶意符号链接。  
  
你点击这个链接，WinRAR会毫不留情地解压它，但这个链接其实指向的是一个恶意的可执行文件。结果，文件直接运行了，而你根本没看到任何“Web标记”的警告对话框。  
  
听起来像是科幻小说的情节对吧？但实际上，这个漏洞完全可能发生，且后果严重。  
  
攻击者通过这样的手段，能够做到以下几件事：  
- 悄悄安装恶意软件  
：病毒、勒索软件、间谍软件等，都可以悄无声息地进入你的系统。  
  
- 窃取你的敏感信息  
：个人信息、银行密码、信用卡信息等重要资料有可能被盗走。  
  
- 远程控制你的电脑  
：攻击者通过这个漏洞可以完全控制你的设备，就像你自己的电脑被别人操控一样。  
  
- 破坏你的系统文件  
：最严重的情况，恶意代码可能会直接删除或破坏你电脑的关键系统文件，导致操作系统崩溃。  
  
  
###   
### 幸运的是，修复来了！  
###   
  
但好消息是，WinRAR官方已经发布了更新，解决了这个漏洞。  
  
WinRAR 7.11版本已经修复了这个安全问题。  
  
所以，如果你还在使用旧版WinRAR，赶紧更新到最新版本！这一步可非常重要，它能有效保护你的电脑免受这个漏洞的威胁。  
###   
### 总结：安全意识不可松懈！  
###   
  
总的来说，WinRAR的这个漏洞让我们再次意识到，即使是我们日常使用的工具，也可能暗藏安全隐患。  
  
要时刻保持警觉，及时更新软件，避免随便点击来源不明的文件。毕竟，只有确保软件和操作系统都处于最新状态，我们才能更好地抵御来自网络世界的威胁。  
  
赶紧去更新你的WinRAR吧！别让这个小小的漏洞，成为黑客入侵你电脑的大门！  
  
  
Source:https://securityonline.info/cve-2025-31334-winrar-flaw-enables-mark-of-the-web-bypass-and-arbitrary-code-execution/  
  
