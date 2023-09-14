# Polkadot & Rise In Bootcamp - Final Project

Hello, welcome to the repo of the **Flipper Application** that I developed on the Polkadot network with Rust language.

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Building Blockchain](#building-blockchain)
- [Simulating a Substrate Network](#simulating-a-substrate-network)
- [Adding Trusted Nodes To a Network](#adding-trusted-nodes-to-a-network)
- [Smart Contracts](#smart-contracts)
- [License](#license)

## Overview

The overview is updating..

## Getting Started

Follow these steps to set up the project locally and start participating in web3 auctions.

### Prerequisites

1. Node.js: You need a package manager to download packages via the terminal.
   We will use node and yarn in this project .
[nodejs.org](https://nodejs.org/)
[yarn](https://nodejs.org/)
3. VS Code: You need an editor to edit the codes locally.
[VS Code](https://code.visualstudio.com/)
4. And finally, we will use brew for easier downloading of packages. (Optional)
[Homebrew](https://brew.sh/)

### Installation

1. As I mentioned before, we install homebrew, which is the package manager.Open terminal on your Mac and enter the following command
```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
2. You can use the following command to check if Brew is installed or not.
```bash
  brew --version
```
PS. If you already have brew and are using an old version, you can update brew with the following command.
```bash
  brew --update
```
3. Since substrate blockchains require standard cryptography to support the generation of private and public keys pairs, we need to install the openssl package.
```bash
 brew install openssl
```
3. Now we can go ahead and install the rustup package that manages the various versions of Rust for us.
```bash
 curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
5. We configure the shell you downloaded to include the cargo.
```bash
 source ~/.cargo/env
```
6. Verify your rustc installation by running the command below.
```bash
 rustc --version
```
7. Configure the Rust toolchain to default to the latest stable version by running the following commands.
```bash
 rustup default stable
```
```bash
 rustup update
```
8. Add the nightly release and the nightly WebAssembly (wasm) targets to your development environment by running the following commands.
```bash
 rustup update nightly
```
```bash
 rustup target add wasm32-unknown-unknown --toolchain nightly
```
9. And finally verify the configuration of your development environment by running the following command.
```bash
 rustup show
```
```bash
 rustup +nightly show
```
## Building Blockchain

1. Compile and starting a substrate node. Start a terminal in the relevant directory for your project.

```bash
 git clone https://github.com/substrate-developer-hub/substrate-node-template
```

2. With the `cd substrate-node-template` command, go to the directory where you just installed the project.

3. Sometimes things you do may not go right and you may want to start over. That's why we will open a separate branch and develop our project there. This way, if there is any error, we can compare it with the original file.
```bash
 git switch -c my-new-branch
```
4. To start the node, we first need to build and compile this project.

PS. We will use the `--release` flag to build optimized artifacts.
```bash
 cargo build --release
```
5. We run our node with the following command.
```bash
 ./target/release/node-template --dev
```
PS. The `–dev` flag indicates that we want the node to run in development mode. This option is great during development because it deletes all active data such as keys, blockchain database and networking information when you stop the node.

#### Install and starting front-end template
Substrate also has a front end template for the dashboard so you can interact with the node through a UI and not just through the terminal.

For the front end, we will use node and yarn that I told you about before.
Since Front End is developed for javascript, we need to use the npm repository.


1. We start by checking whether the javascript and Yarn we installed before are installed.
```bash
node --version
```
```bash
yarn --version
```
2. You may remember that we cloned the project for the backend earlier. Now we will clone the project for the front end.
```bash
git clone https://github.com/substrate-developer-hub/substrate-front-end-template
```
We now need to change our directory to the project we just cloned.Go to the directory where the front end is installed using the `cd substrate-front-end-template` command.

2. Now we will install the project using yarn.
```bash
yarn install
```
2. And now we just have to start the front end with the following command.
```bash
yarn start
```
PS. http://localhost:8000/ will automatically be opened in your default browser once the front end starts, but you can also open it manually. Now from the dashboard, you will be able to interact with the node.

## Simulating a Substrate Network
In the previous section we started a single substrate node. But in reality, nodes don’t operate in isolation and they are usually part of a network.
In this section, let’s simulate a substrate network by adding multiple nodes to the network and see how they interact.

1. Start a terminal, change directory into the project and run the command below to purge old chain data 
```bash
./target/release/node-template purge-chain --base-path /tmp/alice --chain local
```
2. Starting the node
```bash
./target/release/node-template 
--base-path /tmp/alice 
--chain local 
--alice 
--port 30333 
--ws-port 9945 
--rpc-port 9933 
--node-key 0000000000000000000000000000000000000000000000000000000000000001 
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
--validator
```
3. Adding another node

We will open up a new terminal to start and interact with the second node and purge old chain data with the following command.
```bash
./target/release/node-template purge-chain --base-path /tmp/alice --chain local
```
This time we join the node using the Bob account.
```bash
./target/release/node-template 
--base-path /tmp/bob 
--chain local 
--bob 
--port 30334 
--ws-port 9946 
--rpc-port 9934 
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
--validator 
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp
```
3. Verify Block Production

If you see the same blocks in the terminal window as in the image below, it means everything is going right.
You can even see the expression showing that two nodes match each other in the peer section.

## Adding Trusted Nodes To a Network
We will create a complete permissioned blockchain network.

1. Generate account and keys

PS. It will ask you to enter a password and you can do that now.
```bash
./target/release/node-template key generate --scheme Sr25519 --password-interactive
```
PS. This command will generate a seed phrase that you can save for later reference.

You can now use the seed phrase to derive keys using the Ed25519 signature scheme.
```bash
 ./target/release/node-template key inspect --password-interactive --scheme Ed25519 "pig giraffe ceiling enter weird liar orange decline behind total despair fly"
```
2. Create a custom chain specification

After you generate the keys to use with your blockchain, you are ready to create a custom chain specification using those key pairs then share your custom chain specification with trusted network participants called validators.

Cd into the folder where you have compiled the node and export the local chain specification to a file named customSpec.json with the following command
```bash
./target/release/node-template build-spec --disable-default-bootnode --chain local > customSpec.json
```

We will make some changes to the customSpec.json file, so open it up in a text editor.

Modify the name field, for example:
```bash
"name": "My Custom Testnet",
```

Modify the aura field to these values,
```bash
 "aura": { "authorities": [
"5CfBuoHDvZ4fd8jkLQicNL8tgjnK8pVG9AiuJrsNrRAx6CNW", 
"5CXGP4oPXC1Je3zf5wEDkYeAqGcGXyKWSRX2Jm14GdME5Xc5"
]},
```

Modify the grandpa field to these values
```bash
"grandpa": {
"authorities": [
["5CuqCGfwqhjGzSqz5mnq36tMe651mU9Ji8xQ4JRuUTvPcjVN",1],
["5DpdMN4bVTMy67TfMMtinQTcUmLhZBWoWarHvEYPM4jYziqm",1]
]},
```
PS. What we have done is, we’ve added address keys in the aura field for the validator nodes that can create blocks and we’ve added address keys in the grandpa field for the validator nodes that have the authority to finalize blocks.

In this way we can specifically define which nodes can do what in the network.
After you prepare a chain specification with the validator information, you must convert it into a raw specification format before it can be used.

To convert a chain specification to use the raw format:

- Open a terminal shell on your computer.
- Change to the root directory where you compiled the Substrate node template.
- Convert the customSpec. json chain specification to the raw format with the file name customSpecRaw.json by running the following command:
```bash
./target/release/node-template build-spec --chain=customSpec.json --raw --disable-default-bootnode > customSpecRaw.json
```
3. Start the first node
```bash
./target/release/node-template 
--base-path /tmp/node01 
--chain ./customSpecRaw.json 
--port 30333 
--ws-port 9945 
--rpc-port 9933 
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
--validator 
--rpc-methods Unsafe 
--name MyNode01 
--password-interactive
```
PS. You will now be asked for a password, use the same password that you used to generate the keys.

4. Adding keys to keystore

For each node:
- Add the aura authority keys to enable block production.
- Add the grandpa authority keys to enable block finalization.

There are several ways you can insert keys into the keystore. For this tutorial, you can use the key subcommand to insert locally-generated secret keys

```bash
./target/release/node-template key insert --base-path /tmp/node01 
--chain customSpecRaw.json 
--scheme Sr25519 
--suri <your-secret-seed> 
--password-interactive 
--key-type aura
```
PS. Replace `<your-secret-seed>` with the secret phrase or secret seed for the first key pair that you generated.

Insert the grandpa secret key generated from the key subcommand by running a command similar to the following.
```bash
./target/release/node-template key insert 
--base-path /tmp/node01 
--chain customSpecRaw.json 
--scheme Ed25519 
--suri <your-secret-key> 
--password-interactive 
--key-type gran
```
Again, replace `<your-secret-key>` and type in the password after you are prompted for the same.

PS. After this, an important step is to restart the node once you have entered the grandpa key as substrate nodes require a restart at this point.

5. Enable other participants to join

Now we have to allow other validators to join the network.
In the previous section, we have one of our authorized nodes with aura and grandpa keys, so now we can start another node.

Run the following command to start the second node.
```bash
./target/release/node-template 
--base-path /tmp/node02 
--chain ./customSpecRaw.json 
--port 30334 
--ws-port 9946 
--rpc-port 9934 
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
--validator 
--rpc-methods Unsafe 
--name MyNode02 
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX 
--password-interactive
```

Add aura key
```bash
./target/release/node-template key insert --base-path /tmp/node02 
--chain customSpecRaw.json 
--scheme Sr25519 
--suri <second-participant-secret-seed> 
--password-interactive 
--key-type aura
```
PS. Replace `<second-participant-secret-seed>` with the secret phrase or secret seed that you generated. You will be prompted for a password, so we need to enter it.

Now add the grandpa secret key
```bash
./target/release/node-template key insert --base-path /tmp/node02 
--chain customSpecRaw.json 
--scheme Ed25519 --suri <second-participant-secret-seed> 
--password-interactive 
--key-type gran
```
Again replace the secret seed and enter the password.

Just like the previous node, we now have to restart this node as well since we’ve entered the grandpa key.

You can use this command to restart.
```bash
./target/release/node-template 
--base-path /tmp/node02 
--chain ./customSpecRaw.json 
--port 30334 
--ws-port 9946 
--rpc-port 9934 
--telemetry-url 'wss://telemetry.polkadot.io/submit/ 0' 
--validator 
--rpc-methods Unsafe 
--name MyNode02 
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX 
--password-interactive
```

After both nodes have added their keys to their respective keystores `located under /tmp/node01` and `/tmp/node02` and been restarted, you should see the same genesis block and state root hashes.

## Smart Contracts

1. Environment setup
Check whether the `rustc compiler` is installed, 
if not, install it with the 3 commands below.

```bash
rustup target add wasm32-unknown-unknown --toolchain nightly
```

Add rust-src compiler component
```bash
rustup component add rust-src
```
Install the latest version of cargo-contract
```bash
cargo install --force --locked cargo-contract --version 2.0.0-rc
```
Verify the installation and explore the commands available
```bash
cargo contract --help
```

2. Creating a Smart Contract

Specify a directory and create a new project with the `cargo contract new flipper` command
Test it with `cargo test`
Build it the contract with `cargo contract build`

3. Deploy the Smart Contract

Run `substrate-contracts-node`
Run `./substrate-contracts-node --log info,runtime::contracts=debug 2>&1`

Go to the flipper project folder and build the contract using cargo contract build


```bash
cargo contract instantiate --constructor new --args "false" --suri //Alice --salt $(date +%s)
```



4. Interact with the Smart Contract

```bash
cargo contract call --contract 5HciRHsxpEWfJLGXwe4CHNEpyveJ9T4BtS5TKvJBMUfztrtu --message get --suri //Alice --dry-run
```

For the flip function in the smart contract
```bash
cargo contract call --contract 5HciRHsxpEWfJLGXwe4CHNEpyveJ9T4BtS5TKvJBMUfztrtu --message flip --suri //Alice
```

## License

This project is licensed under the [MIT License](LICENSE).

---

Thank you for your interest! For questions or suggestions, reach out to us or open an issue on [GitHub](#). 
Happy building on the blockchain! 🚀
