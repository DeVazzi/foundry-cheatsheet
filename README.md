# Foundry Cheatsheet

Alpha to the people

![Foundry Cheatsheet](foundry.png)

Foundry GitHub - [https://github.com/foundry-rs](https://github.com/foundry-rs)  
Foundry Book - [https://book.getfoundry.sh/](https://book.getfoundry.sh/)  
Installing Foundry - [https://github.com/foundry-rs/foundry#installation](https://github.com/foundry-rs/foundry#installation)

## cast

interacting with a read function / fetching public variables

```sh
cast call 0x3Bf196D4C9cB99f6A1F6919Fb4a3be0D7b2C3bfb "owner()"
```

sending tx

```sh
cast send 0x08646dd622e2e2f668d6964aa9e6d05b7561c4ac "prankTransferFrom()" --rpc-url http://127.0.0.1:8545 --private-key $PRIVATE_KEY
```

Resimulate a boradcasted tranasction

```sh
cast run $TXID --rpc-url $RPC_URL
```

Get contract source code from etherscan

```
cast etherscan-source $ADDRESS -d ./src
```

Decode a raw TX

```sh
cast --from-rlp $TXRAWDATA
```

get balance of an address

```sh
cast balance vitalik.eth
```

conver wei to ether

```sh
cast --to-unit 1000000000000000 ether
```

Calculating the 4 bytes hash (signature) of a function.

```sh
cast sig "setApprovalForAll(address,bool)"
```

Output: `0xa22cb465`

Get storage slot of a contract at slot 0

```sh
cast storage 0x7ceb23fd6bc0add59e62ac25578270cff1b9f619 0
```

Output: `0x0000000000000000000000000000000000000000000000000000000000000000`

## forge

Creating a contract

```sh
forge create --rpc-url $GOERLI_RPC_URL --private-key $PRIVATE_KEY ./src/MyERC20.sol:MyERC20 --etherscan-api-key $ETHERSCAN_API_KEY --verify
```

Solidity scripting

```sh
forge script ./script/Force.s.sol:ForceScript --private-key $PLAYER --rpc-url $GOERLI_RPC_URL --broadcast
```

viewing storage slots

```
forge inspect <path>:<contractName> storage
```

Viewing storage slots of a contract code

```sh
forge inspect ./src/MyContract.sol storage --pretty
```

Get contract sizes for the enitre project

```
forge build --sizes
```

## anvil

Forking a live network

```sh
anvil fork --rpc-url $GOERLI_RPC_URL
```

Fork at a certain block

```
anvil fork --rpc-url $GOERLI_RPC_URL --fork-block-number 123456
```

## Cheat Codes

while testsing we can use cheatcodes in order to test against various cases

### fuzzing

we can simple fuzz tests by inputing parameters to our test functions for example:

```solidity
function testFunc(uint256 _x) public {
    counter.setNumber(x)
}
```

- that way the fuzzer will run the test using values in the range of uint256
- in order to reject some values we can use: `vm.assumes` i.e:

```solidity
vm.assume (x > 100)
```

that way any value above 100 will be rejected

another way which is smarter is by using the bound cheatcode:

```solidity
bound(x, 100, 200)
```

that way we enforcing x to be between 100 and 200 without rejecting any values

## invaraint tests

### write to file

```solidity
vm.writeLine("x.txt", x)
```
