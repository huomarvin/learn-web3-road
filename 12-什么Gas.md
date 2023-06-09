`Gas`是了解以太坊网络的最重要和最基本的方面之一

> `Gas`是以太坊运行的燃料，就像汽车需要汽油才能运行一样。

在之前的教程中，你可能已经注意到了，以太坊上的交易是需要用户支付交易费的。

- 这个交易费用是如何计算的？
- 一笔交易需要支付多少ETH？
- 为什么有些交易比其他交易更贵？
- 为什么会存在`Gas Fee`？

回答这些问题我们要先理解`Gas`的概念。

最近的一次升级，即2021年8月的伦敦升级，稍微改变了交易费用的计算方式和气体的工作方式。出于这个原因，我们将把本教程分成两部分。

- 伦敦升级前
- 伦敦升级后

伦敦升级前的内容很好理解，比伦敦升级后的内容更容易让你初步理解，同时也提供了升级的动机。

## `Gas`一般概念

就像秒是时间单位，米是距离单位一样，气体本身就是以太坊网络上的一个计算单位。

气体单位是用来衡量在以太坊上执行一笔交易所需的计算量。由于每笔交易的执行都需要一定的计算资源，因此需要支付一定的费用，通常称为气体费或交易费。

气体费是用以太坊的原生货币-ehter或ETH支付的。`Gas Fee`的计算方式在伦敦升级之前和之后略有不同。

> 注意：一般来说，当有人说`Gas`时，他们指的是`Gas Fees`而不是单位本身。然而，为了本教程的目的，我们将在技术上正确地说 `Gas`，指的是单位，而 `Gas费用 `指的是以太币的费用。

## 伦敦升级前

在伦敦升级发生之前，你需要为一笔交易支付多少`ether`是用一个简单的公式计算的。

`gas fees = gas spent * gas price`

- **Gas Spent** 是用于执行交易的气体总量（气体单位）。
- **Gas Price**是指你愿意为每一气体单位的执行支付的乙醚数量。

`Gas prices`以gwei计价--ETH的一种计价方式。

1 Gwei = 0.000000001 ETH

1 ETH = 10^9 Gwei

因此，你可以说你的`gas price`是0.000000001 ETH，而不是说你的`gas price`是1 Gwei。

> Gwei是Giga-Wei的缩写，相当于1,000,000,000 (10^9)Wei。Wei是ETH的最小面额。1ETH=10^18 Wei。

### 示例

就执行所需的气体量而言，最便宜的交易只是将ETH从一个账户转移到另一个账户。这笔交易需要21,000个气体单位。

假设Alice想付给Bob 1个ETH。燃气费用为21000燃气。假设天然气价格为200Gwei。

因此，`gas fees = 21,000 * 200 = 4,200,000 Gwei = 0.0042 ETH`

因此，当Alice汇款时，1.0042 ETH将从她的账户中扣除，而Bob将收到1 ETH。0.0042个ETH的费用会给开采包含Alice交易的区块的矿工。

**你可能想知道油价是如何被设定为200Gwei的？**天然气价格设置为多少，由用户决定。天然气价格较高的交易有更高的优先权被纳入区块，因为矿工首先开采这些交易会得到更多的小费。因此，天然气价格基本上就像一个公开拍卖，或对矿工的贿赂。谁愿意向矿工支付最高的价格，或最高的贿赂，他们的交易就会比低价的交易更快地被纳入。

像Metamask这样的钱包会根据当前的网络条件为要执行的交易提供合理的油价估计值--因此大多数用户不需要自己去碰油价值。(虽然，你可以通过Metamask设置启用修改）。

### 燃气费用计算

当智能合约被编译成字节码，在部署到以太坊网络之前，它被编译成OPCODES。这些是可以直接在以太坊虚拟机上运行的简单操作。你可以认为它们类似于可以直接在英特尔或AMD CPU上运行的基本操作。这些OPCODES包括基本操作，如ADD、MUL、DIV、SUB、SHA3等。

每个OPCODE都有一个固定的气体成本。智能合约中一个特定功能的气体成本是它所有OPCODES的气体成本的总和。如果感兴趣，[你可以在这里找到所有OPCODES及其相关气体成本的列表](https://github.com/crytic/evm-opcodes)。

因此，更复杂的交易需要更多的OPCODES来执行，最终会比简单的交易（如将ETH从一个账户转移到另一个账户）使用更多的气体（单位）。

### 气体限制（Gas Limits）

现在，你可以想象，存在很多比从一个账户向另一个账户发送ETH复杂得多的功能。那些涉及循环，或随机性，或依赖用户输入的功能。

对于这样的功能，可能很难准确预测执行所需的气体量，因为它取决于其他变量。

因此，在决定为交易支付多少费用时，你可以指定一个上限限制，而不是指定确切的气体费用。

`Gas Limits`指的是你愿意为交易使用的最大气量（单位）。这是由用户设置的。

同样，像Metamask这样的钱包提供合理的估计。

如果你的交易使用的汽油少于你的限额，未用完的汽油将退还给你的账户。

因此，你的钱包必须有`gas limit * gas price ether`，以便在发送交易时支付gas。任何未使用的气体将在交易被执行和开采时被退还。

但是，如果你的交易使用的汽油超过了你的限额，交易就会失败，你的汽油就会消失。

> 所以是不是不指定更好呢？

### 区块气体限制（Block Gas Limits）

除了用户指定的每笔交易的气体限制外，以太坊网络还对单个区块中允许的最大气体量（单位）进行了限制。

这样做是为了确保每个区块保持在一个可允许的计算成本范围内。由于更复杂的交易需要更长的时间来执行，这确保了节点不会因为计算复杂性的增加而开始与网络的其他部分不同步。

## 伦敦升级后

2021年8月5日--以太坊网络上实施了伦敦升级。这次升级主要引入了三个好处。

- 更好的气体费用估算
- 更快的交易纳入
- 燃烧一定比例的ETH作为交易费用

就本文而言，我们主要对前两点感兴趣。

在伦敦升级之前，像Metamask这样的钱包会根据过去的网络活动提供对天然气价格的估计。每个钱包都使用自己的方法来做到这一点。具体来说，Metamask扫描了以太坊的最后1000个区块，并预测了你的交易的天然气价格。

然而，从伦敦升级开始，每个区块都被设定为有一个基本的天然气价格费用。这是让你的交易包含在这个区块中的每单位气体的最低价格。这是由网络根据区块空间的需求来计算的。这些基本费用将被以太坊网络烧掉，因此永远摆脱了ETH来抵消发行量。由于以太坊没有一个整体的最大供应量（不像比特币，它的最大供应量为2100万个比特币），燃烧有助于ETH供应达到平衡，不会无限膨胀。

除了基本费用之外，还引入了小费（优先费）的概念。随着基本费用被烧毁，小费的存在是为了补偿矿工执行和传播用户交易的费用。这又是由大多数钱包自动设置的，尽管你可以选择手动设置。更高的小费交易往往会得到更高的优先权。

通过这次升级，计算`gas fees `的公式变为如下。

`gas fees = gas spent * (base fees + priority fees)`

### 示例

回到前面的例子，如果爱丽丝要付给鲍勃1个ETH，那么气体成本（单位）是21,000。假设基本费用为100GWei，而爱丽丝决定包括10GWei的小费。

`total gas fees = 21,000 * (100 Gwei + 10 Gwei) = 2,310,000 Gwei = 0.00231 ETH`

### 可变的区块大小

在伦敦升级之前，所有区块的气体限制是恒定的。每个区块的最大容量为15M气体。在需求量大的时候，这导致了糟糕的用户体验，因为区块在满负荷运行，而用户不得不等待需求量减少，才能被纳入一个区块中。

这次升级为以太坊引入了可变大小的区块。每个区块现在有一个15M气体的目标限制，但其大小可以随着网络需求而增加或减少，直到最大的30M气体。

平均来说，通过修改区块大小和基础费用，网络在15M气左右达到了平衡。

**如果区块气体大于15M的目标，下一个区块的基本费用就会增加。同样地，如果区块气体小于15M的目标，下一个区块的基本费用就会减少。**基本费用的调整额度取决于区块气体离15M目标的距离。

花点时间阅读最后几段，充分掌握它们--这是相当迷人的东西，但可能有点棘手，让你的头脑陷入混乱。

### 可变的基本费用

让我们来看看在网络需求高涨的时候，基本费用会发生什么变化。

如果超过目标的15M气体，每个区块的基本费用最多会增加12.5%。这种指数式增长使得区块气体无限期地保持在高位在财务上是不可行的，因此允许节点与网络保持同步，而不是不断地执行30M气体区块。

![Image](/Users/marvin/Web3/learn-web3-road/img/q5fEwNn.png)

在这个例子中，第2区块出现了可能的最大增幅，从目标的15M增加到30M。因此，第3区块的基本费用增加了12.5%，从100吉威增加到112.5吉威。

同样，由于3号区块也达到了30M天然气的最大限度，也就是与目标的最大可能距离，4号区块的基本费用再次增加了12.5%，达到126.6Gwei。以此类推...

这种情况一直在发生，到了第8区，基本费用为202.7 Gwei。比7个区块前增加了102.7%!到了100区块，基本费用是10302608.6Gwei - 这是疯狂的（也是不现实的）。这意味着，在第100区块进行简单的ETH转账将花费（21000 * 10302608.6 Gwei）= 216 ETH。

由于这种基本费用的指数式增长，可以注意到，极不可能看到全块的延长峰值。

应该指出的是，基本费用也最多减少12.5%，在流量开始放缓后，帮助尖峰回归平衡。

### 更好的气体估计

相对于伦敦升级前的机制，这种基础费用机制的变化使得费用预测更加可靠。根据上表，在第9个区块创建交易时，钱包可以让用户100%肯定地知道，下一个区块的最大基本费用是当前的基本费用（前一个区块的基本费用）*112.5%=202.8*112.5/100或228.1Gwei。反之，可以确定最低的基本费用，知道减少的费用只能是12.5%：目前的基本费用（前一区块的基本费用）*87.5%=202.8*87.5/100或177.45Gwei。

因此，钱包现在知道一个最小和最大的基本费用范围，在提供估计时提供给用户。最小范围是当前的基本费用*87.5%，最大范围是当前的基本费用*112.5%。 然后，用户可以直接为矿工调整小费，这通常是基本费用的一个零头。

## Gas为什么会存在

燃气费有助于保持以太坊网络的安全。通过要求在网络上执行的每一次计算都要收费，可以防止坏的行为者向网络发送垃圾邮件。

为了避免智能合约中的意外或恶意的无限循环，这将导致所有以太坊节点永远被卡住，对交易的气体限制对一个交易可以使用的计算量设置了限制。

像这样的代码会用完所有提供的气体，直到限制，然后交易会失败。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Gas {
    uint public i = 0;

    // 你发送的所有气体都会被消耗，并且导致交易失败
    // 状态也并没有改变
    // 花掉的Gas也不会被退回
    function forever() public {
    		// 我们运行一个循环直到所有的gas都被消费掉并且此次交易失败
        while (true) {
            i += 1;
        }
    }
}
```

使这一切成为可能的基本单位是气体。

## 降低燃气费

以太坊的高收费是最近的一个热门话题。以太坊社区曾郑重发誓不伤害网络的去中心化或安全性。因此，做出了有利于安全的权衡，这导致以太坊网络目前的交易费用高于其他区块链，如Solana，它以牺牲安全和去中心化为代价，做出了有利于降低费用的权衡。

以太坊的基本目标是成为一个高度安全和高度去中心化的区块链网络，能够执行智能合约。

但是，如果用户必须不断花费数百美元来移动一美元，那么这一切都不重要。

因此，有很多东西正在研究，有些已经有了，以允许减少汽油费，改善用户体验。

主要是，以太坊将提供的[网络升级](https://ethereum.org/en/upgrades/)将最终解决一些气体问题，这反过来将使网络能够每秒处理成千上万的交易，并在全球范围内扩展。

此外，很多工作都是在第二层扩展方面进行的。我们以后会更深入地了解第二层扩展和第二层平台，但本质上它们是将智能合约的重计算方面转移到其他地方的网络，并使用以太坊主网作为最终结算层，从而继承了以太坊的安全和去中心化优势，并为用户保持低气费和高交易速度。

## 资源

以下资源是推荐的，但可选择的阅读/观看内容，以了解更多关于气体的信息。

- [Video: Ethereum Gas Explained](https://www.youtube.com/watch?v=AJvzNICwcwc)
- [The London Upgrade](https://ethereum.org/en/history/#london)
- [Gas optimizations in smart contracts](https://medium.com/coinmonks/8-ways-of-reducing-the-gas-consumption-of-your-smart-contracts-9a506b339c0a)
- [More on Layer 2 Scaling](https://ethereum.org/en/developers/docs/scaling/layer-2-rollups/)