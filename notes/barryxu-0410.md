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
