---
layout: nodes.liquid
date: Last Modified
title: "Fulfilling Requests"
permalink: "docs/fulfilling-requests/"
whatsnext: {"Performing System Maintenance":"/docs/performing-system-maintenance/", "Miscellaneous":"/docs/miscellaneous/", "Best Security and Operating Practices":"/docs/best-security-practices/"}
metadata: 
  title: "Chainlink Node Operators: Fulfilling Requests"
  description: "Deploy your own Oracle contract and add jobs to your node so that it can provide data to smart contracts."
---
With your own Oracle contract, you can use your own node to fulfill requests. This guide will show you how to deploy your own Oracle contract and add jobs to your node so that it can provide data to smart contracts.

## Prerequisites

- Going through the [Beginner Walkthrough](../beginners-tutorial) will help you obtain Testnet LINK and set up Metamask- [Run an Ethereum Client](../run-an-ethereum-client/) - [Running a Chainlink Node](../running-a-chainlink-node/)

> 👍 Make sure to fund your node address!
> 
> In the `keys` tab, you'll see at the bottom `Account Addresses`. The address of your node is the `regular` one. 
> 
> IN OLDER VERSIONS: Go to `configuration` in your node. You'll see `ACCOUNT_ADDRESS`. This is the address of your node. Send this address ETH. You can find testnet ETH on various [faucets](../link-token-contracts/). If you don't see `ACCOUNT_ADDRESS` there, check the 'Keys' tab and scroll down

## Deploy your own Oracle contract

- Go to <a href="https://remix.ethereum.org/#gist=1d2cb55e777589e59847bc60ebabb005&optimize=true&version=soljson-v0.4.24+commit.e67f0147.js" target="_blank" rel="noreferrer, noopener">Remix</a> and expand the gist menu

![Remix File Explorer](/files/05f12f3-00eeef4-remix001.jpg)

> 📘 Important
> 
> If you open Remix for the first time, you should chose the **Environments** to **Solidity**

- Click on Oracle.sol. The contents of this file will be very minimal, since we only need to import the code hosted on Github.- On the Compile tab, click on the "Compile Oracle.sol" button near the left

[block:image]
{
  "images": [
    {
      "image": [
        "/files/6aa5936-remix002.jpg",
        "remix002.jpg",
        836,
        1382,
        "#e9edf2"
      ]
    }
  ]
}
[/block]
- Change to the Run tab- Select Oracle from the drop-down in the left panel- Copy the line below for your network and paste it into the text field next to the Deploy button

``` text Rinkeby
0x01BE23585060835E02B77ef475b0Cc51aA1e0709
```
``` text Kovan
0xa36085F69e2889c224210F603D836748e7dC0088
```
``` text Mainnet
0x514910771AF9Ca656af840dff83E8264EcF986CA
```

[block:image]
{
  "images": [
    {
      "image": [
        "/files/b9d3620-remix004.jpg",
        "remix004.jpg",
        1216,
        1716,
        "#f4f4f5"
      ]
    }
  ]
}
[/block]
- Click DeployMetamask will prompt you to Confirm the Transaction

> 🚧 Metamask doesn't pop up?
> 
> If Metamask does not prompt you and instead displays the error below, you will need to disable "Privacy Mode" in Metamask. You can do this by clicking on your unique account icon at the top-right, then go to the Settings. Privacy Mode will be a switch near the bottom. 
> 
> The error: **Send transaction failed: invalid address. If you use an injected provider, please check it is properly unlocked.**

A link to Etherscan will display at the bottom, you can open that in a new tab to keep track of the transaction

[block:image]
{
  "images": [
    {
      "image": [
        "/files/b6fe1ac-remix005.jpg",
        "remix005.jpg",
        1486,
        174,
        "#e9eaf0"
      ]
    }
  ]
}
[/block]

Once successful, you should have a new address for the deployed contract

[block:image]
{
  "images": [
    {
      "image": [
        "/files/6858cf3-remix006.jpg",
        "remix006.jpg",
        1072,
        170,
        "#f7f8fa"
      ]
    }
  ]
}
[/block]

> 📘 Important
> 
> Keep note of the Oracle contract's address, you will need it for your consuming contract later.

## Add your node to the Oracle contract

- In Remix, call the `setFulfillmentPermission` function with the address of your node, a comma, and the value `true`, as the input parameters. This will allow your node the ability to fulfill requests to your oracle contract.

[block:image]
{
  "images": [
    {
      "image": [
        "/files/c6925db-remix007.jpg",
        "remix007.jpg",
        1302,
        1542,
        "#eae7e4"
      ]
    }
  ]
}
[/block]
You can get the address of your node when it starts or by looking at one of the following places.

1. Bottom of the Keys page in the Account addresses section on recent versions of Chainlink.
![Screenshot of account addresses](/images/node-operators/node-address.png)

1. Configurations page on older Chainlink node versions
![Screenshot of configuration page from 2018](/files/d2e5225-Screenshot_from_2018-12-17_08-23-16.png)


Once you call the `setFulfillmentPermission` function, Confirm it in Metamask and wait for it to confirm on the blockchain.

## Add jobs to the node

Adding jobs to the node is easily accomplished via the GUI. We have example [Job Specifications](../job-specifications/) below.

> ❗️ Required
> 
> Replace `YOUR_ORACLE_CONTRACT_ADDRESS` below with the address of your deployed oracle contract address from the previous steps. Note that this is different than the `ACCOUNT_ADDRESS` from your node.

If using Chainlink version `0.9.4` or above, you can add a `name` to your job spec. 

```json EthBytes32 (GET)
{ 
  "name": "Get > Bytes32",
  "initiators": [
    {
      "type": "runlog",
      "params": {
        "address": "YOUR_ORACLE_CONTRACT_ADDRESS"
      }
    }
  ],
  "tasks": [
    {
      "type": "httpget"
    },
    {
      "type": "jsonparse"
    },
    {
      "type": "ethbytes32"
    },
    {
      "type": "ethtx"
    }
  ]
}
```
```json EthBytes32 (POST)
{ 
  "name": "Get > Bytes32",
  "initiators": [
    {
      "type": "runlog",
      "params": {
        "address": "YOUR_ORACLE_CONTRACT_ADDRESS"
      }
    }
  ],
  "tasks": [
    {
      "type": "httpget"
    },
    {
      "type": "jsonparse"
    },
    {
      "type": "ethbytes32"
    },
    {
      "type": "ethtx"
    }
  ]
}
```
```json EthInt256
{
  "name": "Get > Int256",
  "initiators": [
    {
      "type": "runlog",
      "params": {
        "address": "YOUR_ORACLE_CONTRACT_ADDRESS"
      }
    }
  ],
  "tasks": [
    {
      "type": "httpget"
    },
    {
      "type": "jsonparse"
    },
    {
      "type": "multiply"
    },
    {
      "type": "ethint256"
    },
    {
      "type": "ethtx"
    }
  ]
}
```
```json EthUint256
{
  "name": "Get > Uint256",
  "initiators": [
    {
      "type": "runlog",
      "params": {
        "address": "YOUR_ORACLE_CONTRACT_ADDRESS"
      }
    }
  ],
  "tasks": [
    {
      "type": "httpget"
    },
    {
      "type": "jsonparse"
    },
    {
      "type": "multiply"
    },
    {
      "type": "ethuint256"
    },
    {
      "type": "ethtx"
    }
  ]
}
```
```json EthBool
{
  "name": "Get > Bool",
  "initiators": [
    {
      "type": "runlog",
      "params": {
        "address": "YOUR_ORACLE_CONTRACT_ADDRESS"
      }
    }
  ],
  "tasks": [
    {
      "type": "httpget"
    },
    {
      "type": "jsonparse"
    },
    {
      "type": "ethbool"
    },
    {
      "type": "ethtx"
    }
  ]
}
```

- From the admin dashboard, click on New Job.

[block:image]
{
  "images": [
    {
      "image": [
        "/files/25bb51c-Screenshot_from_2018-11-02_08-30-26.png",
        "Screenshot from 2018-11-02 08-30-26.png",
        123,
        71,
        "#edeef6"
      ]
    }
  ]
}
[/block]

- Paste the job from above into the text field.

[block:image]
{
  "images": [
    {
      "image": [
        "/files/ee412d6-Screenshot_from_2018-12-15_08-30-43.png",
        "Screenshot from 2018-12-15 08-30-43.png",
        1291,
        823,
        "#faf9fa"
      ]
    }
  ]
}
[/block]

- Click Create Job and you'll be notified of the new JobID creation. Take note of this JobID as you'll need it later.

[block:image]
{
  "images": [
    {
      "image": [
        "/files/a7a2f0c-Screenshot_from_2018-11-02_08-42-22.png",
        "Screenshot from 2018-11-02 08-42-22.png",
        631,
        54,
        "#15bf5f"
      ]
    }
  ]
}
[/block]

- Repeat this process for each of the jobs above.

> 📘 Note
> 
> The address for the jobs will auto-populate with the zero-address. This means the Chainlink node will listen to your Job ID from any address. You can also specify an address in the params object of the RunLog initiator for your oracle contract.

## Create a request to your node

> 📘 Important
> 
> If you're going through this guide on Ethereum mainnet, the TestnetConsumer.sol contract will still work. However, understand that you're sending real LINK to yourself. **Be sure to practice on the test networks multiple times before attempting to run a node on mainnet.**

With the jobs added, you can now use your node to fulfill requests. This last section shows what requesters will do when they send requests to your node. It is also a way to test and make sure that your node is functioning correctly.- In Remix, create a new file named TestnetConsumer.sol and copy and paste the <a href="https://gist.githubusercontent.com/thodges-gh/8df9420393fb29b216d1832e037f2eff/raw/350addafcd19e984cdd4465921fbcbe7ce8500d4/ATestnetConsumer.sol" target="_blank" rel="noreferrer, noopener">TestnetConsumer.sol</a> contract into it.- Click "Start to compile".

[block:image]
{
  "images": [
    {
      "image": [
        "/files/a8a5ecd-Screenshot_from_2019-05-16_15-39-05.png",
        "Screenshot from 2019-05-16 15-39-05.png",
        473,
        433,
        "#f0f2f8"
      ]
    }
  ]
}
[/block]
The contract should compile. You can now deploy it and fund it by sending some LINK to its address. See the [Fund your contract.](../fund-your-contract/) page for instructions on how to do that.- To create a request, input your oracle contract address and the JobID for the EthUint256 job into the `requestEthereumPrice` request method, separated by a comma.

[block:image]
{
  "images": [
    {
      "image": [
        "/files/dfce3b0-Screenshot_from_2019-05-16_17-12-51.png",
        "Screenshot from 2019-05-16 17-12-51.png",
        831,
        38,
        "#eae7eb"
      ]
    }
  ]
}
[/block]
- Click the request method button and you should see your node log the request coming in and its fulfillment.Once the transaction from the node is confirmed, you should see the value updated on your contract by clicking on the `currentPrice` button.
[block:image]
{
  "images": [
    {
      "image": [
        "/files/6741635-Screenshot_from_2019-05-15_15-38-25.png",
        "Screenshot from 2019-05-15 15-38-25.png",
        228,
        99,
        "#eaebf2"
      ]
    }
  ]
}
[/block]

## Withdrawing LINK

To withdraw LINK from the Oracle contract, head to Remix and search for the "withdraw" function in the function list.
[block:image]
{
  "images": [
    {
      "image": [
        "/files/f8ffdc0-c6925db-remix007.jpg",
        "c6925db-remix007.jpg",
        567,
        672,
        "#e9e2de"
      ],
      "caption": "withdraw function in Oracle contract."
    }
  ]
}
[/block]
Paste the address you want to withdraw to, and the amount of LINK, then click "withdraw". Confirm the transaction in Metamask when the popup appears.