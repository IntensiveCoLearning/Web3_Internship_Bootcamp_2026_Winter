---
timezone: UTC+8
---

# ISyaxinLiu

**GitHub ID:** ISyaxinLiu

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
在传统的 [ERC20](https://learnblockchain.cn/tags/ERC20?map=EVM) 代币交互中，用户如果想让第三方合约使用自己的代币，需要先调用 `approve` 函数进行授权，这会消耗 [Gas](https://learnblockchain.cn/tags/Gas?map=EVM) 费用。ERC20 Permit 标准（[EIP-2612](https://learnblockchain.cn/docs/eips/EIPS/eip-2612/)）通过引入链下签名机制，允许用户使用签名来授权代币使用权，从而实现"无 Gas"授权。

## **什么是 ERC20 Permit？**

ERC20 Permit 是 ERC20 的扩展标准，它添加了一个 `permit` 函数，允许用户通过签名来批准代币支出，而不需要发送交易。

### **传统方式 vs Permit 方式**

**传统方式（两笔交易）**：

```
// 交易 1：用户授权（消耗 Gas）
token.approve(spender, amount);

// 交易 2：spender 使用授权
token.transferFrom(user, recipient, amount);
```

**Permit 方式（一笔交易）**：

```
// 链下：用户创建签名（不消耗 Gas）
const signature = await user.signPermit(...);

// 链上：spender 使用签名完成授权和转账
token.permit(user, spender, amount, deadline, v, r, s);
token.transferFrom(user, recipient, amount);
```

![](https://img.learnblockchain.cn/pics/20251227184925.png)

使用 Permit 后，用户无需单独为授权支付 Gas，还将两笔交易减少到一笔。

## **工作原理**

ERC20 Permit 背后的核心机制是 **链下签名 + 链上验证**：

1.  **用户在链下**使用私钥对授权信息进行签名（不消耗 Gas）
    
2.  **签名被提交给**第三方或合约
    
3.  **合约在链上**验证签名的有效性并执行授权
    

在以太坊中，每笔交易都需要用私钥签名，网络通过密码学验证来识别 `msg.sender`。Permit 利用了同样的原理，但将签名用于授权而不是交易本身。

### **approve vs permit 函数对比**

传统的 `approve` 函数：

```
// 传统的 approve 函数
function approve(address spender, uint256 amount) external returns (bool) {
    allowance[msg.sender][spender] = amount;
    emit Approval(msg.sender, spender, amount);
    return true;
}
```

而 `permit` 函数与 `approve` 功能相同，但通过签名验证来实现授权：

```
// permit 函数：通过签名授权（EIP-2612 标准）
function permit(
    address owner,       // 代币持有者
    address spender,     // 被授权者
    uint256 value,       // 授权额度
    uint256 deadline,    // 签名过期时间
    uint8 v,            // 签名参数
    bytes32 r,          // 签名参数
    bytes32 s           // 签名参数
) external {
    // 验证签名和过期时间...
    allowance[owner][spender] = value;
    emit Approval(owner, spender, value);
}
```

**关键区别**：

-   `approve`：通过 `msg.sender` 直接授权，需要用户支付 Gas
    
-   `permit`：通过签名验证 `owner` 身份，任何人都可以提交签名（代付 Gas）
    

本质上，Permit 是一种 **授权委托模式**：用户创建授权签名，其他人可以使用这个签名来执行授权操作。

Permit 基于 EIP-712 结构化数据签名，[EIP-712](https://learnblockchain.cn/article/22662) 定义了一种标准化的方式来对结构化数据进行签名。

> 深入了解 EIP-712： [https://learnblockchain.cn/article/22662](https://learnblockchain.cn/article/22662)

### **EIP-2612 标准**

[EIP-2612](https://learnblockchain.cn/docs/eips/EIPS/eip-2612/) 将 EIP-712 应用到 ERC20 代币上，定义了标准的 `permit` 函数接口。

**接口定义**：

```
interface IERC20Permit {
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    function nonces(address owner) external view returns (uint256);
    function DOMAIN_SEPARATOR() external view returns (bytes32);
}
```

**历史背景**：  
Permit 最早由 MakerDAO 的 [Dai 稳定币](https://github.com/makerdao/dss/blob/master/src/dai.sol)实现，后来被标准化为 EIP-2612。现在推荐使用 [OpenZeppelin](https://learnblockchain.cn/tags/OpenZeppelin?map=EVM) 的 `ERC20Permit` 实现，它完全符合 EIP-2612 标准。

## **Permit 实现剖析**

让我们深入了解 Permit 的实现细节。一个完整的 Permit 实现包含以下核心组件：

1.  `DOMAIN_SEPARATOR`：域分隔符，唯一标识合约
    
2.  `PERMIT_TYPEHASH`：许可类型哈希，标识函数签名
    
3.  `nonces`：防重放攻击的计数器
    
4.  `permit` **函数**：验证签名并执行授权
    

### **1\. DOMAIN\_SEPARATOR（域分隔符）**

`DOMAIN_SEPARATOR` 是一个唯一标识智能合约的哈希值，确保签名只在特定合约上有效。

```
// EIP-712 域分隔符
bytes32 private immutable _CACHED_DOMAIN_SEPARATOR;
uint256 private immutable _CACHED_CHAIN_ID;
address private immutable _CACHED_THIS;

bytes32 private immutable _HASHED_NAME;
bytes32 private immutable _HASHED_VERSION;
bytes32 private immutable _TYPE_HASH;

constructor(string memory name, string memory version) {
    bytes32 hashedName = keccak256(bytes(name));
    bytes32 hashedVersion = keccak256(bytes(version));
    bytes32 typeHash = keccak256(
        "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
    );

    _HASHED_NAME = hashedName;
    _HASHED_VERSION = hashedVersion;
    _CACHED_CHAIN_ID = block.chainid;
    _CACHED_DOMAIN_SEPARATOR = _buildDomainSeparator(typeHash, hashedName, hashedVersion);
    _CACHED_THIS = address(this);
    _TYPE_HASH = typeHash;
}

function DOMAIN_SEPARATOR() public view returns (bytes32) {
    if (address(this) == _CACHED_THIS && block.chainid == _CACHED_CHAIN_ID) {
        return _CACHED_DOMAIN_SEPARATOR;
    } else {
        return _buildDomainSeparator(_TYPE_HASH, _HASHED_NAME, _HASHED_VERSION);
    }
}

function _buildDomainSeparator(
    bytes32 typeHash,
    bytes32 nameHash,
    bytes32 versionHash
) private view returns (bytes32) {
    return keccak256(abi.encode(typeHash, nameHash, versionHash, block.chainid, address(this)));
}
```

**组成要素**：

-   `name`：代币名称
    
-   `version`：合约版本（通常为 "1"）
    
-   `chainId`：链 ID（防止跨链重放）
    
-   `verifyingContract`：合约地址
    

**作用**：

-   确保签名只能在特定合约上使用
    
-   防止签名在不同链或不同版本的合约上被重用
    
-   支持链分叉后自动更新（通过动态检查 chainId）
    

### **2\. PERMIT\_TYPEHASH（许可类型哈希）**

`PERMIT_TYPEHASH` 是函数签名的哈希值，明确标识签名用于哪个函数。

```
// EIP-2612 标准 Permit 类型哈希
bytes32 public constant PERMIT_TYPEHASH = keccak256(
    "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
);
```

**作用**：

-   明确签名的目标函数
    
-   包含函数名和所有参数类型及名称
    
-   验证时如果 `PERMIT_TYPEHASH` 不匹配，交易会回滚
    
-   确保签名只能用于预期的功能
    

### **3\. nonces（防重放计数器）**

`nonces` 映射记录每个地址已使用的签名次数，防止签名被重复使用。

```
mapping(address => uint256) public nonces;
```

**工作机制**：

1.  创建签名时，必须包含当前的 `nonce` 值
    
2.  执行 `permit` 时，合约验证提供的 `nonce` 是否与链上记录匹配
    
3.  验证通过后，`nonce` 自动递增
    
4.  这确保每个签名只能使用一次
    

**三重防护**：

通过 `DOMAIN_SEPARATOR`、`PERMIT_TYPEHASH` 和 `nonce` 三个要素，确保签名：

1.  ✅ 只能在指定的合约上使用（`DOMAIN_SEPARATOR`）
    
2.  ✅ 只能用于指定的函数（`PERMIT_TYPEHASH`）
    
3.  ✅ 只能使用一次（`nonce`）
    

### **4\. permit 函数实现**

`permit` 函数是整个机制的核心，它验证签名并执行授权。

```
// EIP-2612 标准 permit 函数
function permit(
    address owner,       // 代币持有者
    address spender,     // 被授权使用代币的地址
    uint256 value,       // 授权额度
    uint256 deadline,    // 签名过期时间戳
    uint8 v,            // 签名的 v 值
    bytes32 r,          // 签名的 r 值
    bytes32 s           // 签名的 s 值
) external;
```

**参数说明**：

-   前 4 个参数：授权信息（owner、spender、value、deadline）
    
-   后 3 个参数：签名数据（v、r、s）
    

你可能会疑惑：为什么签名时需要包含这些参数，验证时又要传一遍？这是因为：

-   签名本身只能恢复出**签名者的地址**
    
-   我们需要用这些参数来**验证签名的内容**是否与提交的参数一致
    

**验证步骤 1：检查过期时间**

首先检查签名是否在有效期内：

```
// 检查签名是否过期
require(block.timestamp <= deadline, "ERC20Permit: expired deadline");
```

**说明**：

-   使用 `deadline` 时间戳防止过期签名被使用
    
-   与 Dai 不同，EIP-2612 不支持 deadline=0 的永不过期选项
    

**验证步骤 2：使用和更新 nonce**

获取并递增用户的 nonce：

```
// 获取当前 nonce 并递增
uint256 currentNonce = nonces[owner]++;
```

**工作机制**：

-   读取 owner 的当前 `nonce` 值
    
-   立即将 `nonce` 加 1
    
-   确保每个签名只能使用一次
    

**验证步骤 3：计算 digest**

计算消息摘要（digest），必须与用户在链下签名时计算的完全一致：

```
// 计算 EIP-712 结构化数据哈希
bytes32 structHash = keccak256(
    abi.encode(
        PERMIT_TYPEHASH,
        owner,
        spender,
        value,
        currentNonce,
        deadline
    )
);

// 计算最终的消息摘要
bytes32 digest = keccak256(
    abi.encodePacked(
        "\x19\x01",           // EIP-191 前缀
        DOMAIN_SEPARATOR(),   // 域分隔符
        structHash            // 结构化数据哈希
    )
);
```

**关键点**：

-   `\x19\x01`：EIP-191 标准前缀，防止签名被用作交易
    
-   所有参数都参与哈希计算
    
-   链下签名和链上验证必须使用完全相同的数据
    

**验证步骤 4：恢复并验证签名者**

使用 `ecrecover` 从签名中恢复地址，并验证是否为 `owner`：

```
// 从签名恢复地址
address signer = ecrecover(digest, v, r, s);

// 验证签名者
require(signer != address(0), "ERC20Permit: invalid signature");
require(signer == owner, "ERC20Permit: invalid signature");
```

**工作原理**：

-   如果 digest、v、r、s 任何一个不正确，恢复出的地址就不会匹配
    
-   这意味着任何参数（包括 `DOMAIN_SEPARATOR` 中的 `chainId`）不匹配都会导致验证失败
    

**调试提示**⚠️：  
签名验证失败时，所有错误都会显示相同的错误信息，这使得调试变得困难。需要仔细检查：

-   `chainId` 是否正确
    
-   `nonce` 是否是最新的
    
-   `DOMAIN_SEPARATOR` 是否匹配
    
-   参数顺序和类型是否正确
    

**验证步骤 5：执行授权**

所有检查通过后，更新 `allowance` 并触发事件：

```
// 执行授权
_approve(owner, spender, value);
```

内部 `_approve` 函数：

```
function _approve(address owner, address spender, uint256 amount) internal {
    require(owner != address(0), "ERC20: approve from the zero address");
    require(spender != address(0), "ERC20: approve to the zero address");

    allowance[owner][spender] = amount;
    emit Approval(owner, spender, amount);
}
```

**优势**：

-   支持指定具体的授权金额（不像早期 Dai 只能全部或零）
    
-   复用 ERC20 的 `_approve` 逻辑，保持一致性
    

## **创建链下签名（TypeScript）**

现在让我们看看如何在链下创建 Permit 签名。推荐使用现代化的 [viem](https://viem.sh/) 库，它提供了更简洁、类型安全的 API。

Viem 提供了内置的 EIP-712 签名支持，可以非常方便地创建 Permit 签名。

```
import { createWalletClient, createPublicClient, http, parseEther } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';
import { mainnet } from 'viem/chains';

// ERC20 Permit ABI（只需要用到的函数）
const permitABI = [
  {
    name: 'permit',
    type: 'function',
    inputs: [
      { name: 'owner', type: 'address' },
      { name: 'spender', type: 'address' },
      { name: 'value', type: 'uint256' },
      { name: 'deadline', type: 'uint256' },
      { name: 'v', type: 'uint8' },
      { name: 'r', type: 'bytes32' },
      { name: 's', type: 'bytes32' },
    ],
  },
  {
    name: 'nonces',
    type: 'function',
    inputs: [{ name: 'owner', type: 'address' }],
    outputs: [{ name: '', type: 'uint256' }],
  },
  {
    name: 'name',
    type: 'function',
    inputs: [],
    outputs: [{ name: '', type: 'string' }],
  },
] as const;

async function createPermitSignature() {
  // 1. 创建账户（user1 - 签名者）
  const user1Account = privateKeyToAccount('0x...' as `0x${string}`);

  // 2. 创建客户端
  const publicClient = createPublicClient({
    chain: mainnet,
    transport: http(),
  });

  const walletClient = createWalletClient({
    account: user1Account,
    chain: mainnet,
    transport: http(),
  });

  // 3. 合约信息
  const tokenAddress = '0x...' as `0x${string}`;
  const spenderAddress = '0x...' as `0x${string}`;
  const amount = parseEther('100'); // 授权 100 个代币

  // 4. 读取代币名称和当前 nonce
  const [name, nonce] = await Promise.all([
    publicClient.readContract({
      address: tokenAddress,
      abi: permitABI,
      functionName: 'name',
    }),
    publicClient.readContract({
      address: tokenAddress,
      abi: permitABI,
      functionName: 'nonces',
      args: [user1Account.address],
    }),
  ]);

  // 5. 设置过期时间（当前时间 + 1 小时）
  const deadline = BigInt(Math.floor(Date.now() / 1000) + 3600);

  // 6. 使用 signTypedData 创建 EIP-712 签名
  const signature = await walletClient.signTypedData({
    account: user1Account,
    domain: {
      name: name,
      version: '1',
      chainId: mainnet.id,
      verifyingContract: tokenAddress,
    },
    types: {
      Permit: [
        { name: 'owner', type: 'address' },
        { name: 'spender', type: 'address' },
        { name: 'value', type: 'uint256' },
        { name: 'nonce', type: 'uint256' },
        { name: 'deadline', type: 'uint256' },
      ],
    },
    primaryType: 'Permit',
    message: {
      owner: user1Account.address,
      spender: spenderAddress,
      value: amount,
      nonce: nonce,
      deadline: deadline,
    },
  });

  // 7. 分解签名为 v, r, s
  const r = signature.slice(0, 66) as `0x${string}`;
  const s = `0x${signature.slice(66, 130)}` as `0x${string}`;
  const v = parseInt(signature.slice(130, 132), 16);

  console.log('签名完成！');
  console.log('Signature:', signature);
  console.log('v:', v);
  console.log('r:', r);
  console.log('s:', s);

  return {
    owner: user1Account.address,
    spender: spenderAddress,
    value: amount,
    deadline: deadline,
    v,
    r,
    s,
  };
}

// 使用示例
createPermitSignature().then(console.log);
```

**提交签名并执行 Permit**

```
import { createWalletClient, createPublicClient, http } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';
import { mainnet } from 'viem/chains';

async function executePermit(permitData: {
  owner: `0x${string}`;
  spender: `0x${string}`;
  value: bigint;
  deadline: bigint;
  v: number;
  r: `0x${string}`;
  s: `0x${string}`;
}) {
  // user2 账户（支付 Gas 的人）
  const user2Account = privateKeyToAccount('0x...' as `0x${string}`);

  const publicClient = createPublicClient({
    chain: mainnet,
    transport: http(),
  });

  const walletClient = createWalletClient({
    account: user2Account,
    chain: mainnet,
    transport: http(),
  });

  const tokenAddress = '0x...' as `0x${string}`;

  // 调用 permit 函数（由 user2 支付 Gas）
  const hash = await walletClient.writeContract({
    address: tokenAddress,
    abi: permitABI,
    functionName: 'permit',
    args: [
      permitData.owner,
      permitData.spender,
      permitData.value,
      permitData.deadline,
      permitData.v,
      permitData.r,
      permitData.s,
    ],
  });

  console.log('Permit 交易已提交:', hash);

  // 等待交易确认
  const receipt = await publicClient.waitForTransactionReceipt({ hash });
  console.log('Permit 已确认:', receipt.status);

  return receipt;
}
```

**关键点**：

-   ✅ **user1** 使用 `signTypedData` 创建签名（链下，免费）
    
-   ✅ **user2** 提交签名并调用 `permit`（链上，支付 Gas）
    
-   ✅ Viem 自动处理 EIP-712 编码和签名
    
-   ✅ 类型安全，减少错误
    

### **在浏览器中使用（配合钱包）**

如果你在前端使用，可以配合用户的钱包（如 MetaMask）：

```
import { createWalletClient, createPublicClient, custom, parseEther } from 'viem';
import { mainnet } from 'viem/chains';

async function requestPermitSignature() {
  // 连接用户的钱包
  const walletClient = createWalletClient({
    chain: mainnet,
    transport: custom(window.ethereum),
  });

  const [address] = await walletClient.getAddresses();
  const tokenAddress = '0x...' as `0x${string}`;
  const spenderAddress = '0x...' as `0x${string}`;

  // 读取 nonce
  const publicClient = createPublicClient({
    chain: mainnet,
    transport: custom(window.ethereum),
  });

  const nonce = await publicClient.readContract({
    address: tokenAddress,
    abi: permitABI,
    functionName: 'nonces',
    args: [address],
  });

  const deadline = BigInt(Math.floor(Date.now() / 1000) + 3600);

  // 请求用户签名
  const signature = await walletClient.signTypedData({
    account: address,
    domain: {
      name: 'MyToken',
      version: '1',
      chainId: mainnet.id,
      verifyingContract: tokenAddress,
    },
    types: {
      Permit: [
        { name: 'owner', type: 'address' },
        { name: 'spender', type: 'address' },
        { name: 'value', type: 'uint256' },
        { name: 'nonce', type: 'uint256' },
        { name: 'deadline', type: 'uint256' },
      ],
    },
    primaryType: 'Permit',
    message: {
      owner: address,
      spender: spenderAddress,
      value: parseEther('100'),
      nonce: nonce,
      deadline: deadline,
    },
  });

  // 返回签名数据
  return {
    signature,
    v: parseInt(signature.slice(130, 132), 16),
    r: signature.slice(0, 66) as `0x${string}`,
    s: `0x${signature.slice(66, 130)}` as `0x${string}`,
  };
}
```

## **实现 ERC20 Permit 合约**

如果你想实现自己的支持 Permit 的 ERC20 代币，推荐使用 [OpenZeppelin](https://learnblockchain.cn/tags/OpenZeppelin?map=EVM) 的实现：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";

contract MyToken is ERC20, ERC20Permit {
    constructor() ERC20("MyToken", "MTK") ERC20Permit("MyToken") {
        _mint(msg.sender, 1000000 * 10**decimals());
    }
}
```

**就这么简单！** `ERC20Permit` 扩展会自动为你的代币添加：

-   `permit` 函数
    
-   `nonces` 映射
    
-   `DOMAIN_SEPARATOR` 计算
    
-   所有必要的签名验证逻辑
    

## **ERC20 Permit 应用场景**

### **1\. DeFi 协议中的一键交易**

```
// 传统方式：用户需要两笔交易
// 交易 1：approve
token.approve(dex, amount);
// 交易 2：swap
dex.swap(token, amount);

// 使用 Permit：只需一笔交易
// 用户在前端签名（免费）
// 在合约封装包装两个调用：
token.permit(user, dex, amount, deadline, v, r, s);
dex.swap(token, amount);
```

### **2\. Gasless 交易（元交易）**

允许新用户无需持有 ETH 就能使用 [DApp](https://learnblockchain.cn/tags/DApp)：

```
contract GaslessTransfer {
    function transferWithPermit(
        IERC20Permit token,
        address from,
        address to,
        uint256 amount,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external {
        // 使用用户的签名授权
        token.permit(from, address(this), amount, deadline, v, r, s);

        // 执行转账
        token.transferFrom(from, to, amount);
    }
}
```

用户只需要签名，Gas 费用由服务提供方承担。

### **3\. 批量操作**

结合 [Multicall](https://learnblockchain.cn/article/22679) 实现更复杂的操作：

```
// 在一笔交易中完成：授权 + 质押 + 领取奖励
function permitAndStake(
    uint256 amount,
    uint256 deadline,
    uint8 v, bytes32 r, bytes32 s
) external {
    // 1. 使用 permit 授权
    token.permit(msg.sender, address(this), amount, deadline, v, r, s);

    // 2. 质押代币
    token.transferFrom(msg.sender, address(this), amount);
    stakes[msg.sender] += amount;

    // 3. 领取之前的奖励
    _claimRewards(msg.sender);
}
```

## **总结**

ERC20 Permit 通过引入链下签名机制，为代币授权带来了革命性的改进：

-   消除了授权步骤的 [Gas](https://learnblockchain.cn/tags/Gas?map=EVM) 成本
    
-   简化交互流程：从两笔交易减少到一笔
    

推荐所有的新发行代币使用 ERC20 Permit，利用 Permit 机制，能够构建更友好、更高效的代币交互体验！
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

## **什么是 ERC20?**

[ERC20](https://learnblockchain.cn/docs/eips/EIPS/eip-20) 是 Ethereum 网络上最出名且应用最广的代币标准之一。它提供了一个统一的接口标准，用于创建可互换代币，这些代币可以用来代表任何事物，从货币到积分等。

该标准定义了一组 API（应用程序编程接口），涉及到代币在智能合约中的转移方式，如何获取数据（比如各[账户](https://learnblockchain.cn/tags/%E8%B4%A6%E6%88%B7?map=EVM)的代币余额），以及如何接收、记录和使用这些代币。

### **ERC20 核心方法和事件**

ERC20 标准主要定义了以下几个方法和两个事件：

方法：

-   `name() public view returns (string)`：可选；返回一个字符串值，表示代币的名称
    
-   `symbol() public view returns (string)`：可选；返回一个字符串值，表示代币的简写或缩写。
    
-   `decimals() public view returns (uint8)`：可选；返回一个 uint8 类型的值，表示代币可以分割到的小数位数。许多代币选择`18`为其小数值，因为这是 Ether(ETH) 使用的小数位数
    
-   `totalSupply() public view returns (uint256)`：返回代币的总供应量
    
-   `balanceOf(address _owner) public view returns (uint256 balance)`：返回特定地址(\_owner)的代币余额
    
-   `transfer(address _to, uint256 _value) public returns (bool success)`：从调用者的地址转移 \_value 量的代币到地址 \_to，成功返回 true
    
-   `transferFrom(address _from, address _to, uint256 _value) public returns (bool success)`：允许 \_spender 从 \_from 转移 \_value 量的代币到 \_to
    
-   `approve(address _spender, uint256 _value) public returns (bool success)`：允许 \_spender 从调用者的账户多次取回总共 \_value 量的代币
    
-   `allowance(address _owner, address _spender) public view returns (uint256 remaining)`：返回 \_spender 仍然被允许从 \_owner 提取的代币数量
    

事件：

-   `Transfer(address indexed _from, address indexed _to, uint256 _value)`：在代币被转移时触发。
    
-   `Approval(address indexed _owner, address indexed _spender, uint256 _value)`：在调用 approve 方法时触发。
    

## **编写一个简单的 ERC20 代币合约**

下面是一个简单的 ERC20 代币合约的实现

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract BaseERC20 {
    string public name; 
    string public symbol; 
    uint8 public decimals; 

    uint256 public totalSupply; 

    mapping (address => uint256) balances; 

    mapping (address => mapping (address => uint256)) allowances; 

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        name = "MyToken"; 
        symbol = "MTK"; 
        decimals = 18; 
        totalSupply = 100000000 * 10 ** uint256(decimals);

        balances[msg.sender] = totalSupply;  
    }

    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];    
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "ERC20: transfer to the zero address");
        require(balances[msg.sender] >= _value, "ERC20: transfer amount exceeds balance");

        balances[msg.sender] -= _value;
        balances[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_from != address(0), "ERC20: transfer from the zero address");
        require(_to != address(0), "ERC20: transfer to the zero address");
        require(balances[_from] >= _value, "ERC20: transfer amount exceeds balance");
        require(allowances[_from][msg.sender] >= _value,"ERC20: transfer amount exceeds allowance");

        balances[_from] -= _value;
        balances[_to] += _value;

        allowances[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowances[msg.sender][_spender] = _value; 
        emit Approval(msg.sender, _spender, _value); 
        return true; 
    }

    function allowance(address _owner, address _spender) public view returns (uint256 remaining) {
        return allowances[_owner][_spender];
    }
}
```

## **使用 OpenZeppelin 创建 ERC20 代币**

[OpenZeppelin](https://docs.openzeppelin.com/contracts/5.x/) 是一个开源的区块链开发框架，它提供了安全的合约模板来简化开发过程。

对于想要创建 ERC20 代币的开发者来说，使用 OpenZeppelin 可以极大地简化开发过程，因为它内置了遵守 ERC20 标准的可复用合约，这些合约经过严格审计，能够减少潜在的安全风险。

安装好 OpenZeppelin 库后（安装方法见前面的 [Hardhat 开发框架](https://learnblockchain.cn/article/22640) 或者 [Foundry 开发框架](https://learnblockchain.cn/article/22641)），你可以开始编写你的 ERC20 代币合约。

OpenZeppelin 提供了一些基础合约，你可以通过继承和扩展这些合约来创建你自己的代币合约。

下面是使用 OpenZeppelin 创建一个简单的 ERC20 代币的示例：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// 导入 OpenZeppelin 提供的 ERC20 标准合约
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

// 创建一个新的合约，继承自 OpenZeppelin 的 ERC20 合约
contract MyToken is ERC20 {
    // 构造函数将初始化 ERC20 供应量和代币名称
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        // 通过 _mint 函数铸造初始供应量的代币到部署合约的地址
        _mint(msg.sender, initialSupply);
    }
}
```

在这个例子中，我们通过几行代码就可以实现一个 ERC20 合约。

### **常见扩展合约**

在使用 OpenZeppelin 库时，有多种用于扩展功能的合约可用。这些合约经过专门的设计以增加如访问控制、代币经济机制（如燃烧和铸造）以及安全功能（如防止重入攻击）等功能。

常见的扩展合约示例

1.  燃烧代币:
    
    ERC20Burnable：允许代币持有者销毁（burn）一定数量的代币，从而从流通中永久移除这些代币。
    
2.  暂停合约:
    
    ERC20Pausable：允许合约的管理员暂停合约的所有操作，这在遇到安全问题时是一种非常有用的应急措施。
    
3.  授权代币使用：
    
    ERC20Permit：允许代币持有者通过签署一个允许他人在其帐户上花费特定数量代币的许可，从而通过一次交易执行授权。这种机制使用了 EIP-2612 提案中定义的方法。
    

### **实现扩展合约**

要实现一个扩展合约，开发者需要根据需求选择合适的 OpenZeppelin 基础合约，并通过 [Solidity](https://learnblockchain.cn/course/93) 的 `is` 关键字来继承它。

下面是一个简单的示例，说明如何创建一个可燃烧的 ERC20 代币：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract MyToken is ERC20, ERC20Burnable {
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        _mint(msg.sender, initialSupply);
    }
}
```

在这个例子中，MyToken 继承了 OpenZeppelin 的 ERC20 和 ERC20Burnable 合约。这样，它就拥有了 ERC20 代币的标准功能，并加上了可以燃烧代币的能力。

在 ERC20Burnable 合约中，`burn` 函数的实现通常如下：

```
function burn(uint256 amount) public virtual {
    _burn(_msgSender(), amount);
}
```

此函数由代币的持有者调用，并需要一个参数 `amount`，表示要销毁的代币数量。函数调用 `_burn` 方法，此方法定义在 [OpenZeppelin](https://learnblockchain.cn/tags/OpenZeppelin?map=EVM) 的 [ERC20](https://learnblockchain.cn/tags/ERC20?map=EVM) 基础合约中，用于从执行操作的地址中减少相应数量的代币，并相应减少总供应量。

## **总结**

此章节的目的是向你展示如何从零开始创建一个遵循 [ERC20](https://learnblockchain.cn/tags/ERC20?map=EVM) 标准的代币合约，理解其方法的实际应用，以及如何利用 [OpenZeppelin](https://learnblockchain.cn/tags/OpenZeppelin?map=EVM) 合约来简化开发过程。

# **详解 ERC-1363 代币标准**

-   [RareSkills](https://learnblockchain.cn/rareskills)
    
-   发布于 2024-06-28 18:31
    
-   阅读 3873
    

ERC-1363 使智能合约能够检测并响应代币的转账。

## **ERC-1363 解决了什么问题？**

假设用户向合约转账 ERC-20 代币。由于没有机制可以查看是谁进行了转账，智能合约无法为转账用户记账。

虽然事件可以跟踪此信息，但只能被链下消费者使用。智能合约无法在没有预言机的情况下读取事件。

### **传统方案：通知接收者的替代方法是接收者使用 transferFrom 将代币转账给自己**

上述问题的典型解决方法是代币发送方批准接收智能合约代表发送方转账代币。

```
contract ReceivingContract {
	function deposit(uint256 amount) external {
		// 如果未经批准或用户余额不足，将会回滚
		ERC20(token).transferFrom(msg.sender, address.this, amount);

		deposits[msg.sender] += amount;
	}
}
```

然后存款人调用接收智能合约的函数（在上面的示例代码中为 deposit）来从发送方转账代币到合约。由于合约知道它从用户那里转账了代币，因此能够正确记账。

然而，为了批准合约转账代币，需要增加额外的交易费用。

此外，用户在批准合约后应将批准设置为零，否则存在合约被利用的风险，可能导致合约从用户那里提取更多 ERC-20 代币。

## **转账 Hook（钩子）**

转账 Hook 是接收智能合约中的预定义函数，当它接收到代币时将被调用。也就是说，代币合约在接收到转账指令后，会在接收地址上调用预定义函数。

如果函数不存在、回滚或未返回预期的成功值，则转账会回滚。

已经熟悉 [ERC-721](https://learnblockchain.cn/tags/ERC721?map=EVM) 标准中的 onERC721Received 的读者将对转账Hook很熟悉。

### **ERC-1363 扩展了 ERC-20 标准，添加了转账Hook**

要实现该标准，ERC-20 需要额外的函数（稍后会解释）来转账代币以触发接收方的转账Hook，并且接收方必须根据标准实现转账Hook。

### **IERC1363Receiver**

对于希望被通知其已收到 ERC-1363 代币的合约，它们必须实现 IERC1363Receiver（请查看[此处的 OpenZeppelin 实现](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/interfaces/IERC1363Receiver.sol) ），其中包含一个名为 onTransferReceived 的函数：

```
pragma solidity ^0.8.20;

interface IERC1363Receiver {
// 成功时返回`bytes4(keccak256("onTransferReceived(address,address,uint256,bytes)"))`
	function onTransferReceived(
		address operator,
		address from,
		uint256 value,
		bytes calldata data
	) external returns (bytes4);
}
```

-   operator 是发起转账的地址
    
-   from 是从中扣除 ERC-1363 代币的账户
    
-   value 是转账的代币数量
    
-   data 由**operator**指定以转发给接收方
    

**在实现此函数时，请始终检查 msg.sender 是否是你希望接收 ERC-1363 代币的代币，因为任何人都可以使用任意值调用 onTransferReceived()。**

以下是一个接受 ERC-1363 代币的最小示例合约：

```
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/interfaces/IERC1363Receiver.sol";
import "@openzeppelin/contracts/interfaces/IERC1363.sol";

contract TokenReceiver is IERC1363Receiver {
	address internal erc1363Token;

	constructor(address erc1363Token_) {
		erc1363Token = erc1363Token_;
	}

	mapping(address user => uint256 balance) public balances;

	function onTransferReceived(
		address operator,
		address from,
		uint256 value,
		bytes calldata data
	) external returns (bytes4) {
		
		require(msg.sender == erc1363Token, "not the expected token");
		balances[from] += value;
		return this.onTransferReceived.selector;
	}

	function withdraw(uint256 value) external {
		require(balances[msg.sender] >= value, "balance too low");
		balances[msg.sender] -= value;
	
		IERC1363(erc1363Token).transfer(msg.sender, value);
	}

}
```

合约知道自己收到 ERC-20 代币的传统方式是使用 transferFrom 函数，该函数需要首先进行批准，但是使用 ERC-1363 后，合约能够知道自己已收到代币，并且还能够消除批准步骤，因为 transferAndCall 将代币转账给合约（无需批准）并调用 onTransferReceived 函数。·

## **通过 ERC-20 最大化向后兼容性**

新代币标准的问题在于现有协议无法使用它们，除非它们与先前的标准完全兼容。

为了最大化向后兼容性，**ERC-1363 是一种 ERC-20 代币**，它添加了旧协议不需要使用的额外函数。

所有现有的 ERC-20 函数：name、symbol、decimals、totalSupply、balanceOf、transfer、transferFrom、approve 和 allowance 的行为都与 ERC-20 标准规定的完全一致。

ERC-1363 标准添加了新函数到 ERC-20，以便旧协议仍然可以与 ERC-1363 代币交互，就像与 ERC-20 代币一样。但是，如果需要，新协议可以利用 ERC-1363 上的转账Hook。

要成为符合 ERC-1363 标准的代币，代码还必须实现六个额外的函数：

-   两个版本的 transferAndCall
    
-   两个版本的 transferFromAndCall
    
-   两个版本的 approveAndCall
    

顾名思义，这些函数将执行 ERC-20 操作，然后调用接收方的Hook函数。

每个函数都有两个版本，一个带有数据参数，一个不带。数据参数是为了发送方能够将数据转发给接收合约（稍后我们将展示一个示例）。

```
// 有两个 transferAndCall 函数，
// 一个带有数据参数，一个不带

function transferAndCall(
	address to,
	uint256 value
) external returns (bool);

function transferAndCall(
	address to,
	uint256 value,
	bytes calldata data
) external returns (bool);

// 有两个 transferFromAndCall 函数，
// 一个带有数据参数，一个不带

function transferFromAndCall(
	address from,
	address to,
	uint256 value
) external returns (bool);

function transferFromAndCall(
	address from,
	address to,
	uint256 value,
	bytes calldata data
) external returns (bool);

// 有两个 approveAndCall 函数，// 一个带有数据参数，一个不带```
function approveAndCall(
	address spender,
	uint256 value
) external returns (bool);

function approveAndCall(
	address spender,
	uint256 value,
	bytes calldata data
) external returns (bool);
```

## **ERC-721 inspiration: transferFrom vs safeTransferFrom**

与 ERC-721 标准类似，ERC-1363 中 transferFromAndCall 和 transferFrom 之间的区别与 ERC-721 中 transferFrom 和 safeTransferFrom 之间的区别相同。然而，“safe”不是一个理想的函数名称，因为转账挂钩引入了潜在的重入向量，所以它并不“安全”。ERC-1363 使用的“call”一词的添加使得函数正在做什么更加明确：在转账后调用接收者通知其代币已转移给它。

## **参考实现**

可以在这里找到一个 [ERC-1363 实现](https://github.com/vittominacori/erc1363-payable-token/tree/master) 。我们将使用该示例中的大量代码。逐步解释代码库比一次性粘贴实现更容易。对于实现 ERC-1363 代币的人，请使用上面链接的实现。这里的代码仅供说明目的。

ERC-1363 使用与 ERC-20 相同的余额和批准存储变量。它不存储额外信息。

## **ERC-1363 代码概述**

### **继承 ERC-20**

正如前面强调的，ERC-1363 是一个具有附加功能的 ERC-20 代币。构建 ERC-1363 的第一步是继承 ERC-20：

```
//SPDX-License-Identifier: MIT

pragma solidity 0.8.24;
import "@openzeppelin/contracts@5.0.0/token/ERC20/ERC20.sol";

contract ERC1363 is ERC20 {
	constructor(
		string memory name,
		string memory symbol
	)ERC20(name, symbol) {}
}
```

### **transferFromAndCall(address to, uint256 value) external returns (bool)**

仅当接收地址实现 onTransferReceived()并返回 onTransferReceived()的四字节函数选择器时，transferFromAndCall 才成功。

```
function transferFromAndCall(
	address from,
	address to,
	uint256 value,
	bytes memory data
) public virtual returns (bool) {

	// 首先调用父级中的 ERC-20 transferFrom 函数
	if (!transferFrom(from, to, value)) {
		revert ERC1363TransferFromFailed(from, to, value);
	}

	// 然后调用接收者
	_checkOnTransferReceived(from, to, value, data);
	return true;
}

// 此函数没有数据参数，转发空数据

function transferFromAndCall(
	address from,
	address to,
	uint256 value
) public virtual returns (bool) {
	// \`data\`为空
	return transferFromAndCall(from, to, value, "");
}
```

### **transferAndCall(address to, uint256 value) external returns (bool)**

这与 transferFromAndCall 非常相似，只是 from 是 msg.sender。

```
function transferAndCall(
	address to,
	uint256 value,
	bytes memory data
) public virtual returns (bool) {
	if (!transfer(to, value)) {
		revert ERC1363TransferFailed(to, value);
	}
	_checkOnTransferReceived(msgSender(), to, value, data);
	
	return true;
}

function transferAndCall(
	address to,
	uint256 value
) public virtual returns (bool) {

	return transferAndCall(to, value, "");
}
```

### **\_checkOnTransferReceived()**

此函数检查接收者是否为合约，如果不是则回滚。然后尝试调用 onTransferReceived，如果未收到 _onTransferReceived(address,address,uint256,bytes)_ 的函数选择器 0x88a7ca5c，则回滚。如果 onTransferReceived 回滚，则此函数使用从 onTransferReceived 接收的错误消息回滚。

因为此函数如果发送到 EOA（常规钱包）会回滚，所以将 ERC-1363 转账给 EOA 应使用 ERC-20 函数 transfer 或 transferFrom：

```
function _checkOnTransferReceived(
	address from,
	address to,
	uint256 value,
	bytes memory data
) private {
	
	if (to.code.length == 0) { 
		revert ERC1363EOAReceiver(to); 
	}           

	try IERC1363Receiver(to).onTransferReceived(_msgSender(), from, value, data) returns (bytes4 retval) {                
		if (retval != IERC1363Receiver.onTransferReceived.selector) {                    
			revert ERC1363InvalidReceiver(to);                	
		}            
	} catch (bytes memory reason) {                
		if (reason.length == 0) {                    
			revert ERC1363InvalidReceiver(to);                
		} else {                        
		
			// 此代码导致 ERC-1363 回滚                        
			// 与其调用的合约相同的回滚字符串                    
			assembly {                        
				revert(add(32, reason), mload(reason))                    
			}                
		}            
	}
}
```

### **approveAndCall**

在上述工作流程中，被调用的智能合约是 ERC-1363 代币的接收者。

然而，如果我们希望另一个合约成为我们代币的发送者怎么办？例如，路由器合约，如 [Uniswap V2 Router](https://learnblockchain.cn/article/11270)，不持有代币的保管权。它将它们转发给 Uniswap 进行交易。

传统上，这样的架构使用“先批准再转账”工作流程，但使用 ERC-1363，我们可以在一笔交易中完成这个操作。顾名思义，刚刚获得批准以花费另一个地址的代币的合约会得到一个特殊的挂钩函数调用。

与 transferAndCall 函数一样，根据调用的 approveAndCall，向交易提供附加数据是可选的：

```
function approveAndCall(        
	address spender,        
	uint256 value
) public virtual returns (bool) {        
	return approveAndCall(spender, value, "");
}

function approveAndCall(        
	address spender,        
	uint256 value,        
	bytes memory data
) public virtual returns (bool) {        
	if (!approve(spender, value)) {            
		revert ERC1363ApproveFailed(spender, value);        
	}        
	
	_checkOnApprovalReceived(spender, value, data);        
	
	return true;
}

```

### **IERC1363Spender**

类似于 IERC1363Receiver，当调用 approvalAndCall 时，会触发名为 onApprovalReceived 的函数。

这是 OpenZeppelin 提供的 [IERC1363Spender 接口](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/interfaces/IERC1363Spender.sol) 。下面的代码已删除注释：

```
interface IERC1363Spender {
    
	function onApprovalReceived(            
		address owner,            
		uint256 value,            
		bytes calldata data
	) external returns (bytes4);

}
```

只有代币的所有者可以批准另一个地址，因此不需要操作员参数 —— 在批准期间，操作员和所有者必须是相同的地址。value 是批准金额的大小。

以下合约在收到 onApprovalReceived 后将代币转发到数据中指定的地址。

```
import "@openzeppelin/contracts/interfaces/IERC1363Spender.sol";

contract Router is IERC1363Spender {        
	// 需要额外的功能，以便批准的钱包将批准的 ERC-1363 代币添加到此映射中        

	mapping(address => bool) isApprovedToken;        
}

function onApprovalReceived(            
    address owner,            
    uint256 value,            
    bytes calldata data
) external returns (bytes4) {                
    require(isApprovedToken[msg.sender], "not an approved token"); 
               
    // getTarget is not implemented here,                
    // see the next section for how to it work                
    address target = getTarget(data);                
    bool success = IERC1363(msg.sender).transferFrom(owner, target, value);                

    require(success, "transfer failed");                

    return this.onApprovalReceived.selector; 
       
}
```

此函数应检查`msg.sender`是否为代币合约，因为如果任何人都可以调用它，可能会导致意外行为。

使用 ERC-1363 的示例接收合约

以下示例演示了对`data`参数的用例。

```
interface ERC1363Receiver {      
    function onTransferReceived(
        address operator,                                  
        address from,                                  
        uint256 value,                                  
        bytes memory data
    ) external returns (bytes4);
}

contract ReceiverContract is ERC1363Receiver {        
    mapping(address => uint256) public deposits;        
    
    address immutable token;        

    constructor(address token_) {                
        token = token_;        
    }        

    event Deposit(
        address indexed from,                                    
        address indexed beneficiary,                                    
        uint256 value
    );        

    function onTransferReceived(                
        address, // operator                
        address from,                
        uint256 value,                
        bytes memory data
    ) external returns (bytes4) {                
        
        require(msg.sender == token, "Caller not ERC1363 token");                
        address beneficiary;                
        if (data.length == 32) {                        
            beneficiary = abi.decode(data, (address));                
        } else {                        
            beneficiary = from;                
        }                
        
        deposits[from] += value;                
        
        emit Deposit(from, beneficiary, value);                
        return this.onTransferReceived.selector;        
    }
}
```

## **早期尝试解决代币Hook的标准**

ERC-1363 不是第一个向 ERC-20 添加转账挂钩的标准。首先，于 2017 年 5 月提出了 [ERC-223](https://eips.ethereum.org/EIPS/eip-223)，以在 ERC-20 的转账和 transferFrom 中添加转账挂钩。但这意味着智能合约无法接收代币，除非它们实现了转账挂钩。这使得该标准与接受 ERC-20 代币但没有转账挂钩的协议不兼容。

ERC-777 于 2017 年 11 月推出。在此标准中，除非接收方在 [ERC-1820 注册表](https://eips.ethereum.org/EIPS/eip-1820) 中注册了他们的地址，否则接收方不会收到转账挂钩调用。

然而，协议并未设计 ERC-20 的转账或 transferFrom 以调用其他合约。这使得这些合约容易受到重入攻击，因为它们没有预期“ERC-20” 代币会调用其他合约。有关更多信息，请参阅 [Uniswap V1 重入漏洞解析](https://github.com/OpenZeppelin/exploit-uniswap)。

此外，ERC-777 标准在 gas 方面相当昂贵，因为它需要向 ERC-1820 注册表合约发出额外的调用。

ERC-1363 通过完全不改变 ERC-20 标准中的转账和 transferFrom 来解决所有这些问题。所有的转账挂钩都在具有显式调用名称的函数中调用。

## **何时使用 ERC-1363 标准**

ERC-1363 标准可以在任何应用 ERC-20 标准的地方使用。在作者看来，这个标准是 ERC-20 的理想替代品，因为它可以消除 ERC-20 的批准步骤，这导致了大量资金的损失。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# **稳定币**

稳定币（Stablecoin）是指价值绑定（或锚定）到某个稳定资产，如法定货币、黄金或其他商品的加密货币。稳定币旨在结合加密货币的即时处理、安全性和隐私性与传统法定货币的稳定价值。稳定币是Web3世界中的重要组成部分，因为它们提供了一种在去中心化金融（DeFi）生态系统中避免加密市场波动性的方式。

稳定币大致可以分为以下几类（分类方法多种多样，没有一个标准的分类方法，稍做了解即可）：

1.  **法币抵押稳定币**: 这种类型的稳定币与现实世界的法定货币如美元、欧元等1:1抵押。每发行一个稳定币，就有相应的法定货币在银行账户中作为支持。例如，USDT（Tether）、USDC（USD Coin）和PAX（Paxos Standard）都是法币抵押稳定币。
    
2.  **加密货币抵押稳定币**: 这种稳定币是由其他加密资产作为抵押的。为了应对加密资产价格的波动性，这类稳定币通常过度抵押，即抵押的加密资产价值超过稳定币本身的价值。代表性的加密货币抵押稳定币有DAI（由MakerDAO系统发行）。
    
3.  **算法稳定币**: 算法稳定币没有实际资产作为抵押，而是通过算法调节供应量来稳定币值。这一类稳定币试图通过扩大或缩减流通供应量，或者通过激励措施来维持与锚定资产相等的价值。
    

# **合约实现**

接下来我们尝试实现一个**抵押稳定币**

项目地址：[https://github.com/lessurlx/foundry-Defi-StableCoin](https://github.com/lessurlx/foundry-Defi-StableCoin)

## **DecentralizedStableCoin**

实现一个去中心化的稳定币合约`DecentralizedStableCoin.sol`

`forge install openzeppelin/openzeppelin-contracts --no-commit`

在 foundry.toml 中添加 remapping

`remappings = ["@openzeppelin/contracts=lib/openzeppelin-contracts/contracts"]`

我们可以通过 openzeppelin 的模板，快速的完成稳定币的铸造和销毁逻辑，合约代码如下

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ERC20Burnable, ERC20} from "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

/**
 * @title Decentralized Stable Coin
 * Collateral: Exogenous (ETH & BTC)
 * Minting: Algorithmic
 * Relative Stability: Pegged to USD
 *
 * This is the contract meant to be governed by DSCEngine. This contract is just the ERC20 implementation of our stablecoin system.
 */
contract DecentralizedStableCoin is ERC20Burnable, Ownable {
    error DecentralizedStableCoin__AmountMustBeMoreThanZero();
    error DecentralizedStableCoin__BurnAmountExceedsBalance();
    error DecentralizedStableCoin__NotZeroAddress();

    constructor() ERC20("DecentralizedStableCoin", "DSC") Ownable(msg.sender) {}

    function burn(uint256 _amount) public override onlyOwner {
        uint256 balance = balanceOf(msg.sender);
        if (_amount <= 0) {
            revert DecentralizedStableCoin__AmountMustBeMoreThanZero();
        }

        if (balance < _amount) {
            revert DecentralizedStableCoin__BurnAmountExceedsBalance();
        }
        super.burn(_amount);
    }

    function mint(
        address _to,
        uint256 _amount
    ) public onlyOwner returns (bool) {
        if (_to == address(0)) {
            revert DecentralizedStableCoin__NotZeroAddress();
        }
        if (_amount <= 0) {
            revert DecentralizedStableCoin__AmountMustBeMoreThanZero();
        }
        _mint(_to, _amount);
        return true;
    }
}
```

## **OracleLib**

在这个稳定币项目中，时刻更新抵押资产的价值是很重要的，如果抵押资产的价值暴跌，就可能会导致我们的稳定币暴雷。其中预言机的作用就是获取资产价值，我们这里用的是 chainlink 的 price feed，链接如下：[https://docs.chain.link/data-feeds/price-feeds/addresses?network=ethereum&page=1#sepolia-testnet](https://docs.chain.link/data-feeds/price-feeds/addresses?network=ethereum&page=1#sepolia-testnet)

chainlink会在每个心跳时间内更新最新的资产价值，以 BTC 为例，它的 HeartBeat 就是3600s

![Sepolia Testnet](https://leapwhale-1258830694.cos.accelerate.myqcloud.com/image-20240402151438160.png?imageSlim)

我们可以自己创建一个库文件，命名为 OracleLib.sol，用于检测这个心跳是否有效。如果上一次心跳时间过长，我们可以理解为 chainlink 失效了，那么此时的资产价值就无法确定，我们就应该停止合约服务

实现细节如下

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

/*
 * @title OracleLib
 * @notice This library is used to check the Chainlink Oracle for stale data
 * If a price is stale, the function will revert, and render the DSCEngine unusable
 * We want the DSCEngine to freeze if prices become stale.
 *
 * So if the chainlink network explodes and you have a lot of money locked in the protocol, that's too bad
 */
library OracleLib {
    error OracleLib__StalePrice();

    uint256 private constant TIMEOUT = 3 hours;

    function staleCheckLatestRoundData(
        AggregatorV3Interface priceFeed
    ) public view returns (uint80, int256, uint256, uint256, uint80) {
        (
            uint80 roundId,
            int256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        ) = priceFeed.latestRoundData();

        uint256 secondsSince = block.timestamp - updatedAt;
        if (secondsSince > TIMEOUT) {
            revert OracleLib__StalePrice();
        }
        return (roundId, answer, startedAt, updatedAt, answeredInRound);
    }
}
```

## **DSCEngine**

拥有上面的稳定币DSC的合约代码之后，我们还需要一个 `DSCEngine`，来保证稳定币的稳定性，其目标是保持1 DSC 等于 1 美元的价值。稳定币通过算法稳定，并由外部资产（如ETH和BTC）作为抵押品。这个系统设计得非常简洁，类似于MakerDAO的DAI，但没有治理和费用，仅支持WETH和WBTC作为抵押品。

### **交互流程**

1.  用户将抵押品（ETH, BTC等）存入`DSCEngine`合约。
    
2.  根据抵押品的当前市场价值和健康因子，用户可以铸造相应数量的DSC。
    
3.  用户可以随时赎回其抵押品，但必须首先燃烧等值的DSC。
    
4.  如果用户的健康因子低于最小阈值，其抵押品将面临被清算的风险。
    
5.  清算者可以燃烧用户的DSC并以折扣价格接收相应价值的抵押品。
    

### **稳定性保证**

-   **超额抵押和健康因子:** 要求超额抵押保证了即使市场价格波动，抵押品的价值也会超过所铸造DSC的价值。
    
-   **清算机制:** 如果抵押品的价值跌破某个阈值，清算机制会触发，允许其他用户清算不健康的抵押品，保护系统的整体健康。
    
-   **价格喂价:** 实时的市场价格喂价确保抵押品价值的准确性，对于抵押品价值的监控和稳定币价值的维持至关重要。
    

通过这些机制，`DSCEngine`旨在通过算法和市场机制维持DSC的稳定性，尽管没有直接实现价格稳定机制，`DSCEngine`合约通过一系列财务健康检查和激励措施来维护DSC代币的稳定性：

1.  **超额抵押（Over-Collateralization）**: 用户必须存入的抵押品价值超过其想要铸造的DSC数量，这个比率由`LIQUIDATION_THRESHOLD`定义。这意味着如果用户想要铸造价值100的DSC，他们可能需要存入价值100的_DSC_，他们可能需要存入价值200的ETH或BTC。这个过度抵押的要求为价格波动提供了缓冲。
    
2.  **实时价格喂价（Real-Time Price Feeds）**: 合约使用Chainlink的`AggregatorV3Interface`来获取抵押品的最新市场价格。这样可以确保抵押品的价值评估与市场价格同步，以实时反映抵押资产的真实价值。
    
3.  **健康因子（Health Factor）**: 合约计算并监控每个用户的健康因子，这是用户抵押品价值与其DSC债务的比率。如果用户的健康因子低于`MIN_HEALTH_FACTOR`，他们的抵押品可能会被清算。这个机制保证了用户不能铸造过多的DSC而不增加额外的抵押品。
    
4.  **清算机制（Liquidation Mechanism）**: 当用户的健康因子太低，即他们的抵押不足以支持其DSC债务时，其他参与者可以清算这些不健康的抵押品。清算者会以低于市场价的价格（由`LIQUIDATION_BONUS`提供激励）接收抵押品，同时燃烧相应的DSC，减少市场上的DSC供应，帮助维持DSC的价格稳定。
    
5.  **交互限制（Non-Reentrancy）**: 合约继承了OpenZeppelin的`ReentrancyGuard`，这防止了重入攻击，这是保护用户资产和维持合约整体稳定性的重要安全措施。
    

通过这些机制，`DSCEngine`试图保持DSC的稳定价值，使之能在加密货币市场上作为稳定的价值存储和交换媒介。然而，这种稳定性依赖于清算过程的有效执行以及市场对抵押品价格的准确反应。如果市场价格迅速下跌，清算可能无法足够快地发生，从而使系统面临资不抵债的风险。此外，系统的稳定性还依赖于Chainlink价格喂价的准确性和可靠性。

### **函数介绍**

以下是合约中各个函数的作用概述：

**构造函数 (**`constructor`**)**

-   初始化合约，设定了价格喂价（price feeds）地址和可接受的抵押品（collateral）地址，并将这些信息存储在状态变量中。
    
-   初始化`DecentralizedStableCoin`合约的引用。
    

**计算健康因子 (**`_healthFactor`**、**`_calculateHealthFactor`**)**

-   **检查铸币总量**：如果用户没有铸造任何DSC（`totalDscMinted`为0），则健康因子被设定为`type(uint256).max`，这表示用户的健康因子是最大的，因为没有任何债务风险。
    
-   **调整抵押品价值**：计算抵押品价值的调整值，将用户的抵押品总价值（`collateralValueInUsd`）乘以清算阈值（`LIQUIDATION_THRESHOLD`）并除以清算精度（`LIQUIDATION_PRECISION`）。阈值通常设置为用户需要超额抵押的比例（例如200%）。
    
-   **计算健康因子**：最后，将调整后的抵押品价值乘以一个精度值，然后除以用户铸造的DSC总量。这个结果就是健康因子，它表示用户抵押品价值与其DSC债务的比率。
    

**存款抵押品 (**`depositCollateral`**)**

-   允许用户存入指定的抵押品。
    
-   触发了一个记录抵押品存款的事件（`CollateralDeposited`）。
    
-   调用ERC20代币的`transferFrom`方法，将用户的代币转移到合约地址。
    

**赎回抵押品 (**`redeemCollateral`**)**

-   **更新抵押记录**：减去`from`用户账户下对应`tokenCollateralAddress`的抵押品数量，`s_collateralDeposited`状态变量记录了用户所存入的每种抵押品的总量。
    
-   **触发事件**：触发`CollateralRedeemed`事件，记录抵押品从`from`转移到`to`的动作，以及涉及的抵押品地址和数量。
    
-   **转移抵押品**：使用`IERC20(tokenCollateralAddress).transfer`函数将抵押品从合约地址转移到`to`账户。
    
-   **检查转移结果**：检查`transfer`函数调用是否成功。如果没有成功，合约将使用`DSCEngine__TransferFailed()`错误进行回滚。
    
-   **检查健康因子**：如果执行之后健康因子过低，那么回滚所有操作
    

**铸造DSC (**`mintDsc`**)**

-   允许用户铸造DSC。
    
-   检查健康因子，确保用户有足够的抵押品。
    

**燃烧DSC (**`burnDsc`**)**

-   **更新铸币记录**：减去`onBehalfOf`用户账户下铸造的DSC数量，`s_DSCMinted[onBehalfOf]`状态变量记录了用户所铸造的DSC总量。
    
-   **转移代币**：使用`i_dsc.transferFrom`函数从`dscFrom`账户向合约地址转移等同于`amountDscToBurn`的DSC数量。这步骤通常是为了从用户账户中取回代币，以便接下来销毁。
    
-   **检查转移结果**：检查`transferFrom`函数调用是否成功。如果没有成功，合约将使用`DSCEngine__TransferFailed()`错误进行回滚。
    
-   **销毁代币**：调用`i_dsc.burn`函数销毁从`dscFrom`账户转移过来的DSC数量。这一步是实际减少DSC总供应量的操作。
    
-   **检查健康因子**：如果执行之后健康因子过低，那么回滚所有操作
    

**存款和铸币 (**`depositCollateralAndMintDsc`**)**

-   允许用户存入抵押品并铸造DSC。
    
-   调用 `depositCollateral` 和 `mintDsc`
    

**赎回抵押品 (**`redeemCollateralForDsc`**)**

-   允许用户燃烧DSC并赎回相应数量的抵押品。
    
-   调用 `burnDsc` 和 `redeemCollateral`
    

**清算 (**`liquidate`**)**

-   **检查健康因子**：通过调用`_healthFactor`函数，计算传入的用户(`user`)的当前健康因子。如果该健康因子大于或等于`MIN_HEALTH_FACTOR`，则不允许清算，因为用户的抵押品仍然被认为是健康的，合约将会使用`DSCEngine__HealthFactorOk()`错误进行回滚。
    
-   **计算抵押品价值**：调用`getTokenAmountFromUsd`函数来计算清算者将要清算的DSC代表的美元价值等同的抵押品数量。例如，如果`debtToCover`是100 DSC，那么清算者需要获得相当于100美元的抵押品。
    
-   **计算清算奖励**：清算者会获得额外的奖励，这是通过`LIQUIDATION_BONUS`和`LIQUIDATION_PRECISION`常量计算出的奖励比例。在这个例子中，奖励是清算的抵押品价值的10%，所以清算者获得的总抵押品价值是他们清算DSC价值的110%。
    
-   **赎回抵押品**：清算者将收到总计的抵押品，包括基本的抵押品价值加上清算奖励。这一步似乎引用了一个未在代码中定义的内部函数`_redeemCollateral`。这个函数的目的是将计算出的抵押品数量（包括奖励）从不健康用户的账户转移到清算者的账户。
    
-   **燃烧DSC**：清算过程中，代表`debtToCover`的DSC数量将被燃烧，减少系统中的DSC总供应量。这也可能涉及到一个未在代码中定义的内部函数`_burnDsc`。
    
-   **检查健康因子改善情况**：在清算后，再次计算用户的健康因子，以确认清算行动确实提高了用户的健康因子。如果清算后用户的健康因子没有提升，合约将使用`DSCEngine__HealthFactorNotImproved()`错误进行回滚。
    
-   **检查清算者健康因子**：最后，合约将检查清算者的健康因子是否仍然满足要求，以确保清算行动没有破坏清算者自己的财务健康。
    

**获取账户信息 (**`_getAccountInformation`**)**

-   返回用户铸造的DSC总量和抵押品的美元价值。
    

**检查并回滚健康因子 (**`_revertIfHealthFactorIsBroken`**)**

-   检查用户的健康因子是否低于最小值，并在必要时回滚交易。
    

**从USD获取代币数量 (**`getTokenAmountFromUsd`**)**

-   将美元数额转换为对应的代币数量。
    

**获取账户抵押品价值 (**`getAccountCollateralValue`**)**

-   计算用户所有抵押品的总价值（以美元计）。
    

**获取USD价值 (**`getUsdValue`**)**

-   计算给定数量的代币的美元价值。
    

**获取账户信息 (**`getAccountInformation`**)**

-   外部可调用的函数，返回用户铸造的DSC总量和抵押品的美元价值。
    

### **常量与变量**

合约还定义了一些错误处理方法和状态变量，以及与抵押品相关的事件。此外，此外，合约还包含了一些重要的常量和状态变量，以及修饰符（modifier）来进行输入检查。以下是对这些组件的简要概述：

**常量**

-   `LIQUIDATION_THRESHOLD`：清算阈值，设为50，意味着用户必须提供至少200%的超额抵押。
    
-   `LIQUIDATION_BONUS`：清算奖励，设为10%，即在清算时获得的抵押品折扣。
    
-   `LIQUIDATION_PRECISION`：清算精度，用于在清算计算中保持数值精度。
    
-   `MIN_HEALTH_FACTOR`：最小健康因子，用于确定账户是否足够健康以防止被清算。
    
-   `PRECISION`：用于内部计算的精度，通常是1e18。
    
-   `ADDITIONAL_FEED_PRECISION`：用于从价格喂价转换为代币价值的额外精度。
    
-   `FEED_PRECISION`：价格喂价的基础精度。
    

**状态变量**

-   `i_dsc`：`DecentralizedStableCoin`合约的一个不可变私有引用。
    
-   `s_priceFeeds`：用于存储每个代币的价格喂价地址的映射。
    
-   `s_collateralDeposited`：用于存储每个用户的每种代币的抵押品数量的映射。
    
-   `s_DSCMinted`：用于存储每个用户铸造的DSC数量的映射。
    
-   `s_collateralTokens`：用于存储可接受的抵押品代币地址的数组。
    

### **实现细节**

安装 chainlink 库

`forge install smartcontractkit/chainlink-brownie-contracts@0.6.1 --no-commit`

在 remappings 中添加映射

`"@chainlink/contracts/=lib/chainlink-brownie-contracts/contracts/"`

实现代码如下

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {DecentralizedStableCoin} from "./DecentralizedStableCoin.sol";
import {ReentrancyGuard} from "@openzeppelin/contracts/utils/ReentrancyGuard.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
import {OracleLib} from "./libraries/OracleLib.sol";

/*
 * @title DSCEngine
 *
 * The system is designed to be as minimal as possible, and have the tokens maintain a 1 token == $1 peg.
 * This stablecoin has the properties:
 *  - Exogenous Collateral (ETH & BTC)
 *  - Dollar Pegged
 *  - Algoritmically Stable
 *
 *  It is similar to DAI had no governance, no fees, and was only backed by WETH and WBTC.
 */
contract DSCEngine is ReentrancyGuard {
    ///////////////////
    // Errors
    ///////////////////
    error DSCEngine__NeedsMoreThanZero();
    error DSCEngine__TokenAddressesAndPriceFeedAddressesMustBeSameLength();
    error DSCEngine__NotAllowedToken();
    error DSCEngine__TransferFailed();
    error DSCEngine__BreaksHealthFactor(uint256 healthFactor);
    error DSCEngine__MintFailed();
    error DSCEngine__HealthFactorOK();
    error DSCEngine__HealthFactorNotImproved();

    ///////////////////
    // Type
    ///////////////////
    using OracleLib for AggregatorV3Interface;

    ///////////////////
    // State Variables
    ///////////////////
    DecentralizedStableCoin private immutable i_dsc;

    uint256 private constant LIQUIDATION_THRESHOLD = 50; // This means you need to be 200% over-collateralized
    uint256 private constant LIQUIDATION_BONUS = 10; // This means you get assets at a 10% discount when liquidating
    uint256 private constant LIQUIDATION_PRECISION = 100;
    uint256 private constant MIN_HEALTH_FACTOR = 1e18;
    uint256 private constant PRECISION = 1e18;
    uint256 private constant ADDITIONAL_FEED_PRECISION = 1e10;
    uint256 private constant FEED_PRECISION = 1e8;

    mapping(address token => address priceFeed) private s_priceFeeds;
    mapping(address user => mapping(address token => uint256 amount))
        private s_collateralDeposited;
    mapping(address user => uint256 amountDscMinted) private s_DSCMinted;
    address[] private s_collateralTokens;

    ///////////////////
    // Events
    ///////////////////
    event CollateralDeposited(
        address indexed user,
        address indexed token,
        uint256 indexed amount
    );
    event CollateralRedeemed(
        address indexed redeemFrom,
        address indexed redeemTo,
        address token,
        uint256 amount
    ); // if redeemFrom != redeemedTo, then it was liquidated

    ///////////////////
    // Modifiers
    ///////////////////
    modifier moreThanZero(uint256 _amount) {
        if (_amount == 0) {
            revert DSCEngine__NeedsMoreThanZero();
        }
        _;
    }

    modifier isAllowedToken(address token) {
        if (s_priceFeeds[token] == address(0)) {
            revert DSCEngine__NotAllowedToken();
        }
        _;
    }

    ///////////////////
    // Functions
    ///////////////////
    constructor(
        address[] memory tokenAddresses,
        address[] memory priceFeedAddress,
        address dscAddress
    ) {
        if (tokenAddresses.length != priceFeedAddress.length) {
            revert DSCEngine__TokenAddressesAndPriceFeedAddressesMustBeSameLength();
        }
        for (uint256 i = 0; i < tokenAddresses.length; i++) {
            s_priceFeeds[tokenAddresses[i]] = priceFeedAddress[i];
            s_collateralTokens.push(tokenAddresses[i]);
        }
        i_dsc = DecentralizedStableCoin(dscAddress);
    }

    ///////////////////
    // External Functions
    ///////////////////
    function depositCollateralAndMintDsc(
        address tokenCollarteralAddress,
        uint256 amountCollateral,
        uint256 amountDscToMint
    ) external {
        depositCollateral(tokenCollarteralAddress, amountCollateral);
        mintDsc(amountDscToMint);
    }

    /*
     * @notice Deposit collateral to mint DSC
     * @param tokenCollarteralAddress The address of the collateral token
     * @param amountCollateral The amount of collateral to deposit
     */
    function depositCollateral(
        address tokenCollarteralAddress,
        uint256 amountCollateral
    )
        public
        moreThanZero(amountCollateral)
        isAllowedToken(tokenCollarteralAddress)
        nonReentrant
    {
        s_collateralDeposited[msg.sender][
            tokenCollarteralAddress
        ] += amountCollateral;
        // 更新状态的时候最好触发事件
        emit CollateralDeposited(
            msg.sender,
            tokenCollarteralAddress,
            amountCollateral
        );
        bool success = IERC20(tokenCollarteralAddress).transferFrom(
            msg.sender,
            address(this),
            amountCollateral
        );
        if (!success) {
            revert DSCEngine__TransferFailed();
        }
    }

    function redeemCollateralForDsc(
        address tokenCollateralAddress,
        uint256 amountCollateral,
        uint256 amountDscToBurn
    ) external {
        burnDsc(amountDscToBurn);
        redeemCollateral(tokenCollateralAddress, amountCollateral);
        // redeemCollateral already checks health factor
    }

    function redeemCollateral(
        address tokenCollateralAddress,
        uint256 amountCollateral
    ) public moreThanZero(amountCollateral) nonReentrant {
        _redeemCollateral(
            tokenCollateralAddress,
            amountCollateral,
            msg.sender,
            msg.sender
        );
        _revertIfHealthFactorIsBroken(msg.sender);
    }

    /*
     * @notice follows CEI
     * @param amountDscToMint The amount of decentralized stablecoin to mint
     * @notice they must have more collateral value than the minimum threshold
     */
    function mintDsc(
        uint256 amountDscToMint
    ) public moreThanZero(amountDscToMint) nonReentrant {
        s_DSCMinted[msg.sender] += amountDscToMint;
        // if they minted too much
        _revertIfHealthFactorIsBroken(msg.sender);
        bool minted = i_dsc.mint(msg.sender, amountDscToMint);
        if (!minted) {
            revert DSCEngine__MintFailed();
        }
    }

    function burnDsc(uint256 amount) public moreThanZero(amount) {
        _burnDsc(amount, msg.sender, msg.sender);
        // This would never hit...
        _revertIfHealthFactorIsBroken(msg.sender);
    }

    /*
     * @param collateral The erc20 collateral address to liquidate from the user
     * @param user The user who has broken the health factor. Their _healthFactor should be below MIN_HEALTH_FACTOR
     * @param debtToCover The amount of DSC you want to burn to improve the users health factor
     * @notice You can partically liquidate a user
     * @notice You will get a liquidation bonus for taking the users funds
     * @notice This function working assumes the protocol will be roughly 200% over-collateralized in order for this to work
     * @notice A known bug would be if the protocol were 100% or less collateralized, then we wouldn't be able to incentive the liquidators.
     * For example, if the price of the collateral plummeted before anyone could be liquidated.
     */
    function liquidate(
        address collateral,
        address user,
        uint256 debtToCover
    ) external moreThanZero(debtToCover) nonReentrant {
        // need to check health factor before liquidating
        uint256 startingUserHealthFactor = _healthFactor(user);
        if (startingUserHealthFactor >= MIN_HEALTH_FACTOR) {
            revert DSCEngine__HealthFactorOK();
        }

        // We want to burn their DSC "debt"
        // And take their collateral
        uint256 tokenAmountFromDebtCovered = getTokenAmountFromUsd(
            collateral,
            debtToCover
        );

        // give them a 10% bonus
        // we should implement a feature to liquidate in the event the protocol is insolvent
        // and sweep extra amounts into a treasury
        uint256 bonusCollateral = (tokenAmountFromDebtCovered *
            LIQUIDATION_BONUS) / LIQUIDATION_PRECISION;

        _redeemCollateral(
            collateral,
            tokenAmountFromDebtCovered + bonusCollateral,
            user,
            msg.sender
        );
        _burnDsc(debtToCover, user, msg.sender);

        uint256 endingUserHealthFactor = _healthFactor(user);
        // This conditional should never hit, but just in case
        if (endingUserHealthFactor <= startingUserHealthFactor) {
            revert DSCEngine__HealthFactorNotImproved();
        }
        _revertIfHealthFactorIsBroken(msg.sender);
    }

    ///////////////////
    // Private Functions
    ///////////////////
    function _burnDsc(
        uint256 amountDscToBurn,
        address onBehalfOf,
        address dscFrom
    ) private {
        s_DSCMinted[onBehalfOf] -= amountDscToBurn;

        bool success = i_dsc.transferFrom(
            dscFrom,
            address(this),
            amountDscToBurn
        );
        // This conditional is hypothetically unreachable
        if (!success) {
            revert DSCEngine__TransferFailed();
        }
        i_dsc.burn(amountDscToBurn);
    }

    function _redeemCollateral(
        address tokenCollateralAddress,
        uint256 amountCollateral,
        address from,
        address to
    ) private {
        s_collateralDeposited[from][tokenCollateralAddress] -= amountCollateral;
        emit CollateralRedeemed(
            from,
            to,
            tokenCollateralAddress,
            amountCollateral
        );
        bool success = IERC20(tokenCollateralAddress).transfer(
            to,
            amountCollateral
        );
        if (!success) {
            revert DSCEngine__TransferFailed();
        }
    }

    function _getAccountInformation(
        address user
    )
        private
        view
        returns (uint256 totalDscMinted, uint256 collateralValueInUsd)
    {
        totalDscMinted = s_DSCMinted[user];
        collateralValueInUsd = getAccountCollateralValue(user);
    }

    /*
     * Returns how close to liquidation the user is
     * If a user goes below 1, then they are liquidated
     */
    function _healthFactor(address user) private view returns (uint256) {
        // total DSC minted
        // total collateral value
        (
            uint256 totalDscMinted,
            uint256 collateralValueInUsd
        ) = _getAccountInformation(user);

        return _calculateHealthFactor(totalDscMinted, collateralValueInUsd);
    }

    function _calculateHealthFactor(
        uint256 totalDscMinted,
        uint256 collateralValueInUsd
    ) internal pure returns (uint256) {
        if (totalDscMinted == 0) return type(uint256).max;
        uint256 collateralAdjustedForThreshold = (collateralValueInUsd *
            LIQUIDATION_THRESHOLD) / LIQUIDATION_PRECISION;
        return (collateralAdjustedForThreshold * PRECISION) / totalDscMinted;
    }

    // 1. Check health factor (do they have enough collateral?)
    // 2. Revert if they don't
    function _revertIfHealthFactorIsBroken(address user) internal view {
        uint256 userHealthFactor = _healthFactor(user);
        if (userHealthFactor < MIN_HEALTH_FACTOR) {
            revert DSCEngine__BreaksHealthFactor(userHealthFactor);
        }
    }

    function getTokenAmountFromUsd(
        address token,
        uint256 usdAmountInWei
    ) public view returns (uint256) {
        AggregatorV3Interface priceFeed = AggregatorV3Interface(
            s_priceFeeds[token]
        );
        (, int256 price, , , ) = priceFeed.staleCheckLatestRoundData();
        return
            (usdAmountInWei * PRECISION) /
            (uint256(price) * ADDITIONAL_FEED_PRECISION);
    }

    function getAccountCollateralValue(
        address user
    ) public view returns (uint256 totalCollateralValueInUsd) {
        // loop through each collateral token, get the amount they have deposited, and map it to the price, to get the USD value
        for (uint256 i = 0; i < s_collateralTokens.length; i++) {
            address token = s_collateralTokens[i];
            uint256 amount = s_collateralDeposited[user][token];
            totalCollateralValueInUsd += getUsdValue(token, amount);
        }

        return totalCollateralValueInUsd;
    }

    function getUsdValue(
        address token,
        uint256 amount
    ) public view returns (uint256) {
        AggregatorV3Interface priceFeed = AggregatorV3Interface(
            s_priceFeeds[token]
        );
        (, int price, , , ) = priceFeed.staleCheckLatestRoundData();
        return
            ((uint256(price) * ADDITIONAL_FEED_PRECISION) * amount) / PRECISION;
    }

    function getAccountInformation(
        address user
    )
        external
        view
        returns (uint256 totalDscMinted, uint256 collateralValueInUsd)
    {
        (totalDscMinted, collateralValueInUsd) = _getAccountInformation(user);
    }

    function getCollateralBalanceOfUser(
        address user,
        address token
    ) external view returns (uint256) {
        return s_collateralDeposited[user][token];
    }

    function getPrecision() external pure returns (uint256) {
        return PRECISION;
    }

    function getAdditionalFeedPrecision() external pure returns (uint256) {
        return ADDITIONAL_FEED_PRECISION;
    }

    function getLiquidationThreshold() external pure returns (uint256) {
        return LIQUIDATION_THRESHOLD;
    }

    function getLiquidationBonus() external pure returns (uint256) {
        return LIQUIDATION_BONUS;
    }

    function getLiquidationPrecision() external pure returns (uint256) {
        return LIQUIDATION_PRECISION;
    }

    function getMinHealthFactor() external pure returns (uint256) {
        return MIN_HEALTH_FACTOR;
    }

    function getCollateralTokens() external view returns (address[] memory) {
        return s_collateralTokens;
    }

    function getDsc() external view returns (address) {
        return address(i_dsc);
    }

    function getCollateralTokenPriceFeed(
        address token
    ) external view returns (address) {
        return s_priceFeeds[token];
    }

    function getHealthFactor(address user) external view returns (uint256) {
        return _healthFactor(user);
    }
}
```

# **测试**

## **单元测试**

这里提供一部分单元测试的case，并没有补充完全，只是起个抛砖引玉的作用，读者可根据需求自行扩展

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Test} from "forge-std/Test.sol";
import {DeployDSC} from "../../script/DeployDSC.s.sol";
import {DecentralizedStableCoin} from "../../src/DecentralizedStableCoin.sol";
import {DSCEngine} from "../../src/DSCEngine.sol";
import {HelperConfig} from "../../script/HelperConfig.s.sol";
import {ERC20Mock} from "@openzeppelin/contracts/mocks/token/ERC20Mock.sol";

contract DSCEngineTest is Test {
    DeployDSC deployer;
    DecentralizedStableCoin dsc;
    DSCEngine dsce;
    HelperConfig config;
    address ethUsdPriceFeed;
    address btcUsdPriceFeed;
    address weth;

    address public USER = makeAddr("user");
    uint256 public constant AMOUNT_COLLATERAL = 10 ether;
    uint256 public constant STARTING_ERC20_BALANCE = 10 ether;

    function setUp() public {
        deployer = new DeployDSC();
        (dsc, dsce, config) = deployer.run();
        (ethUsdPriceFeed, btcUsdPriceFeed, weth, , ) = config
            .activeNetworkConfig();
        ERC20Mock(weth).mint(USER, STARTING_ERC20_BALANCE);
    }

    address[] public tokenAddresses;
    address[] public priceFeedAddresses;

    function testRevertsIfTokenLengthDoesntMatchPriceFeeds() public {
        tokenAddresses.push(weth);
        priceFeedAddresses.push(ethUsdPriceFeed);
        priceFeedAddresses.push(btcUsdPriceFeed);

        vm.expectRevert(
            DSCEngine
                .DSCEngine__TokenAddressesAndPriceFeedAddressesMustBeSameLength
                .selector
        );
        new DSCEngine(tokenAddresses, priceFeedAddresses, address(dsc));
    }

    // Price Tests
    function testGetUsdValue() public view {
        uint256 ethAmount = 15e18;
        uint expectedUsd = 30000e18;
        uint256 actualUsd = dsce.getUsdValue(weth, ethAmount);
        assertEq(actualUsd, expectedUsd);
    }

    function testGetTokenAmountFromUsd() public view {
        uint256 usdAmount = 100 ether;
        uint256 expectedWeth = 0.05 ether;
        uint256 actualWeth = dsce.getTokenAmountFromUsd(weth, usdAmount);
        assertEq(actualWeth, expectedWeth);
    }

    // depositCollateral Tests
    function testRevertsIfCollateralZero() public {
        vm.startPrank(USER);
        ERC20Mock(weth).approve(address(dsce), AMOUNT_COLLATERAL);
        vm.expectRevert(DSCEngine.DSCEngine__NeedsMoreThanZero.selector);
        dsce.depositCollateral(weth, 0);
        vm.stopPrank();
    }

    function testRevertsWithUnapprovedCollateral() public {
        ERC20Mock ranToken = new ERC20Mock();
        vm.startPrank(USER);
        vm.expectRevert(DSCEngine.DSCEngine__NotAllowedToken.selector);
        dsce.depositCollateral(address(ranToken), AMOUNT_COLLATERAL);
        vm.stopPrank();
    }

    modifier depositedCollateral() {
        vm.startPrank(USER);
        ERC20Mock(weth).approve(address(dsce), AMOUNT_COLLATERAL);
        dsce.depositCollateral(weth, AMOUNT_COLLATERAL);
        vm.stopPrank();
        _;
    }

    function testCanDepositCollateralAndGetAccountInfo()
        public
        depositedCollateral
    {
        (uint256 totalDscMinted, uint256 collateralValueInUsd) = dsce
            .getAccountInformation(USER);

        uint256 expectedTotalDscMinted = 0;
        uint256 expectedDepositAmount = dsce.getTokenAmountFromUsd(
            weth,
            collateralValueInUsd
        );
        assertEq(totalDscMinted, expectedTotalDscMinted);
        assertEq(AMOUNT_COLLATERAL, expectedDepositAmount);
    }
}
```

## **模糊测试**

模糊测试并不像单元测试那样拥有很高的确定性，它会有很高的随机性，会不断的随机调用合约中的函数，并随机发送数据。优点是可以发现开发者未曾想到的边缘情况，并且测试过程高度自动化，能够快速测试大量的输入情况。

我们可以在 foundry.toml 中对模糊测试做一些配置

```ini
[invariant]
runs = 128
depth = 128
fail_on_revert = false
```

其中 runs 表示执行多少次测试，depth 表示每次测试的深度是多少（即调用多少次函数），

fail\_on\_revert是指发生 revert 之后要不要立即停止测试

在模糊测试中，最重要的是要找到合约中的**不变量**，比如我们这个稳定币合约，其中一个重要的不变量就是：**抵押品的价值一定会大于等于铸造出来的DSC的价值**

我们就可以基于这个不变量来写一段模糊测试的逻辑 (Invariants.t.sol)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Test, console} from "forge-std/Test.sol";
import {StdInvariant} from "forge-std/StdInvariant.sol";
import {DeployDSC} from "../../script/DeployDSC.s.sol";
import {DSCEngine} from "../../src/DSCEngine.sol";
import {DecentralizedStableCoin} from "../../src/DecentralizedStableCoin.sol";
import {HelperConfig} from "../../script/HelperConfig.s.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {Handler} from "./Handler.t.sol";

contract InvariantsTest is StdInvariant, Test {
    DeployDSC deployer;
    DSCEngine dsce;
    DecentralizedStableCoin dsc;
    HelperConfig config;
    address weth;
    address btc;
    Handler handler;

    function setUp() external {
        deployer = new DeployDSC();
        (dsc, dsce, config) = deployer.run();
        (, , weth, btc, ) = config.activeNetworkConfig();

        handler = new Handler(dsce, dsc);
        targetContract(address(handler));
    }

    function invariant_protocolMustHaveMoreValueThanTotalSupply() public view {
        uint256 totalSupply = dsc.totalSupply();
        uint256 totalWethDeposited = IERC20(weth).balanceOf(address(dsce));
        uint256 totalBtcDeposited = IERC20(btc).balanceOf(address(dsce));

        uint256 wethValue = dsce.getUsdValue(weth, totalWethDeposited);
        uint256 btcValue = dsce.getUsdValue(btc, totalBtcDeposited);

        console.log("wethValue: ", wethValue);
        console.log("btcValue: ", btcValue);
        console.log("totalSupply: ", totalSupply);
        console.log("times mint called: ", handler.timesMintIsCalled());
        assert(wethValue + btcValue >= totalSupply);
    }
}
```

我们本可以在 setUp 中直接指定当前合约为模糊测试的对象（targetContract），但是这样做没有什么意义，因为它会胡乱调用函数，如果我们不做条件限制，那自然会疯狂 revert

因为我们补充了一个 handler，在 handler 中我们做了如下限制

`mintDsc`

这个函数用于铸造DSC代币。它有以下条件限制：

-   如果没有用户存过抵押品，则函数直接返回，不执行任何铸币操作。
    
-   使用一个地址种子(`addressSeed`)来从已存入抵押品的用户列表(`userWithCollateralDeposited`)中选择一个用户地址来执行铸币操作。
    
-   计算该用户可以铸造的最大DSC数量，为其抵押品的USD价值的一半减去已经铸造的DSC数量。
    
-   如果计算出的最大铸造数量小于0，或者请求铸造的数量(`amount`)为0，则函数直接返回。
    
-   实际铸造的DSC数量被限制在0和最大铸造数量之间。
    
-   使用`vm.startPrank`和`vm.stopPrank`来模拟特定用户的操作，这意味着铸造操作是以该用户的身份执行的。
    

`depositCollateral`

这个函数用于存入抵押品。它的条件限制包括：

-   使用种子(`collateralSeed`)来选择存入的抵押品类型（WETH或WBTC）。
    
-   存入的抵押品数量(`amountCollateral`)被限制在1和`MAX_DEPOSIT_SIZE`之间。
    
-   使用`vm.startPrank`和`vm.stopPrank`来模拟`msg.sender`的操作，允许他们存入抵押品并批准转账到DSCEngine合约。
    
-   将`msg.sender`添加到已存入抵押品的用户列表中。
    

`redeemCollateral`

这个函数用于赎回抵押品。它的条件限制包括：

-   使用种子(`collateralSeed`)来选择赎回的抵押品类型（WETH或WBTC）。
    
-   赎回的抵押品数量(`amountCollateral`)被限制在0和用户在DSCEngine合约中的抵押品余额之间。
    
-   如果请求赎回的数量为0，则函数直接返回。
    

具体实现代码如下

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Test} from "forge-std/Test.sol";
import {StdInvariant} from "forge-std/StdInvariant.sol";
import {DeployDSC} from "../../script/DeployDSC.s.sol";
import {DSCEngine} from "../../src/DSCEngine.sol";
import {DecentralizedStableCoin} from "../../src/DecentralizedStableCoin.sol";
import {ERC20Mock} from "@openzeppelin/contracts/mocks/token/ERC20Mock.sol";
import {MockV3Aggregator} from "../mocks/MockV3Aggregator.sol";

contract Handler is Test {
    DSCEngine dsce;
    DecentralizedStableCoin dsc;

    ERC20Mock weth;
    ERC20Mock wbtc;
    MockV3Aggregator public ethUsdPriceFeed;

    uint256 MAX_DEPOSIT_SIZE = type(uint96).max;

    uint256 public timesMintIsCalled;
    address[] public userWithCollateralDeposited;

    constructor(DSCEngine _dscEngine, DecentralizedStableCoin _dsc) {
        dsce = _dscEngine;
        dsc = _dsc;

        address[] memory collateralTokens = dsce.getCollateralTokens();
        weth = ERC20Mock(collateralTokens[0]);
        wbtc = ERC20Mock(collateralTokens[1]);

        ethUsdPriceFeed = MockV3Aggregator(
            dsce.getCollateralTokenPriceFeed(address(weth))
        );
    }

    function mintDsc(uint256 amount, uint256 addressSeed) public {
        if (userWithCollateralDeposited.length == 0) {
            return;
        }
        address sender = userWithCollateralDeposited[
            addressSeed % userWithCollateralDeposited.length
        ];
        (uint256 totalDscMinted, uint256 collateralValueInUsd) = dsce
            .getAccountInformation(sender);
        uint256 maxDscToMint = (collateralValueInUsd / 2) - totalDscMinted;
        if (maxDscToMint < 0) {
            return;
        }

        amount = bound(amount, 0, uint256(maxDscToMint));
        if (amount == 0) {
            return;
        }

        vm.startPrank(sender);
        dsce.mintDsc(amount);
        vm.stopPrank();
        timesMintIsCalled++;
    }

    // redeem collateral
    function depositCollateral(
        uint256 collateralSeed,
        uint256 amountCollateral
    ) public {
        ERC20Mock collateral = _getCollateralFromSeed(collateralSeed);
        amountCollateral = bound(amountCollateral, 1, MAX_DEPOSIT_SIZE);

        vm.startPrank(msg.sender);
        collateral.mint(msg.sender, amountCollateral);
        collateral.approve(address(dsce), amountCollateral);
        dsce.depositCollateral(address(collateral), amountCollateral);
        vm.stopPrank();

        userWithCollateralDeposited.push(msg.sender);
    }

    function redeemCollateral(
        uint256 collateralSeed,
        uint256 amountCollateral
    ) public {
        ERC20Mock collateral = _getCollateralFromSeed(collateralSeed);
        uint256 maxCollateralToRedeem = dsce.getCollateralBalanceOfUser(
            address(collateral),
            msg.sender
        );
        amountCollateral = bound(amountCollateral, 0, maxCollateralToRedeem);
        if (amountCollateral == 0) {
            return;
        }
        dsce.redeemCollateral(address(collateral), amountCollateral);
    }

    // This breaks our invariant test suite!!
    // function updateCollateralPrice(uint96 newPrice) public {
    //     int256 newPriceInt = int256(uint256(newPrice));
    //     ethUsdPriceFeed.updateAnswer(newPriceInt);
    // }

    function _getCollateralFromSeed(
        uint256 collateralSeed
    ) private view returns (ERC20Mock) {
        if (collateralSeed % 2 == 0) {
            return weth;
        }
        return wbtc;
    }
}
```

通过这种方式，`Handler`合约为模糊测试提供了一个受控的环境，防止了测试过程中可能出现的极端情况，如超过逻辑限制的操作,使得测试能够在合理的操作范围内进行，并能够有效地揭示智能合约中的潜在问题。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



# Solidity 101 详细知识点讲解

## **1\. Hello Web3 (三行代码)**

### 核心概念

Solidity智能合约的基本结构和开发规范。

### 详细代码示例

```solidity
// SPDX-License-Identifier: MIT
// 许可证标识符，告诉他人代码的使用许可（MIT是最宽松的开源许可）
// 不写会有编译警告，但不影响功能

pragma solidity ^0.8.21;
// 指定编译器版本
// ^ 表示兼容 0.8.21 及以上的 0.8.x 版本
// 也可以写成 >=0.8.0 <0.9.0

// 合约定义：类似 Python 的 class
contract HelloWeb3 {
    // 状态变量：存储在区块链上，永久保存
    string public _string = "Hello Web3!";
    // public 自动生成 getter 函数
    
    // 也可以不初始化，后续赋值
    uint256 public number;
    
    // 构造函数在部署时执行一次
    constructor() {
        number = 42;
    }
}
```

### 重要细节

1.  **SPDX标识符**：必须写在文件第一行
    
2.  **pragma**：防止不同版本编译器产生不同结果
    
3.  **合约 vs 类**：
    
    -   Python类在内存中
        
    -   Solidity合约部署到区块链，有唯一地址
        

### 部署流程

```
编写代码 → 编译成字节码 → 部署到区块链 → 获得合约地址 → 通过地址调用
```

* * *

## **2\. 值类型 (Value Types)**

### 详细讲解

2.1 布尔型 (bool)

```solidity
contract BoolExample {
    bool public _bool = true;
    
    // 布尔运算
    bool public _bool1 = !_bool;        // 取非: false
    bool public _bool2 = _bool && _bool1; // 与: false
    bool public _bool3 = _bool || _bool1; // 或: true
    bool public _bool4 = _bool == _bool1; // 相等: false
    bool public _bool5 = _bool != _bool1; // 不相等: true
}
```

2.2 整数型

```solidity
contract IntExample {
    // 整数类型
    int public _int = -1;          // 整数，包括负数
    uint public _uint = 1;         // 无符号整数，只能是正数
    uint256 public _number = 20220330; // 256位无符号整数
    
    // uint 和 uint256 是一样的
    // 还有 uint8, uint16, uint32, ..., uint256（8的倍数）
    // int 同理：int8, int16, ..., int256
    
    // 整数运算
    uint256 public _number1 = _number + 1;   // 加法: 20220331
    uint256 public _number2 = 2**2;          // 指数: 4
    uint256 public _number3 = 7 % 2;         // 取模: 1
    bool public _numberbool = _number2 > _number3; // 比较: true
    
    // 最大值和最小值
    uint8 public maxUint8 = type(uint8).max;   // 255
    uint256 public maxUint256 = type(uint256).max; // 2^256 - 1
    int8 public minInt8 = type(int8).min;      // -128
    int8 public maxInt8 = type(int8).max;      // 127
}
```

**重要：溢出检查**

```solidity
// Solidity 0.8.0+ 自动检查溢出
contract OverflowExample {
    uint8 public x = 255;
    
    function overflow() public {
        x = x + 1;  // ❌ 会 revert，因为 256 > uint8 最大值
    }
    
    // 如果要允许溢出（不推荐），使用 unchecked
    function unsafeOverflow() public {
        unchecked {
            x = x + 1;  // 会变成 0
        }
    }
}
```

2.3 地址类型

```solidity
contract AddressExample {
    // 地址类型：20字节（以太坊地址长度）
    address public _address = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71;
    
    // payable address：可以接收ETH
    address payable public _address1 = payable(_address);
    
    // 地址类型成员
    uint256 public balance = _address1.balance; // 获取地址余额（单位：wei）
    
    function transfer() public {
        // 向地址转账 1 ETH
        _address1.transfer(1 ether);
    }
    
    function send() public {
        // send 返回 bool，需要手动检查
        bool success = _address1.send(1 ether);
        require(success, "Send failed");
    }
    
    function call() public {
        // call 是最灵活的方式（推荐）
        (bool success, ) = _address1.call{value: 1 ether}("");
        require(success, "Call failed");
    }
}
```

**地址对比 Python**

```python
# Python 中地址是字符串
address = "0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71"

# Solidity 中地址是类型，有特殊方法
address.balance  # 获取余额
address.transfer(amount)  # 转账
```

2.4 定长字节数组

```solidity
contract BytesExample {
    // bytes1 到 bytes32
    bytes32 public _byte32 = "MiniSolidity";  // 自动右填充0
    bytes1 public _byte = _byte32[0];  // 0x4d (M的ASCII码)
    
    // bytes32 常用于存储 hash
    bytes32 public hash = keccak256(abi.encodePacked("Hello"));
}
```

2.5 枚举 (Enum)

```solidity
contract EnumExample {
    // 枚举类型：为 uint 赋予名称
    enum ActionSet { Buy, Hold, Sell }
    
    ActionSet public action = ActionSet.Buy;  // 默认第一个元素，值为 0
    
    // 枚举可以与 uint 显式转换
    function enumToUint() external view returns(uint) {
        return uint(action);  // Buy = 0, Hold = 1, Sell = 2
    }
    
    function setAction(ActionSet newAction) external {
        action = newAction;
    }
}
```

* * *

## **3\. 函数**

### 详细语法

```solidity
function <函数名>(<参数类型> <参数名>) 
    <可见性> 
    <状态可变性> 
    [returns (<返回类型>)]
{
    // 函数体
}
```

### 3.1 函数可见性

```solidity
contract FunctionVisibility {
    uint256 private _x = 10;
    
    // 1. public: 内部和外部都可以调用
    function publicFunc() public pure returns(string memory) {
        return "public function";
    }
    
    // 2. private: 只能在合约内部调用，继承的合约也不能调用
    function privateFunc() private pure returns(string memory) {
        return "private function";
    }
    
    // 3. internal: 合约内部和继承的合约可以调用
    function internalFunc() internal pure returns(string memory) {
        return "internal function";
    }
    
    // 4. external: 只能从合约外部调用
    // 不能在合约内部直接调用，需要用 this.externalFunc()
    function externalFunc() external pure returns(string memory) {
        return "external function";
    }
    
    // 测试调用
    function testVisibility() public view returns(uint256) {
        publicFunc();      // ✅ 可以
        privateFunc();     // ✅ 可以
        internalFunc();    // ✅ 可以
        // externalFunc(); // ❌ 不能直接调用
        this.externalFunc(); // ✅ 通过 this 调用
        return _x;
    }
}

// 继承合约
contract Child is FunctionVisibility {
    function testInheritance() public pure returns(string memory) {
        // publicFunc();    // ✅ 可以
        // privateFunc();   // ❌ 不能访问父合约的 private
        // internalFunc();  // ✅ 可以
        return internalFunc();
    }
}
```

**可见性对比表**

| 可见性 | 合约内部 | 继承合约 | 外部调用 | Gas消耗 |
| --- | --- | --- | --- | --- |
| public | ✅ | ✅ | ✅ | 较高 |
| private | ✅ | ❌ | ❌ | 较低 |
| internal | ✅ | ✅ | ❌ | 较低 |
| external | ❌ (需this) | ❌ | ✅ | 最低 |

### 3.2 状态可变性

```solidity
contract StateMutability {
    uint256 public number = 5;
    
    // 1. pure: 不读取也不修改状态变量
    function addPure(uint256 _x, uint256 _y) public pure returns(uint256) {
        return _x + _y;
        // return number; // ❌ 不能读取状态变量
    }
    
    // 2. view: 只读取，不修改状态变量
    function addView() public view returns(uint256) {
        return number + 1;  // ✅ 可以读取
        // number = 10;      // ❌ 不能修改
    }
    
    // 3. 无修饰符: 可以读取和修改状态变量
    function add() public returns(uint256) {
        number = number + 1;  // ✅ 可以修改
        return number;
    }
    
    // 4. payable: 可以接收 ETH
    function deposit() public payable {
        // msg.value 是发送的 ETH 数量
    }
    
    // 获取合约余额
    function getBalance() public view returns(uint256) {
        return address(this).balance;
    }
}
```

**Gas 消耗对比**

-   `pure` 和 `view` 函数从外部调用时**不消耗 gas**
    
-   但如果在其他函数内部调用，仍然消耗 gas
    

```solidity
contract GasExample {
    uint256 public x = 10;
    
    function readX() public view returns(uint256) {
        return x;  // 外部调用不消耗 gas
    }
    
    function updateX() public {
        x = readX() + 1;  // 这里 readX() 会消耗 gas
    }
}
```

### 3.3 特殊函数

```solidity
contract SpecialFunctions {
    // receive: 接收 ETH 时调用（没有数据）
    receive() external payable {
        // 接收 ETH 的逻辑
    }
    
    // fallback: 调用不存在的函数时调用
    fallback() external payable {
        // 回退逻辑
    }
    
    // 示例：发送 ETH
    function sendETH(address payable _to) public payable {
        _to.transfer(msg.value);
    }
}
```

* * *

## **4\. 函数输出**

### 4.1 返回值方式

```solidity
contract ReturnExample {
    // 方式1: return 返回
    function returnMultiple() public pure returns(uint256, bool, uint256[3] memory) {
        return (1, true, [uint256(1), 2, 5]);
    }
    
    // 方式2: 命名式返回（自动初始化）
    function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array) {
        _number = 2;
        _bool = false;
        _array = [uint256(3), 2, 1];
        // 可以省略 return，自动返回命名变量
    }
    
    // 方式3: 命名式返回 + return
    function returnNamed2() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array) {
        _number = 2;
        _bool = false;
        _array = [uint256(3), 2, 1];
        return(_number, _bool, _array);  // 也可以显式返回
    }
}
```

### 4.2 解构赋值

```solidity
contract DestructuringExample {
    function returnMultiple() public pure returns(uint256, bool, uint256[3] memory) {
        return (1, true, [uint256(1), 2, 5]);
    }
    
    function readReturn() public pure {
        // 读取全部返回值
        (uint256 _number, bool _bool, uint256[3] memory _array) = returnMultiple();
        
        // 只读取部分返回值
        (uint256 _number2, , ) = returnMultiple();  // 只要第一个
        ( , bool _bool2, ) = returnMultiple();      // 只要第二个
        ( , , uint256[3] memory _array2) = returnMultiple();  // 只要第三个
    }
}
```

**与 Python 对比**

```python
# Python 解构
def return_multiple():
    return 1, True, [1, 2, 5]

# 接收全部
number, flag, arr = return_multiple()

# 只接收部分
number, *_ = return_multiple()
_, flag, _ = return_multiple()  # Python 不能像 Solidity 这样跳过
```

* * *

## **5\. 变量数据存储和作用域**

### 5.1 数据位置 (Data Location)

这是 Solidity 最重要的概念之一，直接影响 Gas 消耗。

```solidity
contract DataLocation {
    // 状态变量：自动存储在 storage
    uint256[] public storageArray;
    
    function dataLocation() public {
        // 1. storage: 存储在链上，永久保存，消耗gas多
        uint256[] storage sArray = storageArray;
        sArray.push(1);  // 会修改 storageArray
        
        // 2. memory: 存储在内存，函数执行后销毁，消耗gas少
        uint256[] memory mArray = new uint256[](5);
        mArray[0] = 1;  // 只在函数内有效
        
        // 3. calldata: 类似 memory，但不可修改（只读）
        // 只能用于函数参数，节省 gas
    }
    
    // calldata vs memory
    function calldataExample(uint256[] calldata _arr) external pure returns(uint256) {
        // _arr[0] = 1;  // ❌ calldata 不可修改
        return _arr[0];  // ✅ 只读
    }
    
    function memoryExample(uint256[] memory _arr) public pure returns(uint256) {
        _arr[0] = 1;  // ✅ memory 可修改
        return _arr[0];
    }
}
```

**Gas 对比**

```solidity
contract GasComparison {
    uint256[] public data;
    
    // 使用 memory: gas 约 50000
    function useMemory(uint256[] memory _arr) public {
        data = _arr;
    }
    
    // 使用 calldata: gas 约 45000（节省 10%）
    function useCalldata(uint256[] calldata _arr) external {
        data = _arr;
    }
}
```

### 5.2 数据位置规则

```solidity
contract LocationRules {
    struct Student {
        uint256 id;
        uint256 score;
    }
    
    Student public student;
    
    function initStudent() external {
        // storage 引用
        Student storage _student = student;
        _student.id = 1;
        _student.score = 80;
        // student 也被修改了
    }
    
    function copyStudent() external view returns(uint256) {
        // memory 拷贝
        Student memory _student = student;
        _student.score = 100;
        // student 不受影响
        return student.score;  // 仍然是 80
    }
}
```

**关键规则**

1.  **状态变量**默认是 `storage`
    
2.  **函数参数**和**返回值**默认是 `memory`
    
3.  **局部变量**（引用类型）必须显式指定
    
4.  `storage` 赋值给 `storage` → 引用（修改会影响原变量）
    
5.  `storage` 赋值给 `memory` → 拷贝（独立）
    

### 5.3 变量作用域

```solidity
contract Scope {
    // 1. 状态变量：合约层级，存储在链上
    uint256 public stateVar = 10;
    
    function test() public view returns(uint256) {
        // 2. 局部变量：函数内部
        uint256 localVar = 1;
        
        // 3. 全局变量：Solidity 内置
        address sender = msg.sender;       // 调用者地址
        uint256 blockNum = block.number;   // 当前区块号
        uint256 timestamp = block.timestamp; // 当前时间戳
        uint256 value = msg.value;         // 发送的 ETH 数量（wei）
        bytes memory data = msg.data;      // 完整的 calldata
        uint256 gasLeft = gasleft();       // 剩余 gas
        
        return localVar + stateVar;
    }
    
    // 4. 全局变量详解
    function globalVars() public payable returns(bytes memory) {
        // 交易相关
        address sender = msg.sender;  // 调用者
        uint256 value = msg.value;    // 发送的 ETH
        bytes4 sig = msg.sig;         // 函数选择器（前4字节）
        
        // 区块相关
        uint256 blockNumber = block.number;     // 区块号
        uint256 timestamp = block.timestamp;    // 时间戳（秒）
        uint256 difficulty = block.difficulty;  // 难度
        uint256 gasLimit = block.gaslimit;      // gas 限制
        address coinbase = block.coinbase;      // 矿工地址
        
        // 其他
        uint256 gasPrice = tx.gasprice;  // gas 价格
        address origin = tx.origin;      // 交易发起者（原始调用者）
        
        return msg.data;
    }
}
```

**msg.sender vs tx.origin**

```solidity
// 用户 → 合约A → 合约B
// 在合约B中:
// msg.sender = 合约A的地址
// tx.origin = 用户的地址

contract A {
    function callB(address _b) public {
        B(_b).whoCallsMe();
    }
}

contract B {
    function whoCallsMe() public view returns(address, address) {
        return (msg.sender, tx.origin);
        // (合约A地址, 用户地址)
    }
}
```

* * *

## **6\. 引用类型**

### 6.1 数组 (Array)

```solidity
contract ArrayExample {
    // 固定长度数组
    uint256[5] public fixedArray;
    
    // 动态数组
    uint256[] public dynamicArray;
    
    // 初始化
    uint256[] public arr1 = [1, 2, 3];
    
    // memory 数组必须用 new 创建，且长度固定
    function memoryArray() public pure returns(uint256[] memory) {
        uint256[] memory arr = new uint256[](5);
        arr[0] = 1;
        arr[1] = 2;
        // arr.push(3);  // ❌ memory 数组不能 push
        return arr;
    }
    
    // 数组方法
    function arrayMethods() public {
        // push: 添加元素到末尾
        dynamicArray.push(1);
        dynamicArray.push(2);
        
        // pop: 删除最后一个元素
        dynamicArray.pop();
        
        // length: 数组长度
        uint256 len = dynamicArray.length;
        
        // 通过索引访问和修改
        dynamicArray[0] = 100;
    }
    
    // 创建 memory 数组
    function createMemoryArray() public pure returns(uint256[] memory) {
        uint256[] memory arr = new uint256[](3);
        arr[0] = 1;
        arr[1] = 2;
        arr[2] = 3;
        return arr;
    }
}
```

**数组特殊用法**

```solidity
contract AdvancedArray {
    uint256[] public arr;
    
    // 获取整个数组
    function getArray() public view returns(uint256[] memory) {
        return arr;
    }
    
    // 删除元素（留下空位）
    function remove(uint256 index) public {
        delete arr[index];  // 设为 0，但不改变长度
    }
    
    // 删除元素（移动后面的元素）
    function removeAndShift(uint256 index) public {
        require(index < arr.length, "Index out of bounds");
        
        // 方法1: 移动所有后续元素
        for (uint256 i = index; i < arr.length - 1; i++) {
            arr[i] = arr[i + 1];
        }
        arr.pop();
        
        // 方法2: 用最后一个元素替换（改变顺序，但省 gas）
        // arr[index] = arr[arr.length - 1];
        // arr.pop();
    }
}
```

### 6.2 结构体 (Struct)

```solidity
contract StructExample {
    // 定义结构体
    struct Student {
        uint256 id;
        uint256 score;
        string name;
    }
    
    // 结构体变量
    Student public student;
    
    // 结构体数组
    Student[] public students;
    
    // 4种初始化方式
    function initStudent() external {
        // 方式1: 在函数中创建 storage 引用
        Student storage _student = student;
        _student.id = 1;
        _student.score = 80;
        _student.name = "Alice";
        
        // 方式2: 直接修改状态变量
        student.id = 2;
        student.score = 90;
        student.name = "Bob";
        
        // 方式3: 构造函数式（按顺序）
        students.push(Student(3, 85, "Charlie"));
        
        // 方式4: key-value 式
        students.push(Student({
            id: 4,
            score: 95,
            name: "David"
        }));
    }
    
    // 修改结构体
    function updateStudent(uint256 index, uint256 newScore) external {
        Student storage s = students[index];
        s.score = newScore;
    }
}
```

### 6.3 字符串 (String)

```solidity
contract StringExample {
    string public str = "Hello Web3";
    
    // 字符串连接
    function concat(string memory s1, string memory s2) public pure returns(string memory) {
        return string(abi.encodePacked(s1, s2));
    }
    
    // 字符串比较（Solidity 不支持直接比较）
    function compareStrings(string memory s1, string memory s2) public pure returns(bool) {
        return keccak256(abi.encodePacked(s1)) == keccak256(abi.encodePacked(s2));
    }
    
    // 获取字符串长度（需要转换为 bytes）
    function getLength(string memory s) public pure returns(uint256) {
        return bytes(s).length;
    }
}
```

### 6.4 Bytes

```solidity
contract BytesExample {
    // bytes: 动态字节数组
    bytes public dynamicBytes = "Hello";
    
    // bytes32: 定长字节数组
    bytes32 public fixedBytes = "Hello";
    
    function bytesExample() public view returns(bytes1) {
        // 可以通过索引访问
        return dynamicBytes[0];  // 0x48 (H)
    }
    
    // bytes 更省 gas（处理原始数据）
    // string 更易读（处理文本）
    function gasComparison() public {
        bytes memory b = "Hello";     // 更省 gas
        string memory s = "Hello";    // 更易读
    }
}
```

* * *

# Solidity 101 详细讲解（续）

## **7\. 映射类型 (Mapping)**

### 7.1 映射基础

映射是 Solidity 中最重要的数据结构，类似于 Python 的字典 (dict)，但有重要区别。

```solidity
contract MappingBasic {
    // 基本语法: mapping(KeyType => ValueType)
    mapping(uint256 => address) public idToAddress;  // ID -> 地址
    mapping(address => uint256) public balances;     // 地址 -> 余额
    mapping(address => bool) public isRegistered;    // 地址 -> 是否注册
    
    // 添加数据
    function set(uint256 _id, address _addr) public {
        idToAddress[_id] = _addr;
    }
    
    // 读取数据
    function get(uint256 _id) public view returns(address) {
        return idToAddress[_id];
        // 如果 key 不存在，返回默认值（address 的默认值是 0x0）
    }
    
    // 更新数据
    function update(uint256 _id, address _newAddr) public {
        idToAddress[_id] = _newAddr;  // 直接覆盖
    }
    
    // 删除数据（重置为默认值）
    function remove(uint256 _id) public {
        delete idToAddress[_id];  // 重置为 address(0)
    }
}
```

**Mapping vs Python Dict**

```python
# Python 字典
my_dict = {}
my_dict[1] = "Alice"
print(my_dict.get(2))  # None（不存在返回 None）
print(len(my_dict))     # 1（可以获取长度）
for key in my_dict:     # 可以遍历
    print(key)

# Solidity mapping 的限制:
# 1. 不能获取长度
# 2. 不能遍历所有 key
# 3. 不存在的 key 返回默认值（不是 null）
# 4. 只能是 storage，不能是 memory
```

### 7.2 映射的重要特性

```solidity
contract MappingFeatures {
    mapping(address => uint256) public balances;
    
    function testMapping() public {
        // 特性1: 所有可能的 key 都被初始化
        address randomAddr = address(0x123);
        uint256 balance = balances[randomAddr];  // 返回 0，不会报错
        
        // 特性2: 无法判断 key 是否存在
        // 0 可能表示: (a) 从未设置, (b) 设置为 0, (c) 被删除
        
        // 特性3: 不能获取所有 key
        // uint256 length = balances.length;  // ❌ 编译错误
        
        // 特性4: 不能遍历
        // for (address addr in balances) { }  // ❌ 不支持
    }
    
    // 解决方案: 使用辅助数组跟踪 key
    address[] public userList;
    
    function addUser(address _user, uint256 _balance) public {
        if (balances[_user] == 0) {  // 首次添加
            userList.push(_user);
        }
        balances[_user] = _balance;
    }
    
    // 现在可以遍历所有用户
    function getAllBalances() public view returns(address[] memory, uint256[] memory) {
        uint256[] memory allBalances = new uint256[](userList.length);
        for (uint256 i = 0; i < userList.length; i++) {
            allBalances[i] = balances[userList[i]];
        }
        return (userList, allBalances);
    }
}
```

### 7.3 嵌套映射

```solidity
contract NestedMapping {
    // 嵌套映射: mapping 的值也是 mapping
    // 例: ERC20 代币的授权额度
    mapping(address => mapping(address => uint256)) public allowances;
    // owner => spender => amount
    
    // 设置授权
    function approve(address _spender, uint256 _amount) public {
        allowances[msg.sender][_spender] = _amount;
    }
    
    // 查询授权额度
    function getAllowance(address _owner, address _spender) public view returns(uint256) {
        return allowances[_owner][_spender];
    }
    
    // 复杂嵌套: 三层映射
    // 游戏示例: 玩家 => 关卡 => 物品 => 数量
    mapping(address => mapping(uint256 => mapping(uint256 => uint256))) public inventory;
    
    function addItem(uint256 level, uint256 itemId, uint256 amount) public {
        inventory[msg.sender][level][itemId] += amount;
    }
}
```

### 7.4 映射的高级用法

```solidity
contract AdvancedMapping {
    // 1. 映射 + 结构体
    struct User {
        string name;
        uint256 age;
        bool isActive;
    }
    
    mapping(address => User) public users;
    
    function registerUser(string memory _name, uint256 _age) public {
        users[msg.sender] = User({
            name: _name,
            age: _age,
            isActive: true
        });
    }
    
    // 2. 映射 + 数组
    mapping(address => uint256[]) public userScores;
    
    function addScore(uint256 _score) public {
        userScores[msg.sender].push(_score);
    }
    
    function getScores(address _user) public view returns(uint256[] memory) {
        return userScores[_user];
    }
    
    // 3. 映射存储复杂数据
    struct Post {
        string content;
        uint256 timestamp;
        uint256 likes;
        address author;
    }
    
    mapping(uint256 => Post) public posts;  // postId => Post
    mapping(address => uint256[]) public userPosts;  // 用户的所有帖子ID
    uint256 public postCount;
    
    function createPost(string memory _content) public {
        uint256 postId = postCount++;
        posts[postId] = Post({
            content: _content,
            timestamp: block.timestamp,
            likes: 0,
            author: msg.sender
        });
        userPosts[msg.sender].push(postId);
    }
    
    function likePost(uint256 _postId) public {
        posts[_postId].likes++;
    }
}
```

### 7.5 实际应用：简单的 ERC20 代币

```solidity
contract SimpleToken {
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    // 余额映射
    mapping(address => uint256) public balanceOf;
    
    // 授权映射（嵌套）
    mapping(address => mapping(address => uint256)) public allowance;
    
    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * 10**uint256(decimals);
        balanceOf[msg.sender] = totalSupply;  // 创建者获得所有代币
    }
    
    // 转账
    function transfer(address _to, uint256 _value) public returns(bool) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        return true;
    }
    
    // 授权
    function approve(address _spender, uint256 _value) public returns(bool) {
        allowance[msg.sender][_spender] = _value;
        return true;
    }
    
    // 从授权账户转账
    function transferFrom(address _from, address _to, uint256 _value) public returns(bool) {
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Allowance exceeded");
        
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        return true;
    }
}
```

* * *

## **8\. 变量初始值**

### 8.1 值类型的初始值

```solidity
contract DefaultValues {
    // 值类型默认值
    bool public _bool;              // false
    string public _string;          // ""（空字符串）
    int public _int;                // 0
    uint public _uint;              // 0
    address public _address;        // 0x0000000000000000000000000000000000000000
    
    enum ActionSet { Buy, Hold, Sell }
    ActionSet public _enum;         // 第一个元素 Buy (0)
    
    // 查看默认值
    function test() public view returns(bool, uint, address) {
        return (_bool, _uint, _address);
        // 返回: (false, 0, 0x0000000000000000000000000000000000000000)
    }
}
```

**默认值对照表**

| 类型 | 默认值 | 十六进制表示 |
| --- | --- | --- |
| bool | false | - |
| uint/int | 0 | 0x00 |
| address | 0x0000…0000 | 20字节的0 |
| bytes32 | 0x0000…0000 | 32字节的0 |
| string | “” | - |
| enum | 第一个元素 | 0 |

### 8.2 引用类型的初始值

```solidity
contract ReferenceDefaults {
    // 引用类型默认值
    uint256[] public _array;        // []（空数组）
    
    struct Student {
        uint256 id;
        string name;
    }
    Student public _student;        // Student(0, "")
    
    mapping(uint256 => address) public _mapping;  // 所有值为默认值
    
    // 测试数组
    function testArray() public view returns(uint256) {
        return _array.length;  // 0
    }
    
    // 测试结构体
    function testStruct() public view returns(uint256, string memory) {
        return (_student.id, _student.name);  // (0, "")
    }
    
    // 测试映射
    function testMapping(uint256 key) public view returns(address) {
        return _mapping[key];  // address(0)
        // 注意: 任何未设置的 key 都返回默认值
    }
}
```

### 8.3 delete 操作符

`delete` 会将变量重置为初始值，而不是真正删除。

```solidity
contract DeleteExample {
    uint256 public number = 10;
    bool public flag = true;
    uint256[] public arr = [1, 2, 3];
    
    struct Person {
        string name;
        uint256 age;
    }
    Person public person = Person("Alice", 25);
    
    mapping(address => uint256) public balances;
    
    function testDelete() public {
        // 删除值类型
        delete number;  // number = 0
        delete flag;    // flag = false
        
        // 删除数组（清空整个数组）
        delete arr;     // arr = []
        
        // 删除结构体（所有字段重置）
        delete person;  // person = Person("", 0)
        
        // 删除映射中的某个值
        balances[msg.sender] = 100;
        delete balances[msg.sender];  // balances[msg.sender] = 0
        
        // 注意: delete 不能删除整个 mapping
        // delete balances;  // ❌ 编译错误
    }
    
    // delete 数组元素
    function deleteArrayElement() public {
        arr = [1, 2, 3, 4, 5];
        delete arr[2];  // arr = [1, 2, 0, 4, 5]（长度不变）
        // 删除元素会留下"空洞"，值变为 0
    }
}
```

### 8.4 初始值的实际应用

```solidity
contract InitialValueUsage {
    mapping(address => bool) public hasVoted;
    
    // 利用默认值 false 判断是否投过票
    function vote() public {
        require(!hasVoted[msg.sender], "Already voted");
        hasVoted[msg.sender] = true;
        // 投票逻辑
    }
    
    // 问题场景: 无法区分 "未设置" 和 "设置为0"
    mapping(address => uint256) public scores;
    
    function getScore(address _user) public view returns(uint256) {
        return scores[_user];
        // 返回 0: 可能是未设置，也可能是真的得了 0 分
    }
    
    // 解决方案1: 使用额外的 bool 映射
    mapping(address => bool) public hasScore;
    
    function setScore(address _user, uint256 _score) public {
        scores[_user] = _score;
        hasScore[_user] = true;
    }
    
    function getScoreSafe(address _user) public view returns(bool, uint256) {
        return (hasScore[_user], scores[_user]);
    }
    
    // 解决方案2: 使用特殊值（如 type(uint256).max）表示"未设置"
    uint256 constant NOT_SET = type(uint256).max;
    mapping(address => uint256) public scores2;
    
    function initScores2() public {
        scores2[msg.sender] = NOT_SET;
    }
}
```

* * *

## **9\. 常数**

### 9.1 constant（编译时常量）

```solidity
contract ConstantExample {
    // constant: 必须在声明时赋值，编译时确定
    uint256 public constant MY_UINT = 123;
    address public constant MY_ADDRESS = 0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc;
    
    // 常见用法: 配置参数
    uint256 public constant MAX_SUPPLY = 1000000 * 10**18;
    uint256 public constant DECIMALS = 18;
    string public constant TOKEN_NAME = "MyToken";
    
    // ❌ 不能这样写
    // uint256 public constant BLOCK_NUM = block.number;  // 运行时才知道
    // address public constant SENDER = msg.sender;       // 运行时才知道
    
    function getConstant() public pure returns(uint256) {
        return MY_UINT;  // 直接返回值，不读取存储
    }
}
```

### 9.2 immutable（部署时常量）

```solidity
contract ImmutableExample {
    // immutable: 可以在构造函数中赋值，部署后不可变
    address public immutable OWNER;
    uint256 public immutable DEPLOY_TIMESTAMP;
    uint256 public immutable INITIAL_SUPPLY;
    
    // 也可以声明时赋值
    uint256 public immutable FIXED_FEE = 100;
    
    constructor(uint256 _supply) {
        OWNER = msg.sender;                    // 部署者地址
        DEPLOY_TIMESTAMP = block.timestamp;    // 部署时间
        INITIAL_SUPPLY = _supply;              // 构造参数
    }
    
    // immutable 可以在构造函数中计算
    uint256 public immutable COMPUTED_VALUE;
    
    constructor(uint256 a, uint256 b) {
        COMPUTED_VALUE = a * b + 1000;
    }
}
```

### 9.3 constant vs immutable vs 普通变量

```solidity
contract CompareConstants {
    // 1. 普通状态变量: 可以修改，存储在 storage
    uint256 public normalVar = 100;
    
    // 2. constant: 编译时确定，不占用 storage
    uint256 public constant CONST_VAR = 100;
    
    // 3. immutable: 构造时确定，不占用 storage
    uint256 public immutable IMMUTABLE_VAR;
    
    constructor() {
        IMMUTABLE_VAR = 100;
    }
    
    // Gas 消耗对比
    function getNormal() public view returns(uint256) {
        return normalVar;  // 读取 storage: ~2100 gas
    }
    
    function getConstant() public pure returns(uint256) {
        return CONST_VAR;  // 直接返回值: ~200 gas
    }
    
    function getImmutable() public view returns(uint256) {
        return IMMUTABLE_VAR;  // 直接返回值: ~200 gas
    }
    
    // 修改测试
    function modify() public {
        normalVar = 200;      // ✅ 可以修改
        // CONST_VAR = 200;   // ❌ 编译错误
        // IMMUTABLE_VAR = 200; // ❌ 编译错误
    }
}
```

**对比总结**

| 特性 | constant | immutable | 普通变量 |
| --- | --- | --- | --- |
| 赋值时机 | 声明时 | 构造函数 | 任何时候 |
| 能否修改 | ❌ | ❌ | ✅ |
| 存储位置 | 字节码中 | 字节码中 | storage |
| Gas 消耗 | 极低 | 极低 | 高 |
| 使用场景 | 固定配置 | 部署时配置 | 可变状态 |

### 9.4 实际应用示例

```solidity
contract RealWorldConstants {
    // 代币配置（不会改变）
    string public constant NAME = "MyToken";
    string public constant SYMBOL = "MTK";
    uint8 public constant DECIMALS = 18;
    uint256 public constant MAX_SUPPLY = 21_000_000 * 10**DECIMALS;
    
    // 手续费配置
    uint256 public constant TRANSFER_FEE = 1;  // 0.01% = 1/10000
    uint256 public constant FEE_DENOMINATOR = 10000;
    
    // 时间常量
    uint256 public constant ONE_DAY = 86400;  // 秒
    uint256 public constant ONE_WEEK = 7 * ONE_DAY;
    
    // 部署时确定的值
    address public immutable FACTORY;
    address public immutable OWNER;
    uint256 public immutable CREATED_AT;
    
    constructor(address _factory) {
        FACTORY = _factory;
        OWNER = msg.sender;
        CREATED_AT = block.timestamp;
    }
    
    // 使用常量计算
    function calculateFee(uint256 amount) public pure returns(uint256) {
        return amount * TRANSFER_FEE / FEE_DENOMINATOR;
    }
    
    // 检查是否超过最大供应量
    function checkSupply(uint256 newSupply) public pure returns(bool) {
        return newSupply <= MAX_SUPPLY;
    }
}
```

* * *

## **10\. 控制流**

### 10.1 if-else 条件语句

```solidity
contract IfElseExample {
    function ifElseTest(uint256 _number) public pure returns(string memory) {
        // 基本 if-else
        if (_number > 10) {
            return "Greater than 10";
        } else if (_number > 5) {
            return "Greater than 5";
        } else {
            return "5 or less";
        }
    }
    
    // 三元运算符: condition ? trueValue : falseValue
    function ternaryTest(uint256 x) public pure returns(uint256) {
        return x >= 10 ? 1 : 2;
    }
    
    // 嵌套 if
    function nestedIf(uint256 a, uint256 b) public pure returns(string memory) {
        if (a > 0) {
            if (b > 0) {
                return "Both positive";
            } else {
                return "Only a positive";
            }
        } else {
            return "a not positive";
        }
    }
    
    // 实际应用: 权限检查
    address public owner;
    
    function restrictedFunction() public view {
        if (msg.sender != owner) {
            revert("Not owner");
        }
        // 只有 owner 能执行到这里
    }
}
```

### 10.2 for 循环

```solidity
contract ForLoopExample {
    // 基本 for 循环
    function forLoopTest() public pure returns(uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < 10; i++) {
            sum += i;
        }
        return sum;  // 0+1+2+...+9 = 45
    }
    
    // 遍历数组
    function sumArray(uint256[] memory arr) public pure returns(uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
        return sum;
    }
    
    // 使用 continue（跳过当前循环）
    function skipEven() public pure returns(uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < 10; i++) {
            if (i % 2 == 0) {
                continue;  // 跳过偶数
            }
            sum += i;
        }
        return sum;  // 1+3+5+7+9 = 25
    }
    
    // 使用 break（终止循环）
    function findFirst() public pure returns(uint256) {
        for (uint256 i = 0; i < 100; i++) {
            if (i == 50) {
                break;  // 找到就停止
            }
        }
        return 50;
    }
}
```

**⚠️ Gas 陷阱：避免无界循环**

```solidity
contract GasTrap {
    uint256[] public data;
    
    // ❌ 危险: 循环次数不可控
    function badLoop() public view returns(uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < data.length; i++) {
            sum += data[i];
        }
        return sum;
        // 如果 data 有 100,000 个元素，gas 会耗尽，交易失败
    }
    
    // ✅ 安全: 限制循环次数
    function goodLoop(uint256 start, uint256 end) public view returns(uint256) {
        require(end - start <= 100, "Too many iterations");
        uint256 sum = 0;
        for (uint256 i = start; i < end && i < data.length; i++) {
            sum += data[i];
        }
        return sum;
    }
    
    // ✅ 更好: 分批处理
    function batchProcess(uint256 batchSize, uint256 batchNumber) public view returns(uint256) {
        uint256 start = batchSize * batchNumber;
        uint256 end = start + batchSize;
        if (end > data.length) {
            end = data.length;
        }
        
        uint256 sum = 0;
        for (uint256 i = start; i < end; i++) {
            sum += data[i];
        }
        return sum;
    }
}
```

### 10.3 while 循环

```solidity
contract WhileLoopExample {
    // while 循环
    function whileTest() public pure returns(uint256) {
        uint256 sum = 0;
        uint256 i = 0;
        
        while (i < 10) {
            sum += i;
            i++;
        }
        return sum;  // 45
    }
    
    // do-while 循环（至少执行一次）
    function doWhileTest() public pure returns(uint256) {
        uint256 sum = 0;
        uint256 i = 0;
        
        do {
            sum += i;
            i++;
        } while (i < 10);
        
        return sum;  // 45
    }
    
    // while 的实际应用: 查找
    function findElement(uint256[] memory arr, uint256 target) public pure returns(bool, uint256) {
        uint256 i = 0;
        while (i < arr.length) {
            if (arr[i] == target) {
                return (true, i);  // 找到了
            }
            i++;
        }
        return (false, 0);  // 没找到
    }
}
```

### 10.4 控制流实际应用

```solidity
contract ControlFlowPractice {
    struct Proposal {
        string description;
        uint256 voteCount;
        bool executed;
    }
    
    Proposal[] public proposals;
    mapping(uint256 => mapping(address => bool)) public hasVoted;
    
    // 创建提案
    function createProposal(string memory _description) public {
        proposals.push(Proposal({
            description: _description,
            voteCount: 0,
            executed: false
        }));
    }
    
    // 投票（使用 if 判断）
    function vote(uint256 _proposalId) public {
        if (_proposalId >= proposals.length) {
            revert("Invalid proposal");
        }
        if (hasVoted[_proposalId][msg.sender]) {
            revert("Already voted");
        }
        
        proposals[_proposalId].voteCount++;
        hasVoted[_proposalId][msg.sender] = true;
    }
    
    // 获取所有提案（使用 for 循环）
    function getAllProposals() public view returns(Proposal[] memory) {
        Proposal[] memory allProposals = new Proposal[](proposals.length);
        for (uint256 i = 0; i < proposals.length; i++) {
            allProposals[i] = proposals[i];
        }
        return allProposals;
    }
    
    // 查找最高票数提案（使用 for + if）
    function findTopProposal() public view returns(uint256) {
        require(proposals.length > 0, "No proposals");
        
        uint256 maxVotes = 0;
        uint256 topProposalId = 0;
        
        for (uint256 i = 0; i < proposals.length; i++) {
            if (proposals[i].voteCount > maxVotes) {
                maxVotes = proposals[i].voteCount;
                topProposalId = i;
            }
        }
        return topProposalId;
    }
    
    // 批量处理（防止 gas 耗尽）
    function executeProposals(uint256 start, uint256 count) public {
        uint256 end = start + count;
        if (end > proposals.length) {
            end = proposals.length;
        }
        
        for (uint256 i = start; i < end; i++) {
            // 三元运算符
            bool shouldExecute = proposals[i].voteCount > 100 ? true : false;
            if (shouldExecute && !proposals[i].executed) {
                proposals[i].executed = true;
                // 执行提案逻辑
            }
        }
    }
}
```

* * *

## **11\. 构造函数和修饰器**

### 11.1 构造函数 (Constructor)

构造函数在合约部署时执行一次，用于初始化状态。

```solidity
contract ConstructorExample {
    address public owner;
    uint256 public createdAt;
    string public name;
    
    // 构造函数: 使用 constructor 关键字
    constructor(string memory _name) {
        owner = msg.sender;          // 部署者成为 owner
        createdAt = block.timestamp; // 记录创建时间
        name = _name;                // 初始化名称
    }
    
    // ❌ 旧版本写法（Solidity 0.4.x，已废弃）
    // function ConstructorExample() public { }
}
```

**构造函数特性**

1.  只在部署时执行一次
    
2.  可以有参数（部署时传入）
    
3.  可以是 `public` 或 `internal`
    
4.  不能有 `returns`
    
5.  不能调用 `virtual` 或 `override`
    

```solidity
contract ConstructorFeatures {
    uint256 public value;
    address public owner;
    bool public initialized;
    
    // 带参数的构造函数
    constructor(uint256 _initialValue) {
        require(_initialValue > 0, "Value must be positive");
        value = _initialValue;
        owner = msg.sender;
        initialized = true;
    }
    
    // 部署示例:
    // new ConstructorFeatures(100);  // value = 100
}
```

### 11.2 修饰器 (Modifier)

修饰器用于在函数执行前/后添加条件检查或逻辑。

```solidity
contract ModifierBasic {
    address public owner;
    uint256 public counter;
    bool public paused;
    
    constructor() {
        owner = msg.sender;
    }
    
    // 1. 基本修饰器: 权限检查
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;  // _ 表示被修饰函数的代码在这里执行
    }
    
    // 2. 带参数的修饰器
    modifier minValue(uint256 _min) {
        require(msg.value >= _min, "Insufficient payment");
        _;
    }
    
    // 3. 多条件修饰器
    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }
    
    // 使用修饰器
    function changeOwner(address _newOwner) public onlyOwner {
        owner = _newOwner;
    }
    
    function deposit() public payable minValue(1 ether) {
        // 只有发送 >= 1 ETH 才能执行
        counter++;
    }
    
    function increment() public whenNotPaused {
        counter++;
    }
    
    // 多个修饰器（按顺序执行）
    function specialFunction() public onlyOwner whenNotPaused {
        // 先检查 onlyOwner，再检查 whenNotPaused
        counter += 10;
    }
}
```

### 11.3 修饰器的执行顺序

```solidity
contract ModifierOrder {
    uint256 public value;
    
    modifier modifier1() {
        value = 1;
        _;  // 执行函数
        value = 10;
    }
    
    modifier modifier2() {
        value = 2;
        _;  // 执行函数
        value = 20;
    }
    
    // 修饰器执行顺序: modifier1 → modifier2 → 函数 → modifier2收尾 → modifier1收尾
    function test() public modifier1 modifier2 {
        value = 100;
    }
    
    // 执行流程:
    // 1. modifier1: value = 1
    // 2. modifier2: value = 2
    // 3. test():    value = 100
    // 4. modifier2: value = 20
    // 5. modifier1: value = 10
    // 最终 value = 10
}
```

### 11.4 修饰器高级用法

```solidity
contract AdvancedModifiers {
    address public owner;
    mapping(address => bool) public admins;
    uint256 private lockCounter;
    
    constructor() {
        owner = msg.sender;
        admins[msg.sender] = true;
    }
    
    // 1. 角色权限修饰器
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner");
        _;
    }
    
    modifier onlyAdmin() {
        require(admins[msg.sender], "Only admin");
        _;
    }
    
    // 2. 防重入修饰器
    modifier nonReentrant() {
        lockCounter++;
        uint256 localCounter = lockCounter;
        _;
        require(localCounter == lockCounter, "Reentrant call");
    }
    
    // 3. 时间锁修饰器
    uint256 public unlockTime;
    
    modifier afterUnlock() {
        require(block.timestamp >= unlockTime, "Still locked");
        _;
    }
    
    // 4. 值范围检查
    modifier validAmount(uint256 amount) {
        require(amount > 0 && amount <= 1000 ether, "Invalid amount");
        _;
    }
    
    // 5. 地址验证
    modifier validAddress(address _addr) {
        require(_addr != address(0), "Zero address");
        require(_addr != address(this), "Contract address");
        _;
    }
    
    // 使用示例
    function setUnlockTime(uint256 _seconds) public onlyOwner {
        unlockTime = block.timestamp + _seconds;
    }
    
    function withdraw() public onlyOwner afterUnlock nonReentrant {
        // 安全的提款函数
        (bool success, ) = msg.sender.call{value: address(this).balance}("");
        require(success, "Transfer failed");
    }
    
    function addAdmin(address _admin) public onlyOwner validAddress(_admin) {
        admins[_admin] = true;
    }
}
```

### 11.5 实际应用：完整的访问控制系统

```solidity
contract AccessControl {
    // 状态变量
    address public owner;
    mapping(address => bool) public blacklist;
    mapping(address => uint256) public lastActionTime;
    bool public paused;
    
    // 常量
    uint256 public constant COOLDOWN = 1 minutes;
    
    // 事件
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event Paused(address account);
    event Unpaused(address account);
    
    // 构造函数
    constructor() {
        owner = msg.sender;
        emit OwnershipTransferred(address(0), msg.sender);
    }
    
    // 修饰器
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }
    
    modifier notBlacklisted() {
        require(!blacklist[msg.sender], "Blacklisted");
        _;
    }
    
    modifier whenNotPaused() {
        require(!paused, "Paused");
        _;
    }
    
    modifier whenPaused() {
        require(paused, "Not paused");
        _;
    }
    
    modifier cooldown() {
        require(
            block.timestamp >= lastActionTime[msg.sender] + COOLDOWN,
            "Cooldown not finished"
        );
        _;
        lastActionTime[msg.sender] = block.timestamp;
    }
    
    // 管理函数
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Zero address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
    
    function addToBlacklist(address _user) public onlyOwner {
        blacklist[_user] = true;
    }
    
    function removeFromBlacklist(address _user) public onlyOwner {
        blacklist[_user] = false;
    }
    
    function pause() public onlyOwner whenNotPaused {
        paused = true;
        emit Paused(msg.sender);
    }
    
    function unpause() public onlyOwner whenPaused {
        paused = false;
        emit Unpaused(msg.sender);
    }
    
    // 业务函数（带多重保护）
    function doSomething() public 
        whenNotPaused 
        notBlacklisted 
        cooldown 
    {
        // 实际业务逻辑
        // 只有满足所有条件才能执行
    }
}
```

### 11.6 构造函数 + 修饰器综合示例

```solidity
contract CompleteExample {
    // 状态变量
    string public name;
    address public owner;
    uint256 public totalSupply;
    uint256 public createdAt;
    mapping(address => uint256) public balances;
    
    // 构造函数初始化
    constructor(string memory _name, uint256 _initialSupply) {
        name = _name;
        owner = msg.sender;
        totalSupply = _initialSupply;
        createdAt = block.timestamp;
        balances[msg.sender] = _initialSupply;  // 创建者获得全部代币
    }
    
    // 修饰器
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }
    
    modifier hasBalance(uint256 amount) {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        _;
    }
    
    // 使用修饰器的函数
    function transfer(address to, uint256 amount) public hasBalance(amount) {
        balances[msg.sender] -= amount;
        balances[to] += amount;
    }
    
    function mint(address to, uint256 amount) public onlyOwner {
        balances[to] += amount;
        totalSupply += amount;
    }
}
```

* * *
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




今天好忙 先打卡占位 等会来补
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





**写合约需要特殊的语言:Solidity**

区块链上运行的是一个特殊的"虚拟计算机"叫EVM(以太坊虚拟机)。就像你的手机只能装手机App,EVM也只能运行特定语言写的程序。Solidity就是专门为EVM设计的编程语言。

**Remix:网页版的编辑器**

这就像Word是写文档的工具,Remix是写智能合约的在线工具。你在网页里写代码,点一下按钮就能编译、部署到区块链上。它的好处是:

-   不用在自己电脑装软件,打开网页就能用
    
-   界面简单,适合新手学习
    
-   可以立刻看到合约运行的效果
    

**Hardhat/Foundry:专业开发工具**

当你的项目变复杂,Remix就不够用了。就像你用记事本能写购物清单,但写一本书就得用Word。

Hardhat是本地开发环境,它能让你:

-   在自己电脑上模拟一个假的区块链,不花真钱就能测试
    
-   写自动化测试,检查合约的每个功能是否正常
    
-   管理多个合约、多个版本
    
-   一键部署到不同的网络(测试网、正式网)
    

**MetaMask:你的数字钱包**

这是一个浏览器插件,相当于你的"区块链身份证+钱包"。它帮你:

-   管理你的账户和私钥(私钥就像银行卡密码,丢了就找不回来)
    
-   签署交易(每次和区块链交互都需要你用私钥签名证明"这真是我本人操作")
    
-   支付gas费(区块链上做任何事都要付费,就像寄快递要付运费)
    

好，我用最简单的方式给你解释这整个过程。

## 核心思路：写代码到部署的完整流程

想象你要**开一家自动售货机店**，整个过程是这样的：

### 第一步：写说明书（写Solidity代码）

你用人类语言写了一份详细的操作手册：

-   “如果有人投入1元，就给他一瓶水”
    
-   “如果有人投入5元，就给他一包薯片”
    
-   “只有老板能补货”
    

这就是你的**Solidity源代码**，人类能看懂。

* * *

## 第二步：编译 - 把说明书翻译成多个版本

但是机器看不懂人类语言，所以编译器要把你的代码翻译成多份不同的东西：

### 1\. **字节码（Bytecode）** - 机器能执行的指令

这是**机器语言版本**的说明书，像这样：

```
0x6080604052348015...（一长串十六进制数字）
```

机器（EVM）只认这个，人类完全看不懂。

**分两种：**

**创建字节码（Creation Bytecode）**

-   这是"安装说明书"
    
-   包含怎么把售货机组装起来、第一次启动的步骤
    
-   只在部署时用一次
    

**运行时字节码（Runtime Bytecode）**

-   这是"日常操作说明"
    
-   售货机装好后，每天运行的逻辑
    
-   这个会永久存在链上，每次有人用售货机都执行这个
    

**类比：**

-   创建字节码 = 宜家家具的组装图纸，只用一次
    
-   运行时字节码 = 组装好的柜子，每天都在用
    

* * *

### 2\. **ABI（Application Binary Interface）** - 使用说明书

这是给**外部程序看的菜单**，用JSON格式写成：

json

````json
{
  "functions": [
    {
      "name": "buyWater",
      "inputs": [{"type": "uint256", "name": "money"}],
      "outputs": [{"type": "bool"}]
    }
  ]
}
```

**用途：**
- 前端网页想调用你的合约，需要看这个ABI才知道：
  - 这个合约有哪些功能？（有buyWater、buyChips等函数）
  - 每个功能需要什么参数？（buyWater需要传入金额）
  - 会返回什么结果？（成功true/失败false）

**类比：**
- ABI = 餐厅菜单
- 字节码 = 厨房里的实际烹饪流程
- 顾客（前端）只需要看菜单（ABI）点菜，不需要懂厨房怎么做菜（字节码）

---

### 3. **元数据（Metadata）** - 产品质检报告

这是一个JSON文件，记录了：
- 这个合约是用哪个版本的编译器编译的
- 源代码存在哪里（比如IPFS链接）
- ABI的完整内容
- 编译时的各种设置

**用途：**
- 让别人能**验证**你的合约是不是真的用这份源代码编译的
- Etherscan等网站用它来显示源代码和ABI
- 防止有人部署恶意代码后声称是别的东西

**类比：**
- 元数据 = 食品包装上的成分表+生产日期+厂家信息
- 让消费者知道这个产品从哪来、怎么做的、是不是正品

**特别的地方：**
编译器会把元数据的哈希值**悄悄附加在字节码末尾**，像产品序列号一样，方便追溯。

---

### 4. **其他调试用的东西**（不太常用）

**AST（抽象语法树）**
- 这是代码的**内部结构图**
- 编译器用它来理解你代码的逻辑
- 安全工具（如Slither）用它来找漏洞

**类比：** 机器的电路图，维修师傅需要，普通用户不看

**Source Map（源映射）**
- 记录"第123条字节码指令对应源代码第45行"
- 调试时能让你看到"当前执行到哪一行代码"

**类比：** 电影字幕的时间轴，调试时定位问题用

**Assembly / Yul**
- 比字节码稍微好读一点的"汇编代码"
- 专家用来优化性能或深度审计

**类比：** 机械设计图纸，工程师优化用，普通人不看

---

## 第三步：部署 - 把售货机放到街上

**你把字节码上传到区块链**：
1. 发起一笔"部署交易"
2. 支付gas费（相当于运输费+安装费）
3. 矿工验证后，把你的字节码写入区块链

**部署成功后你会得到：**

### **合约地址（Contract Address）**
- 这是你售货机的**街道地址**
- 比如：`0x1234567890abcdef...`
- 以后所有人要用你的售货机，都访问这个地址

**类比：** 你的售货机装在"区块链大街123号"，这就是门牌号

### **部署交易哈希**
- 这次部署操作的**收据编号**
- 可以在区块浏览器（Etherscan）查看部署的详细过程

---

## 完整流程串起来

**编译阶段（在你电脑上）：**
```
Solidity代码
    ↓ 编译器处理
    ├─ 字节码（给机器执行）
    ├─ ABI（给前端调用）
    ├─ 元数据（给验证用）
    └─ 调试信息（给开发者调试）
```

**部署阶段（上传到区块链）：**
```
1. 把字节码发送到区块链
2. 支付gas费
3. 得到合约地址
4. 在本地保存ABI文件
```

**使用阶段（前端调用）：**
```
前端代码：
1. 读取本地的ABI文件（知道合约有什么功能）
2. 用合约地址定位合约（找到售货机在哪）
3. 调用ABI中定义的函数（点菜）
4. 通过MetaMask签名交易（付款）
5. 合约执行字节码逻辑（售货机出货）
```

---

## 为什么需要这么多东西？

**字节码** - 必须有，机器要执行  
**ABI** - 必须有，前端要调用  
**元数据** - 可选但强烈建议，用于验证透明度  
**其他调试信息** - 可选，开发和审计时用  

---

## 实际操作中你会遇到的

**用Hardhat部署后：**

你的项目文件夹会有这些：
```
my-project/
├── contracts/
│   └── MyContract.sol          # 你写的源代码
├── artifacts/
│   └── contracts/
│       └── MyContract.sol/
│           └── MyContract.json  # 包含ABI+字节码+元数据
└── scripts/
    └── deploy.js                # 部署脚本
```

**部署后，终端会显示：**
```
Contract deployed to: 0xABC123...  （合约地址）
Transaction hash: 0xDEF456...      （部署交易哈希）
````

**前端要用时：**

javascript

```javascript
// 读取编译产生的ABI
const ABI = require('./artifacts/.../MyContract.json').abi;

// 用地址+ABI连接合约
const contract = new ethers.Contract(
  '0xABC123...',  // 合约地址
  ABI,            // ABI告诉前端怎么调用
  signer          // 你的钱包
);

// 调用合约函数
await contract.buyWater(1);
```

* * *

## 一句话总结

**编译 = 把人类语言翻译成机器语言 + 生成说明书**  
**部署 = 把机器语言上传到区块链，得到地址**  
**使用 = 用地址找到合约，用ABI调用功能**

你可以想象成：

-   编译 = 工厂生产产品（字节码）+ 写说明书（ABI）
    
-   部署 = 把产品放到商店（区块链）某个货架（地址）
    
-   使用 = 顾客看说明书（ABI）去货架（地址）买东西
    

好，我把这段拆成两部分给你讲清楚：**第一部分是合约的组成部分**，**第二部分是部署要花多少钱**。

* * *

# 第一部分：智能合约的组成部分

把智能合约想象成一个**自动售货机**，它由几个核心部分组成：

## 1\. 状态变量（State Variables）- 售货机的库存记录

**这是什么？** 永久存储在区块链上的数据，就像售货机的库存账本。

**举例：**

```solidity
uint public colaCount = 100;      // 可乐库存100瓶
address public owner;             // 售货机老板的地址
mapping(address => uint) public credits;  // 每个人的储值余额
```

**特点：**

-   **持久性**：断电重启也不会丢失（永久存在链上）
    
-   **可见性**：
    
    -   `public`：任何人都能查看（像售货机显示屏，所有人能看库存）
        
    -   `private`：只有合约内部能访问（像老板的私人记账本）
        
    -   `internal`：合约自己和"子合约"能访问
        

**类比：**

-   状态变量 = 售货机的库存记录本
    
-   每次有人买东西，库存数字就改变
    
-   这个记录本永久保存在区块链上，所有人都能验证
    

* * *

## 2\. 函数（Functions）- 售货机的按钮

**这是什么？** 执行操作的代码块，就像售货机上的按钮。

**举例：**

```solidity
// 购买可乐的按钮
function buyCola() public payable {
    require(msg.value >= 1 ether, "钱不够");
    require(colaCount > 0, "卖光了");
    colaCount--;  // 库存减1
    // 给你可乐...
}

// 查看库存的按钮（只读）
function getStock() public view returns (uint) {
    return colaCount;
}

// 老板补货的按钮
function refill(uint amount) public {
    require(msg.sender == owner, "只有老板能补货");
    colaCount += amount;
}
```

**函数的可见性：**

-   `public`：任何人都能按这个按钮（顾客可以买可乐）
    
-   `external`：只能从外部按（必须是外来的人，合约自己内部不能调用）
    
-   `private`：只有内部代码能用（内部检查机制，外人看不到）
    
-   `internal`：自己和继承的合约能用
    

**函数的状态特性：**

-   `view`：只看不改（查看库存，不花钱）
    
-   `pure`：既不看也不改（纯计算，比如2+2=4）
    
-   `payable`：可以收钱的函数（买东西要付钱）
    
-   默认：可以修改状态（补货、扣库存）
    

**特殊函数：**

-   `receive()`：专门接收别人转账的（有人直接往售货机里塞钱）
    
-   `fallback()`：按了不存在的按钮时触发（按了个坏掉的按钮）
    

* * *

## 3\. 事件（Events）- 售货机的小票

**这是什么？** 合约发出的"通知"，记录发生了什么事。

**举例：**

```solidity
event ColaSold(address buyer, uint amount, uint timestamp);

function buyCola() public payable {
    // ... 卖可乐的逻辑
    emit ColaSold(msg.sender, 1, block.timestamp);  // 发出事件
}
```

**为什么需要事件？**

-   **便宜**：事件存在日志里，比状态变量便宜很多
    
-   **通知前端**：网页可以监听事件，实时更新界面（"刚卖出一瓶可乐！"）
    
-   **历史记录**：所有销售记录都保存着，可以追溯
    

**类比：**

-   事件 = 售货机打印的小票
    
-   每次有人买东西，打印一张小票
    
-   小票存档方便以后查账
    

**indexed参数：**

```solidity
event ColaSold(address indexed buyer, uint amount);
```

`indexed`就像给小票加标签，方便查找"所有张三买的可乐"。

* * *

## 4\. 构造函数（Constructor）- 售货机的安装程序

**这是什么？** 部署合约时只执行一次的初始化代码。

**举例：**

```solidity
constructor(uint initialStock) {
    owner = msg.sender;           // 部署者成为老板
    colaCount = initialStock;     // 设置初始库存
}
```

**特点：**

-   **只执行一次**：售货机安装时运行，安装完就不能再跑
    
-   **初始化设置**：设定老板是谁、初始库存多少
    

**类比：** 售货机刚运到的时候，技术人员：

-   设置老板密码
    
-   装入第一批货物
    
-   校准找零系统 这些操作只在安装时做一次，之后就不需要了。
    

* * *

## 5\. 修饰符（Modifiers）- 权限检查门卫

**这是什么？** 在函数执行前自动检查条件的代码。

**举例：**

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "你不是老板");
    _;  // 继续执行函数
}

function refill(uint amount) public onlyOwner {
    colaCount += amount;
}
```

**作用：**

-   避免重复写检查代码
    
-   像门卫一样：只有老板才能进补货间
    

**类比：**

-   Modifier = 门禁卡检查
    
-   每次进入特定区域，自动刷卡验证身份
    
-   不是老板？门都进不去，别谈补货了
    

* * *

## 6\. 结构体和枚举（Struct/Enum）- 复杂数据类型

**Struct - 打包的数据**

```solidity
struct Product {
    string name;
    uint price;
    uint stock;
}

Product public cola = Product("Coca Cola", 1 ether, 100);
```

**类比：** 把商品信息打包成一个整体（名字+价格+库存）

**Enum - 有限的选项**

```solidity
enum Status { Pending, Active, Cancelled }
Status public machineStatus = Status.Active;
```

**类比：** 售货机的状态只能是：待机、运行中、维修中（不能是其他乱七八糟的状态）

* * *

## 7\. 自定义错误（Custom Errors）- 更清晰的错误提示

**旧方法：**

```solidity
require(amount <= balance, "Insufficient balance");
```

**新方法（更省gas）：**

```solidity
error InsufficientBalance(uint requested, uint available);

function withdraw(uint amount) public {
    if (amount > balance) {
        revert InsufficientBalance({
            requested: amount,
            available: balance
        });
    }
}
```

**好处：**

-   更省gas费
    
-   错误信息更详细（告诉你要多少、有多少）
    

* * *

# 第二部分：部署合约要花多少钱？

部署合约就像**开一家店**，需要付各种费用。核心公式：

```
总费用 = Gas消耗量 × Gas价格
```

## 1\. Gas消耗量（Gas Used）- 你做了多少事

**部署时消耗Gas的地方：**

### 基础费用：21,000 Gas

任何交易的基本开销，就像快递的起步价。

### 创建合约费用：32,000 Gas

使用CREATE指令创建新合约的固定成本。

### 存储代码费用：200 Gas/字节

你的合约代码要永久存在链上，每个字节收200 Gas。

**举例：**

-   你的合约编译后是10,000字节
    
-   存储成本 = 10,000 × 200 = 2,000,000 Gas
    

**类比：** 开店要租仓库，仓库越大租金越贵

### 构造函数执行费用

构造函数里做的所有操作都要花钱：

-   简单的赋值：便宜
    
-   复杂的循环计算：很贵
    

**举例：**

```solidity
// 便宜的构造函数
constructor() {
    owner = msg.sender;  // 只花几千Gas
}

// 昂贵的构造函数
constructor() {
    for(uint i = 0; i < 1000; i++) {
        data[i] = i;  // 循环1000次写存储，非常贵！
    }
}
```

### 初始化存储费用：20,000 Gas/槽

第一次往存储位置写数据（从0变成非0），每个槽约20,000 Gas。

**这是最贵的操作！**

**类比：**

-   在笔记本上写字：便宜（临时变量）
    
-   在石碑上刻字：超贵（永久存储）
    

**总结：** 一个中等复杂的合约，部署通常消耗：**几十万到几百万Gas**

* * *

## 2\. Gas价格 - 每单位Gas多少钱

以太坊现在用的是**EIP-1559机制**（2021年升级后）：

```
总费用 = Gas消耗量 × (基础费 + 小费)
```

### Base Fee（基础费）- 网络拥堵税

-   **由协议自动决定**，你没法讨价还价
    
-   **网络越堵，基础费越高**
    
-   这部分钱会被**直接销毁**（不给矿工）
    

**类比：**

-   高峰期打车，平台加价（2倍、3倍）
    
-   深夜打车，价格便宜
    
-   这个加价部分平台直接销毁（为了控制通胀）
    

### Priority Fee（小费）- 加急费

-   **你自己决定**给矿工多少小费
    
-   小费越高，矿工越愿意先处理你的交易
    
-   这部分钱进矿工口袋
    

**类比：**

-   餐厅服务员，你给小费多，他服务更积极
    
-   不给小费也行，但可能要等很久
    

### 实际设置

你在MetaMask里设置两个参数：

-   `maxFeePerGas`：你愿意支付的最高单价
    
-   `maxPriorityFeePerGas`：你愿意给的最高小费
    

**举例：**

-   Base Fee当前是 5 Gwei
    
-   你设置Priority Fee = 1 Gwei
    
-   实际支付 = 5 + 1 = 6 Gwei/Gas
    

* * *

## 3\. 实际计算例子

**假设你要部署一个投票合约：**

### 第一步：估算Gas消耗

-   基础交易：21,000 Gas
    
-   创建合约：32,000 Gas
    
-   代码存储（假设8KB）：8,000 × 200 = 1,600,000 Gas
    
-   构造函数执行：50,000 Gas
    
-   初始化变量：100,000 Gas
    
-   **总计：约 1,803,000 Gas**
    

### 第二步：查看当前Gas价格

假设现在是平峰期：

-   Base Fee = 20 Gwei
    
-   Priority Fee = 2 Gwei（你愿意给的小费）
    
-   **总单价 = 22 Gwei**
    

### 第三步：计算总费用

```
总费用 = 1,803,000 × 22 Gwei
       = 39,666,000 Gwei
       = 0.0396 ETH
```

### 第四步：换算成人民币

假设ETH价格是 $3,000（约¥21,600）：

```
成本 = 0.0396 ETH × $3,000
     = $118.8
     ≈ ¥856
```

**所以部署这个合约大约要花800多元人民币！**

* * *

## 4\. 什么因素影响部署成本？

### ① 合约复杂度

-   **代码越长 → 字节码越大 → 存储费越贵**
    
-   **构造函数越复杂 → 执行费越贵**
    
-   **初始化的变量越多 → 存储费越贵**
    

**省钱技巧：**

-   简化代码逻辑
    
-   减少不必要的注释（编译时会去掉，但还是会影响开发习惯）
    
-   构造函数里少写存储操作
    

### ② 网络拥堵程度

**Base Fee波动范围：**

-   深夜闲时：可能只有 **1-5 Gwei**
    
-   正常时段：**20-50 Gwei**
    
-   NFT抢购/牛市高峰：**100-500+ Gwei**
    

**类比：**

-   同样的快递，双11期间贵10倍
    
-   错峰部署可以省很多钱
    

**省钱技巧：**

-   用Gas追踪工具（如Etherscan Gas Tracker）看实时价格
    
-   选择欧美深夜时段部署（北京时间上午）
    
-   不急的话，等Base Fee降下来
    

### ③ 部署在哪条链

**以太坊主网（L1）：**

-   最安全、最去中心化
    
-   **最贵**！部署可能要几十到几百美元
    

**Layer 2（如Arbitrum、Optimism、Base）：**

-   费用只有主网的 **1/10 到 1/100**
    
-   2024年升级后更便宜（EIP-4844，用blob存储数据）
    
-   部署可能只要几美元
    

**类比：**

-   L1 = 北京市中心开店（最贵但最好）
    
-   L2 = 郊区开店（便宜很多，但依然能服务用户）
    

* * *

## 5\. 怎么控制部署成本？

### 实战Checklist：

**① 先在测试网试一遍**

```bash
npx hardhat run scripts/deploy.js --network sepolia
```

看看实际消耗多少Gas，心里有数。

**② 查看实时Gas价格**

-   访问 etherscan.io/gastracker
    
-   看当前Base Fee和建议的Priority Fee
    
-   选低价时段部署
    

**③ 优化代码**

-   开启Solidity编译器优化：
    

```javascript
// hardhat.config.js
solidity: {
  version: "0.8.20",
  settings: {
    optimizer: {
      enabled: true,
      runs: 200  // 优化次数
    }
  }
}
```

-   减少构造函数里的存储操作
    
-   把固定数据改成常量（constant/immutable）
    

**④ 考虑在L2部署** 如果你的应用不一定要在主网：

-   Arbitrum、Optimism、Base等L2便宜很多
    
-   用户体验也更好（交易快、费用低）
    

* * *

## 总结对比

**智能合约 = 自动售货机**

| 组件 | 作用 | 类比 |
| --- | --- | --- |
| 状态变量 | 永久存储的数据 | 库存记录本 |
| 函数 | 执行操作 | 售货机按钮 |
| 事件 | 记录日志 | 打印小票 |
| 构造函数 | 初始化设置 | 安装程序 |
| 修饰符 | 权限检查 | 门禁系统 |

**部署成本 = 开店成本**

| 费用项 | 影响因素 | 省钱方法 |
| --- | --- | --- |
| Gas消耗 | 代码复杂度 | 简化逻辑、优化代码 |
| Base Fee | 网络拥堵 | 选低峰时段 |
| 存储链 | L1 vs L2 | 优先用L2 |

**一个中等合约部署在主网，可能花费：$50-200（几百到上千元人民币）**
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







# 以太坊学习

### 以太坊的两种账户类型

想象以太坊是一个"数字世界"，里面有两种居民：

**1\. EOA（外部拥有账户）- 就是"普通人"**

-   就像你的钱包，由你（一个真实的人）控制
    
-   你有一把"私钥"（就像银行密码），用它来证明"这个账户是我的"
    
-   可以主动做事：转账、买东西、使用智能合约
    

**2\. 合约账户 - 就是"自动售货机/机器人"**

-   是一段程序代码，按照预设规则自动运行
    
-   没有私钥，不能"自己"做决定
    
-   只有当别人来触发它时才会工作
    

### 关键类比理解

| 概念 | 现实世界类比 | 解释 |
| --- | --- | --- |
| 私钥 | 你家门的钥匙 | 只有有这把钥匙的人才能进门、动用里面的东西 |
| 公钥/地址 | 你家的地址 | 别人可以知道，可以给你寄东西（转账） |
| 助记词 | 配钥匙的模具 | 丢了可以重新配出所有钥匙，所以要藏好 |
| Nonce  随机数 | 支票编号 | 每张支票有唯一编号，用过就作废，防止被复印使用 |

## 一、CREATE vs CREATE2（创建合约的两种方式）

### 🎯 核心区别：能不能提前知道"新房子"的地址

想象你要盖房子（部署智能合约），房子盖好后会有一个地址：

### **CREATE（传统方式）**

```
合约地址 = hash(建造者地址 + 建造者盖过几栋房子了)
                               ↑
                            这是nonce
```

**类比：**

-   就像你在一条街上盖房子，门牌号由"你是谁"+"你是这条街第几个盖房子的"决定
    
-   **无法提前知道**确切地址，因为你不知道轮到你盖的时候前面有多少人盖了
    
-   地址会受到盖房子顺序的影响
    

**例子：**

```
Alice盖第1栋房子 → 地址可能是 0xAAA111
Alice盖第2栋房子 → 地址可能是 0xAAA222
Bob盖第1栋房子   → 地址可能是 0xBBB111
```

### **CREATE2（可预测方式）**

```
合约地址 = hash(0xff + 建造者地址 + salt + 房子设计图的hash)
                                    ↑        ↑
                            你选的随机数   代码哈希
```

**类比：**

-   就像你用"指纹锁"：指纹（代码）+ 密码（salt）→ 固定的门牌号
    
-   **可以提前计算**出地址，即使房子还没盖
    
-   无论什么时候盖，只要用相同的设计图和salt，地址就一样
    

**例子：**

```
Alice用设计图A + salt=123 → 地址永远是 0xCCC111
（不管Alice什么时候盖，盖第几栋房子）

在以太坊主网盖 → 0xCCC111
在Polygon盖    → 0xCCC111（跨链地址一致！）
```

### **为什么需要CREATE2？**

**实际应用场景：**

1.  **工厂合约**
    
    -   想批量创建相同类型的合约，地址可预测方便管理
        

2.  **跨链部署**
    
    -   在多条链上部署同一个合约到相同地址
        

3.  **"反事实合约"（Counterfactual Contracts）**
    
    -   神奇的地方：可以先用地址（收钱、交互），真正需要时再部署合约
        
    -   类比：你提前告诉别人"我的新房地址是123号"，别人可以先寄东西过来，等你真需要的时候再盖房子
        

太好了！我来一个个给你讲清楚 💡

* * *

## 一、CREATE vs CREATE2（创建合约的两种方式）

### 🎯 核心区别：能不能提前知道"新房子"的地址

想象你要盖房子（部署智能合约），房子盖好后会有一个地址：

### **CREATE（传统方式）**

```
合约地址 = hash(建造者地址 + 建造者盖过几栋房子了)
                               ↑
                            这是nonce
```

**类比：**

-   就像你在一条街上盖房子，门牌号由"你是谁"+"你是这条街第几个盖房子的"决定
    
-   **无法提前知道**确切地址，因为你不知道轮到你盖的时候前面有多少人盖了
    
-   地址会受到盖房子顺序的影响
    

**例子：**

```
Alice盖第1栋房子 → 地址可能是 0xAAA111
Alice盖第2栋房子 → 地址可能是 0xAAA222
Bob盖第1栋房子   → 地址可能是 0xBBB111
```

### **CREATE2（可预测方式）**

```
合约地址 = hash(0xff + 建造者地址 + salt + 房子设计图的hash)
                                    ↑        ↑
                            你选的随机数   代码哈希
```

**类比：**

-   就像你用"指纹锁"：指纹（代码）+ 密码（salt）→ 固定的门牌号
    
-   **可以提前计算**出地址，即使房子还没盖
    
-   无论什么时候盖，只要用相同的设计图和salt，地址就一样
    

**例子：**

```
Alice用设计图A + salt=123 → 地址永远是 0xCCC111
（不管Alice什么时候盖，盖第几栋房子）

在以太坊主网盖 → 0xCCC111
在Polygon盖    → 0xCCC111（跨链地址一致！）
```

### **为什么需要CREATE2？**

**实际应用场景：**

1.  **工厂合约**
    
    -   想批量创建相同类型的合约，地址可预测方便管理
        
2.  **跨链部署**
    
    -   在多条链上部署同一个合约到相同地址
        
3.  **"反事实合约"（Counterfactual Contracts）**
    
    -   神奇的地方：可以先用地址（收钱、交互），真正需要时再部署合约
        
    -   类比：你提前告诉别人"我的新房地址是123号"，别人可以先寄东西过来，等你真需要的时候再盖房子
        

* * *

## 二、私钥、公钥、助记词的关系

### 🔑 三者的继承关系

```
助记词（12/24个英文单词）
    ↓ 通过数学算法
  种子
    ↓ 可以生成很多个
私钥1, 私钥2, 私钥3...
    ↓ 每个私钥对应一个
公钥1, 公钥2, 公钥3...
    ↓ 哈希处理
地址1, 地址2, 地址3...
```

### **1\. 助记词（Mnemonic）**

**是什么：**

```
例如：apple banana cherry dog elephant fish 
      grape house island juice kiwi lemon
```

**作用：**

-   是"主钥匙"，可以恢复所有账户
    
-   人类友好：12个单词比一长串16进制数字好记多了
    

**类比：**

-   就像你家族的"传家宝配方"
    
-   知道配方，就能复制出所有家传菜肴（所有私钥）
    

### **2\. 私钥（Private Key）**

**是什么：**

```
0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
（一个256位的超大随机数）
```

**作用：**

-   用来"签名"交易，证明"我是账户主人"
    
-   **绝对不能泄露**
    

**类比：**

-   就像你的指纹 + 密码 + 身份证的组合
    
-   有了它就能动用账户里的所有钱
    

### **3\. 公钥（Public Key）**

**是什么：**

-   从私钥通过数学算法生成（单向，不可逆）
    

**作用：**

-   用来验证签名是不是私钥签的
    
-   不怕公开
    

**类比：**

-   就像你的签名样本
    
-   别人可以拿你的签名样本来验证"这个签名是不是你签的"，但不能用签名样本去伪造签名
    

### **4\. 地址（Address）**

**是什么：**

```
0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
（公钥哈希后取后20字节）
```

**作用：**

-   你的"银行账号"，可以公开
    
-   别人用这个给你转账
    

### 🎨 形象化理解

```
┌─────────────────────────────────────┐
│  助记词（主配方）                      │
│  "apple banana cherry..."           │
│  ⚠️ 最高机密，掌握了就掌握一切          │
└──────────────┬──────────────────────┘
               ↓ 生成
┌──────────────────────────────────────┐
│  私钥（专属印章）                        │
│  0x123abc...                         │
│  ⚠️ 机密，用来签名证明身份                │
└──────────────┬───────────────────────┘
               ↓ 数学变换
┌──────────────────────────────────────┐
│  公钥（验证器）                          │
│  可以验证签名但不能伪造                   │
└──────────────┬───────────────────────┘
               ↓ 哈希
┌──────────────────────────────────────┐
│  地址（银行账号）                        │
│  0x742d35Cc...                       │
│  ✅ 完全公开，用来接收转账                │
└──────────────────────────────────────┘
```

### **BIP-39 和 BIP-44 是什么？**

-   **BIP-39**：定义了"助记词 ↔ 种子"的标准
    
    -   确保不同钱包用相同助记词能恢复相同账户
        
-   **BIP-44**：定义了"从种子派生多个地址"的路径规则
    
    -   比如：`m/44'/60'/0'/0/0` 就是以太坊的第1个地址
        
    -   规定了一个种子可以管理多个币种、多个账户
        

* * *

## 三、Nonce：防止"重放攻击"

### 🎯 核心问题：如果没有Nonce会怎样?

**场景：**

```
你发了一笔交易：
"从我的账户转10 ETH给Bob"
签名：[你的签名]
```

**问题来了：**

-   这笔交易已经上链了
    
-   但攻击者**保存了这笔交易的完整数据**（包括你的签名）
    
-   他能不能把这笔交易再广播一次，让你再转10 ETH给Bob？
    

**如果没有Nonce → 可以！** 😱

### **Nonce的解决方案**

Orchestrated explanation of Nonce's replay attack prevention mechanism.  
Nonce 防重放攻击机制的系统性阐述。

用户需要理解Nonce如何防止重放攻击。我需要用简单的例子说明。

每个账户有一个**计数器**：

```
你的账户：
- Nonce: 0  （初始状态，还没发过交易）
- Balance: 100 ETH

第1笔交易：
- Nonce必须填: 0
- 交易内容: "转10 ETH给Bob"
- 签名
→ 交易成功后，你的Nonce变成: 1 ✓

第2笔交易：
- Nonce必须填: 1
- 交易内容: "转5 ETH给Alice"
- 签名
→ 交易成功后，你的Nonce变成: 2 ✓
```

**如果攻击者想重放第1笔交易：**

```
攻击者广播：
- Nonce: 0  ← 但你的账户Nonce已经是2了！
- 交易内容: "转10 ETH给Bob"
- 签名: [有效签名]

节点检查：
- 这笔交易的Nonce是0
- 但账户当前Nonce是2
- Nonce不匹配 → ❌ 拒绝交易
```

### **类比理解**

**支票编号：**

```
第1张支票：编号001，支付给Bob
第2张支票：编号002，支付给Alice

现在你开到003号了

有人拿着001号支票的复印件来银行：
银行：编号001已经用过了，作废！❌
```

### **Nonce的规则**

1.  **必须连续：** 不能跳号
    

```
   ✓ Nonce 0 → 1 → 2 → 3
   ✗ Nonce 0 → 2（跳过1，会被拒绝）
```

2.  **一个Nonce只能用一次**
    
3.  **卡住问题：** 如果Nonce=5的交易在等待，Nonce=6的交易不会被处理
    

```
   Nonce 5: ⏳ pending...
   Nonce 6: 等待中（必须等5先确认）
   Nonce 7: 等待中
```

* * *

## 四、Gas费用：以太坊的"汽油费"

### 🚗 核心类比：开车需要汽油

在以太坊上做任何事情都需要**计算资源**：

```
你要做的事           需要的计算量        现实世界类比
────────────────────────────────────────────
转账                 21,000 Gas        开车10公里
部署简单合约          ~100,000 Gas      开车50公里  
复杂合约交互          ~500,000 Gas      开车200公里
```

### **Gas的三个概念**

**1\. Gas Limit（油箱容量）**

-   你愿意**最多**用多少Gas
    
-   设置太低 → 交易可能执行到一半没油了，失败但Gas费不退
    
-   设置太高 → 没关系，用不完会退给你
    

```
例子：
你设置 Gas Limit = 100,000
实际只用了 50,000
→ 退回 50,000 的费用给你 ✓
```

**2\. Gas Price（油价）**

-   每单位Gas你愿意付多少钱（以Gwei计）
    
-   1 Gwei = 0.000000001 ETH  
    1 单位 Gwei = 0.000000001 单位 ETH
    

```
类比：
今天汽油价格是8元/升
你想快点加油 → 愿意出10元/升（高Gas Price）
不急 → 愿意等便宜的，出6元/升（低Gas Price）
```

**3\. 总Gas费用  3. 总 Gas 费用**

```
总费用 = Gas Used × Gas Price

例子：
- 你的交易用了 50,000 Gas
- Gas Price 是 20 Gwei

总费用 = 50,000 × 20 = 1,000,000 Gwei
       = 0.001 ETH
```

### **EIP-1559后的变化（2021年8月）**

新的费用模型：

```
总费用 = (Base Fee + Priority Fee) × Gas Used

Base Fee（基础费）:
- 网络自动调整
- 会被"烧掉"（销毁）
- 网络拥堵时自动升高

Priority Fee（小费）:
- 给矿工的奖励
- 你设置，想快点确认就多给
```

### **为什么需要Gas费？**

**1\. 防止垃圾攻击**

```
如果Gas免费：
→ 攻击者可以无限循环、占用所有计算资源
→ 整个网络瘫痪

有了Gas费：
→ 攻击要花真金白银
→ 经济上不划算 ✓
```

**2\. 激励矿工/验证者**

```
处理交易需要计算资源
Gas费就是给他们的报酬
```

### **实际例子**

```
场景：你要在Uniswap上交换代币

查看MetaMask预估：
┌─────────────────────────┐
│ Gas Limit: 150,000      │
│ Base Fee: 15 Gwei       │
│ Priority Fee: 2 Gwei    │
│ Max Fee: 20 Gwei        │
│                         │
│ 预计总费用: ~0.003 ETH   │
│ (约 $6，假设ETH=$2000)  │
└─────────────────────────┘

确认后：
- 实际用了 140,000 Gas（比限制少）
- 退回 10,000 Gas 的费用
```

别急！我来把"合约账户"这个概念彻底讲透 🎯

* * *

## 一、合约账户 vs EOA：两种完全不同的"居民"

### 📱 先回顾：EOA（外部拥有账户）

```
EOA = 你的钱包
- 有私钥（你控制）
- 能主动做事（转账、调用合约）
- 没有代码，就是个"钱包"
```

### 🤖 合约账户：一个"机器人"

```
合约账户 = 一段程序
- 没有私钥（没人能"控制"它）
- 不能主动做事（只能被调用）
- 有代码（智能合约代码）
- 有存储（记住数据）
```

* * *

## 二、形象化理解：自动售货机 🏪

### **EOA（普通账户）= 你本人拿着钱包**

```
你（有钱包密码）:
- 想买东西 → 你主动掏钱
- 想转账 → 你主动操作
- 一切由你控制
```

### **合约账户 = 自动售货机**

```
自动售货机（合约）:
┌─────────────────────────┐
│  [代码/规则]             │
│  如果投入5元 →          │
│    给可乐               │
│  如果投入10元 →         │
│    给矿泉水             │
│                         │
│  [钱箱] 100元           │ ← 这是合约的余额
│  [库存] 可乐×10         │ ← 这是合约的storage
└─────────────────────────┘

特点：
- 没人能"控制"它（没有钥匙孔）
- 规则写死了，自动执行
- 只有投币（调用）才会工作
- 不会自己跑出来找你要钱
```

* * *

## 三、合约账户的"出生"过程 👶

### 🛠️ 步骤拆解（造一台自动售货机）

**第1步：编写设计图（Solidity代码）**

```solidity
// 这是人类能看懂的代码
contract 自动售货机 {
    mapping(address => uint) public 余额;
    
    function 买可乐() public payable {
        require(msg.value == 5 ether, "请投5元");
        // 给可乐...
    }
}
```

**类比：** 你画了售货机的设计图纸

**第2步：编译成机器语言（字节码）**

```
设计图（Solidity）
    ↓ 编译器
机器码（Bytecode）

人类代码: function 买可乐() {...}
↓
机器码: 0x6080604052348015...（一堆16进制数字）
```

**分成两部分：**

-   **Init Code（安装代码）**：造机器时执行的步骤
    
-   **Runtime Code（运行代码）**：机器造好后的工作代码
    

**类比：**

-   Init Code = 安装说明书（组装售货机的步骤）
    
-   Runtime Code = 售货机的工作手册（怎么卖可乐）
    

**第3步：发起"创建合约交易"**

```
你（EOA）发起交易：
┌────────────────────────┐
│ From: 你的地址          │
│ To: null (空地址)       │ ← 空地址=要创建合约
│ Data: [init code]      │ ← 机器的安装说明书
│ Gas: 100万             │ ← 制造费用
└────────────────────────┘
```

**类比：** 你付钱请工厂造售货机

**第4步：EVM执行，"组装"合约**

```
矿工/验证者接到订单：
1. 看到 To = null，知道是要造新合约
2. 运行 Init Code（安装说明书）
   - 设置初始参数
   - 准备工作空间
3. Init Code 返回 Runtime Code
   - "这是造好的售货机工作代码！"
4. EVM 把 Runtime Code 永久存到链上
5. 分配一个地址给这个新合约

结果：
┌─────────────────────────┐
│ 新合约诞生！             │
│ 地址: 0xABC123...       │
│ 代码: [runtime code]    │
│ 余额: 0 ETH             │
│ Storage: [初始状态]     │
└─────────────────────────┘
```

**类比：** 工厂按图纸造好了售货机，放在街角（链上），给了个地址"XX街123号"

* * *

## 四、合约地址怎么算出来的？🔢

### **方式1：CREATE（传统方式）**

```
合约地址 = hash(造机器的人 + 他造过几台机器了)
```

**例子：**

```
Alice第1次部署合约:
地址 = hash(Alice的地址 + 0)  → 0xAAA111

Alice第2次部署合约:
地址 = hash(Alice的地址 + 1)  → 0xAAA222

Bob第1次部署合约:
地址 = hash(Bob的地址 + 0)    → 0xBBB111
```

**特点：**

-   提前不知道地址（因为不知道你是第几个部署的）
    
-   顺序影响地址
    

### **方式2：CREATE2（可预测）**

```
合约地址 = hash(0xff + 造机器的人 + 自选密码 + 设计图hash)
                                    ↑          ↑
                                  salt      init code
```

**例子：**

```
Alice用设计图A + 密码123:
地址 = hash(0xff + Alice + 123 + hash(设计图A))
    = 0xCCC111

无论什么时候造，地址都是 0xCCC111！
甚至在不同的区块链上，地址也一样！
```

**为什么有用？**

**场景1：跨链统一地址**

```
在以太坊: 用CREATE2部署 → 0xCCC111
在Polygon: 用CREATE2部署 → 0xCCC111
在Arbitrum: 用CREATE2部署 → 0xCCC111
```

**场景2："反事实合约"（超级神奇！）**

```
1. 你计算出合约地址会是 0xCCC111
2. 别人可以先往 0xCCC111 转账
3. 你暂时不部署合约（省gas）
4. 等真需要时再部署
5. 部署后，之前转的钱都在！
```

**类比：**

-   CREATE = 你造机器，政府分配门牌号
    
-   CREATE2 = 你提前预定门牌号，什么时候造都行
    

* * *

## 五、合约账户的"身体结构" 🏗️

### 一个合约账户包含4个部分：

```
合约账户 0xABC123:
┌─────────────────────────────────┐
│ 1. nonce (计数器)                │
│    = 这个合约创建过几个子合约     │
│    （对EOA是发过几笔交易）        │
├─────────────────────────────────┤
│ 2. balance (余额)                │
│    = 100 ETH                     │
│    （合约也能收钱、存钱）         │
├─────────────────────────────────┤
│ 3. code (代码)                   │
│    = 0x6080604052...             │
│    （智能合约的运行代码）         │
│    ⚠️ 部署后不能改！              │
├─────────────────────────────────┤
│ 4. storage (存储)                │
│    = {                           │
│      用户A余额: 50,              │
│      用户B余额: 30,              │
│      可乐库存: 100               │
│    }                             │
│    ✅ 可以随调用改变              │
└─────────────────────────────────┘
```

### 类比理解：

| 部分 | 类比 | 说明 |
| --- | --- | --- |
| code | 售货机的主板程序 | 固定的，不能随便改 |
| storage | 售货机的库存和记账本 | 能改变，记录谁买了什么 |
| balance | 售货机的钱箱 | 收到的钱都在这里 |
| nonce | 售货机制造过几台子机器 | 比如自动售货机又造了个小推车 |

* * *

## 六、合约账户能做什么？不能做什么？

### ✅ 能做的事：

```
1. 收钱、存钱
   - 可以持有 ETH
   - 可以持有代币（ERC-20、NFT）

2. 执行复杂逻辑
   - 交易所（Uniswap）
   - 游戏（Decentraland）
   - 投票（DAO）

3. 和其他合约交互
   - 合约A可以调用合约B
   - 甚至创建新合约C

4. 转账给别人
   - 按代码规则自动转账
```

**例子：自动分红合约**

```solidity
contract 分红池 {
    function 分红() public {
        // 自动把池子里的钱分给股东
        股东A.transfer(1 ether);
        股东B.transfer(2 ether);
    }
}
```

### ❌ 不能做的事：

```
1. 主动发起交易
   - 售货机不会自己跑出来找你要钱
   - 必须有人调用它才工作

2. 签名
   - 没有私钥，不能像人一样签字

3. 随便改代码
   - 一旦部署，代码就固定了
   - （除非设计时留了升级机制）
```

* * *

## 七、完整流程图 🎬

```
【合约的一生】

第1幕：出生
───────────────────────────────────
开发者写Solidity代码
    ↓
编译成字节码
    ↓
EOA发起"创建合约交易"（To=null）
    ↓
EVM执行Init Code
    ↓
合约诞生！获得地址、代码、存储


第2幕：工作
───────────────────────────────────
用户（EOA）发起调用：
"调用 0xABC123 的 买可乐() 函数"
    ↓
EVM找到 0xABC123 的代码
    ↓
执行代码逻辑
    ↓
更新storage（库存-1）
    ↓
给用户可乐


第3幕：持续运行
───────────────────────────────────
合约永久在链上
只要有人调用就工作
代码不会消失（除非自毁）
```

* * *

## 八、实际例子：一个简单的存钱罐合约 🐷

### **Solidity代码（人类可读）**

```solidity
contract 存钱罐 {
    mapping(address => uint) public 存款;
    
    // 存钱
    function 存() public payable {
        存款[msg.sender] += msg.value;
    }
    
    // 取钱
    function 取(uint 金额) public {
        require(存款[msg.sender] >= 金额, "余额不足");
        存款[msg.sender] -= 金额;
        payable(msg.sender).transfer(金额);
    }
}
```

### **部署后变成什么？**

```
合约地址: 0x123ABC...
├─ code: 0x608060... (编译后的字节码)
├─ balance: 0 ETH (初始没钱)
└─ storage: 
    - 存款[Alice地址] = 0
    - 存款[Bob地址] = 0
```

### **Alice存10 ETH：**

```
Alice发交易：
- To: 0x123ABC (存钱罐合约地址)
- Data: "调用 存() 函数"
- Value: 10 ETH

执行后：
├─ balance: 10 ETH ← 合约收到钱了
└─ storage: 
    - 存款[Alice] = 10 ETH ← 记账了
    - 存款[Bob] = 0
```

### **Alice取5 ETH：**

```
Alice发交易：
- To: 0x123ABC
- Data: "调用 取(5 ETH) 函数"

执行后：
├─ balance: 5 ETH ← 合约转出5 ETH
└─ storage: 
    - 存款[Alice] = 5 ETH ← 记账减少
    - 存款[Bob] = 0
```

* * *

## 九、关键对比总结 📊

| 维度 | EOA（你的钱包） | 合约账户（售货机） |
| --- | --- | --- |
| 谁控制 | 有私钥的人 | 代码自己（无人控制） |
| 能否主动发交易 | ✅ 能 | ❌ 不能 |
| 有没有代码 | ❌ 没有 | ✅ 有 |
| 能否存钱 | ✅ 能 | ✅ 能 |
| 能否修改行为 | 想做啥做啥 | 只能按代码执行 |
| 创建方式 | 生成私钥 | 部署交易 |
| nonce含义 | 发过几笔交易 | 创建过几个子合约 |

* * *
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









# 搭建区块链（一）

今天完成：

-   项目初始化
    
-   Block（区块）类的创建
    
-   测试驱动开发（TDD）
    
-   创世区块（Genesis Block）的实现
    

## 第一步：安装必要软件

### 1\. 安装 Node.js

-   访问 [nodejs.org](http://nodejs.org)
    
-   下载并安装 LTS 版本（长期支持版）
    
-   安装时保持默认选项即可
    

### 2\. 安装 Git Bash（强烈推荐）

-   访问 [git-scm.com](http://git-scm.com)
    
-   下载 Git for Windows 并安装
    
-   Git Bash 会随 Git 一起安装
    
-   **为什么要用它？** 因为它提供类似Mac/Linux的命令行环境，课程中的命令可以直接使用
    

## 第二步：验证安装

打开 **Git Bash**（安装后在开始菜单可以找到），输入以下命令检查：

bash

```bash
node -v
# 应该显示版本号，比如 v18.x.x

npm -v
# 应该显示版本号，比如 9.x.x
```

## 第三步：创建项目

在 Git Bash 中按顺序执行：

bash

```bash
# 1. 创建项目文件夹
mkdir cryptoChain

# 2. 进入文件夹
cd cryptoChain

# 3. 初始化 Node.js 项目
npm init -y

# 4. 安装测试库 Jest
npm i jest --save-dev
```

## 第四步：验证成功

bash

```bash
# 查看 package.json 文件内容
cat package.json
{
  "name": "cryptochain",           // 项目名称
  "version": "1.0.0",               // 版本号
  "description": "",                // 项目描述（暂时为空）
  "main": "index.js",               // 入口文件
  "scripts": {                      // 可执行的命令
    "test": "..."                   // 测试命令（稍后会修改）
  },
  "keywords": [],                   // 关键词（暂时为空）
  "author": "",                     // 作者（可以填你的名字）
  "license": "ISC",                 // 开源协议
  "type": "commonjs",               // 模块类型
  "devDependencies": {              // 开发依赖
    "jest": "^23.6.0"               // ✅ Jest 已安装！
  }
}
```

### 1\. 区块（Block）的四个核心数据

每个区块包含：

-   **timestamp**（时间戳）- 记录区块创建时间
    
-   **lastHash**（前区块哈希）- 前一个区块的哈希值
    
-   **hash**（当前哈希）- 当前区块的哈希值
    
-   **data**（数据）- 存储的实际内容（交易、信息等）
    

### 2\. JavaScript 类（Class）的基础

-   使用 `class` 关键字定义类
    
-   `constructor` 构造方法 - 创建实例时自动执行
    
-   `this` 关键字 - 指向当前实例
    
-   `new` 关键字 - 创建类的实例
    

### 3\. 编程最佳实践

-   当参数≥3个时，用**对象**包裹参数（而不是单独传参）
    
-   好处：**不用记住参数顺序**，减少错误
    

* * *

## 🛠️ 操作流程

### 第一步：创建 block.js 文件

在项目根目录下创建文件：

bash

```bash
# 在 Git Bash 中，确保你在 cryptoChain 目录
touch block.js
```

或者直接在 VS Code 中右键新建文件。

* * *

### 第二步：编写 Block 类

打开vscode, 在 `block.js` 中写入以下代码：

javascript

```javascript
// 定义 Block 类
class Block {
  // 构造函数 - 用对象包裹参数（最佳实践）
  constructor({ timestamp, lastHash, hash, data }) {
    this.timestamp = timestamp;  // 设置时间戳
    this.lastHash = lastHash;    // 设置前区块哈希
    this.hash = hash;            // 设置当前哈希
    this.data = data;            // 设置数据
  }
}

// 创建一个区块实例（测试用）
const block1 = new Block({
  timestamp: '01/01/01',
  lastHash: 'foo-last-hash',
  hash: 'foo-hash',
  data: 'foo-data'
});

// 打印查看
console.log('block1', block1);
```

* * *

### 第三步：运行测试

在命令行执行：

bash

````bash
node block.js
```

**预期输出：**
```
block1 Block {
  timestamp: '01/01/01',
  lastHash: 'foo-last-hash',
  hash: 'foo-hash',
  data: 'foo-data'
}
````

## 📚 核心知识点

### 1\. 测试驱动开发（TDD - Test Driven Development）

**定义：先写测试，再写代码**

**工作流程：**

```
1. 写测试 → 2. 测试失败（红色❌）→ 3. 写代码 → 4. 测试通过（绿色✅）→ 重复
```

### 2\. TDD 的三大好处

| 好处 | 说明 |
| --- | --- |
| 代码可靠 | 确保新功能不会破坏旧代码 |
| 提高效率 | 不用运行整个项目，只测试特定功能 |
| 学习机会 | 先看测试需求，自己实现代码 |

### 3\. Jest 测试框架的语法

javascript

```javascript
// describe - 定义测试组
describe('Block', () => {
  // it - 定义单个测试
  it('has a timestamp, lastHash, hash and data', () => {
    // expect - 断言/期望
    expect(block.timestamp).toEqual(timestamp);
  });
});
```

* * *

## 🛠️ 操作流程

### 第一步：修改 block.js

**1\. 注释掉测试代码：**

javascript

```javascript
class Block {
  // 先注释掉整个构造函数
  // constructor({ timestamp, lastHash, hash, data }) {
  //   this.timestamp = timestamp;
  //   this.lastHash = lastHash;
  //   this.hash = hash;
  //   this.data = data;
  // }
}

// 删除测试代码（block1 和 console.log）

// 添加导出语句
module.exports = Block;
```

* * *

### 第二步：创建测试文件 block.test.js

**文件命名规则：**`原文件名.test.js`

javascript

```javascript
// 1. 引入 Block 类
const Block = require('./block');

// 2. 定义测试组
describe('Block', () => {
  
  // 3. 声明测试变量
  let timestamp, lastHash, hash, data, block;
  
  // 4. 赋值（注意：相同键值可以简写）
  timestamp = 'a-date';
  lastHash = 'foo-hash';
  hash = 'bar-hash';
  data = ['blockchain', 'data'];
  
  block = new Block({
    timestamp,  // 等同于 timestamp: timestamp
    lastHash,
    hash,
    data
  });
  
  // 5. 编写测试
  it('has a timestamp, lastHash, hash and data', () => {
    expect(block.timestamp).toEqual(timestamp);
    expect(block.lastHash).toEqual(lastHash);
    expect(block.hash).toEqual(hash);
    expect(block.data).toEqual(data);
  });
});
```

* * *

### 第三步：配置 package.json

修改测试脚本：

json  无标题

```json
{
  "scripts": {
    "test": "jest --watchAll"
  }
}
```

`--watchAll` **的作用：**

-   自动监听文件变化
    
-   保存代码后自动重新运行测试
    

* * *

### 第四步：运行测试

在 Git Bash 中执行：

bash

````bash
npm run test
```

**第一次运行：**
```
❌ FAIL  block.test.js
  Block is not defined
```

**修复：** 在 block.test.js 顶部添加 `const Block = require('./block');`

---

### 第五步：逐步让测试通过（TDD 核心）

**1. 第一次运行测试 - 全部失败：**
```
❌ Expected: 'a-date'
   Received: undefined
```

**解决：** 取消 `constructor({ timestamp, lastHash, hash, data })` 的注释

---

**2. 第二次运行 - timestamp 通过：**
```
✅ timestamp 通过
❌ lastHash 失败
```

**解决：** 取消 `this.timestamp = timestamp;` 的注释

---

**3. 第三次运行 - lastHash 通过：**
```
✅ timestamp 通过
✅ lastHash 通过
❌ hash 失败
```

**解决：** 取消 `this.lastHash = lastHash;` 的注释

---

**4. 继续直到全部通过：**
```
✅ timestamp 通过
✅ lastHash 通过
✅ hash 通过
✅ data 通过

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
````

## 📚 核心知识点

### 1\. 什么是创世区块（Genesis Block）？

**问题：区块链的第一个区块怎么办？**

| 普通区块 | 创世区块 |
| --- | --- |
| 需要前一个区块的哈希 | 没有前一个区块！ |
| 数据来自真实交易 | 数据是硬编码的默认值 |
| 动态生成 | 程序启动时就存在 |

**比喻：**

-   创世区块 = 家谱的第一代祖先（没有父母信息）
    
-   普通区块 = 后代（有父母信息）
    

* * *

### 2\. 静态函数（Static Function）

**定义：** 不需要创建实例就能调用的函数

javascript

```javascript
// 普通函数 - 需要 new 创建实例
const block1 = new Block({...});
block1.someMethod();  // 在实例上调用

// 静态函数 - 直接在类上调用
Block.genesis();  // 不需要 new，直接调用
```

**为什么需要静态函数？**

-   创世区块只需要创建一次
    
-   不属于任何特定的区块实例
    
-   是整个区块链的起点
    

* * *

### 3\. SCREAMING\_SNAKE\_CASE 命名规范

javascript

```javascript
const GENESIS_DATA = { ... }  // 全大写 + 下划线
```

**含义：** 一眼就能看出这是**硬编码的全局常量**

| 命名风格 | 用途 | 示例 |
| --- | --- | --- |
| camelCase  驼峰命名法 | 普通变量/函数 | genesisBlock |
| PascalCase  帕斯卡命名法 | 类名 | Block |
| SCREAMING_SNAKE_CASE | 全局常量 | GENESIS_DATA |

* * *

## 🛠️ 操作流程

### 第一步：创建配置文件 config.js

**作用：** 存放全局常量和硬编码值

javascript

```javascript
// config.js
const GENESIS_DATA = {
  timestamp: 1,           // 时间戳设为 1
  lastHash: '-----',      // 前区块哈希（随便设）
  hash: 'hash-one',       // 当前哈希（随便设）
  data: []                // 数据为空数组
};

// 导出（注意是在对象里导出）
module.exports = { GENESIS_DATA };
```

**为什么用对象包裹？**

javascript

```javascript
// 方式1：直接导出
module.exports = GENESIS_DATA;
// 导入时：const GENESIS_DATA = require('./config');

// 方式2：对象包裹（推荐）
module.exports = { GENESIS_DATA };
// 导入时：const { GENESIS_DATA } = require('./config');
// 好处：可以导出多个常量
```

* * *

### 第二步：编写测试 block.test.js

在已有的 `describe('Block')` 里**再嵌套**一个 `describe`：

javascript

```javascript
const Block = require('./block');
const { GENESIS_DATA } = require('./config');  // 新增：导入创世数据

describe('Block', () => {
  
  // 原有测试...
  
  // 新增：创世区块测试组
  describe('genesis()', () => {
    
    // 创建创世区块实例
    const genesisBlock = Block.genesis();
    
    // 测试1：返回的是 Block 实例
    it('returns a Block instance', () => {
      expect(genesisBlock instanceof Block).toBe(true);
    });
    
    // 测试2：数据等于 GENESIS_DATA
    it('returns the genesis data', () => {
      expect(genesisBlock).toEqual(GENESIS_DATA);
    });
  });
});
```

* * *

## 💡 新语法详解

### 1\. 对象解构导入

javascript

```javascript
// config.js 导出
module.exports = { GENESIS_DATA };

// block.test.js 导入
const { GENESIS_DATA } = require('./config');
//      ↑ 解构语法，从对象中提取 GENESIS_DATA
```

**等价于：**

javascript

```javascript
const config = require('./config');
const GENESIS_DATA = config.GENESIS_DATA;
```

* * *

### 2\. instanceof 操作符

**作用：** 检查某个对象是不是某个类的实例

javascript

```javascript
const block = new Block({...});

console.log(block instanceof Block);  // true
console.log(block instanceof Array);  // false
```

**在测试中使用：**

javascript

```javascript
expect(genesisBlock instanceof Block).toBe(true);
//     ↑ 检查是否是 Block 类型        ↑ 期望结果是 true
```

* * *

### 3\. .toBe() vs .toEqual()  
3\. .toBe() 与 .toEqual()

| 方法 | 用途 | 示例 |
| --- | --- | --- |
| .toBe() | 比较基本类型（严格相等） | expect(true).toBe(true) |
| .toEqual() | 比较对象内容 | expect({a:1}).toEqual({a:1}) |

javascript

```javascript
// toBe - 用于布尔值、数字、字符串
expect(5).toBe(5);
expect(true).toBe(true);

// toEqual - 用于对象、数组
expect({name: 'Alice'}).toEqual({name: 'Alice'});
expect([1, 2, 3]).toEqual([1, 2, 3]);
```

* * *

### 4\. 嵌套 describe

javascript

````javascript
describe('外层测试组', () => {
  
  describe('内层测试组1', () => {
    it('测试1', () => {...});
  });
  
  describe('内层测试组2', () => {
    it('测试2', () => {...});
  });
});
```

**输出结果：**
```
Block
  ✓ has timestamp, lastHash, hash and data
  genesis()
    ✓ returns a Block instance
    ✓ returns the genesis data
````

* * *

## 📋 完整文件结构

### config.js（新建）

javascript

```javascript
const GENESIS_DATA = {
  timestamp: 1,
  lastHash: '-----',
  hash: 'hash-one',
  data: []
};

module.exports = { GENESIS_DATA };
```

* * *

### block.test.js（修改）

javascript

```javascript
const Block = require('./block');
const { GENESIS_DATA } = require('./config');  // ← 新增

describe('Block', () => {
  
  // === 原有测试 ===
  let timestamp, lastHash, hash, data, block;
  
  timestamp = 'a-date';
  lastHash = 'foo-hash';
  hash = 'bar-hash';
  data = ['blockchain', 'data'];
  
  block = new Block({
    timestamp,
    lastHash,
    hash,
    data
  });
  
  it('has a timestamp, lastHash, hash and data', () => {
    expect(block.timestamp).toEqual(timestamp);
    expect(block.lastHash).toEqual(lastHash);
    expect(block.hash).toEqual(hash);
    expect(block.data).toEqual(data);
  });
  
  // === 新增测试 ===
  describe('genesis()', () => {
    const genesisBlock = Block.genesis();
    
    it('returns a Block instance', () => {
      expect(genesisBlock instanceof Block).toBe(true);
    });
    
    it('returns the genesis data', () => {
      expect(genesisBlock).toEqual(GENESIS_DATA);
    });
  });
});
```

* * *

### block.js（暂时不改）

保持原样，因为还没写 `genesis()` 函数

* * *

## 🔄 运行测试

bash

````bash
npm run test
```

**预期结果：**
```
❌ FAIL  ./block.test.js
  Block
    ✓ has a timestamp, lastHash, hash and data
    genesis()
      ✗ Block.genesis is not a function
````

**这是正常的！** 因为我们还没写 `Block.genesis()` 函数。

* * *

## 🎯 下节课预告

下节课会实现：

javascript

```javascript
class Block {
  // ...原有代码...
  
  // 新增静态函数
  static genesis() {
    return new Block(GENESIS_DATA);
  }
}
```

## 📚 核心知识点

### 1\. 静态方法（Static Method）

**定义：** 属于类本身，而不属于实例的方法

javascript

```javascript
class Block {
  // 静态方法 - 直接在类上调用
  static genesis() {
    return new Block(GENESIS_DATA);
  }
  
  // 普通方法 - 需要实例才能调用
  someMethod() {
    // ...
  }
}

// 调用方式对比
Block.genesis();              // ✅ 静态方法
const block = new Block({...});
block.someMethod();           // ✅ 普通方法
```

* * *

### 2\. 工厂方法模式（Factory Method Pattern）  
2\. 工厂方法模式

**定义：** 不直接用构造函数，而是用专门的函数来创建实例

javascript

```javascript
// 方式1：直接用构造函数
const block = new Block({
  timestamp: 1,
  lastHash: '-----',
  hash: 'hash-one',
  data: []
});

// 方式2：工厂方法（更优雅）
const block = Block.genesis();
```

**好处：**

-   隐藏创建细节
    
-   代码更简洁
    
-   便于维护（只需改工厂方法）
    

* * *

### 3\. 静态方法中的 this

javascript

```javascript
class Block {
  static genesis() {
    return new Block(GENESIS_DATA);  // ← 可以写成这样
    return new this(GENESIS_DATA);   // ← 也可以写成这样
  }
}
```

**在静态方法中：**

-   `this` = 类本身
    
-   `this` = `Block`
    

* * *

## 🛠️ 操作流程（TDD 让测试通过）

### 第一步：准备工作

**布局窗口：**

-   左侧：命令行（运行 `npm run test`）
    
-   右侧：代码编辑器
    

**Jest 的监听模式：**

-   保存代码 → 自动重新运行测试
    
-   实时看到测试结果
    

* * *

### 第二步：解决第一个错误

**错误信息：**

bash

```bash
❌ Block.genesis is not a function
```

**解决方法：** 在 block.js 中添加静态方法

javascript

```javascript
// block.js
class Block {
  constructor({ timestamp, lastHash, hash, data }) {
    this.timestamp = timestamp;
    this.lastHash = lastHash;
    this.hash = hash;
    this.data = data;
  }
  
  // 新增：静态方法
  static genesis() {
    // 先留空
  }
}

module.exports = Block;
```

**保存后自动测试：**

bash

```bash
❌ Expected: Block instance
   Received: undefined
```

* * *

### 第三步：返回 Block 实例

javascript

```javascript
static genesis() {
  return new Block();  // 返回新实例
}
```

**保存后测试：**

bash

```bash
❌ Cannot destructure 'timestamp' of undefined or null
```

**原因：** 构造函数需要参数，但我们没传！

* * *

### 第四步：导入 GENESIS\_DATA

**在 block.js 顶部添加：**

javascript

```javascript
const { GENESIS_DATA } = require('./config');

class Block {
  // ...
}
```

* * *

### 第五步：使用 GENESIS\_DATA

javascript

```javascript
static genesis() {
  return new Block(GENESIS_DATA);
}
```

**保存后测试：**

bash

```bash
✅ PASS  block.test.js
  Block
    ✓ has timestamp, lastHash, hash and data
    genesis()
      ✓ returns a Block instance
      ✓ returns the genesis data
```

**全部通过！🎉**

* * *

### 第六步：优化代码（可选）

**将** `Block` **改为** `this`**：**

javascript

```javascript
static genesis() {
  return new this(GENESIS_DATA);  // this = Block
}
```

**测试依然通过！**

* * *

### 第七步：调试验证

**临时添加 console.log：**

javascript

````javascript
// block.test.js
describe('genesis()', () => {
  const genesisBlock = Block.genesis();
  
  console.log('genesisBlock', genesisBlock);  // ← 临时添加
  
  it('returns a Block instance', () => {
    expect(genesisBlock instanceof Block).toBe(true);
  });
  
  // ...
});
```

**命令行输出：**
```
genesisBlock Block {
  timestamp: 1,
  lastHash: '-----',
  hash: 'hash-one',
  data: []
}
````

**验证完后删除 console.log**
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










以太坊网络本质是一个 **没有中央管理员、全球所有人共同维护的公开账本**（记录所有以太坊交易和数据），但这个账本有一套严格的 “记账规矩”（比如：怎么算一笔交易有效、怎么更新账本、怎么防造假）。**客户端软件**，就是把这些 “记账规矩” 翻译成电脑能看懂的程序，相当于给你的电脑装了一套 \*\*「合规记账工具 + 验真助手」\*\*它的核心工作：

1.  **按规矩验真假**：别人发来新的账本页（区块链里的「区块」），它会检查这笔账是不是符合规则，防止有人篡改数据；
    
2.  **同步账本信息**：把你的电脑账本，和全球其他电脑的账本对齐，保证大家的账本一模一样；
    
3.  **守护网络安全**：如果发现有人提交假账本，它会拒绝接收，不让造假信息混入全网。
    

简单说：**客户端是让电脑 “懂以太坊规则” 的软件，没有它，电脑根本不知道怎么和以太坊网络打交道。**

### 再搞懂：节点 = 以太坊网络的「联网成员」

**节点**，就是 **“装了客户端软件，并且连上网、和其他同类电脑互通的设备”**（电脑、服务器都算）。

举个实际例子，比如常见的以太坊客户端有 **Geth、Besu** 这两款（相当于不同品牌的 “记账工具”，但都遵守同一套规矩）。你在自己的笔记本上装了 Geth，打开软件后，它会自动连接到全球其他装了 Geth/Besu 的电脑，这时 **你的笔记本就变成了一个以太坊节点**，可以和全网一起同步账本、验证交易了。

执行客户端（EL）就是 “干活的”，共识客户端（CL）是 “定规矩 / 验对错的”。简单说：**执行客户端就是以太坊的 “实际办事员”，所有和 “具体交易、合约运行、数据存储” 相关的活，都是它干的**。Geth、Nethermind 这些，就是不同公司做的 “执行客户端套装”—— 就像格力、美的都做空调，功能都是制冷，但品牌 / 实现方式不同，都符合以太坊的 “办事规矩”。既然 EL 是 “干活的”，那得有人监督它干得对不对、决定该干哪份活，这就是共识客户端（CL）的事（相当于工厂的 “质检部 + 决策委员会”）；如果想赚质押收益（相当于给工厂当 “特邀质检员”），就需要验证者客户端 —— 它是挂在 CL 上的 “插件”，你质押 ETH 当押金，帮 CL 投票决策，干得好就拿收益。

以太坊节点的「搭积木结构」你的钱包/DApp（比如MetaMask、Uniswap）

↓ （想查余额/转ETH/用合约）

调用执行客户端（EL）的JSON-RPC接口 → 相当于“当事人找书记员办事”

↓ （执行客户端干活：处理交易、更新账户余额、运行智能合约）

执行客户端把处理好的“交易结果”交给共识客户端（CL） → 相当于“书记员把案卷交给审判长”

↓ （共识客户端决策：组织验证者投票）

共识客户端指挥验证者 → 提议区块+投票 → 确认这个区块合法

↓ （同步给全网）

执行客户端+共识客户端，各自通过自己的P2P网络 → 把结果同步给其他节点 → 相当于“法院把判决书发往全国”

## 为啥需要 Engine API？

合并前，执行客户端和共识客户端是 **“同一个软件里的两个模块”** —— 就像书记员和审判长在**同一个办公室**，一抬头就能说话，不用额外工具。合并后，它们变成了 **“两个完全独立的程序”** —— 相当于书记员和审判长被分到了**两个不同的楼层**，没法直接喊对方，也不能用法院对外的公开电话（比如给当事人用的查询电话），否则会泄露内部信息。这时候，就需要一根 **“内部专属专线电话”** —— 这就是 **Engine API**。它的唯一作用：**只让执行客户端（EL）和共识客户端（CL）之间秘密沟通，不对外公开**。它就是合并后以太坊节点的 **“内部沟通桥梁”** —— 让 “干活的书记员（EL）” 和 “拍板的审判长（CL）” 能高效、安全地配合，既不会耽误事，也不会泄露内部处理细节。

### 关键区别：Engine API ≠ 对外的 JSON-RPC 接口

很多人会混淆这两个接口，用法院的电话系统就能分清：

| 接口类型 | 相当于法院的什么电话 | 给谁用 | 用途 |
| --- | --- | --- | --- |
| Engine API | 书记员 ↔ 审判长的 内部专线 | 仅 EL 和 CL 之间 | 传递区块骨架、执行结果、合法性校验 |
| 执行客户端的 JSON-RPC 接口 | 法院对外的 服务大厅查询电话 | 钱包（MetaMask）、DApp（Uniswap）、普通 | 查账户余额、发起转账、调用智能合约 |

# 节点通信的 “社交流程”

以太坊节点就像一个个 **“社恐但必须社交的打工人”**，刚入职（启动）时谁都不认识，得先 **找到靠谱同事（节点）→ 拉上安全的私聊通道 → 再八卦传消息**。我们重点讲前两步。

## 第一步：节点发现（UDP + Kademlia）= 新人找同事，快速拓展人脉

### 1.1 Discovery 协议（Ping/Pong/FindNode/Neighbors）= 找同事的 4 句 “社交黑话”  
1.1 发现协议（Ping/Pong/FindNode/Neighbors）= 找同事的四句“社交黑话”

这四个指令就是节点之间 “打招呼、要联系方式” 的固定话术，我们对应到新人入职场景：

| 协议指令 | 新人入职大白话 | 实际作用 |
| --- | --- | --- |
| Ping  品 | 新人给老员工发微信：“在吗？” | 新节点给引导节点发消息，确认对方是否在线 |
| Pong | 老员工回：“在呢！” | 引导节点回复，证明自己在线，同时附带自己的 “身份 ID” |
| FindNode | 新人接着问：“能不能给我点其他同事的微信呀？” | 新节点向引导节点请求 “活跃邻居节点的联系方式” |
| Neighbors/Nodes  邻居/节点 | 老员工甩来一个微信群：“这里都是活跃同事，加吧！” | 引导节点返回一批相邻节点的地址，帮新节点扩充 “好友列表” |

**关键操作**：新节点拿到这批邻居地址后，会 **重复上述 4 步** —— 给新邻居发 Ping/Pong、要更多联系方式，直到好友列表塞满（达到预设上限）。

### 1.2 Kademlia（DHT 分布式哈希表）= 高效找同事的 “智能通讯录”

你可以把 Kademlia 理解为节点的 **“智能人脉管理工具”**，核心解决 “怎么快速找到目标节点” 的问题，类比成 **“快递分拨中心”** 更直观：

| Kademlia 规则 | 快递分拨中心大白话 | 节点实际操作 |
| --- | --- | --- |
| 每个节点有唯一 256bit ID | 每个快递分拨中心有唯一编号（比如 A001、A002、B001） | 节点的 ID 是一串唯一代码，相当于 “网络身份证” |
| 用 XOR 算 “距离”（不是地理距离） | 距离 = 编号的相似程度：A001 和 A002 距离近，A001 和 B001 距离远 | 节点不看物理位置，只看 ID 的相似性，相似 ID 就是 “近邻” |
| 找节点时 “逐级转发” | 想寄快递到 A100，先发给 A001→A001 转给更近的 A050→A050 转给 A100 | 节点找目标时，只把请求发给 “离目标 ID 更近的邻居”，不用全网广播，效率超高 |

**小结第一步**：新节点靠 UDP 发短信（Ping/Pong）确认在线，用 FindNode 要通讯录，再靠 Kademlia 这个智能工具快速拓展人脉，填满好友列表。

## 第二步：建立安全连接（TCP + RLPx/devp2p）= 拉上加密的 “私聊专线”

找到靠谱邻居后，不能再用短信（UDP）传重要数据了 —— 短信容易丢、容易被偷看，节点之间要传 **交易、区块** 这种核心信息，得用更靠谱的方式。

### 2.1 TCP = 从 “发短信” 升级到 “打电话”

TCP 协议相当于 **“手机通话”**，和 UDP（短信）的核心区别：

| UDP（短信） | TCP（通话） |
| --- | --- |
| 快，发完就走，不管对方收没收到 | 稳定，先确认对方能接，再传数据，丢包了会重发 |
| 适合 “打招呼、要地址” | 适合传 “交易、区块” 这种重要内容 |

节点之间用 TCP 建立连接，就像打一通 **“保证能接通、能听清”** 的电话，为后续传数据铺好稳定的管道。

### 2.2 RLPx/devp2p = 给通话加密，还能 “开多线私聊”

RLPx 是跑在 TCP 之上的 “安全工具”，devp2p 是以太坊的 P2P 协议栈，合在一起就是 **“加密的多任务通话线”**，拆成两个核心功能讲：

功能 1：加密会话 = 给电话加 “防窃听装置”

节点建立 TCP 连接后，第一步会 **交换密钥**，相当于给通话装了加密器 —— 第三方就算截获了数据，也看不懂内容，保证交易、区块不被篡改、不被偷看。

功能 2：多路复用 = 一条电话线，同时聊好几件事

这是 RLPx 最核心的操作，类比成 **“视频会议的分屏功能”**：

> 你和同事拉了一条视频会议线（TCP 连接），这条线不用拆，就能同时开 3 个小窗口：
> 
> 1.  窗口 1：传工作文件（同步区块）
>     
> 2.  窗口 2：发即时消息（传播交易）
>     
> 3.  窗口 3：请示工作（轻客户端请求数据）
>     

对应到以太坊节点：

-   一条 TCP 加密通道，通过 RLPx 同时跑多种子协议（比如 `eth/66` 同步区块、`les` 服务轻节点）；
    
-   不用为每种任务单独拉一条连接，节省资源，通信效率直接拉满。
    

**小结第二步**：节点用 TCP 搭好稳定管道，再用 RLPx 给管道加密 + 开 “分机”，实现安全高效的多任务通信。

* * *

## 最终总结：节点通信的两步走

| 步骤 | 技术组合 | 生活化类比 | 核心目的 |
| --- | --- | --- | --- |
| 1. 找邻居 | UDP + Kademlia | 新人发微信找同事，用智能通讯录拓展人脉 | 快速填满 “好友列表”，找到靠谱的通信对象 |
| 2. 建专线 | TCP + RLPx/devp2p | 和同事拉加密视频会议，一条线聊多件事 | 安全、稳定地传输交易、区块等核心数据 |

简单说：**UDP 负责 “打招呼找朋友”，TCP 负责 “正经聊正事”，RLPx 负责 “把正事聊得安全又高效”**。

## 一、Gossip 协议：办公室八卦的 “病毒式扩散”

把 **新交易 / 新区块** 比作 **办公室的一条劲爆八卦**（比如 “小王要升职了”），把 **节点** 比作 **同事**，传播过程分 4 步，和现实中传八卦一模一样：

| 以太坊节点操作 | 办公室八卦大白话 | 核心逻辑 |
| --- | --- | --- |
| 1. 某节点收到新交易 / 区块 | 1. 前台小姐姐先听到 “小王升职” 的八卦 | 消息总有第一个 “知情者”（比如刚发起转账的钱包节点、刚打包区块的验证者节点） |
| 2. 它随机挑几个没听过这个消息的邻居节点转发 | 2. 前台小姐姐赶紧偷偷告诉座位附近的 3 个同事：“我跟你说个事，别告诉别人啊！”（只挑没听过的人说，避免重复） | 不群发、不广播，只给 “不知情的人” 传，节省资源 |
| 3. 这些邻居节点收到后，再各自挑自己的不知情邻居转发 | 3. 这 3 个同事又转头告诉自己的好朋友：“我听说小王要升职了！” | 一传十、十传百，每一轮都让消息覆盖更多人 |
| 4. 几轮后，全网几乎所有节点都收到消息 | 4. 半天不到，全公司上百号人都知道 “小王升职” 了 | 不用中心指挥，消息自己就能铺满整个网络 |

### Gossip 协议的 3 个核心优点（对应八卦传播的优点）

| 技术优点 | 八卦传播的对应好处 |
| --- | --- |
| 完全去中心化，不用中心服务器 | 不用等领导开会通知，同事私下传就能让所有人知道 |
| 鲁棒性好（局部故障不影响全局） | 就算某几个同事请假了（节点离线），还有其他同事传，八卦照样能传遍公司 |
| 传播速度超快（几秒传遍全网） | 办公室八卦的传播速度，比发公司邮件快 10 倍 |

### 补充：执行层和共识层的 “八卦圈是分开的”

以太坊的执行层（EL）和共识层（CL）有各自的 P2P 网络，就像办公室里 **“业务部八卦圈” 和 “人事部八卦圈”** 互不干扰：

-   **执行层 Gossip**：传的是 **交易、普通区块**（比如 “张三转 ETH 给李四” 这种业务消息），用的是 devp2p 协议里的 eth/\* 子协议；
    
-   **共识层 Gossip**：传的是 **信标块、投票信息**（比如 “审判长选了哪个区块合法” 这种决策消息），用的是共识客户端自己的 gossip 协议。
    

* * *

## 二、请求 / 响应模式：主动 “追问八卦”，查漏补缺

光靠 “别人传八卦” 不够 —— 有时候你可能漏掉了某个消息（比如上班摸鱼没听到），这时候就需要 **主动去问**，这就是以太坊的第二种消息传播模式：**请求 / 响应**。

继续用办公室类比：

| 以太坊节点操作 | 办公室大白话 | 核心作用 |
| --- | --- | --- |
| 1. 节点发现自己缺了某个区块 / 交易 | 1. 你上班摸鱼，没听到 “小王升职” 的八卦，只听到大家在讨论，却不知道具体情况 | 节点同步数据时，发现自己的账本少了几页，或者漏了某笔交易 |
| 2. 它向邻居节点发送 “请求”：“请把 XX 区块发给我” | 2. 你主动去问同事：“他们说的小王升职是咋回事啊？快跟我讲讲！” | 节点主动向邻居索要自己缺失的数据 |
| 3. 邻居节点收到请求后，给它 “响应”：把对应的区块 / 交易发过去 | 3. 同事把八卦的来龙去脉详细讲给你听 | 邻居节点把缺失的数据传给它，帮它补全账本 |

### 两种模式的配合：八卦扩散 + 主动追问 = 全网数据一致

Gossip 协议负责 **“主动扩散新消息”**，保证新交易 / 区块快速传遍全网；

请求 / 响应模式负责 **“被动查漏补缺”**，解决个别节点漏听、漏收的问题。

两者结合，就能让以太坊成千上万的节点，都保持一模一样的账本。

* * *

## 最终总结

以太坊节点传消息，就像办公室传八卦：

-   **Gossip 协议** = 同事主动口口相传，几秒让消息铺满全网；
    
-   **请求 / 响应** = 没听到八卦的人主动追问，补全自己的信息；
    
-   执行层和共识层各有各的 “八卦圈”，互不干扰，各司其职。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->












## **Web3的核心运作原理**

### **1\. 区块链：去中心化的账本**

区块链是Web3的基础设施，可以把它想象成一个"全民公开的记账本"：

传统银行转账时，银行的服务器记录"你给张三转了100元"。如果银行服务器出问题或者银行耍赖，你的记录可能就没了。

而区块链是这样运作的：全世界成千上万台电脑（叫"节点"）都保存着同一份账本。当你要转账时，这笔交易会被广播到所有节点，大家一起验证这笔交易是否合法（你账户里确实有这么多钱），验证通过后，所有人同时更新账本。这样一来，没有任何一个中心机构能篡改记录，因为要篡改就得同时控制全球51%以上的节点，这几乎不可能。

### **2\. 智能合约：自动执行的协议**

智能合约是部署在区块链上的程序，它的特点是"代码即法律"——一旦部署，任何人都无法修改，它会严格按照预设规则自动执行。举个例子：你和朋友打赌明天会不会下雨，赌注100元。传统方式需要找个中间人保管钱，但你们可能不信任他。用智能合约的话：你们各自把100元锁进合约，合约会自动从天气数据源获取明天的天气信息，然后自动把200元转给赢的人。整个过程透明、自动、不可篡改，不需要信任任何人。

### **3\. 钱包：你的数字身份**

在Web3世界里，钱包不只是存钱的工具，它是你的数字身份。

每个钱包有一对密钥：

-   **公钥**（像你的银行账号）可以公开，别人用它给你转账
    
-   **私钥**（像你的密码）必须保密，用它来证明你是钱包的主人
    

重要的是：**私钥就是一切**。谁掌握私钥，谁就拥有钱包里的资产。没有"找回密码"这个选项，丢了私钥就永远丢了。

### **4\. 共识机制：如何达成一致**

既然没有中心化的权威，这么多节点怎么就一笔交易达成共识呢？

**工作量证明（PoW）**：就像比特币用的方式。矿工们竞相解一道超级复杂的数学题，谁先解出来，谁就有权记录这一批交易，并获得奖励。这需要消耗大量电力。

**权益证明（PoS）**：比如以太坊现在用的。节点需要"质押"一定数量的代币，然后系统随机选择节点来验证交易。如果作恶会损失质押的代币，这样确保诚实。这种方式更节能。

## **Web3的实际运作流程举例**

假设你要在一个去中心化交易所（DEX）上用以太坊换USDC：

1.  **连接钱包**：你用MetaMask等钱包连接到DEX网站
    
2.  **授权交互**：钱包会弹窗让你签名授权，证明这个操作确实是你发起的
    
3.  **提交交易**：你设定要换多少，DEX的智能合约会计算兑换比例
    
4.  **支付Gas费**：你需要支付一笔"燃料费"给矿工/验证者，让他们把你的交易打包到区块链上
    
5.  **广播和验证**：交易被广播到网络，节点们验证合法性
    
6.  **打包入块**：交易被打包进新的区块，连接到区块链上
    
7.  **完成确认**：等待几个区块确认后，交易不可逆转，你的钱包里就收到了USDC
    

这整个过程可能需要几秒到几分钟，取决于网络拥堵程度和你愿意支付的Gas费。

## **为什么工作量证明（PoW）耗能巨大？**

### **1\. 挖矿竞赛的本质**

以比特币为例，它用的就是PoW机制。运作方式是这样的：想象一个场景：每10分钟要从全球矿工中选出一个"记账员"，让他有权记录这段时间的所有交易，并获得奖励（新产生的比特币+手续费）。

\*\*选择方式是什么呢？\*\*比拼谁先解出一道超级复杂的数学题。这道题没有巧妙解法，**只能靠暴力穷举**——就是不停地猜测、计算、验证，直到碰对答案。就像你要从1万亿个箱子里找到唯一装着钥匙的那个，只能一个个打开试。

### **2\. 全球矿工同时竞争**

关键在于：**全世界所有矿工都在同时拼命计算，但最终只有一个人能赢**。

比特币全网每秒进行约**600,000,000,000,000,000次**（60亿亿次）哈希计算。想象一下，全球几百万台专业矿机，7×24小时不停地高速运转，只为了每10分钟那一次中奖机会。那些没中奖的矿工，他们的所有计算都**白费了**——这就是能源浪费的根源。

### **3\. 为什么要设计得这么"浪费"？**

这不是bug，而是feature（特性）！这种"浪费"恰恰是安全性的来源：**防止攻击的成本**：如果你想篡改区块链记录，就需要控制全网51%以上的算力。以比特币为例，这意味着你要：

-   购买全球一半以上的矿机（几百亿美元）
    
-   支付巨额电费持续运行
    
-   即使攻击成功，比特币信誉崩塌，你的投入也血本无归
    

**用物理世界的资源（电力）保护数字世界的安全**——这就是PoW的逻辑。

### **4\. 耗能到底有多大？**

比特币网络年耗电量约**150-200太瓦时**，相当于：

-   荷兰或阿根廷全国一年的用电量
    
-   约占全球总用电量的0.5-0.7%
    

以太坊在2022年9月之前也用PoW，耗电量约为比特币的一半。

## **为什么权益证明（PoS）节能？**

### \*\*完全不同的逻辑：\*\*PoS不是比拼算力，而是比拼"质押的代币数量"。

**运作方式**：

1.  想当验证者？先锁定一定数量的代币（比如以太坊需要32个ETH，约10万美元）
    
2.  系统随机或按权重选择验证者来打包交易
    
3.  如果你诚实工作，获得奖励；如果作恶，质押的代币会被罚没（Slashing）
    

**为什么节能？**

验证一个区块只需要：

-   运行验证软件（一台普通电脑就够）
    
-   检查交易合法性（几秒钟的计算）
    
-   数字签名（密码学运算，几乎不耗能）
    

**没有任何"竞赛式计算"**，不需要成千上万台机器同时拼算力。

### **节能效果有多显著？**

以太坊从PoW转为PoS后（"The Merge"升级）：

-   **能耗下降了99.95%**
    
-   从原来的荷兰级别降到一个小镇的用电量
    
-   每笔交易的碳排放从20kg降到0.01kg
    

## **一、去中心化金融（DeFi）是什么？**

DeFi = Decentralized Finance（去中心化金融）  
DeFi = 去中心化金融

传统金融做什么事都要通过银行、券商、交易所这些机构，**DeFi就是把这些金融服务搬到区块链上，用智能合约代替银行职员**。

**对比理解**：

-   传统：你去银行存钱→银行员工审核→存入你的账户→银行用你的钱放贷赚利息→分你一点利息
    
-   DeFi：你直接把钱放进智能合约→合约自动匹配借款人→利息自动分给你，没有中间商
    

* * *

## **二、Uniswap（去中心化交易所）**

### **传统交易所 vs 去中心化交易所（DEX）**

**传统交易所**（比如币安、Coinbase）：

-   你要先把钱充值到交易所账户（把钱给他们保管）
    
-   交易所撮合买卖双方
    
-   你的资产实际上在交易所手里，你只是看到"余额"
    

**DEX（Decentralized Exchange）**：  
去中心化交易所（DEX）：

-   你的钱始终在自己钱包里，不需要充值到平台
    
-   直接用钱包连接网站，点几下就能交易
    
-   交易完成后，新币直接到你钱包，全程你掌控资产
    

### **AMM（自动做市商）是什么？**

AMM = Automated Market Maker（自动做市商）

**传统撮合机制**（像股票交易）：

-   买家挂单：“我要1000元买1个ETH”
    
-   卖家挂单：“我要1100元卖1个ETH”
    
-   需要买卖双方价格匹配才能成交
    

**AMM机制**（Uniswap用的）：

-   不需要买卖双方匹配
    
-   而是建一个"流动性池子"，里面放着大量的ETH和USDC
    
-   你想买ETH？直接从池子里拿ETH，把USDC放进去
    
-   价格由一个数学公式自动计算（简化版：池子里ETH越少，价格越贵）
    

**打个比方**：

-   传统交易所像"菜市场"：买菜的和卖菜的讨价还价
    
-   Uniswap像"自动售货机"：你投币，机器立刻给你商品，价格是固定公式算出来的
    

* * *

## **三、Aave（借贷协议）**

### **传统借贷 vs DeFi借贷**

**传统银行借贷**：

1.  你去银行申请贷款
    
2.  银行查你征信、审核材料
    
3.  可能等几天或几周
    
4.  批准后钱打到你账户
    
5.  按月还款，逾期会催收
    

**Aave这样的DeFi借贷**：

1.  你在Aave抵押10个ETH（假设价值3万美元）
    
2.  立刻借出价值2万美元的USDC（只能借抵押物价值的一定比例，比如70%）
    
3.  不需要审核、不需要征信、几分钟完成
    
4.  利息实时计算（可能年化3-8%，随市场变化）
    
5.  什么时候还都行，但如果ETH价格暴跌，你的抵押物不够了，会被自动清算（系统卖掉你的ETH还债）
    

### **TVL是什么？**

TVL = Total Value Locked（锁仓总价值）  
TVL = 总锁仓价值

就是**有多少钱放在这个协议里**。

比如Aave的TVL是100亿美元，意味着全球用户在Aave里存了100亿美元的各种加密资产。TVL越高，说明协议越受信任、规模越大。

**类比**：就像银行说"我们管理的资产规模是1000亿"，TVL是DeFi协议的"资产规模"。

* * *

## **四、MakerDAO 和 DAI稳定币**

### **什么是稳定币？**

加密货币价格波动太大（ETH今天3000美元，明天可能2500美元），没法当"钱"用。

**稳定币**就是锚定法定货币的加密货币，比如：

-   1 USDT ≈ 1美元
    
-   1 DAI ≈ 1美元
    

### **两种稳定币类型**

**1\. 中心化稳定币（USDT、USDC）**：

-   像Tether公司发行USDT，他们说"我们银行账户里有10亿美元，所以发行10亿个USDT"
    
-   你要相信这个公司真的有这么多钱
    
-   他们可以冻结你的USDT
    

**2\. 去中心化稳定币（DAI）**：

-   MakerDAO用智能合约发行DAI
    
-   你抵押1.5万美元的ETH，可以铸造1万个DAI
    
-   如果ETH跌价，系统会自动卖掉你的ETH，保证DAI价值稳定
    
-   没有公司能冻结你的DAI
    

**打个比方**：

-   USDT像"人民币兑换券"：银行说可以1:1换人民币，你要信银行
    
-   DAI像"黄金兑换券"：后面真的锁着黄金在金库里，智能合约自动管理
    

* * *

## **五、Lido 和 EigenLayer（质押相关）**

### **什么是质押（Staking）？**

还记得我之前讲的PoS共识机制吗？以太坊现在用的就是PoS。

**质押**就是：你把32个ETH锁进以太坊网络，成为验证者，帮忙验证交易，然后赚取奖励（年化大概3-5%）。

**问题**：

-   需要整整32个ETH（约10万美元），普通人没这么多钱
    
-   ETH被锁住了，不能卖、不能用，失去了流动性
    

### **Lido解决了什么问题？**

Lido提供**流动性质押**：

1.  你只有1个ETH？没关系，存到Lido
    
2.  Lido把大家的ETH凑一起，达到32个就帮你们去质押
    
3.  同时给你1个**stETH**（质押凭证）
    
4.  stETH可以自由交易、可以在其他DeFi协议里用
    
5.  你既赚到质押奖励，又保留了流动性
    

**类比**：

-   传统质押像"定期存款"：钱锁死了，到期才能取
    
-   Lido像"活期存款"：一样赚利息,但随时能用
    

### **EigenLayer 再质押是什么？**

**再质押（Restaking）** = 一份资产多重使用

1.  你在Lido质押ETH，得到stETH，赚取3%年化
    
2.  你把stETH再存到EigenLayer  你将 stETH 重新存入 EigenLayer
    
3.  EigenLayer用你的stETH为其他区块链项目提供安全保障
    
4.  你额外再赚一层奖励（可能再加2%年化）
    
5.  总收益可能达到5%，但风险也更高（两层都可能被罚）
    

**类比**：

-   传统质押：你把钱存银行赚利息
    
-   再质押：你用银行存款证明做抵押，再去贷款投资，赚双份收益（但也有双倍风险）
    

* * *

## **六、RWA（现实世界资产上链）**

RWA = Real-World Assets（现实世界资产）  
RWA = 现实世界资产

### **这是什么意思？**

把现实世界的资产（房产、国债、黄金、公司股权等）**代币化**，在区块链上交易。

**传统方式买美国国债**：

1.  需要开美股账户
    
2.  可能需要几千美元起步
    
3.  交易时间受限（美股开盘时间）
    
4.  可能要几天结算
    

**RWA方式买代币化国债**：

1.  用加密钱包就能买
    
2.  10美元也能买
    
3.  24/7随时交易
    
4.  几分钟结算
    

### **BlackRock（贝莱德）为什么做这个？**

BlackRock是全球最大资产管理公司（管理10万亿美元）。

他们发行的**BUIDL基金**：

-   投资美国国债和现金
    
-   代币化放在区块链上
    
-   每个代币值1美元，每天分红
    
-   普通人用加密钱包就能买"美国国债"
    

**意义**：

-   传统金融巨头进入Web3，说明这个行业在成熟
    
-   普通人能轻松投资以前门槛很高的资产
    
-   资产可以24小时交易、全球流通
    

**类比**：就像支付宝的余额宝把货币基金普及给普通人，RWA把各种传统金融资产普及给全球有网络的人。

* * *

## **总结：这些协议在干什么？**

| 协议 | 传统金融对应 | 普通人能干什么 |
| --- | --- | --- |
| Uniswap | 交易所 | 随时兑换各种代币，不需要开户 |
| Aave | 银行借贷 | 存币赚利息，或抵押借贷，秒到账 |
| MakerDAO/DAI | 稳定币发行商 | 用DAI在链上支付、避免币价波动 |
| Lido | 理财产品 | 小额也能质押ETH赚收益 |
| EigenLayer | 杠杆投资 | 用质押的币再赚一层收益 |
| RWA | 基金公司 | 在链上买美国国债、房产份额 |

**核心理念**：把银行、券商、基金公司做的事，用智能合约实现，让全球任何有网络的人都能参与，不需要开户非常好的问题！这个问题触及了Web3最核心的争议。我给你一个诚实、理性的分析。

**传统金融体量**：

-   全球银行业资产：约**140万亿美元**
    
-   全球股市市值：约**100万亿美元**
    
-   全球债券市场：约**130万亿美元**
    
-   全球支付交易量：每天**数万亿美元**
    

**DeFi体量**（2025年初）：

-   整个DeFi锁仓总值（TVL）：约**1000-1500亿美元**
    
-   占传统金融的比例：**0.03%左右**
    

**现实**：DeFi现在连传统金融的零头都不到。说"取代"还为时尚早。

* * *

## **为什么传统金融很难被完全取代？**

### **1\. 普通人真的需要"去中心化"吗？**

**大多数人的真实想法**：

-   "我的钱丢了，我要找银行客服"✅
    
-   "我忘了密码，我要重置密码"✅
    
-   "有人盗刷我的卡，我要冻结账户"✅
    

**DeFi的现实**：

-   私钥丢了=钱永远丢了，没有客服
    
-   转错地址=钱永远找不回，没有撤销
    
-   被黑客盗了=自己承担损失，没有保险
    

**举个真实例子**： 2021年，有人在给某DeFi协议转账时，错误地把手续费设成了700ETH（当时价值200万美元），本该只花几美元。这笔钱永远找不回来，因为没有"撤销"功能。

**普通人真的准备好承担这种责任吗？** 大部分人不是。

### **2\. 法律和监管的保护**

**传统金融的保护**：

-   存款保险：银行倒闭，政府赔你（中国50万，美国25万美元）
    
-   反欺诈保护：信用卡被盗刷，银行给你退款
    
-   法律追诉：出问题可以起诉银行、报警
    
-   争议解决：可以投诉、仲裁
    

**DeFi的现实**：

-   智能合约有bug被黑，你自己承担（已经发生几十次了，损失数十亿美元）
    
-   被钓鱼网站骗签名授权，钱被转走，找谁都没用
    
-   项目方跑路（Rug Pull），你报警警察都不知道怎么处理
    

**真实案例**：

-   2022年，Luna/UST崩盘，投资者损失**400亿美元**，至今大部分人血本无归
    
-   2023年，FTX交易所破产，客户**80亿美元**被挪用，CEO被判25年，但钱大部分拿不回来
    

### **3\. 传统金融的效率优势**

很多人以为DeFi更高效，实际上：

**支付速度**：

-   支付宝/微信支付：**秒到**，还能撤销
    
-   以太坊转账：15秒-几分钟，还要等几个确认，**不可撤销**
    
-   比特币转账：10分钟-1小时
    

**交易成本**：

-   银行转账（国内）：**免费**或几毛钱
    
-   支付宝/微信：**免费**
    
-   以太坊转账：网络拥堵时可能**几十到几百美元**（2021年牛市时，转个账要50美元Gas费）
    

**用户体验**：

-   银行App：忘记密码→手机验证→重置→继续用
    
-   DeFi钱包：私钥丢失→**游戏结束**，几万几十万美元说没就没
    

### **4\. 大部分金融服务需要"信任"和"人的判断"**

**举些例子**：

**房贷**：

-   银行会评估你的收入、工作稳定性、未来还款能力
    
-   智能合约怎么评估？只能看"你现在有多少加密资产"
    
-   DeFi只能做**超额抵押**（抵押150元借100元），这对买房没意义
    

**保险**：

-   传统保险会调查理赔真实性、有人工审核
    
-   DeFi保险遇到灰色地带怎么判断？完全依赖"预言机"（提供外部数据的服务），如果预言机出错或被操纵呢？
    

**信用卡**：

-   你可以"先消费后还款"，因为银行相信你会还
    
-   DeFi做不到，因为智能合约无法判断你的信用
    

* * *

## **但DeFi也有传统金融做不到的事**

### **1\. 金融普惠（Financial Inclusion）**

**真实场景**：

-   阿富汗女性在塔利班统治下被禁止开银行账户，但她们可以用加密钱包接收国际援助
    
-   委内瑞拉恶性通胀（年通胀率1000%+），人们用稳定币保护资产
    
-   移民工人跨国汇款，传统渠道收费10-20%，用加密货币可能只要1-2%
    

**传统金融的门槛**：

-   全球约**17亿人**没有银行账户
    
-   跨境汇款平均成本：**6-7%**
    
-   开户需要身份证明、居住证明、最低存款额
    

**DeFi只需要**：

-   一部手机+网络
    
-   不需要身份证明
    
-   1美元也能参与
    

### **2\. 透明度和可组合性**

**传统金融**：

-   你不知道银行拿你的钱做了什么
    
-   各个金融产品相互隔离，无法组合
    

**DeFi**：

-   所有交易公开可查（虽然匿名）
    
-   我可以把Aave借的钱→在Uniswap交易→存到Lido质押→用stETH在Curve做流动性挖矿，一气呵成
    
-   这叫"金融乐高"，传统金融做不到
    

### **3\. 抗审查和资产自主权**

**真实案例**：

-   2022年，加拿大政府冻结参与卡车司机抗议者的银行账户
    
-   很多国家政府可以冻结"不听话"的人的银行账户
    
-   塞浦路斯2013年银行危机，政府直接从储户账户扣款
    

**DeFi**：

-   没有人能冻结你的加密钱包（除非你用中心化交易所）
    
-   你真正拥有自己的资产
    

* * *

## **更可能的未来：融合，而非取代**

### **现实中正在发生的事**

**传统金融拥抱区块链技术**：

-   **摩根大通**：开发了JPM Coin，用于机构间结算
    
-   **花旗银行**：测试代币化存款
    
-   **贝莱德**：发行代币化货币基金BUIDL
    
-   **Visa/Mastercard**：支持加密货币支付
    
-   **香港、新加坡**：央行数字货币（CBDC）试点
    

**他们的逻辑**：

-   用区块链技术提高效率（特别是跨境支付、证券结算）
    
-   但保留中心化控制和监管合规
    
-   这叫**许可链（Permissioned Blockchain）** 或 **CeDeFi（中心化DeFi）**
    

### **未来可能的分工**

| 场景 | 更适合传统金融 | 更适合DeFi |
| --- | --- | --- |
| 日常支付 | ✅ 快、便宜、可撤销 | ❌ 慢、贵、不可逆 |
| 储蓄 | ✅ 有保险、稳定 | ❌ 风险高 |
| 贷款（信用） | ✅ 能评估信用 | ❌ 只能超额抵押 |
| 跨境汇款 | ❌ 慢、贵（2-5天，5-10%费用）  ❌ 缓慢、昂贵（2-5 天，5-10%费用） | ✅ 快、便宜（分钟级，1-2%） |
| 金融普惠 | ❌ 门槛高 | ✅ 低门槛 |
| 抗审查需求 | ❌ 政府可干预 | ✅ 难以审查 |
| 投机/套利 | ❌ 限制多 | ✅ 24/7开放 |
| 资产证券化 | ❌ 流程复杂 | ✅ 智能合约自动化 |

* * *

## **我的诚实看法**

### **不会完全取代，原因如下**：

1.  **大多数人不需要"去中心化"** —— 他们需要的是便利、安全、有人兜底
    
2.  **监管不会消失** —— 政府不会允许不受控的金融系统
    
3.  **技术还不成熟** —— 扩容、用户体验、安全性都有大问题
    
4.  **惯性太强** —— 几百年的金融体系有巨大的既得利益和社会惯性
    

### **但会长期共存，各有价值**：

**传统金融的未来**：吸收区块链技术，变得更高效（但保留中心化控制）

**DeFi的未来**：

-   服务传统金融覆盖不到的人群（无银行账户、受制裁国家）
    
-   提供特殊需求（隐私、抗审查、跨境）
    
-   作为创新实验场，推动传统金融改革
    

**打个比方**：

-   就像互联网没有"取代"线下世界，而是创造了新的空间
    
-   电商没有"取代"实体店，但改变了商业格局
    
-   DeFi不会"取代"传统金融，但会重塑金融版图
    

### **对你的建议**：

1.  **不要被极端言论洗脑** —— 无论是"DeFi革命论"还是"区块链骗局论"
    
2.  **理解技术，但保持理性** —— Web3有价值,但不是万能的
    
3.  **风险意识** —— 如果要参与DeFi,只投入你能承受损失的钱
    
4.  **关注实际应用** —— 看它到底解决了什么真实问题，而不是炒作概念
    

* * *

**思考题**： 在中国，DeFi有多大发展空间？考虑到支付宝/微信支付已经这么方便，央行数字货币（数字人民币）也在推进，普通中国人还需要DeFi吗？
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->














第一天打卡：

主要做了三件事：

1.  阅读021 学习以太坊第 1 章 了解了一些关于区块链和以太坊的基本知识
    
2.  听Web3 行业全局介绍 & 岗位概览，了解了大致的岗位以及努力的方向
    
3.  创建了metamask钱包，并第一次领取了活水，也转了一点给同学
    

以下是详细的内容：

## 基本概念

**以太坊（Ethereum）**是一个去中心化、开源并且具备智能合约功能的公共区块链平台。

**智能合约（Smart Contract）** 是一种写在区块链上的自动化协议程序，可以在满足预先设定条件时，自动执行约定好的操作，而不需要人工干预或第三方中介。

**以太坊虚拟机（Ethereum Virtual Machine，EVM）**是以太坊的核心组件，它是

一个图灵完备的虚拟机，能够执行智能合约代码，让区块链不再只是“记账”，而是可以运行各类程序和应用。

**以太（Ether，缩写为 ETH）**加密货币作为其内部交易的“燃料”（Gas），用于支付交易费用和计算服务

**燃料(Gas)** 是以太坊里对“计算工作量”的计价方式，用 ETH 来支付。

**不可替代代币（NFT，Non-Fungible Token）**技术上，NFT是一段写在区块链上的智能合约记录，记录了：

-   唯一编号（Token ID）
    
-   拥有者地址
    
-   相关规则（如转让、版税）
    

**铸造 NFT** \= 把“某个东西的所有权信息”写进区块链，在区块链上创建一个新的、唯一的 NFT

**第 1 步：准备“作品”**

可以是：图片（JPG / PNG）音乐（MP3）视频（MP4）甚至一段文字⚠️ 注意：作品本身一般不直接存到区块链上

**第 2 步：把作品存到“链下**

因为区块链存储很贵，所以通常：上传到 IPFS / Arweave / 云存储得到一个唯一链接（Hash）👉 区块链只保存“指向它的地址”

**第 3 步：创建 NFT 智能合约**

智能合约会规定：NFT 的标准（如 ERC-721 / ERC-1155）每个 NFT 的唯一编号（Token ID）谁是初始拥有者 是否有转售版税

**第 4 步：发起“铸造交易”**

用钱包（如 MetaMask）：调用合约的 mint 函数支付 Gas（用 ETH）这一步是真正上链的关键

**第 5 步：NFT 诞生** 区块链记录：NFT 已存在 Token ID 是多少 拥有者是谁 指向哪份作品 👉 全世界都可以验证

**去中心化金融（DeFi）：**

借贷协议、去中心化交易所、衍生品、稳定币、收益聚合器……

**去中心化自治组织（DAO）：**

规则写进合约，由代币持有者投票治理，无“董事会拍脑袋”。

### 验证者具体在做什么？

以太坊 PoS 中，验证者主要做两件事：

**提议区块（propose）**：某个时隙里会选出一个验证者来打包区块

**证明/投票（attest）**：其他验证者对区块投票确认（证明它有效）

如果验证者：

-   **离线**：会被扣一点收益/罚一点（轻罚，逼你保持在线）
    
-   **作恶（比如双签）**：会触发 **slashing 削减**（重罚，可能损失很大）
    

### 打包区块是什么意思？

“打包区块”就是：把**一堆待处理的交易**（比如转账、调用智能合约）按规则**整理成一个新区块**，并把这个新区块**发布到以太坊链上**，让全网认可它成为“最新一页账本”。

把以太坊想成一本**不断追加的新账本**：

-   **交易** = 一条条记账请求（“A 给 B 转 1 ETH”“我在 Uniswap 换币”）
    
-   **区块** = 一页账本
    
-   **打包区块** = 负责把这一页写好、盖章、交给全网确认的人（PoS 里是“被选中的验证者”）
    

在以太坊 PoS 里，一次“出一个区块”通常是：

-   **1 个验证者被随机选中当“提议者”（proposer）**：负责把交易打包成新区块并发布。
    
-   **很多其他验证者当“证明者/投票者”（attesters）**：对这个区块投票确认它有效。
    

所以是：**1 个负责写这一页账本 + 一群负责盖章确认**。

### 为什么要一群人投票？

因为这样才能做到“去中心化的共识”：

-   如果只有 1 个验证者说了算，他就可能乱写账。
    
-   有很多验证者一起核对投票，作恶更难、成本更高。
    

### 那“最终确认”需要多少验证者？

不需要你记精确数字，只要抓住这个直觉：

-   区块刚出来时，是\*\*“被一批验证者投票支持”\*\*，就算比较稳了（常说的 _finality 之前的确认_）。
    
-   过一段时间，达到协议的最终性（finality）后，区块基本就**不可逆**了——这也依赖**大量验证者的投票**来完成。
    

如果你问的是更直观的问题： **“每个区块是不是只由一个验证者产生？”**——是的，**提议者只有一个**； 但\*\*“每个区块是不是只需要一个验证者认可？”\*\*——不是，**需要很多验证者一起认可**。

## web3交易过程

## 0）你在钱包里点“发送”之前：交易其实是一份“指令包”

一笔以太坊交易本质上包含这些关键信息（你不一定都手填，但钱包都会帮你填）：

-   **from**：你的地址（由签名自动确定）
    
-   **to**：收款地址或合约地址
    
-   **value**：转多少 ETH（如果只是调用合约也可能是 0）
    
-   **data**：调用合约函数时的参数（普通转账通常为空）
    
-   **nonce**：你这个地址发出的第几笔交易（同一地址必须按 nonce 顺序上链）
    
-   **gas limit**：你愿意给这笔交易最多用多少“计算量”
    
-   **费用参数（EIP-1559）**：
    
    -   **maxFeePerGas**：你愿意支付的“每单位 gas 的最高单价”
        
    -   **maxPriorityFeePerGas**：你愿意给打包者的小费（tip）
        
    -   实际上交易要至少覆盖 **base fee + tip**，其中 base fee 会被销毁，tip 给打包者/验证者
        

> 直觉：nonce 决定“排队顺序”，费用决定“别人愿不愿意优先处理你”。

* * *

## 1）签名：你把这份“指令包”变成不可伪造的交易

你点确认后，钱包会用你的私钥对交易签名（不会把私钥发出去）。  
签完以后，这笔交易就有了唯一的 **交易哈希 TxHash**（像收据编号）。

* * *

## 2）广播：交易进入 mempool（交易池）

钱包会把签名后的交易发给一个以太坊节点（可能是你用的钱包/Infura/Alchemy/自建节点）。

节点会先做“基础检查”：

-   签名对不对、nonce 对不对
    
-   你余额是否够支付 `value + 最高可能手续费`
    
-   gas limit、费用参数是否合理（至少有机会覆盖当前 base fee）
    

通过后，它会把交易放进 **mempool**，并转发给其他节点——这时你在浏览器里会看到 **Pending（待打包）**。

* * *

## 3）打包前：谁决定“这笔交易进哪个区块”？

在 PoS 里，以太坊把时间切成：

-   **slot：12 秒一个**
    
-   **epoch：32 个 slot**
    

每个 slot 会随机选出 **1 个验证者当“提议者（proposer）”** 去提出新区块 。

但“新区块里具体放哪些交易、交易怎么排序”在现实里经常是：

-   由提议者自己从 mempool 挑（更简单的情况）
    
-   或者通过“构建者/区块构建市场”拿到一个更赚钱的交易排序方案（你可以先把它理解成：大家都倾向选**手续费更高/更赚钱/更可执行**的交易顺序）
    

所以你**无法在 pending 阶段精确确定**它会进第几个区块，只能用费用等因素提高“尽快进区块”的概率。

* * *

## 4）区块内执行：交易不是“写进去就算完”，还要真正跑一遍

当某个 proposer 把你的交易放进区块后，全网执行层会**按区块里给定的顺序执行交易**：

-   普通转账：扣你余额、加对方余额（固定 21,000 gas）
    
-   调合约：EVM 执行 `data` 里的函数逻辑，读写合约存储、发事件日志等
    
-   如果执行中**触发 revert**：
    
    -   状态改动会回滚
        
    -   **但 gas 仍然消耗**（因为计算已经做了）
        

执行完会生成一份 **receipt（回执）**，里面有：

-   status（成功/失败）
    
-   gasUsed
    
-   logs（事件日志，用于前端/索引器显示“发生了什么”）
    
-   blockNumber、transactionIndex（你最终要找“它在哪个区块”就看这两个）
    

* * *

## 5）确认与“最终性”：什么时候算真的不可逆？

区块提出后，其他验证者会对区块进行投票/证明（attestation），区块就会逐渐“更难被回滚”。

在以太坊当前的 PoS 设计下，通常在**约两个 epoch**后能达到非常强的经济意义上的“最终性（finality）”，也就是大约 **12.8 分钟**（理想网络条件下）。

你在区块浏览器里看到的：

-   **1 confirmation、2 confirmations…**：只是“在它后面又接了多少个区块”，越多越稳
    
-   **finalized**（或类似标记）：基本可以认为不可逆（除非极端大故障/大规模作恶）。
    

## web3技术人员主要工作内容

### 1) 智能合约工程师（Solidity / 协议开发）

-   设计与实现合约：代币、AMM/借贷、NFT、拍卖、治理、权限系统
    
-   写测试、做形式化/模糊测试、Gas 优化
    
-   处理升级（Proxy）、跨链、预言机集成、权限/多签方案
    
-   上线部署与版本管理（主网/测试网、多链）
    

**常见技术栈**

-   语言：**Solidity**（以太坊系主流）、Vyper（少量）
    
-   框架：**Foundry**（forge/cast/anvil）、Hardhat（也很常见）
    
-   合约库：OpenZeppelin、Solmate
    
-   测试：forge test / hardhat test、fuzz（Foundry/Echidna）
    
-   安全工具：Slither、Mythril、Echidna、Manticore（视团队成熟度）
    
-   调试/模拟：Tenderly、anvil/fork、mainnet-fork
    

* * *

### 2) Web3 前端工程师（DApp 前端）

-   做钱包连接、签名、交易发送、状态展示、错误与网络切换
    
-   处理链上数据读取：余额、头寸、NFT、交易历史、价格等
    
-   兼容多钱包/多链，提升交互体验（失败重试、gas 估算、Pending 状态）
    

**常见技术栈**

-   Web：**React / Next.js / TypeScript**
    
-   钱包/链交互：**ethers.js** 或（更现代）**viem + wagmi**
    
-   钱包标准：WalletConnect、EIP-1193 Provider、SIWE（Sign-In with Ethereum）
    
-   UI/状态：TanStack Query、Redux/Zustand（看团队习惯）
    

* * *

### 3) 链下后端 / 区块链基础设施工程师

-   提供 API：用户数据、资产聚合、报价、风控、活动等
    
-   监听链上事件、解析交易，做索引与缓存（否则前端直接读链会很慢/贵）
    
-   管理节点/RPC：自建或对接服务商，做限流、容灾、监控
    
-   处理任务队列：发奖、清算、自动化执行、批处理
    

**常见技术栈**

-   语言：Node.js/TypeScript、Go、Python、Java（都常见）
    
-   数据：Postgres、Redis、Kafka/RabbitMQ
    
-   索引：The Graph（subgraph）、自建 indexer（监听 logs + 解析 ABI）
    
-   存储：IPFS/Arweave（元数据）、对象存储
    
-   云与运维：Docker、K8s、CI/CD、监控告警（Prometheus/Grafana）
    

* * *

### 4) 安全工程师 / 审计方向

-   做威胁建模：重入、权限绕过、价格操纵、闪电贷、MEV 风险
    
-   静态/动态分析、fuzz、形式化验证（成熟团队会做得更深）
    
-   代码审计与上线清单：多签、timelock、权限最小化、应急开关策略
    

**常见技术栈**

-   Slither / Mythril / Echidna / Foundry fuzz
    
-   形式化：Certora / Scribble / K Framework（取决于预算与要求）
    

## 来自群里森理的钱包教程

## MetaMask钱包版

图文教程版：[https://humdrum删-volleyball-4a2删.notion删.site/2e54867ff3e180b5bc84e33a7e2f9835](https://humdrum删-volleyball-4a2删.notion删.site/2e54867ff3e180b5bc84e33a7e2f9835)

省流版：安装MetaMask -> 开启测试网（Google ETH） -> 复制地址领币（俗称领水） -> 转账 -> 拿链接交作业

纯文字版：

在chrome扩展商店搜索MetaMask，安装

安装完毕——创建新钱包——注册

进入钱包

最开始我们是看不到测试网Sepolia的，点击右上角—网络—滑倒最底下，显示测试网络

这时我们的钱包就准备好了

点击收款，复制接收地址（我一般复制Ethereum）

然后去测试网领币（领水）—— [https://cloud删.google删.com/application/web3/faucet/ethereum](https://cloud删.google删.com/application/web3/faucet/ethereum)

选择Ethereum Sepolia（0.05ETH）这一项

粘贴自己的收款地址，ETH到手

然后查看账户，到账了以后就可以打款给别的同学了

选择发送ETH，填入别的同学的钱包地址与发送金额

确认交易，稍等片刻

确认交易完成后点击这个交易

点击在区块浏览器上查看

然后我们就会跳转Etherscan的页面但是不重要

复制顶部的这一串网址，即为我们所需的tx link
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
