[#]: subject: (What you need to know about security policies)
[#]: via: (https://opensource.com/article/21/7/what-security-policy)
[#]: author: (Chris Collins https://opensource.com/users/clcollins)
[#]: collector: (lujun9972)
[#]: translator: (FelixYFZ )
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( )
关于安全策略你所需要知道的
======
学习如何使用Selinux,Kubernets pod security 和防火墙去保护你的个人电脑,服务器以及云系统.
![Lock][1]
安全策略是指一套对系统的访问权限的控制设置,这个系统可以是一个组织,一台电脑,一个网络,一套应用,一个文件,或者任何其他的资源.安全策略经常是从上往下开始,默认是任何人都不能做任何事,然后去设置特殊的许可.

在一台个人电脑上，默认的策略是在没人登录前，电脑和用户之间是没有任何互动操作的。一旦你成功的登录进去，你的账户会继承一套实现特定功能行为的权限(会以元数据的形式附加到你的账户里)。这些默认的行为功能同样适用于在互联网上的你的手机，服务器，或者网络设备，或者是任何其他在云上的节点。

有为文件系统，防火墙，服务，进程和个人文件设计的安全策略。保护你的数字化基础架构是一项永远不会结束的工作，而且那可能会是让人沮丧和令人紧张不安的。然而，在有安全策略保障的情况下，你就不需要去担心任何人或其他什么对你的数据的访问。对安全问题的精通熟悉是很重要的，在早餐的时间阅读已知的一些安全问题(比如NIST上的对CVE条目的该概要)比一杯好的咖啡更能令人开阔眼界。但是同样重要的是对可以给你建立合理的预设值的任由你自由支配的工具的熟悉。这些是要根据你所要设置安全保护的内容来做区分，所以这篇文章重点关注三个领域：你的个人电脑，服务器，以及云服务。

### SELinux

对你的个人电脑，服务器，云服务真的Linux 节点来说，Selinux是一个标记系统。在一套现代的运行Selinux的Linux系统上，每一个进程，每一个文件以及每一个目录都会有一个标签。实际上，任何一个系统对象都会有一个标签，幸运的是， 你不需要去手动的来打标签。这些标签都是Selinux自动创建的。

策略规则来管理控制赋予标记的进程和对象的权限。内核来执行这些规则。从另一方面来说，Selinux能够确保一个行为动作是安全的，无论是一个用户获得了执行某个动作或或者没有获得的权限。它通过理解进程被赋予的权限来执行这些操作管控。它通过理解你的电脑的所有组件的预期互动行为来保护你的系统免受一个通过安全渗透或者趁你去咖啡续杯的时候来到你的桌前获取高级权限的破坏者的行为。

想获取更多关于SelinuxDefinitely信息，请阅读Dan Walsh的[illustrated guide to SELinux]。想要学习如何使用Selinux，请阅读Alex Callejas的[A sysadmin's guide to SELinux][7],免费下载[SELinux cheat sheet][8].

### Kubernetes pod security

在Kubernets云计算中，有**安全策略**和**安全上下文**

Pod安全策略是Kubernets pod安全资源的一个实现。他们是描述特殊条件，为了被接受或者安排pod必须要遵守的内置资源。比如，pod安全策略可以管控限制哪种存储卷允许挂载，或者什么用户或者组的ID是允许使用的。和安全上下文不一样，这些是被集群控制系统来限制的，它来决定一个给定的pod是否允许在kubernets系统里，甚至是在这个pod被创建之前。如果pod的创建描述不符合Pod安全策略，它会被拒绝创建的。

安全上下文和安全策略类似，它描述了一个pod或者容器在容器运行时的上下文环境中哪些能做，哪些不能做。我们回想pod 安全策略是在控制平台中执行的。安全上下文是包含在pod的描述文件中的，来决定容器的运行时间(比如，docker,CRI-O,扽等)尤其是pod如何运行。在权限限制种类方面安全策略和安全上下文有很多重叠的地方。可以参考下面这样的不同表现形式前者是“这些是pod在这些策略中可以做的”而后者是“这个pod必须按照这些特定的规则来运行”。

#### Pod安全策略的状态

Pod安全策略被弃用了，而且将会在Kubernets 1.25版本中被移除。Kubernets社区兴趣小组的Tabitha Sabl写了关于弃用和替换安全策略的文章[deprecation and replacement of Pod Security Policies][11]. 有一个开放的请求提议，通过一个新的管理控制器来加强pod安全标准的Kubernets改进[Kubernetes enhancements][12].这被用来建议作为弃用的安全测策略的替代。架构已经被确认了，然而，有一个带有很多附件软件和相互关联的服务需要被混合和匹配来提供覆盖范围的庞大的系统来满足组织的需求。

目前，Kubernets已经发布了pod安全标准[Pod Security Standards][13]描述了策略类型的整个分层的概念，从完全无限制的特权的pods到最小限制的最基本权限，然后是严格管控限制的策略，而且发布了这些策略案列作为Pod的安全策略。文档描述了不同种类的配置文件的组成部分而且对于熟悉不同类型可能会应用于pod的安全提升来说提供了一个很不错的开始。

#### 安全策略的未来

在Kubernetes中的pod安全的未来可能会包括一个管理控制器就像PR强化中的提议的那样，而且一个混合的附加软件来调整pods在集群中如何运行，比如[Open Policy Agent][14] (OPA).Kubernets非常灵活，因为它的工作非常复杂，而且这个变更遵循了同样的模式而使得Kubernets非常的成功：优秀的容器管理编排能力，允许带附件软件和工具的整个系统来强化和扩展内核从而让它并不是一个通用的解决方案。

### 防火墙

保护网络和保护网络中的电脑一样重要。所以，有了防火墙。有些防火墙是内置在路由器中的，但是电脑也是有防火墙的，在较大的组织中，他们为整个网络运行防火墙。

典型的防火墙策略设置阻止所有的流量，然后配置明确的必要的进出通讯的额外策略。个人用户可以通过Seth Kenlon的[Getting started with Linux firewalls][15]的文章来学习'firewall-cmd'.
系统管理员可以通过Seth的[Secure your network with firewall-cmd][16]来学习。无论是普通用户还是管理员都能够通过我们免费的[firewall-cmd cheat sheet][17]来获取想要的知识技能。

### 安全策略

安全策略对于任何系统的用户和他们的数据的保护是很重要的。建筑和技术会议需要安全策略来保障人们的的物理安全，电脑需要安全策略来保证数据安全防止被滥用。
在你的生活中花些时间来来想想系统的安全性，熟悉默认的策略，为你鉴别出的不同的风险选择合适的等级。然后创建一套安全策略并专注于它。并准备一个备份计划，安全问题不会被解决除非它很容易，所以保持良好的安全实践作为你的第二天性。

--------------------------------------------------------------------------------

via: https://opensource.com/article/21/7/what-security-policy

作者：[Chris Collins][a]
选题：[lujun9972][b]
译者：[译者ID](https://github.com/FelixYFZ)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://opensource.com/users/clcollins
[b]: https://github.com/lujun9972
[1]: https://opensource.com/sites/default/files/styles/image-full-size/public/lead-images/security-lock-password.jpg?itok=KJMdkKum (Lock)
[2]: https://nvd.nist.gov/feeds/xml/cve/misc/nvd-rss-analyzed.xml
[3]: https://nvd.nist.gov/vuln/data-feeds#APIS
[4]: https://opensource.com/article/21/6/breakfast
[5]: https://en.wikipedia.org/wiki/Security-Enhanced_Linux
[6]: https://opensource.com/business/13/11/selinux-policy-guide
[7]: https://opensource.com/article/18/7/sysadmin-guide-selinux
[8]: https://opensource.com/downloads/cheat-sheet-selinux
[9]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/
[10]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[11]: https://kubernetes.io/blog/2021/04/06/podsecuritypolicy-deprecation-past-present-and-future/
[12]: https://github.com/kubernetes/enhancements/issues/2579
[13]: https://kubernetes.io/docs/concepts/security/pod-security-standards/
[14]: https://www.openpolicyagent.org/
[15]: https://opensource.com/article/20/2/firewall-cheat-sheet
[16]: https://www.redhat.com/sysadmin/secure-linux-network-firewall-cmd
[17]: https://opensource.com/downloads/firewall-cheat-sheet
[18]: https://opensource.com/article/19/3/backup-solutions
