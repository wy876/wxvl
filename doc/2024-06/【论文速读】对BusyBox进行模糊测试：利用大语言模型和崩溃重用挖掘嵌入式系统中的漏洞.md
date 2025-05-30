#  【论文速读】|对BusyBox进行模糊测试：利用大语言模型和崩溃重用挖掘嵌入式系统中的漏洞   
原创 知识分享者  安全极客   2024-06-19 19:42  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8QnnD31IwPBrSKf17icRKpDUvcHfLa8rlE0mAfF2ARoSfPeicoyHaAVEbR7vweXNdicr6Rs1Y8kCiaHLg/640?wx_fmt=jpeg&from=appmsg "")  
  
本次分享论文：Fuzzing BusyBox: Leveraging LLM and Crash Reuse for Embedded Bug Unearthing  
  
**基本信息**  
  
  
**原文作者：**Asmita, Yaroslav Oliinyk, Michael Scott, Ryan Tsang, Chongzhou Fang, Houman Homayoun  
  
**作者单位：**1. University of California, Davis 2. NetRise  
  
**关键词：**模糊测试，大语言模型，嵌入式系统，BusyBox，漏洞检测  
  
**原文链接：**  
  
****https://arxiv.org/pdf/2403.03897v1  
  
**开源代码：**  
  
https://github.com/asmitaj08/FuzzingBusyBox_LLM  
  
**论文要点**  
  
  
**论文简介：**  
  
BusyBox 是一个开源软件包，集成了300多个基本的Linux命令，广泛用于基于Linux的嵌入式设备中。BusyBox中的漏洞可能会对大量设备产生严重影响。这项研究对BusyBox的使用情况进行了深入分析，发现实际嵌入式产品中普遍存在旧版本的BusyBox，从而促使研究者对BusyBox进行模糊测试。研究者提出了两种技术来提高软件测试的效率。  
  
第一种技术利用大语言模型（LLM）生成目标特定的初始种子，大幅度提高了崩溃数量，展示了LLM在生成高效种子方面的潜力。第二种技术是重用先前获取的崩溃数据，这一策略能够在不进行传统模糊测试的情况下识别最新BusyBox版本中的崩溃，从而有效提升软件测试和嵌入式系统漏洞检测的效率。  
  
**研究目的：**  
  
随着物联网（IoT）设备的迅速增长，嵌入式设备在IoT安全中占据了重要地位。根据IoT Analytics的数据，全球IoT设备数量已达到16.7亿，年增长率为16%。由于嵌入式设备中的漏洞可能会危及整个系统的安全，对其进行持续分析显得尤为重要。固件是控制系统行为的核心，通常由许多第三方软件组件组成，这些组件在各种产品中重复使用。因此，研究者的研究重点是基于嵌入式Linux的固件，尤其是BusyBox。  
  
BusyBox是一个关键组件，提供了300多个常用的Unix工具，非常适合资源受限的嵌入式设备。然而，它也存在较大的安全风险，因为它通常以高权限运行并处理用户输入，容易成为攻击目标。本文的研究问题包括：  
  
1. BusyBox的不同变种在实际设备中有多普遍？如何高效识别这些变种中的类似漏洞？  
  
2. 如何利用大语言模型（LLM）改进嵌入式Linux实用程序（如BusyBox）的模糊测试？  
  
**研究贡献：**  
  
1. 识别BusyBox的使用情况：研究发现许多实际嵌入式设备仍在使用包含已知漏洞的旧版本BusyBox，强调了更新的重要性。  
  
2. 引入LLM种子生成技术：通过利用大语言模型（LLM）生成目标特定的初始种子，提高了模糊测试的效率和漏洞检测能力。  
  
3. 提出崩溃重用策略：重用之前获取的崩溃输入，快速识别不同版本的相同软件组件中的重复漏洞，节省测试时间。  
  
4. 实验证明：在最新版本的BusyBox中成功识别崩溃并进行手动分类，验证了LLM和崩溃重用技术的有效性。  
  
**引言**  
  
  
物联网（IoT）设备数量的迅速增长引发了严重的网络安全问题。根据IoT Analytics的数据，全球IoT设备数量达到16.7亿，年增长率为16%。这种扩展使得嵌入式设备在IoT安全中占据了重要位置，因为其中的漏洞可能会危及整个系统的安全。固件在控制系统行为的各个方面起着至关重要的作用，通常包含许多第三方软件组件，这些组件在各种产品中重复使用，放大了它们的安全问题。因此，对这些组件的持续分析是必要的。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8RhvdCNMN1YwML1Q0micKNFcHkcicFWiaRTnlednOTNKpWZ2Q2icYq9rT7ShFDFCG2SnZ0urpVIPcLlWA/640?wx_fmt=jpeg&from=appmsg "")  
  
研究者的研究重点是基于嵌入式Linux的固件，尤其是BusyBox。BusyBox是一个关键组件，提供了300多个常用的Unix工具，非常适合资源受限的嵌入式设备。然而，它也存在较大的安全风险，因为它通常以高权限运行并处理用户输入，容易成为攻击目标。  
  
**研究背景**  
  
  
BusyBox是一个单一的二进制可执行文件，包含了多个Unix工具，专为资源受限的嵌入式设备设计。它是开源的、轻量级的，占用空间小，允许制造商在不显著增加固件大小的情况下包含必要的Linux工具。此外，它具有高度可定制性，可以配置为仅包含嵌入系统功能所需的特定工具。  
  
然而，BusyBox也存在潜在的安全风险。它通常以高权限运行，处理用户输入，如果这些命令没有正确清理，可能导致命令注入、缓冲区溢出等漏洞。鉴于BusyBox在许多嵌入式系统中的关键角色及其潜在的安全风险，通过适当的配置、定期更新、代码审查和安全评估来确保BusyBox的安全至关重要。  
  
**相关工作**  
  
  
模糊测试是一种广泛使用的软件测试方法，利用覆盖率反馈来识别可能导致程序崩溃的输入，并随后分析这些崩溃以发现漏洞。模糊测试可以描述为输入测试，由于无法进行详尽的输入测试，通常采用半随机化的方式。在黑盒模糊测试中，模糊引擎生成基于一组规则或策略的输入，并将其输入到程序中，观察程序的执行覆盖情况。模糊测试过程可以重复多次，直到找到新的覆盖路径或程序崩溃。  
  
与黑盒模糊测试相比，白盒模糊测试假设对程序内部结构的完全了解，可以利用污点分析或符号执行等技术指导输入生成。灰盒模糊测试则介于黑盒和白盒之间，通常通过代码覆盖率信息来指导输入生成。  
  
American fuzzy lop（AFL）是一种基于覆盖率的灰盒模糊测试工具，通过编译时插桩和多种算法高效地对程序进行模糊测试。AFL++是AFL的社区驱动后继者，包含了新的增强功能、模糊策略和性能改进。AFL++提供详细的文档说明支持的所有功能。在源代码不可用的情况下，AFL++的QEMU模式可以在运行时执行内部插桩。  
  
**研究实验**  
  
  
本研究重点在于对BusyBox的AWK工具进行模糊测试，通过生成符合AWK语法的脚本作为初始种子，提高模糊测试效率。研究者使用OpenAI的GPT-4模型生成初始种子，并对比使用随机种子的模糊测试结果，发现使用LLM生成的种子显著提高了崩溃数量和覆盖路径。接着，研究者将收集到的崩溃输入应用到最新版本的BusyBox上，通过重用崩溃数据快速识别漏洞，而不需要进行全面的模糊测试。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8RhvdCNMN1YwML1Q0micKNFcO90PiaU9VobRxSH4tviaO2xkibZF0b3HVenbypmI9eUlRPSZUEj2ibbZicQ/640?wx_fmt=jpeg&from=appmsg "")  
  
**研究结果**  
  
  
通过实验，研究者验证了利用大语言模型（LLM）生成初始种子和重用崩溃数据的有效性。结果表明，使用LLM生成的初始种子可以显著提高模糊测试的覆盖率和崩溃数量。在对BusyBox进行模糊测试时，LLM生成的种子比随机生成的种子检测出更多崩溃，显现出LLM在生成高效种子方面的巨大潜力。此外，通过重用先前版本的崩溃数据，研究者能够快速识别最新BusyBox版本中的漏洞，大幅度减少了测试时间和资源投入。具体而言，研究者在最新版本的BusyBox中识别出97个崩溃，其中19个为独特崩溃。这一结果验证了崩溃重用策略在实际应用中的高效性，并为嵌入式系统的漏洞检测提供了一种创新且有效的解决方案。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8RhvdCNMN1YwML1Q0micKNFcEW6wKHFxEan0jCtmPeC9TMA0picIVibssiac8nIHrjJCTDUchicdHC3UicA/640?wx_fmt=jpeg&from=appmsg "")  
  
**研究讨论**  
  
  
本文提出的两种技术在提升模糊测试效率和漏洞检测方面表现出了显著的效果。LLM生成的初始种子提高了模糊测试的覆盖率和崩溃数量，而重用崩溃数据则节省了测试时间和资源。此外，通过对最新版本的BusyBox进行手动崩溃分类，验证了这些技术在实际应用中的有效性。然而，这些技术也有其局限性，特别是在处理新的或不熟悉的目标时，可能需要额外的调整和优化。  
  
**论文结论**  
  
  
本研究提出了利用大语言模型（LLM）和崩溃重用技术来提高BusyBox模糊测试效率的方法，并通过实验证明了这些技术的有效性。研究结果表明，通过使用LLM生成初始种子和重用崩溃数据，可以显著提高模糊测试的效率和漏洞检测能力。未来的工作可以进一步扩展这些技术的应用范围，针对更多类型的嵌入式系统和固件进行测试和优化。  
  
原作者：论文解读智能体  
  
校对：小椰风  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8QUiaoVSRUj5mYbjUmJD851ZfeHK53vb8AhFcQrc6BkgibD9wibwGCcAaXI6r1yl5fRiaE5FH6cywPq7w/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
****  
![](https://mmbiz.qpic.cn/mmbiz_gif/D9wGKNiaQYpx7bvaHqVZibq0ogu5pckjQMepnZgmhgM01uFQsoFz5QDDE0iapRkuUumSGfk8Dz7mjnbvibwPk7jISg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1&tp=webp "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8TJEwkcn2XjMSfUe9LTbIRaIZ39RRQt0W4lIyvs88aaGrmAH8A8yxaYiaTRUkYIRNsYWo2siaqWflGQ/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&retryload=1&tp=webp "")  
  
