- 👋 Hi, I’m @MRGREASU
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
MRGREASU/MRGREASU is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
pragma solidity ^0.7.1;

import "https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/math/SafeMath.sol";
import "https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/payment/Payment.sol";

contract GoldBackedCryptocurrency {
using SafeMath for uint;
using Payment for *;uint public totalSupply;
uint public goldBalance;
address public owner;
mapping(address => uint) public balances;
string public name;
string public symbol;
uint8 public decimals;

constructor() public {
    owner = msg.sender;
    totalSupply = 0;
    goldBalance = 0;
    name = "GoldBackedCryptocurrency";
    symbol = "GBC";
    decimals = 18;
}

function buy(uint amount) public payable {
    require(payable(msg.value), "Payment must be made in Ether");
    require(msg.value.mul(70).div(100) <= amount, "Insufficient balance");
    balances[msg.sender] = balances[msg.sender].add(amount);
    totalSupply = totalSupply.add(amount);
    goldBalance = goldBalance.add(msg.value.mul(30).div(100));
    msg.sender.transfer(msg.value);
}

function sell(uint amount) public {
    require(amount <= balances[msg.sender], "Insufficient balance");
    balances[msg.sender] = balances[msg.sender].sub(amount);
    totalSupply = totalSupply.sub(amount);
    goldBalance = goldBalance.sub(amount.mul(30).div(100));
    owner.transfer(amount);
}
