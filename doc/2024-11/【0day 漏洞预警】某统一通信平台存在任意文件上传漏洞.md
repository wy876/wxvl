#  【0day 漏洞预警】某统一通信平台存在任意文件上传漏洞   
原创 xiachuchunmo  银遁安全团队   2024-11-18 23:01  
  
**需要EDU SRC邀请码的师傅可以私聊后台，免费赠送EDU SRC邀请码（邀请码管够）**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**漏洞简介**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**某****统一通信平台是一个涵盖了多种通信功能的综合平台，通常包括文字、语音、视频通讯等功能，并且可能提供了一系列的通讯工具和服务。这样的平台通常旨在提升用户的沟通效率和便利性，为用户提供一个统一的通信环境。某统一通信平台存在任意文件上传漏洞，未经授权的攻击者可通过该漏洞获取服务器权限。**  
****  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**资产详情**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
```
title="网动统一通信平台(Active UC)"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5icd6PF3njeDM16qOvT3wYXluIVbF7cw9VvHxXCZDA0jribI8wGRtmInVhv4ibuyYJhww8KXUez2hI9A/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5icd6PF3njeDM16qOvT3wYXltpvWbClpxlMTzlvSdNk1ugvJ28AmHayB0sicTvRVgiaDCRsDADqibXVUA/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**漏洞复现**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**上传文件测试**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5icd6PF3njeDM16qOvT3wYXlQNsoEJSW0ZjgOU8NK88EJW2lCicIu8Kg3lFpnbAYdV3qx2QfhbExQ9g/640?wx_fmt=png&from=appmsg "")  
  
**访问上传文件测试**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5icd6PF3njeDM16qOvT3wYXl6CmG1GqEpqP2N50ZFtoNAjy5jM2J8zdol2wWic86LfNlAeA4Df2P4OA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**Nuclei测试**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx59l5tmRaic1icJ8rTpS7iaibXsem4QVIKr4FH0dFjvBeEXhR7zjofDsBvvxKeBqRia2Pc85ytIibjNBiax4g/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**圈子介绍**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
高  
质量漏洞利用工具分享社区，每天至少更新一个0Day/Nday/1day及对应漏洞的批量利用工具，团队内部POC分享，星球不定时更新内外网攻防渗透技巧以及最新学习研究成果等。  
如果师傅还是新手，内部的交流群有很多行业老师傅可以为你解答学习上的疑惑，内部分享的POC可以助你在Edu和补天等平台获得一定的排名。  
如果师傅已经  
是老手了，有一个高质量的LD库也能为你的工作提高极大的效率，实战或者攻防中有需要的Day我们也可以通过自己的途径帮你去寻找。  
  
**【圈子服务】**  
  
1，一年至少365+漏洞Poc及对应漏洞批量利用工具  
  
2，Fofa永久高级会员  
  
3，24HVV内推途径  
  
4，Cmd5解密，各种漏洞利用工具及后续更新，渗透工具、文档资源分享  
  
5，内部漏洞  
库情报分享  
（目前已有2000+  
poc，会定期更新，包括部分未公开0/1day）  
  
6，加入内部微信群，认识更多的行业朋友（群里有100+C**D通用证书师傅），遇到任何技术问题都可以进行快速提问、讨论交流；  
  
  
圈子目前价格为**129元**  
**(交个朋友啦！)**  
，现在星球有近900+位师傅相信并选择加入我们，人数满1000  
**涨价至149**，圈子每天都会更新内容。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/yeJvia5dNx5ibmJXuUnaww610JyW2aMD7XyghxoOEL12QTuKPMtygJ7abCibjickyRUpBPDf52hoXPRu3nWEmjzt5A/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**圈子近期更新LD库**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5ib2O29UeBntrAgS16oXwLZvdELfGVX4sNVxPSiatCrY9NMx1M47giajq1iayjibDHo3axOViaY9L4ovZ5g/640?wx_fmt=png&from=appmsg "")  
  
**每篇文章均有详细且完整WriteUp**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/yeJvia5dNx5ibK7Dv4GdXpIuKGmLtjmiaeQuz0vbaPubOpv3oWAehI3Pr5flA7KQSUWtKIyycZezdAxmic5rpy2tHw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
**星球提供免费F**a**![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5ic3icibHW6nDoAAoX9Spv9mREyEWTD0kRZWwApI2LKZDlqDfFj0Hnbja85ppeKPty1oOKFD80G5iadWQ/640?wx_fmt=png&from=appmsg "")  
**C**D通用证书********一起愉快的刷分**![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5ibz6ibFL4b8QBThc3t0ok0Gb7jsseWcrYsxNbv9qyQ0uhDib7TkUcLIIos2iaYlzL6TcF3ia2Rric1EH1g/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yeJvia5dNx5ickvP43lKosLxs8SB5kCSQQEP05NRM08qqN1YIrU1QF8ILRniaF4Vu2jbHhTypliczTnEuK5TXobk9A/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
**免责声明**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/4yJaCArQwpACMJuBxI11jPgvHCxQZFQxPrt5iaQRibgGl0aIzFo4hDCYcFuyViag6zhuqNEjjeasfMEAy1rkaOahw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/HVNK6rZ71oofHnCicjcYq2y5pSeBUgibJg8K4djZgn6iaWb6NGmqxIhX2oPlRmGe6Yk0xBODwnibFF8XCjxhEV3K7w/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
文章所涉及内容，仅供安全研究与教学之用，由于传播、利用本文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。  
  
