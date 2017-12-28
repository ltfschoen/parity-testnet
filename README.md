# Parity Node using Kovan and Ropsten Test Networks

## Setup Kovan Test Network

* Create kovan-config.toml

* Create a signer account for use by the Miner in the Kovan Network. Entered and confirmed a fake password when prompted. It returns the account address (i.e. 0x983696f4594180ab677e76835834f871b21fe5b2) and creates a key in testnet/keys/kovan

```
parity account new --chain kovan --keys-path "/Users/Ls/Library/Application Support/io.parity.ethereum/testnet/keys" --db-path "Users/Ls/Library/Application Support/io.parity.ethereum/testnet/chains"
```

  * Note: Running the above avoids error:

```
Consensus signer account not found for the current chain. You can create an account via RPC, UI or `parity account new --chain kovan --keys-path /Users/Ls/Library/Application Support/io.parity.ethereum/testnet/keys`.
```

* Replace relevant addresses for Operation Options and Miner Options in kovan-config.toml with the account address that was generated and ensure that use `/Users/Ls` instead of `$HOME` for the path value. See https://github.com/paritytech/parity/issues/7392

* Create Log file and Passwords file for Kovan Testnet
  
```
mkdir -p "$HOME/Library/Application Support/io.parity.ethereum/testnet/log/";
touch "$HOME/Library/Application Support/io.parity.ethereum/testnet/log/parity.log";

mkdir -p "$HOME/Library/Application Support/io.parity.ethereum/testnet/passwords";
touch "$HOME/Library/Application Support/io.parity.ethereum/testnet/passwords/node.pwds";
echo 'test123456' >> "$HOME/Library/Application Support/io.parity.ethereum/testnet/passwords/node.pwds";
```

* Run the Parity Node with the custom configuration that uses the Kovan Network 

  * UI version. Change UI Options to `force = true`

```
parity ui --config ./kovan-config.toml --ui-no-validation
```

  * UI version without no validation flag (NOT WORKING). See https://github.com/paritytech/parity/issues/7393

```
parity ui --config ./kovan-config.toml -lrpc=trace
```

  * It prompts you with the following:

    > Unable to make a connection to the Parity Secure API. To update your secure token or to generate a new one, run parity signer new-token and paste the generated token into the space below. Ensure that both the Parity node and this machine connecting have computer clocks in-sync with each other and with a timestamp server, ensuring both successful token validation and block operations.

  * Generate a Token from your computer and paste the output into the browser

```
parity signer new-token --config /Users/Ls/code/blockchain/parity-docker/kovan-config.toml --db-path "Users/Ls/Library/Application Support/io.parity.ethereum/testnet/chains"
```

  * NOTE: This does not work, it just shows warnings that indicate the an invalid token was provided.

  * CLI version

```
parity --config ./kovan-config.toml -lrpc=trace
```

* Acquire Kovan Ether https://github.com/kovan-testnet/config

* Kill Kovan Network Blockchain Database `parity --chain kovan db kill`

## Setup Ropsten Test Network

* Create ropsten-config.toml

* Add Ropsten peers to ropstenpeers.txt (to allow connection to peers for syncing) - https://github.com/ethereum/ropsten

```
echo -e "enode://20c9ad97c081d63397d7b685a412227a40e23c8bdc6688c6f37e97cfbc22d2b4d1db1510d8f61e6a8866ad7f0e17c02b14182d37ea7c3c8b9c2683aeb6b733a1@52.169.14.227:30303\nenode://6ce05930c72abc632c58e2e4324f7c7ea478cec0ed4fa2528982cf34483094e9cbc9216e7aa349691242576d552a2a56aaeae426c5303ded677ce455ba1acd9d@13.84.180.240:30303" >> ropstenpeers.txt
```

* Create a signer account for use by the Miner in the Ropsten Network. Entered and confirmed a fake password when prompted. It returns the account address (i.e. 0x56183a35c057f32dc71307dec8b8c60da055eae2) and creates a key in testnet/keys/ropsten

```
parity account new --chain ropsten --keys-path "/Users/Ls/Library/Application Support/io.parity.ethereum/testnet/keys" --db-path "Users/Ls/Library/Application Support/io.parity.ethereum/testnet/chains"
```

* Replace relevant addresses for Operation Options and Miner Options in kovan-config.toml with the account address that was generated

* Remove `engine_signer = "0x56183a35c057f32dc71307dec8b8c60da055eae2"` since cannot set engine signer on a PoW chain

* Run the Parity Node with the custom configuration that uses the Ropsten Network 

  * UI version

```
parity ui --config ./kovan-config.toml --ui-no-validation -lrpc=trace
```

  * CLI version

```
parity --config ./ropsten-config.toml -lrpc=trace
```

  * Note: It will start synchronising with the Ropsten Network having ~2.3k blocks,
  where each 100k blocks use up about 0.8Gb of hard drive space in:
    * /Users/Ls/Library/Application Support/io.parity.ethereum/testnet/chains/test/db

    * Check if client is on revived chain by comparing to the block numbers at:
      * http://pool.ropsten.ethereum.org
      * http://ropsten.etherscan.io

  * [ ] - TODO - Setup EthMiner 
    * https://medium.com/@joshua_e_k/setting-up-ethereum-smart-contract-development-using-parity-on-ubuntu-abca4da3dce2

## References:

* Parity Basic Usage - https://github.com/paritytech/parity/wiki/Basic-Usage
* Parity Configuration - https://github.com/paritytech/parity/wiki/Configuring-Parity
* Parity Testnet CLI Example - https://github.com/paritytech/parity/issues/4618
* Parity Setting up two nodes witn PoA - https://github.com/paritytech/parity/wiki/Demo-PoA-tutorial

* Common
  * Etherscan TestNets - https://testnet.etherscan.io/

* Kovan - 
  * https://github.com/kovan-testnet/proposal
  * Kovan TestNet Config - https://github.com/kovan-testnet/config
  * https://kovan.etherscan.io/
* Ropsten - https://ropsten.etherscan.io/
* Rinkeby
  * Rinkeby Testnet - http://rinkeby.io
    * Crypto Faucey - Obtain Test Ether
  * https://rinkeby.etherscan.io/



