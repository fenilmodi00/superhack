# Bochamon

_Fully on-chain turn-based_ game with multi-chain rewards. Bochamon is an _Autonomous World_ that can be deployed in any OP rollup and its rewards can be bridged between all the supported chains by _Layer-Zero_.

![bochamon banner](banner.png)

## Description

This project combines _Pokemon-like_ games and _Blockchain_.

While playing this game, your current position on the map is stored in the blockchain and _updated after every move_. You can pick your Bochamons and _set up your team_ (also registered in the blockchain), and you can _fight other players_ (every action is also a blockchain transaction).

After winning a duel, you get _rewarded in BochaCoins_. These coins are deployed as an ERC20 contract in OP Mainnet, and we set up a bridge to Base with the Layer-Zero. Talking with the Exchange NPC allows you to _bridge_ the coins!

Our initial idea was to deploy it also to _Base Mainnet_ and _Zora_, so you can move your BochaCoins between all the chains, but the cost of deploying to Mainnet the contracts was too high.

## How It's Made

The first step to build this project was deciding which library to use for creating the game contracts. We already have experience using _MUD_, so we decided to use it for this project. It also allows us to use the open-source library _Garnet_ to create the backend that integrates automatically with MUD.

With the idea of building the game using MUD, we deployed a Testnet for local development. We started to look for _free assets_ for the game and set up a _Unity project_ to have a game client.

To connect the Blockchain with Unity, we needed a _backend_, and we created one in _Go_. This backend provides a WebSocket connection to the Unity client. By connecting to that WebSocket, the clients can _read the state_ of the game, _send transactions_, and _register_ new users.

One of our goals was to make the game multi-chain, so we made a quick demo with _Layer-Zero_ connecting _OP Mainnet_ and _Base_ to send ERC20 between both chains. It took some time, but we were able to make it work. After that, we just needed to add the new message to the backend and create the same transaction programmatically there.

At this point, we had everything ready to deploy to OP Mainnet. We created a free account at _Alchemy_ to have a REST endpoint available, and we _bridged_ some ETH to OP Mainnet.

The last step was deploying the contracts to OP Mainnet. Here, we encountered a _problem_: MUD's deployment scripts were breaking when connecting to OP Mainnet. We found out that the issue was that the contracts were too big to estimate the _gas consumption_ using the REST endpoints. We "hacked" the _node modules_ folder and hardcoded the _gas limit_ instead of using the estimation. After that change, we were able to deploy to OP Mainnet!

With everything on OP Mainnet and little time left before the hackathon ended, we moved a little bit on the map, set up our Bochamons, fought a duel, and used the bridge to record the video.

We realized that the costs of keeping that public would be high, so after the video was recorded, we _turned off the backend_ pointing to OP Mainnet and released a new version _running on Localnet_ without the bridge active to keep it public, so anybody can _play the game_.

## How to Play It

Controls:

- You can move your character using `WASD`.
- Interact with objects: `F`
- Cancel action with: `G`

 (It's the _Testnet_ version)

## Dependencies

We used _open-source_ libraries to build the game and read the state of the Blockchain:

- All the Solidity contracts are built using [MUD](https://github.com/latticexyz/mud).
- Using [Garnet](https://github.com/bocha-io/garnet), we can index all the MUD transactions and access the information via an in-memory database.
- Using [txbuilder](https://github.com/bocha-io/txbuilder), we can create, sign, and broadcast transactions.
- Using [game-backend](https://github.com/bocha-io/game-backend), we can expose all the functionality using WebSockets.

_NOTE: The first version of the last three libraries was created in a previous hackathon [AWs-Garnet](https://ethglobal.com/showcase/garnet-bkgrp)._

The client was written using Unity, which was connected to the Backend's WebSocket to get chain information and send user actions.

## Mainnet Info

### Game

- Main World Contract: [0x69e5e379c4264a9df3581c7743b3c0031cf0a817](https://optimistic.etherscan.io/address/0x69e5e379c4264a9df3581c7743b3c0031cf0a817)
- The same wallet that deployed the contracts is the one in charge of registering the match actions: [0x773fd42078335a1e60b4f856d37f33b901cc9953](https://optimistic.etherscan.io/address/0x773fd42078335a1e60b4f856d37f33b901cc9953)
- Example player wallet: [0x79A57fFb8909d1E41d77370C7f9E4878D1FbE281](https://optimistic.etherscan.io/address/0x79A57fFb8909d1E41d77370C7f9E4878D1FbE281)

### Tokens

- OP Mainnet deploy ERC20 token [0xe0BA560EF4fA8f2DC647d3DefF900005d53f8607](https://optimistic.etherscan.io/address/0xe0BA560EF4fA8f2DC647d3DefF900005d53f8607)
- Deploy proxyOFT contract in OP Mainnet [0xE77710Ae15c5F9F1b8E31135ca4f5FBe5bEc2097](https://optimistic.etherscan.io/address/0xE77710Ae15c5F9F1b8E31135ca4f5FBe5bEc2097)
- Deploy OFT contract and connect it to OP Mainnet [0xe0BA560EF4fA8f2DC647d3DefF900005d53f8607](https://basescan.org/address/0xe0ba560ef4fa8f2dc647d3deff900005d53f8607)

### Bridge

- Approve sender: [0x4c8c1e3acd7ce53901df808e3f80b3c1d00d264edb378794df88b2c8b414cbbc](https://optimistic.etherscan.io/tx/0x4c8c1e3acd7ce53901df808e3f80b3c1d00d264edb378794df88b2c8b414cbbc)
- SendFrom OP to Base: [0xe3e8b1fea3dfad814bcb687cdd411329c6f82533c4fca469def8c00c1ad79b5c](https://optimistic.etherscan.io/tx/0xe3e8b1fea3dfad814bcb687cdd411329c6f82533c4fca469def8c00c1ad79b5c)
- Layer-Zero scan: [0xe3e8b1fea3dfad814bcb687cdd411329c6f82533c4fca469def8c00c1ad79b5c](https://layerzeroscan.com/111/address/0xe77710ae15c5f9f1b8e31135ca4f5fbe5bec2097/message/184/address/0xe0ba560ef4fa8f2dc647d3deff900005d53f8607/nonce/3)
- Dst account on Base: [0x79A57fFb8909d1E41d77370C7f9E4878D1FbE281](https://basescan.org/address/0x79A57fFb8909d1E41d77370C7f9E4878D1FbE281)

## Project Structure

- MUD contracts: `./backend/contracts-builder/contracts`
- Go backend: `./backend`
- Unity client: `./frontend`
- Bridge contracts: `./layerzero-contracts`

## Configuration and Usage

To run the code locally:

- Set up your private key in the `.env` file in `backend/contracts-builder/contract`.

- Edit the Makefile to support your `nvm` path, or if you have Node globally installed, just remove the `nvm` path from the `init-contracts` and `contracts` actions.

- Deploy the game contracts:

```sh
cd backend
make init-contracts
make contracts
```

- OPTIONAL: Deploy your Layer-Zero contracts

; they are located in the `layerzero-contracts` folder. The code there is just a "copy and paste" from their Solidity examples. We have them in that folder to generate the ABI.

- Configure the backend by editing the `run.sh` file to fit your needs.

- Run the backend with `./run.sh` inside the `backend` folder. It will create an `indexerlogs.log` file.

- Open the `frontend` folder in Unity and build the project to run it.

## Things Missing

We wanted to implement more things but ran out of time:

- Display ETH and BochaCoins balances in the client.
- Validate ETH and BochaCoins balances before sending a transaction in the backend.
- Instead of sending User actions as plain text to the server, it should be a personal Ethereum message that can be validated on-chain.
- Add music to the game.
- Deploy the game on more than one network.
- Clean up WebSocket connections in the backend to fix a memory leak.

## Assets

### Bochamons

- Tobishimi, Baobaffe, Firomenis, Crobarett design.
- Howliage, Mobiusk design.
- Flarezael design.
- Sunnydra design.

### Map

- Map tiles.

### Characters

- Trainer.
- NPCs.
