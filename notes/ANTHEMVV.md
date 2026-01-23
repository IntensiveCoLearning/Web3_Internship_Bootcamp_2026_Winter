---
timezone: UTC+8
---

# ANTHEMVV

**GitHub ID:** ANTHEMVV

**Telegram:** Emvogu

## Self-introduction

通信CS背景，想系统性地学习Web3相关的知识

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
今天学习了Solidity的基本知识，马上把Solidity by example的basic部分的代码全部理解完了，已经写了大概800行代码的基础知识，还完成了Gas优化的案例，了解了Gas在ETH当中是一个很重要的优化部分
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

今天学习的编程内容以及笔记如下：

```remix-solidity
// Enum 对应的其实就是一个下拉菜单 最多有256个，因为单位是uint8
// 底层是数字对应的就是01234……，只能定义名字系统会强制从0开始编号，无法改变这个顺序也无法改变值
// User defined Value Type
type Duration is uint64;
//底层是整数，但是在编译器中是完全不同的type
type Timestamp is uint64;

type Clock is uint128;

library LibClock {
    function wrap(Duration _duration, Timestamp _timestamp)
        internal
        pure
        returns (Clock clock_)
    {
        assembly {
            // data | Duration | Timestamp
            // bit  | 0 ... 63 | 64 ... 127
            clock_ := or(shl(0x40, _duration), _timestamp)
        }
        //将两个自定义变量打包成一个Clock Duration占64位，Timestamp占64位 Clock占128位
    }

    function duration(Clock _clock)
        internal
        pure
        returns (Duration duration_)
    {
        assembly {
            duration_ := shr(0x40, _clock)
        }//解包，求duration
    }

    function timestamp(Clock _clock)
        internal
        pure
        returns (Timestamp timestamp_)
    {
        assembly {
            timestamp_ := shr(0xC0, shl(0xC0, _clock))
        }//解包，求timestamp
    }
}

// Clock library without user defined value type
library LibClockBasic {
    function wrap(uint64 _duration, uint64 _timestamp)
        internal
        pure
        returns (uint128 clock)
    {
        assembly {
            clock := or(shl(0x40, _duration), _timestamp)
        }
    }
}

contract Examples {
    function example_no_uvdt() external view {
        // Without UDVT
        uint128 clock;
        uint64 d = 1;
        uint64 t = uint64(block.timestamp);
        clock = LibClockBasic.wrap(d, t);
        // Oops! wrong order of inputs but still compiles
        clock = LibClockBasic.wrap(t, d);
    }

    function example_uvdt() external view {
        // Turn value type into user defined value type
        Duration d = Duration.wrap(1);
        Timestamp t = Timestamp.wrap(uint64(block.timestamp));
        // Turn user defined value type back into primitive value type
        // uint64 d_u64 = Duration.unwrap(d);
        // uint64 t_u64 = Timestamp.unwrap(t);

        // LibClock example 编译器会检测参数的顺序，数据类型不对不会编译
        Clock clock = Clock.wrap(0);
        clock = LibClock.wrap(d, t);
        // Oops! wrong order of inputs
        // This will not compile
        // clock = LibClock.wrap(t, d);
    }
}
//使用assembly是进行位打包，把两个变量存进一个slot节省存储空间
// struct Solidty当中只有数据不包含函数与python中的class不同
// 假设 todos[0] 是 "Run"

// 情况 A: Memory (复印件)
// Todo memory copy = todos[0];
// copy.text = "Walk"; 
// todos[0] 还是 "Run"！因为你只改了复印件。

// 情况 B: Storage (原件指针)
// Todo storage original = todos[0];
// original.text = "Walk";
// todos[0] 变成了 "Walk"

// Data location Solidity当中必须手动指定复杂的数据类型存放在哪里，为了节省Gas费
// Storage：写入的数据会永远保存在区块链上，即使函数执行结束，数据依旧存在。极其昂贵，可以随时修改里面的内容
// 所有定义在函数外部的状态变量都默认是storage
// Memory：只存在当前函数执行期间，比storage便宜得多，可读可写（草稿纸）
// Calldata：只读，不能改，所以避免了将数据复制到内存的开销，交易结束就会消失，一般只能用于函数的输入参数
// 当操作不涉及到修改参数，优先考虑calldata
// Transient Storage：专门为单笔交易存在的存储区域，一旦交易所有数据清空
```

除此之外还完成了一个漏洞仿真学习案例
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


今天参加了两次分享会并且继续学习Solidity的基础知识，目前已经完成了很多内容的学习，开始着手实践内容
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



主要学习编程内容，基本的知识点都快看完了，这是笔记：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ANTHEMVV/images/2026-01-20-1768922817109-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ANTHEMVV/images/2026-01-20-1768922834854-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ANTHEMVV/images/2026-01-20-1768922826815-image.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




今天正在完成了配置RemixIDE并且上手编程，现在在做**完成挑战 Challenge #0 - Tokenization，但是还没有全部做完，将环境以及第一二步做完了，然后继续学习021以太坊以及实习手册当中智能合约部分**
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





这篇笔记就作为第一周的个人小总结吧：

虽然报名的是编程的赛道，第一周大部分知识点与编程无关，而是有着在这个行业已经接触了很久的大佬给我们分享行业的知识以及经验，受益匪浅，每次分享会我都参加了但还都没发过言。总的来说这一周让我了解到了Web3的岗位职责以及发展路径，让我更加明确以及肯定了要在Web3道路上深耕，要努力学习底层原理以及运用AI工具，这周因为研究生的任务繁忙不过都已经结项了，下周有更多的时间学习Web3了，下周继续加油！！！！
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






今天学习了Solidity的基本知识，是在网站CryptoZombies进行学习的，也看了助教老师们总结的资料，学习成果如图所示：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ANTHEMVV/images/2026-01-17-1768657575613-image.png)

-   状态变量永久存储在合同当中，是写进以太坊区块链中的
    
-   Solidity的数学计算与大部分编程语言相同
    
-   在Solidity中，函数默认是公开的，也可以将函数标记为私有函数
    
-   以太坊内置了哈希函数keccak256,是将输入映射到一个随机的256位的十六进制数当中。输入的轻微变化会导致哈希值大幅变化
    
-   学习了事件，事件是合约向应用前端传达区块链上发生了某事的方式，前端可以监听事件并在事件发生时采取行动。
    

暂时学习的内容就是这些，并且都有自己进行编写代码。同时看到第二周技术方向的任务更新了，学习了Web3实习手册中智能合约开发部分，安装了Remix IDE。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







今天是一周的例会了，收听了其他同学讲的PPT以及一些经验分享，这一周比较忙都在忙学业的事情，但已经告一段落了，明天以及之后的笔记会写一些干货的东西。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->








AI+Web3,今天参加了分享会讲了AI的一些基础知识，我目前研究生的研究方向也与AI相关，不过我对于AI的了解也只基于Web2当中。

1.  ERC-8004：以太坊社区专门为AI设计的身份标准，基于ERC721实现，是智能体经济的信任基础设施，汇聚了协议的三大功能，身份：为AI智能体上链登记；声誉：为AI智能体记录和评分；验证：AI智能体的能力验证并存储。
    
2.  X402：是让网页、AI、API自己收费的Web3支付协议；AI向服务器请求资源，服务器返回一个提示，例如：要用这个服务，请支付0.05ETH。支付完成后自动返回结果。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->









今天由于学校开组会并没有完成太多任务，还是依照成长手册继续学习，参加了Web3安全和合规两个会议。了解到如今WEB3在国内还并不成熟，有许多风险，例如出入金和冻卡，还没有明确的牌照制度规范。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










今天因为忙一些其他的事情耽误了学习，将基础任务中的钓鱼攻防战做完了，以及继续看了实习手册当中的内容，查看了WEB3运行原理，了解一些钱包，助记词，交易的一些gas开支的基本原理，总体来说今天学习时间还不够明天继续努力学习Solidity的相关知识。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->











2026.1.12

1.  完成了创建钱包，并且尝试了测试币转账
    
2.  在My First NFT Mint了第一个NFT
    
3.  在Co-Learning了解到了Web3的一些职位分享以及一些技术向的学习路线
    
4.  看了Web3实习手册的入门导读以及区块链岗位全景图还有智能合约开发的一部分，还在学习。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
