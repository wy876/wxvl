#  SAP NetWeaver漏洞利用进入第二波威胁活动   
原创 David  信息安全D1net   2025-05-12 07:54  
  
点击上方“**蓝色字体**  
”，选择 “**设为星标**  
”  
  
关键讯息，D1时间送达！  
  
网络安全机构发现，利用SAP NetWeaver Visual Composer高危漏洞（CVE-2025-31324，CVSS 10分）的第二波攻击浪潮正在全球蔓延。Onapsis与Mandiant已追踪到横跨能源、制造等关键行业的数百起入侵事件，攻击者利用远程命令执行漏洞建立持久控制。研究证实黑客自1月起就开展侦查，比原定时间提前两月，并采用"就地取材"技术隐藏行踪。SAP已发布紧急补丁，但新型威胁组织Chaya_004的出现加剧了防御复杂性，Forescout监测到其使用伪造云证书及多国黑客工具的混合攻击模式。  
  
  
研究人员正在追踪全球范围内的数百起案件，并警告称，相关风险比之前已知的更为严重。  
  
据研究人员称，第二波网络攻击正瞄准SAP NetWeaver Visual Composer中的一个关键漏洞。  
  
继4月份披露的初步威胁活动后，机会主义威胁行为者正利用之前通过CVE-2025-31324漏洞建立的webshell进行攻击。据Onapsis的研究人员称，该漏洞的CVSS评分为10分，允许未经身份验证的攻击者上传任意文件并完全控制系统。  
  
Onapsis和Mandiant正在追踪全球数百起已确认的入侵事件，这些案件涉及多个行业，包括公用事业、制造业、石油和天然气以及其他关键基础设施领域。  
  
网络安全和基础设施安全局(CISA)在4月底将该漏洞添加到了其已知被利用漏洞目录中。  
  
Onapsis的研究人员表示，威胁活动的严重性比最初了解的要高，部分原因是威胁行为者似乎比防御者之前意识到的更熟悉SAP系统。Onapsis现在认为，黑客早在1月份就开始了对易受攻击的SAP系统的初步探测，这比之前认为的时间早了两个月。  
  
“情况发生了变化，最初人们认为攻击者是利用远程文件上传漏洞来部署webshell，然后利用这些webshell来入侵系统，”Onapsis的首席技术官Juan Pablo (JP) Perez-Etchegoyen通过电子邮件表示。  
  
然而，根据Onapsis重建的有效载荷，黑客实际上是在利用远程命令执行漏洞。  
  
多家安全研究公司表示，活跃的入侵和破坏行为似乎始于3月份。  
  
专家表示，威胁行为者似乎对SAP系统有深入的了解，并可能正在使用“就地取材”(living-off-the-land)技术来隐藏其踪迹并维持持久性。  
  
SAP在4月底发布了一个更新，其中包含一个变通办法，可以从无法打补丁的系统中完全移除该应用程序。SAP的缓解工具为已确认的客户提供密码保护。  
  
该公司敦促所有客户更新其系统，安装4月24日发布的紧急补丁，该漏洞最初由Reliaquest的研究人员披露。  
  
与此同时，Forescout的研究人员发现了一个新的威胁行为者，他们将其命名为Chaya_004，该行为者正在利用SAP漏洞进行攻击。  
  
“我们尚未将此活动集群与任何已知的威胁行为者联系起来，”Forescout威胁狩猎高级经理Sai Molige通过电子邮件表示。“不过，根据他们的基础设施和工具库，他们更可能是犯罪分子，而非国家资助的。”  
  
研究人员发现了一个网络，该网络在787多个IP地址上伪造了Cloudflare证书。  
  
分析人员还观察到了使用SuperShell、Cobalt Strike、SoftEther VPN和多种中文渗透测试工具的实时入侵行为。  
  
版权声明：本文为企业网D1Net编译，转载需在文章开头注明出处为：企业网D1net，如果不注明出处，企业网D1net将保留追究其法律责任的权利。  
  
  
（来源：企业网D1net）  
  
**关于企业网D1net(www.d1net.com)**  
  
  
  
  
国内  
头部  
ToB IT门户  
，同时在运营国内最大的甲方CIO专家库和智力输出及社交平台-信众智(www.cioall.com)。旗下运营19个IT行业公众号(  
微信搜索D1net即可关注  
)  
  
  
  
如果您在企业IT、网络、通信行业的某一领域工作，并希望分享观点，欢迎给企业网D1Net投稿。  
封面图片来源于摄图网  
  
**投稿邮箱：**  
  
editor@d1net.com  
  
**合作电话：**  
  
010-58221588（北京公司）  
  
021-51701588（上海公司）   
  
**合作邮箱：**  
  
Sales@d1net.com  
  
企业网D1net旗下信众智是CIO（首席信息官）的专家库和智力输出及资源分享平台，有六万多CIO专家，也是目前最大的CIO社交平台。  
  
  
信众智对接CIO为CIO服务，提供数字化升级转型方面的咨询、培训、需求对接等落地实战的服务。也是国内最早的toB共享经济平台。同时提供猎头，选型点评，IT部门业绩宣传等服务。  
  
**扫描 “**  
**二维码**  
**” 可以查看更多详情**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/OuQdh6iaViaXaIOY0mjrTgicElErUqymD4icjEneq6YYVpiadU3pDLRHwqFrW9Y2Ht0uKeuIEjO3hDxfiatbI5KcibHIA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
