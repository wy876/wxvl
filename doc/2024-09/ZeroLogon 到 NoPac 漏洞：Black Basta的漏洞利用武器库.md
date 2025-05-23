#  ZeroLogon 到 NoPac 漏洞：Black Basta的漏洞利用武器库   
 安全客   2024-09-23 14:43  
  
*****  
  
在网络安全领域，漏洞利用的演变与黑客组织的攻击手段息息相关。近年来，ZeroLogon（CVE-2020-1472）与 NoPac（CVE-2021-36761）漏洞的利用引起了广泛关注，尤其是由黑客组织 Black Basta的攻击活动。自 2022 年 4 月以来，Black Basta 勒索软件以其双重勒索策略，造成了各行业的重大破坏，成为当前网络安全的重大威胁。  
  
  
Qualys 发布了一份综合报告，揭示了 Black Basta 勒索软件，该组织在勒索软件即服务 （RaaS） 模式下运营，以其双重勒索计划而闻名，受害者不仅面临为恢复加密数据而支付赎金的要求，还受到第二重威胁——若不支付赎金，敏感信息将被发布。  
  
  
Black Basta 常被与另一著名勒索团伙 Conti 相提并论，因其操作战术具有相似性。根据 Qualys 的详细描述，Black Basta 已利用其方法攻击了全球超过 500 个组织，涵盖了关键基础设施、医疗保健和金融服务等行业，波及北美、欧洲和澳大利亚。  
  
  
该组织通过常见方法在受害者网络中获得初步访问权限，包括网络钓鱼、Qakbot 感染、Cobalt Strike 信标和利用已知漏洞。其中包括 ZeroLogon（CVE-2020-1472）、PrintNightmare（CVE-2021-34527）和 NoPac（CVE-2021-42287）等，这些漏洞使攻击者能够获得管理权限并保持持久性。  
  
  
  
ZeroLogon 漏洞允许攻击者在未授权的情况下重置 Windows 域控制器的密码，进而获取域管理员权限。这使得攻击者能够控制整个网络环境，造成广泛影响。攻击者通过发送特制的 Netlogon 消息，绕过正常的身份验证过程。虽然 Microsoft 已发布补丁，但许多组织未能及时更新，导致该漏洞长期存在。  
  
  
NoPac 漏洞同样涉及 Windows Netlogon 认证过程，允许未授权攻击者进行身份验证，获得对域控制器的访问权限。与 ZeroLogon 一样，许多组织未能有效防护，导致数据泄露和资产损失。NoPac 的攻击依赖于身份验证机制的缺陷，攻击者可以伪造请求获取敏感信息，增加了网络防护的难度。  
  
  
一旦进入网络，Black Basta 就会使用 Mimikatz 等工具收集凭据，通过 PowerShell 进行远程执行，并利用其他合法软件来隐蔽地融入网络环境。这一侦察阶段对于在部署勒索软件有效负载之前识别高价值目标至关重要。  
  
  
值得注意的是，报告暗示 Black Basta 与 FIN7 组织之间可能存在联系，后者是一个以经济动机驱动的犯罪集团。两者均倾向于使用旨在绕过检测系统的复杂工具包，包括 Cobalt Strike 和 SystemBC。Black Basta 还善于使用 RClone 和 WinSCP 等远程管理软件在加密之前窃取关键数据。  
  
  
Black Basta 攻击的一个显著特征是使用 ChaCha20 加密算法，该算法因其速度和强度而广为认可。该密钥使用 RSA-4096 进一步保护，令受害者在没有攻击者合作的情况下几乎无法解密。受害者发现其文件被加密，并在系统中留下“readme.txt”或“instructions_read_me.txt”等威胁性赎金说明，提供如何谈判支付的指示。  
  
  
ZeroLogon 和 NoPac 漏洞的曝光不仅是单一事件，更是企业在网络安全防护方面深层次缺陷的缩影。随着 Black Basta 及其他黑客组织不断创新其攻击手段，企业需要意识到网络安全不仅仅是技术问题，更是整体管理和体制的问题。  
  
  
为了有效应对当前和未来的威胁，企业必须实施全面的安全战略，包括定期风险评估、持续的员工培训和健全的应急响应计划。这些措施不仅可以减少数据泄露和经济损失的风险，更能够重建客户和公众的信任。最重要的是，企业需要从根本上培养一种安全优先的文化，将网络安全视为整个组织的共同责任。  
  
  
在这个快速变化的网络环境中，保持警惕和灵活应变的能力将是企业存活与繁荣的关键。只有通过不断学习与适应，才能在这场网络安全的持久战中获得主动权，确保信息资产的安全与完整。  
  
  
文章参考：  
  
https://securityonline.info/zerologon-to-nopac-vulnerability-black-basta-groups-exploit-arsenal-revealed/  
  
  
**推荐阅读**  
  
  
  
  
  
<table><tbody><tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:10.classicTable1:0"><td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:10.classicTable1:0.td@@0" style="border-color: rgb(62, 62, 62);border-style: none;padding: 0px;" width="100.0000%"><section><section style="display: flex;flex-flow: row;margin-top: 10px;margin-right: 0%;margin-left: 0%;justify-content: flex-start;"><section style="display: inline-block;vertical-align: middle;width: auto;min-width: 10%;height: auto;flex: 0 0 auto;align-self: center;box-shadow: rgb(0, 0, 0) 0px 0px 0px;"><section style="font-size: 14px;color: rgb(5, 193, 183);line-height: 1;letter-spacing: 0px;text-align: center;"><p><strong>01</strong></p></section></section><section style="display: inline-block;vertical-align: middle;width: auto;flex: 100 100 0%;align-self: center;height: auto;"><section style="font-size: 14px;letter-spacing: 1px;line-height: 1.8;color: rgb(140, 140, 140);"><p style="text-wrap: wrap;"><span style="color: rgb(224, 224, 224);">｜</span><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzA5ODA0NDE2MA==&amp;mid=2649786915&amp;idx=1&amp;sn=e4c2e82b77e9944412dc67d88c93313e&amp;chksm=8893ba4cbfe4335af8353e819d8c708841db4fa89ea7c5453f662f3d490b2081d951396aea6f&amp;scene=21#wechat_redirect" textvalue="Cybertruck电动皮卡车被称远程禁用" linktype="text" imgurl="" imgdata="null" data-itemshowtype="0" tab="innerlink" data-linktype="2"><span style="font-size: 12px;">Cybertruck电动皮卡车被称远程禁用</span></a><span style="font-size: 12px;"></span></p></section></section></section><section style="margin: 5px 0%;"><section style="background-color: rgb(224, 224, 224);height: 1px;"><svg viewBox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section></section></section></td></tr><tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:10.classicTable1:1"><td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:10.classicTable1:1.td@@0" style="border-color: rgb(62, 62, 62);border-style: none;padding: 0px;" width="100.0000%"><section><section style="display: flex;flex-flow: row;margin-top: 10px;margin-right: 0%;margin-left: 0%;justify-content: flex-start;"><section style="display: inline-block;vertical-align: middle;width: auto;min-width: 10%;height: auto;flex: 0 0 auto;align-self: center;"><section style="font-size: 14px;color: rgb(5, 193, 183);line-height: 1;letter-spacing: 0px;text-align: center;"><p><strong>02</strong></p></section></section><section style="display: inline-block;vertical-align: middle;width: auto;flex: 100 100 0%;align-self: center;height: auto;"><section style="font-size: 14px;letter-spacing: 1px;line-height: 1.8;color: rgb(140, 140, 140);"><p style="text-wrap: wrap;"><span style="color: rgb(224, 224, 224);">｜</span><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzA5ODA0NDE2MA==&amp;mid=2649786899&amp;idx=1&amp;sn=bbccbabb0cd6f3b71153055471fb5aed&amp;chksm=8893ba7cbfe4336acb84cad9022cde88bf79e7b906c07af7d8e8ff2d0ed7993e2ca74eca0da6&amp;scene=21#wechat_redirect" textvalue="全球首起通信设备武器化事件" linktype="text" imgurl="" imgdata="null" data-itemshowtype="0" tab="innerlink" data-linktype="2"><span style="font-size: 12px;">全球首起通信设备武器化事件</span></a><span style="font-size: 12px;"></span></p></section></section></section><section style="margin: 5px 0%;"><section style="background-color: rgb(224, 224, 224);height: 1px;"><svg viewBox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section></section></section></td></tr><tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:10.classicTable1:2"><td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:10.classicTable1:2.td@@0" style="border-color: rgb(62, 62, 62);border-style: none;padding: 0px;" width="100.0000%"><section><section style="display: flex;flex-flow: row;margin-top: 10px;margin-right: 0%;margin-left: 0%;justify-content: flex-start;"><section style="display: inline-block;vertical-align: middle;width: auto;min-width: 10%;height: auto;flex: 0 0 auto;align-self: center;"><section style="font-size: 14px;color: rgb(5, 193, 183);line-height: 1;letter-spacing: 0px;text-align: center;"><p><strong>03</strong></p></section></section><section style="display: inline-block;vertical-align: middle;width: auto;flex: 100 100 0%;align-self: center;height: auto;"><section style="font-size: 14px;letter-spacing: 1px;line-height: 1.8;color: rgb(140, 140, 140);"><p style="text-wrap: wrap;"><span style="color: rgb(224, 224, 224);">｜</span><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzA4MTg0MDQ4Nw==&amp;mid=2247575538&amp;idx=1&amp;sn=d4f16a71cfcfb3f1b589348504e754cf&amp;chksm=9f8d37faa8fabeec106d46c0090ee77dd24e861f99c7707e1a57f61b6dd78a05495c79cc585e&amp;scene=21#wechat_redirect" textvalue="通信设备武器化背后的供应链战争" linktype="text" imgurl="" imgdata="null" data-itemshowtype="0" tab="innerlink" data-linktype="2"><span style="font-size: 12px;">通信设备武器化背后的供应链战争</span></a><span style="font-size: 12px;"></span></p></section></section></section><section style="margin: 5px 0%;"><section style="background-color: rgb(224, 224, 224);height: 1px;"><svg viewBox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section></section></section></td></tr></tbody></table>  
  
  
**安全KER**  
  
  
安全KER致力于搭建国内安全人才学习、工具、淘金、资讯一体化开放平台，推动数字安全社区文化的普及推广与人才生态的链接融合。目前，安全KER已整合全国数千位白帽资源，联合南京、北京、广州、深圳、长沙、上海、郑州等十余座城市，与ISC、XCon、看雪SDC、Hacking Group等数个中大型品牌达成合作。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Ok4fxxCpBb64b6mYFiaT7LhhJgUJ5RuORm1pBZT2l1tsGiczenAPtn2PwS00UTGicuflicib4HmXYibvgyQ5VLo7HibHA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Ok4fxxCpBb64b6mYFiaT7LhhJgUJ5RuORFyaib3VOnNtmPG7RwRzAufYFicU1XXdaSIjNPLcVHjpb3mAUr25wnHEw/640?wx_fmt=png&from=appmsg "")  
  
**注册安全KER社区**  
  
**链接最新“圈子”动态**  
  
