# How to use fe-shared package from BGD Labs

In this tutorial we’ll go through building very simple counter app, where anyone can change the number by interacting with interface. 

Typical web3 web app is like a normal web2 app, but with 2 additional aspects 
- Reading data from RPC and smart contracts instead of API
- Signing data with connected for writes
The issue is these aspects are quite complicated, even with libraries like [ethers.js](https://github.com/ethers-io/ethers.js) and [web3-react](https://github.com/Uniswap/web3-react) it’s still can be very cumbersome to setup wallet connection, signing, transaction history etc because there is no 1 size fit all for all the apps and that’s why we in BGD decided to build `fe-shared` is as set of ready to go flows which can be used as a foundation for web3 apps. 

Fe-shared is very opinionated and more like an architecture model, rather than library. It suppose to handle some complicated parts of the whole flow, but not the whole flow.

## What are we going to build?
To understand why exactly there is always an RPC and wallet. Let’s go through writing very simple smart contract and deploy it on local test net, the steps would be
1. Write a smart contract
2. Deploy to local anvil testnet
3. Generate typescript types 
4. Start local front-end 
5. Integrate typechain types into freshly created app
6. Connect custom code to fe-shared
This way we’ll have an idea of the whole process and can easily tweak smart contract to experiment.

## Smart contract

## The contract
The whole contract is a bit modified example of [SimpleStorage example](https://docs.soliditylang.org/en/develop/introduction-to-smart-contracts.html#storage-example)

Source code looks like this 
```Solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.10;

contract SimpleStorage {
    uint256 storedData;

    function increment() public {
        storedData++;
    }

    function decrement() public {
        if (storedData > 0) {
            storedData--;
        }
    }

    function getCurrentNumber() public view returns (uint256) {
        return storedData;
    }
}

```

As you can see the contract is very simple it has only 1 stored property, which anyone, can either increment or decrement.

