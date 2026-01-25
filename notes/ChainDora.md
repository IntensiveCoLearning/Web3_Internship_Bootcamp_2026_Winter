---
timezone: UTC+8
---

# Girasol

**GitHub ID:** ChainDora

**Telegram:** @LewisCAD

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# ZK Vote 学习笔记

## 1\. 投票方式对比

| 方式 | 优点 | 缺点 |
| --- | --- | --- |
| 传统链上投票 | 公开透明、不可篡改、智能合约自动计票、端到端可验证 | 隐私缺失，投票记录与钱包地址永久绑定，容易关联真实身份 |
| ZK 投票 | 可验证且匿名，保护隐私，防止重复投票 | 技术复杂度高，Proof 生成耗时，需本地计算 |

## 2\. 零知识证明 基础

-   **核心矛盾**：既要保证投票可验证，又要保护投票者匿名。
    
-   **模型**：
    
    -   Prover（证明者）
        
    -   Verifier（验证者）
        
    -   Statement（语句）
        
    -   Witness（见证）
        
-   **三大性质**：
    
    -   完备性：合法投票不会被拒绝
        
    -   可靠性：无法作弊
        
    -   零知识性：不泄露身份与投票内容
        

## 3\. ZK 投票流程

1.  本地生成身份秘密 `identitySecret` 与承诺 `identityCommitment`
    
2.  加入提案，将承诺写入选民集合的 Merkle 树
    
3.  本地生成零知识证明（约 2–5 秒）
    
4.  提交投票交易，合约验证 proof 与 nullifier
    
5.  链上记录结果，可审计但不泄露身份与选票
    

## 4\. 技术细节

-   **zk-SNARK**：常用证明系统（Groth16、PLONK、Halo2）
    
-   **Merkle 树**：存储选民集合
    
-   **Nullifier**：防止重复投票
    
-   **电路约束**：定义投票逻辑
    
-   **Setup 阶段**：生成证明/验证密钥
    
-   **Proof 验证**：合约验证 proof 与 nullifier
    

## 5\. 工程实践与常见的问题

-   Proof 生成较慢，需本地计算
    
-   交易 pending：可能因网络拥堵或 Gas 不足
    
-   Proof 失败：可能因身份丢失、数据不同步或设备性能不足
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

## **ERC721笔记**

NaN.  对于实例化的差异，
      
      ```
      IERC20 public token;
      ERC721 public nft;
      ```
      
      -   **ERC20 接口：** ERC20 标准非常简单，包含 transfer、approve、`transferFrom` 等基本方法。大多数情况下，我们只需要这些标准方法，因此直接用 IERC20 接口就足够了。接口（interface）只定义函数签名，不包含具体实现，这样可以与任何符合 ERC20 标准的合约进行交互，而不关心其内部实现细节。
          
      -   **ERC721 实现：** 对于 ERC721 来说，虽然也有对应的接口（如 IERC721），但在实际开发中，我们往往需要调用一些额外的功能（例如 `tokenURI`、metadata 扩展函数等），这些功能通常是在`OpenZeppelin` 提供的 ERC721 实现中定义的。
          
          -   如果你只需要最基本的 NFT 功能（比如`transferFrom、ownerOf`等），使用 IERC721 接口也是可以的。
              
          -   但在很多案例中，开发者倾向于直接使用 ERC721 实现，因为这样可以直接访问例如`tokenURI`这样的扩展方法，而不必额外编写代码去处理。
              
          -   ERC20合约中\_totalSupply是对应铸造给所有代币的合约
              
          -   对于ERC20的授权代币和对应的铸造代币
              
      
      总结来说，使用 IERC20 接口是因为 ERC20 的功能非常标准且简单，而对 ERC721 而言，直接使用具体实现（如 `OpenZeppelin`的 ERC721 合约）能让你利用更多附加功能和扩展（比如元数据处理），从而更好地满足 NFT 市场等应用的需求。
      

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
​
contract NFTmarket  {
// 引入对应的NFT合约
    ERC721 public nft;
    // 引入对应的ERC20合约
    IERC20 public token;
    // 创造对应NFT信息，储存对应的上架是否和价格
    // 其中对应的tokenID为key，对应的上架状态和价格为value
    // 其中的value为一个结构体，储存对应的上架状态和价格
    struct NFTinfo{
        bool isOnsale;
        uint256 price;
    }
    // 创造一个mapping，储存对应的NFT信息
    mapping(uint256 => NFTinfo) public nftinfo;
    // 创造一个mapping，储存对应的NFT的主人
    mapping(uint256 => address) public nftowner;
    // 创建对应上架的事件,记录对应的NFT的ID和价格
    event Onsale(uint256 indexed tokenID,uint256 price);
    // 创建对应下架的事件,记录对应的NFT的ID
    event Offsale(uint256 indexed tokenID);
    // 创建对应购买的事件,NFT的卖家和买家，以及对应的价格
    event Buy(address indexed seller,address indexed buyer,uint256 price);
    // 创建对应的构造函数，引入对应的NFT合约和ERC20合约
    constructor(address _nft,address _token){
        // 引入对应的NFT合约
        nft = ERC721(_nft);
        // 引入对应的ERC20合约
        token = IERC20(_token);
    }
    // 创建对应上架的方法，其中传入的参数因为对应的NFT的ID和价格
    function onsale(uint256 tokenID,uint256 price)external{
        // 首先判断对应的NFT是否存在
        require(nft._exists(tokenID),"NFT does not exist");
        // 然后判断对应的NFT是否已经被上架
        require(!nftinfo[tokenID].isOnsale,"NFT is already on sale"); 
        // 然后判断对应的NFT是否是对应的主人
        require(nft.ownerOf(tokenID) == msg.sender,"You are not the owner"); 
        // 然后判断对应的价格是否大于0
        require(price > 0,"Price must be greater than 0");
// 将合约转移至市场合约
        nft.transferFrom(msg.sender, address(this), tokenID);
        // 然后将对应的NFT的信息储存到对应的mapping中
        nftinfo[tokenID] = NFTinfo(true,price);
        // 设置对应的NFT的主人
        nftowner[tokenID] = msg.sender;
        // 然后发出对应的事件
        emit Onsale(tokenID,price);
    }
    // 创建对应下架的方法，其中传入的参数为对应的NFT的ID
    function offsale(uint256 tokenID)external{
        // 首先判断对应的NFT是否存在D),"NFT does not exist");
        // 然后判断对应的NFT是否已经被上架
        require(nftinfo[tokenID].isOnsale,"NFT is not on sale");
        // 然后判断对应的NFT是否是对应的主人
        require(nft.ownerOf(tokenID) == msg.sender,"You are not the owner");
        // 然后将NFT转移至对应的主人
        nft.transferFrom(address(this), msg.sender, tokenID);
        // 然后将对应的NFT的信息储存到对应的mapping中
        nftinfo[tokenID] = NFTinfo(false,0);
        require(nft._exists(tokenI  
        // 然后将对应的NFT的主人设置为0
        nftowner[tokenID] = address(0);、
        // 然后发出对应的事件
        emit Offsale(tokenID);
    }
    // 创建对应购买的方法，其中传入的参数为对应的NFT的ID
    function buy(uint256 tokenID)external{
        // 首先判断对应的NFT是否存在
        require(nft._exists(tokenID),"NFT does not exist"); 
        // 然后判断对应的NFT是否已经被上架
        require(nftinfo[tokenID].isOnsale,"NFT is not on sale"); 
        // 然后判断对应的NFT是否是对应的主人
        require(nft.ownerOf(tokenID) != msg.sender,"You are the owner"); 
        // 然后判断对应的价格是否大于0
        require(nftinfo[tokenID].price > 0,"Price must be greater than 0");
        // 然后判断对应的授权的代币是否足够
        require(token.allowance(msg.sender, address(this)) >= nftinfo[tokenID].price,"You do not have enough allowance");
        // 先将代币转移给给卖家
        token.transferFrom(msg.sender, nftowner[tokenID], nftinfo[tokenID].price);
        // 然后将NFT转移给对应的买家
        // 此时市场合约是对应的NFT的主人
        nft.transferFrom(address(this), msg.sender, tokenID);
//        // 然后将对应的NFT的信息储存到对应的mapping中
        nftinfo[tokenID] = NFTinfo(false,0);
        // 然后将对应的NFT的主人设置为对应的买家
        nftowner[tokenID] = msg.sender;
        // 然后发出对应的事件
        emit Buy(nftowner[tokenID],msg.sender,nftinfo[tokenID].price);
​
    }
​
}
```

### **1\. 关于 NFT 上架时为何选择转移 NFT 而不是仅仅授权**

**使用授权（Approval）方案：**

-   如果采用 ERC721 的授权方法（例如调用 `setApprovalForAll` 或 `approve`），NFT 实际上依然保留在卖家钱包中，而市场合约只是获得了操作该 NFT 的权限。
    
-   这种方式要求在 NFT 交易达成时，市场合约再通过调用 `transferFrom` 将 NFT 从卖家转移给买家。
    

**直接转移 NFT 的好处：**

-   **托管（Escrow）机制：** 将 NFT 转移到市场合约中后，市场就持有该 NFT。这确保了在 NFT 上架期间，卖家无法把 NFT 转给其他人，也避免了卖家撤销授权的风险。买家在交易时更有信心，因为 NFT 已经由合约托管，确保交易过程的顺利进行。
    
-   **简化逻辑：** 一旦 NFT 被转移到合约，后续交易（买家购买或卖家取消上架）只需要在合约内部操作 NFT，而不需要担心授权撤销或外部干预的问题。
    
-   **安全性：** 托管的方式防止了卖家在 NFT 上架后再私下转移 NFT，从而导致交易无法完成的情况。
    

**总结：** 虽然通过授权也能让市场合约操作 NFT，但直接转移 NFT 到合约中（即采用托管方式）能更好地保证交易双方的安全性和交易的确定性。这是很多 NFT 市场设计中常见的一种模式。

* * *

### **2\. 关于购买 NFT 时为何检查买家的代币授权而非仅检测余额**

**ERC20 代币转账机制：**

-   ERC20 标准中，转账函数 `transfer` 是直接从调用者余额中扣除；而 `transferFrom` 则需要先由用户通过 `approve` 授权给调用者一定额度的代币，才能由调用者从用户账户中扣除对应金额。
    
-   当市场合约调用 `token.transferFrom(msg.sender, seller, price)` 时，除了确保买家余额足够外，还必须确保买家已通过 `approve` 授权市场合约能够扣除至少 `price` 数量的代币。
    

**为什么不能只检测余额：**

-   **授权机制是必需的：** 即使买家的余额足够，如果买家没有调用 `approve` 给予市场合约足够的支出额度，`transferFrom` 操作也会失败。ERC20 设计的核心就在于分离“余额”和“授权额度”，从而确保用户明确许可第三方可以支配他们账户中的资金。
    
-   **安全性与控制：** 通过检查 `allowance`（授权额度），合约可以提前拒绝交易，提示买家先执行授权操作。这样可以避免因余额充足但未授权而导致交易失败的情况，同时让用户更清楚地了解他们已经同意让合约扣款的具体额度。
    

**总结：** 检测买家的代币授权额度是 ERC20 标准操作流程中的必要步骤，因为只有授权了相应额度，市场合约才能安全调用 `transferFrom` 执行转账操作。仅检测余额无法保证合约有权限支配买家的代币，因此授权检查是必不可少的。

* * *

总体来说，这两种设计选择都出于安全和交易流程可控的考虑：

-   **托管 NFT** 保证了 NFT 在上架期间不会被其他途径转走。
    
-   **授权检查** 确保市场合约在扣款时有足够的权限，从而使交易流程更加顺畅和安全。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


-   figma对于平行元素只是部分元素不相同的部分只需要先将其中的一个元素建立好，部分的内容再做修改
    
-   平行图标的使用可以统一对应的大小，间距等
    
-   除了状态栏之外，下方通常会设置一个对应的标题栏大约是44px
    
-   双击对应的图标再加上对应的点就可以画出对应的不规则的图形
    
-   选中对应的元素，如底部的导航栏，在对应的position中选中对应的约束在下部分就可以让元素吸附于底部的边框
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->



-   figma实际操作过程中一些状态栏和对应的导航栏始终没有变化，我们直接从原生拿过来锁死即可，锁死的时候，在右侧选择对应的元素
    
-   按住对应的ctrl键加上对应的上下左右可以快速的调节对应的大小
    
-   标尺的使用，两边各预留16px的宽度（非必须）
    
-   选中多个元素（或图标）可以统一对多个元素的尺寸，同时可以将多个元素设置于一排之中
    
-   蒙版对应的操作可以将溢出的内容进行对应的切割
    
-   对于画面的阴影可以通过设置对应的负数值达到对应的效果
    
-   渐变色同样也可以在对应的属性栏中去设置
    
-   部分元素只需要使用对应的边框加入即可达成对应的效果
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->




## 字体的设置

> figma的字体会遵循前端字体的设计逻辑

-   字重等元素，都需要选中对应的字体才可以把设置，如中文选择对应的alibb这种
    
-   行高的设置使用默认就好，不可以使用对应的行高不可以低于对应的行高
    
-   layout的样式，可以对文字内容的排版设置对应的限制（如固定的宽度，固定的高高宽）
    
-   同时可以选择对应的对齐方式，其中有些需要选择对应的
    

## 图片和蒙版

> 导入图片其实也就相当于导入对应的矩形，设置基本逻辑保持一致

### 导入图片的几种方式

1.  直接复制粘贴，再调节对应的尺寸至合适即可
    
2.  选中对应的容器之后在导入（粘贴），图片会根据对应的容器的大小进行对应的缩放和裁切
    
    -   同时注意需要填充进对应的图像模型，如果填入对应的section的话只会放置在section的的居中位置
        
3.  对应的图片有时也需要通过不同的裁切等等操作，选择对应的图片然后再使用crop操作
    

### 蒙版的使用

需要将原来的素材和当前进行一个重叠，需要将对应展示的轮廓叠入对应的位置，然后选择`use as mask` 即可，同时我们在调整对应的位置时，按住shift或者选择对应的模块即可
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





右侧即为**属性栏**为详细的一些调整其中有对应的design，prototype，也就是对应的静态设计和原型模式，一般我们会先设计出对应的静态网页，如一些钱包界面，转账，出块这种，然后通过对应的图标和连线使整个过程可以串联起来，同时上方还有基本的演示按钮，如果设计出原型即可使用对应的功能

![b3f380b27ecd20b7230c37f2e966d564.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ChainDora/images/2026-01-20-1768924120711-b3f380b27ecd20b7230c37f2e966d564.png)

-   使用alt键的同时悬浮至其他元素会显示出对应的距离
    

## 图层

-   可以同时选中后多个图形后右击选择对应的group selection，同时对多个图形进行对应的操作
    
-   如果要选择对应的图形直接使用长按对应的ctrl就好悬浮鼠标就好
    
-   同时有时也会有元素的重复，同级的元素该改变对应的顺序即可
    
-   如果几个对应的元素使用对应的group分组，如果去调节对应的组的大小对应内部的元素会变得
    
-   同时也可以修改对应的名称
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






# figma基础学习

## 优势

外界：figma的设计基于云端，带来了协作，共享等优势

-   通过远端的服务器运行程序和储存数据
    

产品力：自动布局 聚合相关的开发模式，官方社区等等

> figma没事对应的中文版本

## 软件界面

![2de22d74089eccbf1985650464ce2f26.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ChainDora/images/2026-01-19-1768831382141-2de22d74089eccbf1985650464ce2f26.png)

总体分为三个部分，左侧为对应的的元素，例如这张图中左侧不同的页面选中不同的元素，当我们选中对应的元素的时候，即可在右侧进行基本的编辑

中间部分为**画布**，同时当开始时使用f选择对应的电脑比例，手机都是可以进行修改的，，下方为基本的工具栏，基本工具，图标，图形的，基本工具栏的右侧为对应的不同的模式，分别为草稿，设计以及对应的开发者模式

![ec7601ae2b3af545c9b16a608e1220bf.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ChainDora/images/2026-01-19-1768831407588-ec7601ae2b3af545c9b16a608e1220bf.png)

-   其中的scale（缩放）功能用于选中对应的图标
    
-   其中的section（分区）用于将一些模块进行整合
    
-   其中图形在使用的过程中，结合shift键会有不一样的功能
    

总体分为三个部分，左侧为对应的的元素，例如这张图中左侧不同的页面选中不同的元素，当我们选中对应的元素的时候，即可在右侧进行基本的编辑

中间部分为**画布**，同时当开始时使用f选择对应的电脑比例，手机都是可以进行修改的，，下方为基本的工具栏，基本工具，图标，图形的，基本工具栏的右侧为对应的不同的模式，分别为草稿，设计以及对应的开发者模式

-   其中的scale（缩放）功能用于选中对应的图标
    
-   其中的section（分区）用于将一些模块进行整合
    
-   其中图形在使用的过程中，结合shift键会有不一样的功能
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







## Web3 行业简历撰写指导总结

### 1\. 简历理念

-   **去中心化思维**：简历应像区块链一样透明、可追溯、可量化。
    
-   **价值来源**：链上贡献、社区参与度、可验证成果。
    
-   **三大核心**：价值（持续产出）、稀缺性（独特技能组合）、贴合度（与岗位需求匹配）。
    

### 2\. 推荐简历结构

-   **个人信息**：Telegram、Twitter、GitHub、Discord、邮箱、电话等。
    
-   **学历**：相关课程与知识体系可重点突出。
    
-   **经历**：精选 3 段以内，采用 **STAR 模型**（情境-任务-行动-结果）。
    
-   **技能栈 & 工具**：列出 Web3 技能并附项目链接。
    
-   **社区与 DAO 角色**：展示参与角色及投票/提案证明。
    
-   **奖项**：黑客松等官方背书奖项。
    

### 3\. 三板斧核心观点

-   **链上战绩 > 宏大叙事**：避免空泛描述，强调量化成果与证据。
    
-   **参与度曲线 > 从属关系**：展示持续贡献和成长路径，而非仅公司头衔。
    
-   **技能—结果双链路**：技能与实战成果结合更具说服力。
    

### 4\. 写作要领

-   **定位公式**：目标岗位 + 细分赛道 + 最大卖点。
    
-   **关键贡献**：动作 + 量化结果 + 引证，控制在 2 行以内。
    
-   **STAR 模型**：突出个人贡献与量化结果。
    
-   **技能 & 工具**：技能旁边挂作品链接。
    
-   **社区角色排序**：Core Contributor > Lead > Contributor > Bounty Hunter。
    

### 5\. 量化指标参考

-   **运营向**：社区成员增长、活动参与率、内容传播量。
    
-   **用户增长**：钱包数、交互交易量、活跃用户数。
    
-   **技术向**：合约部署次数、审计结果、代码提交量。
    
-   **注意事项**：简历控制在 1–2 页，关键贡献前置，统一格式。
    

### 6\. 模板与工具

-   使用在线简历生成器（如 Canva 模板）。
    
-   保持简洁，突出岗位相关技能与经验。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->








## Web3 求职全流程要点总结

### 1\. 招聘平台选择黄金法则

-   **及时**：岗位仍在开放（避免过期信息）。
    
-   **真实**：岗位确实存在，不是虚假编造。
    
-   **准确**：岗位描述、薪资、地点、面试流程与实际情况匹配。
    

### 2\. 推荐的优质招聘平台

-   [**Web3Career.build**](http://Web3Career.build)：官方实习计划平台，适合系统参与。
    
-   [**Web3.career**](http://Web3.career)：全球性 Web3 招聘平台，职位更新快、覆盖广。
    
-   **DeJob**：专注 Web3 与远程招聘，国内项目更新快。
    
-   **SmartDeer**：新兴平台，注重人才质量与项目可靠性。
    

### 3\. 其他信息渠道

-   **LXDAO**：社区 DAO，提供招聘与 Bounty 岗位。
    
-   **ETHPanda**：华语区与全球以太坊生态桥梁，定期发布机会。
    
-   **官方社区**：Discord / Telegram。
    
-   **Twitter(X)**：关注 KOL、创始人、招聘专家。
    
-   **LinkedIn**：成熟公司与转型部门常用。
    
-   **行业媒体与研究报告**：CoinDesk、The Block、Messari、Nansen。
    
-   **线下活动 / AMA**：会议、Meetup、线上问答。
    

### 4\. 项目方可靠性评估（满分 120 分）

-   12 项指标：愿景、声誉、创始人背景、上市情况、办公地点、领英可检索性、市值排名、创始年份、合规性、价值观、投资机构支持、社区运营情况。
    
-   **及格线：72 分**，低于需谨慎。
    

### 5\. 常见招聘骗局特征

-   要求先付费（培训费/押金/代币）。
    
-   要求安装不明软件或插件。
    
-   公司信息无法核验。
    
-   岗位描述宽泛且承诺高回报。
    
-   要求通过非官方渠道沟通。
    

**自我保护清单**：核验主体、拒绝付费招聘、只用官方渠道、不运行未知程序、双重确认。

### 6\. Web3 职业发展路径

-   **运营向**：
    
    -   大使计划（门槛低，经验积累）。
        
    -   实习生（远程为主，待遇更高）。
        
    -   正式职工：社区经理、市场经理、BD、产品运营、内容创作。
        
-   **技术向**：
    
    -   岗位：核心开发者、智能合约工程师、Dapp 后端、审计师、密码学工程师、DevOps。
        
    -   积累经验方式：参与开源项目、黑客松、构建个人项目、学习课程、阅读白皮书。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









# Web3 合规与法律风险

-   **中国监管态度**：全面禁止金融属性（ICO、交易所、支付工具），有限容忍技术创新。
    
-   **代币发行风险**：ICO/IEO/IDO 等均属非法融资，技术人员参与也可能被追责。
    
-   **赌博/传销/洗钱风险**：链游、NFT、DAO 等若涉及抽奖、返利、跨境换汇，可能触犯刑法。
    
-   **民商事争议**：虚拟货币交易合同常被认定无效，投资纠纷风险自担。
    

## 全球监管趋势

-   **欧盟 MiCA**：首个全面加密货币监管框架，禁止算法稳定币。
    
-   **美国**：SEC、CFTC 等将多数代币视为证券，重点监管交易所与 DeFi。
    
-   **新加坡/香港**：采取监管沙盒或许可制度，允许创新但要求严格 KYC/AML。
    
-   **FATF Travel Rule**：要求 VASP 在转账时收集并传输用户信息。
    
-   **稳定币监管**：防范系统性风险、货币政策冲击，确保储备资产支持。
    

## Web3 入职风险

-   **雇佣关系**：境外注册公司常无法签订有效劳动合同，缺乏社保公积金保障。
    
-   **薪酬结构**：常见“人民币+Token”或“全 USDT”，可能导致工资支付无效或权益受损。
    
-   **虚拟货币出金风险**：C2C 出金易卷入涉赌涉诈资金，可能触发“帮信罪”。
    
-   **项目合法性审查**：需核查白皮书、Token 分发机制、返利结构等，避免被牵连。
    

## 常见刑事风险与案例

-   **开设赌场罪**：链游带提现功能易被认定为赌博平台。
    
-   **非法经营罪**：虚拟货币换汇、支付结算可能构成非法经营。
    
-   **非法吸收公众存款罪**：挖矿返利、代币投资模式常触发。
    
-   **传销罪**：层级返利、发展下线的链游或平台可能涉嫌传销。
    
-   **洗钱罪**：虚拟货币跨境兑换常被用于掩饰犯罪资金。
    

## 网络安全风险与防护

-   **钓鱼攻击**：伪造面试软件、奖学金空投、假客服。
    
-   **木马/恶意软件**：剪贴板劫持、浏览器插件后门、远程控制。
    
-   **社交工程**：冒充 HR/好友诱导转账或泄露信息。
    
-   **供应链攻击**：恶意插件、开源库后门。
    
-   **账号安全**：弱密码、邮箱/SIM 劫持、缺乏 2FA。
    
-   **防护清单**：只用官方会议工具、谨慎安装软件、启用 2FA、冷钱包存储、定期检查授权。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->










## Web3 社区运营指南要点

### 一、社区运营核心职责

-   **日常内容与社群维护**：定期更新社媒内容，维持活跃度；管理秩序、答疑、打击垃圾信息；设计话题引导讨论。
    
-   **内容发布与互动引导**：活动预热公告、AMA 宣传、Twitter Space 提醒；推送品牌相关内容，引导讨论与转发。
    
-   **活动策划与组织**：线上（Twitter Space、课程、黑客松）、线下（Meetup、Workshop）；涵盖策划、执行、复盘全流程。
    
-   **对外合作与社区联动**：联合 AMA、跨社区活动；与 KOL、媒体保持合作，扩大曝光。
    

### 二、常用工具与平台

-   **社交媒体渠道**：Twitter/X（主阵地）、微信公众号、Medium/Substack。
    
-   **社群沟通平台**：Discord（分频道管理）、Telegram（轻量群聊）、微信群（中国市场常用）。
    
-   **内容创作工具**：Notion、ChatGPT、Figma/Canva、Tally/Typeform。
    
-   **数据与行业调研工具**：Etherscan、Dune Analytics、CoinGecko/CMC、Token Terminal。
    

### 三、任务案例模板

-   **线下活动策划模板**：涵盖策划（主题、目标、预算、合规）、筹备（嘉宾邀约、宣传设计、物资准备）、执行（场地布置、签到、协调）、复盘（数据分析、报告总结）。
    

### 四、关键实践要点

-   活动需明确目标与受众，量化预期效果。
    
-   宣传节奏要分阶段推进，多渠道联动。
    
-   嘉宾邀约与物料设计需提前确认。
    
-   活动后及时产出总结内容并进行数据分析。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->











## Web3 四大核心领域

-   **DeFi（去中心化金融）**
    
    -   代表案例：Uniswap（自动做市商 AMM）、Compound（去中心化借贷）、MakerDAO/Sky（稳定币系统）。
        
    -   特点：无需中介，智能合约自动执行，流动性池与手续费分配机制。
        
    -   风险点：价格波动可能触发清算。
        
-   **NFT（非同质化代币）**
    
    -   本质：数字资产的唯一性与所有权。
        
    -   技术：区块链记录 + 智能合约自动化交易与版税。
        
    -   案例：CryptoPunks（先锋收藏品）、OpenSea（最大交易平台）。
        
-   **DAO（去中心化自治组织）**
    
    -   社区治理，无需传统公司架构。
        
    -   案例：Nouns DAO（NFT艺术社区）、LXDAO（支持 Web3 公共物品）、ConstitutionDAO（竞拍美国宪法原稿）。
        
    -   优势：公开透明、社区驱动；劣势：隐私与资金安全挑战。
        
-   **MEME 币**
    
    -   特点：社区驱动、文化属性强，投机性高。
        
    -   案例：DOGE（狗狗币）、PEPE（青蛙币）、SHIB（柴犬币）。
        
    -   风险提醒：高度波动，依赖情绪与名人效应，需控制仓位。
        

## 交叉创新

-   **DeFi + NFT**：NFT 抵押借贷、碎片化流动性（如 BendDAO、Sudoswap）。
    
-   **DAO + MEME**：社区文化与治理结合（如 ShibaSwap、FriendTech）。
    
-   **AI + DeFi**：智能化金融服务（Yearn Finance V3、Gauntlet、Chaos Labs）。
    
-   **Web3 + 乡建**：南塘 DAO 探索乡村自治与区块链结合。
    

## 未来趋势

1.  **Intent-Based 交易**：意图驱动的自动化交易（UniswapX、1inch Fusion、CoW Protocol）。
    
2.  **账户抽象与智能钱包**：ERC-4337 标准，支持 Gas 代付、社交恢复（Safe、Argent）。
    
3.  **模块化区块链**：分层架构提升扩展性（Celestia、Polygon CDK、OP Stack）。
    
4.  **AI + Web3 融合**：去中心化 AI 训练、预测市场、AI 创作（[Fetch.ai](http://Fetch.ai)、SingularityNET）。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->












## 以太坊学习要点

### 1\. 基本介绍

-   以太坊是“区块链 2.0”，不仅是加密货币 ETH，更是支持智能合约的全球共享计算机。
    
-   由 Vitalik Buterin 在 2013–2014 年提出，2014 年正式启动。
    
-   ETH 是全球市值第二的加密货币，仅次于比特币。
    

### 2\. 核心创新

-   **智能合约**：自动执行代码逻辑，无需人工干预。
    
-   支持 DeFi、NFT、DAO 等应用生态。
    
-   交易需支付 **Gas 费**，类似燃料费用。
    

### 3\. 与比特币的差异

| 维度 | 比特币 | 以太坊 |
| --- | --- | --- |
| 定位 | 数字货币，强调稀缺性 | 去中心化平台，支持智能合约 |
| 编程能力 | 脚本有限 | 图灵完备语言（Solidity） |
| 共识机制 | PoW 挖矿 | 已转向 PoS（The Merge） |
| 区块速度 | 10 分钟 | 约 12 秒 |
| 经济模型 | 总量固定 | 灵活，可能通缩 |

### 4\. 演进与升级

-   **以太坊 1.0**：PoW 挖矿，能耗高，TPS 低。
    
-   **The Merge (2022)**：转向 PoS，能耗降低 99.95%。
    
-   **未来路线图**：
    
    -   **分片技术**：数据分片 + Layer 2，预计 2025–2026 启动。
        
    -   **EIP-4844 (2024 已上线)**：Blob 交易降低 L2 成本。
        
    -   **ZK-Rollup**：零知识证明提升效率与安全。
        
    -   其他优化：Verkle 树、EVM 性能提升。
        

### 5\. 生态系统

-   **L1 主网**：安全与共识。
    
-   **L2 扩展**：Rollup（Arbitrum、Optimism、zkSync Era 等）。
    
-   **侧链**：Polygon PoS、xDAI。
    
-   **应用层**：Uniswap、Aave、OpenSea、MetaMask 等。
    
-   **协议层**：EVM、Gas 机制、客户端软件。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













## 区块链基础概念

### 一、区块链是什么

-   去中心化的分布式账本技术，记录不可篡改、透明的交易数据
    
-   区块由交易信息 + 上一区块哈希组成，按时间顺序串联
    
-   特性：不可篡改、公开透明但匿名、交易快速
    

### 二、比特币与激励机制

-   节点提供服务可获得奖励（如比特币）
    
-   比特币具备货币属性：有限供应、可自由转账
    
-   矿工通过验证交易、打包区块获得代币奖励和手续费（Gas Fee）
    

### 三、区块链核心组成

-   **去中心化网络**：全球分布的节点共同维护账本
    
-   **区块链本身**：记录所有交易信息
    
-   运行流程：用户发起交易 → 广播 → 节点验证 → 打包成块 → 上链 → 奖励发放
    

### 四、公链 vs 联盟链 vs 私链

-   **公链**：完全开放，任何人可参与，透明但效率低（如比特币、以太坊）
    
-   **联盟链**：由多方机构共同管理，效率较高，隐私较好（如银行联盟）
    
-   **私链**：由单一机构控制，效率高、隐私强，但缺乏透明性（企业内部应用）
    

### 五、Web2 / Web 3.0 / Web3 对比

-   **Web2**：中心化平台，用户生产内容但不拥有数据
    
-   **Web 3.0**：语义网，强调数据结构化与语义关系
    
-   **Web3**：去中心化互联网，用户掌握数据与资产，智能合约自动执行
    
-   对比矩阵：
    
    -   控制权：Web2 平台垄断 → Web 3.0 部分开放 → Web3 用户自治
        
    -   数据存储：中心服务器 → 混合存储 → 区块链/IPFS
        
    -   支付系统：信用卡/支付宝 → 集成支付 → 加密货币
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
