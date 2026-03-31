# Yield-Optimizer-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract YieldOptimizer {
    IERC20 public token;
    uint256 public totalStaked;
    mapping(address => uint256) public userStaked;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint256 amount) public {
        token.transferFrom(msg.sender, address(this), amount);
        userStaked[msg.sender] += amount;
        totalStaked += amount;
    }

    function withdraw(uint256 amount) public {
        require(userStaked[msg.sender] >= amount, "Insufficient");
        userStaked[msg.sender] -= amount;
        totalStaked -= amount;
        token.transfer(msg.sender, amount);
    }

    // Simulate yield - in real use call external strategy
    function getAPY() public pure returns (uint256) {
        return 12; // 12% APY example
    }
}

interface IERC20 {
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
    function transfer(address to, uint256 amount) external returns (bool);
}
