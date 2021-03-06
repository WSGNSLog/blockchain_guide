## 常见问题

### 通用问题
** 问：区块链是谁发明的，有什么特点？**

答：区块链相关的思想最早是比特币的发明者-中本聪（化名）在论文中提出（但没有明确定义）作为比特币网络的核心支持技术。自那以后，区块链技术逐渐脱离比特币网络，成为一种通用的可以支持分布式记账能力的底层技术，具有去中心化和加密安全等特点。

** 问：区块链和比特币是什么关系？**

答：比特币是基于区块链技术的一种数字现金（cash）应用；区块链技术最早在比特币分布式系统中得到应用和验证，确保了比特币系统在 2009 年上线后，在完全自治情况下可以正常运转。

** 问：区块链和分布式数据库是什么关系？**

答：两者定位完全不同。分布式数据库是解决高可用和可扩展场景下的数据存储问题；区块链则是在多方（无须中心化中介角色存在）之间提供一套可信的记账和合约履行机制。

** 问：区块链有哪些种类？**

答：根据部署场景公开程度，可以分为公有链（Public Chain）、联盟链（Consortium Chain）和私有链（Private Chain）；从功能上看，可以分为以支持数字货币为主的数字货币区块链（如比特币网络）、支持智能合约的通用区块链（如以太坊网络）、面向复杂商业应用场景支持权限管理的分布式账本平台（如超级账本）。

** 问：（公有链情况下）区块链是如何保证没有人作恶的？ **

答：区块链并没有试图保障每一个人都不作恶，每个参与者都默认在最长的链上进行扩展。当某个作恶者尝试延续一个非法链的时候，实际上在跟所有的“非作恶”者进行竞争。因此，当作恶者超过一半（还要保持选择一致）时，在概率意义上才能破坏规则。而代价是一旦延续失败，所有付出的资源（例如算力）都将浪费掉。

** 问：区块链的智能合约应该怎么设计？**

答：首先，智能合约类似其它应用程序，在架构上即可以采取 monolithic 的方式（一个合约针对一个具体商业应用，功能完善而复杂），也可以采取 microservice 的方式（即每个合约功能单一，多个合约一起构建应用）。 选择哪种模式根本上取决于其上商业应用的特点。从灵活性角度，推荐适当对应用代码进行切分，划分到若干个智能合约，尽量保持智能合约的可复用性。

** 问：如何查看 PEM 格式证书内容？**

答：可以通过如下命令转换证书内容进行输出：`openssl x509 -noout -text -in <ca_file>`；例外，还可以通过如下命令来快速从证书文件中提取所证明的公钥内容：`openssl x509 -noout -pubkey -in <ca_file>`。

** 问：已知私钥，如何生成公钥？**

答：对于椭圆曲线加密算法，可以通过如下命令生成公钥：`openssl ec -pubout -outform PEM -in <private_key>`。

** 问：如何校验某证书是否被根证书签名？**

答：已知根证书文件 <root_cafile> 和待验证证书文件 <ca_to_verify> 情况下，可以使用如下命令进行验证：`openssl verify -CAfile <root_cafile> <ca_to_verify>`。

** 问：为何 Hash 函数将任意长的文本映射到定长的摘要，很少会发生冲突？**

答：像 SHA-1 这样的 Hash 函数可以将任意长的文本映射到相对很短的定长摘要。从理论上讲，从一个很大的集合映射到一个小的集合上必然会出现冲突。Hash 函数之所以很少出现冲突的原因在于虽然输入的数据长度可以很大，但其实人类产生的数据并非全空间的，这些数据往往是相对有序（低熵值）的，实际上也是一个相对较小的集合。当然，这个集合自身可能比输出的结果要大，但这个冲突的概率远没有输入是全空间集合时那么夸张。

### 比特币、以太坊相关

** 问：比特币区块链为何要设计为每 10 分钟才出来一个块，快一些不可以吗？**

答：这个主要是从公平的角度，当某一个新块被计算出来后，需要在全球的比特币网络内公布。临近的矿工将最先拿到消息并开始新一轮的计算，较远的矿工则较晚得到通知。最坏情况下，可能需要数十秒的延迟。为尽量确保矿工们都处在同一起跑线上，这个时间不能太短。但太长了又会导致每个交易的“最终”确认时间过长，目前看，10 分钟左右是一个相对合适的折中。另外，也是从存储代价的角度，让拥有不太大存储的普通节点可以参与到网络的维护。

** 问：比特币区块链每个区块大小为何是 1 MB，大一些不可以吗？**

答：这个也是折中的结果。区块产生的平均时间间隔是固定的 10 分钟，大一些，意味着发生交易的吞吐量可以增加，但节点进行验证的成本会提高（Hash 处理约为 100 MB/s），同时存储整个区块链的成本会快速上升。区块大小为 1 MB，意味着每秒可以记录 1 MB/(10*60)=1.7 KB 的交易数据，而一般的交易数据大小在 0.2 ~ 1 KB。

实际上，之前比特币社区也曾多次讨论过改变区块大小的提案，但都未被最终接受。

** 问：以太坊网络跟比特币网络有何关系？ **

答：以太坊网络所采用的区块链结构，源于比特币网络。基于同样设计原理上，以太坊提出了许多改善设计，包括支持更灵活的智能合约、支持除了 PoW 之外的更多共识机制（尚未实现）等。

### 超级账本项目

** 问：超级账本项目与传统公有区块链有何不同？**

答：超级账本是首个面向联盟链场景的开源项目，在这种场景下，参与账本的多方存在一定的信任前提，并十分看重对接入账本各方的权限管理、审计功能、传输数据的安全可靠等特性。超级账本在考虑了商业网络的这些复杂需求后，提出了创新的架构和设计，是首个在企业应用场景中得到大规模部署和验证的开源项目。

** 问：区块链最早是公有链形式，为何现在联盟链在很多场景下得到更多推崇？**

答：区块链技术出现以前，中心化的信任机制可以实现很高的性能和便捷的监管，但一旦中心机制出现故障，则导致系统的信任前提发生破坏。区块链技术可以提供无中介化情况下的信任保障。公有链情况下，任何人都可以参与监督，可以实现信任的最大化，但随之而来带来包括性能低下、缺乏监管等问题。

联盟链在两者之间取得了平衡。非中心化的联盟共识，让系统可信任度以指数形式增加；同时，联盟形成的信任前提，可以在不影响信任的情况下实现更优化的性能，并支持权限管理。这对复杂应用场景特别企业场景可以提供更好的支持。

** 问：采用 BFT 类共识算法时，节点掉线后重新加入网络，出现无法同步情况？**

答：这是某些算法设计导致的情况。掉线后的节点重新加入到网络中，其视图（View）会领先于其它节点。其它节点正常情况下不会发生视图的变更，发生的交易和区块内容不会同步到掉线节点。出现这种情况，可以有两种解决方案：一个是强迫其它节点出现视图变更，例如也发生掉线或者在一段时间内强制变更；另一种情况是等待再次产生足够多的区块后触发状态追赶。

** 问：超级账本 Fabric 里的安全性和隐私性是如何保证的？**

答：首先，Fabric 1.0 及往后的版本提供了对多通道的支持，不同通道之间的链码和交易是不可见的，即交易只会发送到该通道内的 Peer 节点。此外，在进行背书阶段，客户端可以根据背书策略来选择性的发送交易到通道内的某些特定 Peer 节点。更进一步的，用户可以对交易的内容进行加密（基于证书的权限管理）或 Hash 处理，同时，只有得到授权的节点或用户才能访问到交易。另外，排序节点无须访问到交易内容，因此，可以选择不将完整交易（对交易输入数据进行隐藏，或者干脆进行加密或 Hash 处理）发送到排序节点。最后，所有数据在传输过程中可以通过 TLS 来进行安全保护。许多层级的保护需要配合使用来获得不同层级的安全性。

实践过程中，也需要对节点自身进行安全保护，通过防火墙、IDS 等防护对节点自身的攻击；另外可以通过审计和分析系统对可疑行为进行探测和响应。
