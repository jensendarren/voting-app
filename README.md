# Simple Voting App on Blockchain

Based on [this tutorial](https://medium.com/@mvmurthy/full-stack-hello-world-voting-ethereum-dapp-tutorial-part-1-40d2d0d807c2).

## Setup development environment

1. Init a new blockchain using `genesis.json` file: `geth init --datadir=./chaindata/ ./genesis.json`
1. Start the Ethereum virtual machine: `geth --datadir=./chaindata --nodiscover`
1. Attach to the node: `geth attach ipc:./chaindata/geth.ipc`
1. Within the console create a new account: `personal.newAccount();` 
1. Set the etherbase if its not set already: `miner.setEtherbase(eth.accounts[0]);`
1. Now, start a miner on the node: `miner.start(1);`

## Build and deploy a Smart Contract

Compile the contract (in Visual Studio Code there is a plugin available) and then copy the JSON file into the Geth Javascript Console and set to the variable `contractJson`.

Once you have the variable `contractJson` set in the Geth Javascript Console, follow along as below:

```
//Unlock the Ethereum account that you created in the step above
web3.personal.unlockAccount(web3.personal.listAccounts[0],"<your password>", 15000)

//These two lines require the contractJson variable to be set
abiDefinition = JSON.parse(contractJson.abi);
bytecode = "0x" + contractJson.bytecode;

//This creates the contract interface
VotingContract = web3.eth.contract(abiDefinition)
//This deploys the contract bytecode and constructor data to the blockchain
deployedContract = VotingContract.new(['Rama','Nick','Jose'],{data: bytecode, from: web3.eth.accounts[0], gas: 4700000})

//The value for deployedContract.address  maybe 'undefined'. If so you will need to check the output of your Geth node and copy in the address instead:
contractInstance = VotingContract.at(deployedContract.address)
```

## Interact with the contract via a console

Now that you have a contractInstance available in the Geth Javascript Console, you can play around with it!

```
contractInstance.totalVotesFor.call('Rama')
contractInstance.voteForCandidate('Rama', {from: web3.eth.accounts[0]})
contractInstance.totalVotesFor.call('Rama')
contractInstance.totalVotesFor.call('Nick')
```

## Interact with the contract via a custom web application using web3.js