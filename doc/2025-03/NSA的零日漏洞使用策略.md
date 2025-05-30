#  NSA的零日漏洞使用策略   
原创 天御  天御攻防实验室   2025-03-26 15:56  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hPq2VZ0zUBAwZQYIRcMGdob0eTGKx525Ddp9DrwAwWLOGwL1HNIwiayA2mzhHsdiakoCUfBmN7fib078lq2yjXTMg/640?wx_fmt=other "")  
  
# 零日漏洞的“过度崇拜”  
  
我们今天要聊的是“零日漏洞”，以及为什么人们总是对它过度关注——甚至可以说是一种“崇拜”。零日漏洞的关注度远远超出了它的实际价值。  
  
先简单解释一下，零日漏洞是指那些已经被某些人发现，但软件厂商还不知道的漏洞。由于没有补丁或缓解措施，攻击者可以利用它入侵系统、窃取数据，甚至搞破坏。  
  
零日漏洞之所以备受关注，部分原因是当它们被公开时，厂商和安全社区会大力宣传，敦促用户尽快修补。比如Log4j漏洞，所有人都讨论它，就是为了让大家赶紧修复。而那些存在多年的老漏洞，反而没人提了——因为该打补丁的人早就打了。  
## 技术美学与实战差异  
  
最近我参加了一场网络安全攻防研讨会，发现一个有趣的现象：非从业者（比如学者）对零日漏洞特别着迷，而真正的实战人员反而不怎么关心它。  
  
从技术美学角度看，0day确实令人着迷。比如NSO Group某次攻击中，攻击者仅在获得极小初始权限后，就能在漏洞利用链中构建微型虚拟机来逐步突破——这种将两字节权限扩展成完整控制链的技艺堪称奇迹。  
  
但站在国家级网空行动角度看，0day绝非日常首选。以NSA为例，其3万人的规模决定了必须建立稳定可靠的常规渗透体系，不可能事事依赖"魔法武器"。这种精妙但脆弱的攻击链成功率虽高，在大规模行动中仍显不足——更何况频繁使用会增加暴露风险，导致漏洞被修补而失效。  
  
