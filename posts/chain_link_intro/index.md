# Chain Link入门


### 1. Chain Link的作用

由于区块链上只有转账和计算数据，其他的数据都要由其他人输入，因此区块链需要一种提供可靠数据的服务。Chain Link给区块链提供了可靠的现实世界的数据，例如ETH对USD的价格、ETH对BTC的价格。Chain Link本身搭建了一个POS的区块链，在其他链上（例如以太坊）可以调用Chain Link提供的代理（proxy）智能合约来获取现实世界中的数据。

在这里介绍一个在Rinkeby Testnet上面获取ETH价格的智能合约。本教程基于Chain Link提供的在Kovan Testnet上的[教程](https://docs.chain.link/docs/consuming-data-feeds/)

### 2. 支持

首先要在MetaMask中切换到测试网络Rinkeby，并且在测试网络中获得免费的LINK和ETH（[领取网址](https://faucets.chain.link/rinkeby)）。

之后需要找到Rinkeby网络中提供ETH / USD价格数据的只能合约地址，也就是：

```
0x8A753747A1Fa494EC906cE90E9f37563A8AF630e
```

全部的合约地址的列表可以在[Chain Link的网站](https://docs.chain.link/docs/ethereum-addresses/)上面获取。
整个合约使用Remix，并且在部署合约的时候使用Rinkeby网络，也就是选择Injected Web3。这样就可以使用Metamask进行确认，实际体验和真实的区块链相同。

### 3. 合约部署

整个合约的代码如下。可以看到整个代码非常简单，只需要用AggregatorV3Interface调用对应地址的智能合约就可以获得价格priceFeed。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Rinkeby
     * Aggregator: ETH/USD
     * Address: 0x8A753747A1Fa494EC906cE90E9f37563A8AF630e
     */
    constructor() {
        priceFeed = AggregatorV3Interface(0x8A753747A1Fa494EC906cE90E9f37563A8AF630e);
    }

    /**
     * Returns the latest price
     */
    function getLatestPrice() public view returns (int) {
        (
            uint80 roundID, 
            int price,
            uint startedAt,
            uint timeStamp,
            uint80 answeredInRound
        ) = priceFeed.latestRoundData();
        return price;
    }
}
```

部署完成后可以点击getLatestPrice就可以得到ETH的价格，如下：
<p align="center">
  <img src="pic\getprice.png" alt="getprice"  />
</p>

