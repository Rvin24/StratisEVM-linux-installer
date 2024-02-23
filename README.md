# StratisEVM-linux-installer

This repository contains an easy to use installer to run a StratisEVM node on Linux.


## Prerequisites

- Linux 64-Bit
- OpenSSL 3

This installer is specifically built for Linux Ubuntu Operating System. If you use other Operating System please use official guide

## Port Requirement

You can add new firewall rules. This is optional. Add this rules only if your node not running well

- GETH: Allow: 30303/UDP+TCP in+out; Block: 8545/TCP all
- Prysm beacon-chain: Allow: */TCP+UDP out, 13000/TCP in+out, 12000/UDP in+out; Block: 3500/TCP all, 8551/TCP all, 4000/TCP all

## Resource Requirement

Before we're start configurations for this node, create some folder, download all the files needed with type and run this command:
```sh
mkdir stratisEVM
```
```sh
cd stratisEVM
```
```sh
mkdir bin
```
```sh
cd bin
```
```sh
mkdir config
```
```sh
cd config
```
```sh
mkdir testnet
```
You are on /bin/config/testnet directory now, and then run this command to create a file which needed for future step
```sh
openssl rand -hex 32 | tr -d "\n" > "jwtsecret"
```
Now you can back to bin directory
```sh
cd
```
```sh
cd bin
```

After that, download all the required files below and Extract all of them:

- geth : 
```sh
wget https://github.com/stratisproject/go-stratis/releases/download/0.1.1/geth-linux-amd64-5c4504c.tar.gz
```
```sh
tar -xvzf geth-linux-amd64-5c4504c.tar.gz
```
- beacon-chain : 
```sh
wget https://github.com/stratisproject/prysm-stratis/releases/download/0.1.1/beacon-chain-linux-amd64-0ebd251.tar.gz
```
```sh
tar -xvzf beacon-chain-linux-amd64-0ebd251.tar.gz
```
- validator :
```sh
wget https://github.com/stratisproject/prysm-stratis/releases/download/0.1.1/validator-linux-amd64-0ebd251.tar.gz
```
```sh
tar -xvzf validator-linux-amd64-0ebd251.tar.gz
```
- Staking Deposit CLI :
```sh
wget https://github.com/stratisproject/staking-deposit-cli/releases/download/0.1.0/staking-deposit-cli-linux-amd64.zip
```
```sh
unzip staking-deposit-cli-linux-amd64.zip
```
If you face the error when unzip the file above, run this command:
```sh
apt-get install unzip
```
Then run the unzip command again

## Configuration and Instalation
Before we start configure and instalations, you have to know this node needs run 3 programs together. So you must create 1 screen for 1 program each. We start from that

### Run the Execution Client
type 'screen' and enter twice. Now you are in screen 1
run this command :
```sh
./geth --auroria --http --http.api eth,net,engine,admin --datadir=data\testnet\geth --authrpc.addr=127.0.0.1 --authrpc.jwtsecret=jwtsecret --syncmode=full
```
Congrats !! you're done. The execution client now running BUT needs other process to be running well and fully syncing. Next step is running the Beacon node

### Run a Beacon Node
type 'screen' and enter twice again. Now you are in screen 2
run this command :
```sh
./beacon-chain --auroria --datadir=data\testnet\beacon --execution-endpoint=http://localhost:8551 --jwt-secret=jwtsecret
```
Congratulations - you’re now running a full StratisEVM node.

### Setup Validator to Stake
Go through to this link to claim faucet
[FAUCET](https://auroria.faucet.stratisevm.com/)

After that, you can go to this link to generate keys for staking
[Generate Keys (Auroria)](https://auroria.launchpad.stratisevm.com/en/generate-keys)

type 'screen' and enter twice again. Now you are in screen 3
Please review and accept all the descriptions on the web untill you get this command:
```sh
./deposit new-mnemonic --num_validators=1 --mnemonic_language=english --chain=auroria --eth1_withdrawal_address=<INSERT ADDRESS>
```
`--eth1_withdrawal_address is your EVM address`
`insert your password if needed and remember it!`


1.  A **new mnemonic seed phrase**. This is **highly sensitive** and should never be exposed to other people or networked hardware.
2.  A `validator_keys` folder. This folder will contain two files:
    1.  `deposit_data-*.json` - contains deposit data that you’ll later upload to the Stratis launchpad.
    2.  `keystore-m_*.json` - contains your public key and encrypted private key.

After that, run this command:
./validator accounts import --keys-dir=<YOUR_FOLDER_PATH> --auroria

YOUR_FOLDER_PATH is you validator keys folder which previously generated. The Default is `validator_keys`

You’ll be prompted to specify a wallet directory provide the path to your future wallets directory.
Now, you are ready for next step

Back to Stratis [Staking Launchpad](https://auroria.launchpad.stratisevm.com/en/generate-keys) site and continue through the prompts to upload your `deposit_data-*.json` file. You’ll be prompted to connect your wallet.

Now, run this command for start run the validator:
```sh
./validator --wallet-dir=<YOUR_FOLDER_PATH> --auroria --suggested-fee-recipient=<YOUR_WALLET_ADDRESS>
```

Edit the --wallet-dir with your wallet directory which generated before. The default is `/root/.eth2validators/prysm-wallet-v2`
Edit the --suggested-fee-recipient with your `EVM Address`

Congratulations !! Now your node are running well. Some error may happens with some peoples so if you need more help you can answer here

