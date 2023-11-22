# CreateAndMintToken

This project contains the program for the submission of the assessment Project: Create and Mint Token.

## Description

The goal of the project is to create my own ERC20 token and deploy it using HardHat or Remix. Once deployed, the contract owner should be able to mint tokens to a provided address and any user should be able to burn and transfer tokens. In this project:

- Only the contract owner is able to mint tokens.
- Any user can transfer tokens.
- Any user can burn tokens.

## Getting Started

### Executing program

In order to run this program, it is recommended to copy the whole code and run it in Remix or HardHat. The code can be copied below:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.23;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

interface TokenInterface {

    function mint(address to, uint256 amount) external;
    function transfer(address to, uint amount) external returns (bool success);
    function burn(uint256 amount) external;
    function getSupply() external view returns (uint);
     
    event Mint(address indexed account, uint256 value);
    event Burn(address indexed account, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 amount);
}

contract Token is TokenInterface {

    uint private _totalSupply;
    string private _tokenName;
    string private _tokenSymbol;
    address private _tokenOwner;

    mapping(address => uint) private _balances;

    constructor(string memory tokenName, string memory tokenSymbol, uint initialSupply) {
        _tokenName = tokenName;
        _tokenSymbol = tokenSymbol;
        _tokenOwner = msg.sender;
        _totalSupply = initialSupply;
        _balances[_tokenOwner] = initialSupply;
    }

   modifier onlyOwner() {
        require(msg.sender == _tokenOwner, "Access Denied.");
        _;
    }

    function mint(address to, uint amount) external onlyOwner {
        require(to != address(0), "Invalid address");

        _totalSupply += amount;
        _balances[to] += amount;
        emit Mint(to, amount);
    }

    function transfer(address to, uint amount) external returns (bool success) {
        require(to != address(0), "Invalid address");
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        
        _balances[msg.sender] -= amount;
        _balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function burn(uint amount) external {
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        
        _balances[msg.sender] -= amount;
        _totalSupply -= amount;
        emit Burn(msg.sender, amount);
    }

    function getSupply() external view returns (uint) {
        return _totalSupply;
    }
    
    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }
    
}
```

After Deploying it in remix, fill the boxes requires and the check the mint, transation function and lastly match it with getBalance.

## Submitted by:

Abhishek Kumar
