---
timezone: UTC+8
---

# barryxu-0410

**GitHub ID:** barryxu-0410

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
今天认真听了周会，感觉大家都好厉害，刚好这周忙完了，下周一定要卷起来

up up up！！！！
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

今日学习内容总结如下：

**1\. AI 的演进：从聊天机器人到自主智能体 (Agent)**

演讲者 Rick 首先阐述了 AI 发展的两个阶段。2023 年是“大语言模型（LLM）”的爆发期，本质是基于概率的文本生成工具，主要功能是陪聊或回答问题，但缺乏记忆和行动能力。这类传统 LLM 存在三大局限：**无状态**（每次对话从零开始，不记得历史）、**无工具**（无法联网或调用 API）、**无身份**（不能签署协议或持有资产）。  
展望 2026 年，AI 将进化为 **Agent（智能体）**。它不仅是“大脑”，还拥有“手脚”（工具/API）和“身份”。它不再仅仅是生成文本，而是能够替人类打工、做决策、甚至赚钱。为了实现这一跨越，AI 需要从单纯的信息处理转向具备经济行为能力的实体。

**2\. 现有的基础设施缺口与 Web3 的必要性**

目前的互联网（Web2）架构无法支撑 AI Agent 的大规模经济活动，主要存在三大痛点：

**· 身份验证（Identity）：** 在 HTTP 协议下，无法区分请求来自真人还是 AI，也无法验证 AI 的归属权。这导致了安全隐患，如商业间谍伪装成合法 Agent 窃取数据。

**· 审计与追责（Auditability）：** AI 的决策日志若存储在中心化服务器上，可以被篡改或删除。一旦 AI 签署错误合同造成损失，缺乏不可篡改的证据链来追责。

**· 支付与结算（Payment）：** 传统金融体系（信用卡/银行卡）对 AI 不友好。

**o KYC 障碍：** AI 无法进行人脸识别或实名认证。

**o 微支付成本：** AI 调用 API 可能只需支付 0.001 美元，但传统支付的手续费远超交易额。

**o 结算速度：** 银行跨行/跨境转账需要 T+1 甚至 T+3，而 AI 的高频交互需要毫秒级或秒级结算。

因此，**Web3 与 AI 的结合是刚需**。区块链提供了不可篡改的账本（审计）、去中心化的身份（ID）、以及可编程的货币（无须许可的支付），成为了 AI Agent 的金融操作系统。

**3\. 核心协议标准：ERC-8004 与 HTTP 402**

为了解决上述问题，Rick 介绍了两个关键的协议标准：

**· ERC-8004（AI 链上身份标准）：** 被称为“AI 的护照”。它基于 NFT（ERC-721）为每个 Agent 铸造唯一的链上身份。

**o 功能：** 包含元数据（Agent 类型、能力声明、模型哈希）、所有者地址以及声誉注册表。

**o 声誉系统：** 防止刷分。只有发生过链上交互（支付过 Gas 费）的地址才能评价，评价不可篡改且具有时间衰减性（近期的评价权重更高）。

**o 可验证性：** 结合 TEE（可信执行环境）或 ZK（零知识证明），确保 AI 确实运行了特定的模型代码。

**· HTTP 402（支付协议）：** 复活了 HTTP 协议中保留的 “402 Payment Required” 状态码。它允许在 API 调用层直接嵌入支付逻辑。Agent 可以在一次交互中自动完成“请求资源 -> 支付 -> 获取数据”的闭环，支持微支付和批量结算。

**4\. Spoon OS：AI Agent 的 Web3 框架**

Rick 团队开发了 **Spoon OS**，这是一个类似于“AI 安卓系统”的开发框架，旨在降低开发者将 AI 接入 Web3 的门槛。它将复杂的底层技术（私钥管理、签名、链上交互）封装成简单的 API。  
Spoon OS 的架构分为四层：

**· 数据层：** 负责数据的采集、清洗、向量化存储（支持 Vector DB），为 AI 提供记忆。

**· 内核层（Kernel）：** 统一的 LLM 接口（适配 OpenAI、Claude 等），包含任务编排引擎和上下文管理。

**· 协议层（Protocol）：** 核心层。实现了 ERC-8004 身份管理和 HTTP 402 支付网关。提供私钥托管（支持 TEE 安全存储），确保开发者无需直接触碰私钥即可让 AI 拥有钱包功能。

**· 行动层（Action）：** 负责与外部世界交互，连接 DeFi 协议（如 Uniswap）、跨链桥或其他 Web2 API。

**5\. 未来展望：Agent 经济**

Rick 预测，未来的经济主体将从人类转向 AI Agent。Coinbase 的数据显示，稳定币的月交易量已超过 Visa，其中大量交易来自 Bot 和 DeFi 自动化。随着 Agent 渗透率从 2024 年的不到 1% 增长到 2028 年预期的 33%，机器与机器之间（M2M）的高频、微额交易将爆发。区块链将成为承载这一海量交易的各种 Agent 的基础设施。

**6\. 问答环节重点 (Q&A)**

**· AI 交易是否必须上链？**

o 如果是个人自用的量化策略，可以使用中心化交易所托管。但如果是面向公众募资或需要建立信任的基金（如链上对冲基金），则必须拥有链上身份以实现资金透明和逻辑可验证。

**· Web3 的效率问题：**

o 虽然区块链 TPS 可能低于支付宝，但在**跨境结算**和**去中介化**方面效率极高。Web2 依赖中心化服务器和繁琐的银行合规流程，而 Web3 实现了全天候、无国界的自动化结算。

**· 声誉系统防作恶：**

o 当前声誉绑定在 Agent 的 NFT ID 上。虽然开发者可以放弃坏名声的 Agent 重新部署，但重新积累声誉和链上交互历史的成本很高（Gas 费）。

**· Spoon OS 的定位：**

o 它目前是一个 SDK/开发框架，帮助开发者快速构建具备 Web3 能力的 Agent。未来目标是发展成一个 Agent 网络生态。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->



今日准备继续学习solidity基础语法：

```
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.21;
contract helloweb3{
    string public _string = "Hello Web3";
}

contract Function {
    uint public number = 5;
    function add(uint _number)public pure  returns(uint newnumber){
        newnumber=_number+1;
    }

    function returnMuti() public pure returns(uint,bool,uint256[3] memory){
        return (1,true,[uint256(1),2,4]);
    }
}

contract DataStorage{
    //storge>memory>calldata  #消耗gas
    function fcalldata(uint[] calldata _x) public pure returns(uint[] calldata){
        return (_x);
    }

uint[] x = [1,2,3]; // 状态变量：数组 x

function fStorage() public{
    //声明一个storage的变量 xStorage，指向x。修改xStorage也会影响x
    uint[] storage xStorage = x;
    xStorage[0] = 100;
}
}

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
contract c {
  struct Students{
    uint id;
    uint score;
  }
  Students  public s;
  function st() public  {
    Students storage _st=s;
    _st.id=10;
    _st.score=100;
   
  }
  
}

contract map {
    mapping (uint => address) public i;
    function p(uint _id,address _address)public {
        i[_id]=_address;
    }
}

contract test {
    function iF(uint _number)public pure returns(bool){
        if(_number==0){
            return (true);
        }else{
            return(false);
        }
    }

    function For() public pure returns(uint256){
        uint sum=0;
        for(uint i = 0;i<10;i++){
            sum+=i;
        }
        return (sum);
    }

    function While() public pure returns(uint256){
        uint sum =0;
        uint i =0;
        do{
            sum += i;
            i++;
        }while(i<10);
        return(sum);
    }
}
```
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->





今日继续阅读web3实习手册，参加了线上co-learning和Bruce老师的Web3运营原理分享

俩会议中，co-learning探讨了DAO组织的治理，钱的影响是否违背了去中心化的初衷；晚上的分享会议上，Bruce老师介绍了web3的技术原理，我了解许多新名词如RPC，EIP，ERC，EVM。

另外我还自学了solidity语法，今日学习了error，interface等部分

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
interface ICounter {
    function count() external view returns (uint256);
    function increment() external;
}

contract Counter is ICounter{
    uint256 public count;
    function increment() external override{
        count +=1;
    }
}

contract MyRobot{
    function call(address _counterAddress) external {
        ICounter counterScript = ICounter(_counterAddress);
        counterScript.increment();
    }

}
```
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->









今天是web3实习的第一天，今日学习内容：阅读web3实习手册,阅读《021学习以太坊》，参加emily老师的行业分享会议以及以太坊中文周会

之前有过对web3行业碎片化的了解，今天通过上述资源进一步系统性全面认识了web3，内容包括：以太坊，比特币，行业赛道概览，以及就业现状；

会议纪要如下图：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/barryxu-0410/images/2026-01-12-1768228159593-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
