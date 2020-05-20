<p align="center">
  <a href="https://circleci.com/gh/PhilippLgh/ethereum-binaries"><img src="https://img.shields.io/circleci/project/github/PhilippLgh/ethereum-binaries/master.svg" alt="Build Status"></a>
  <a href="https://npmcharts.com/compare/ethbinary?minimal=true"><img src="https://img.shields.io/npm/dm/ethbinary.svg" alt="Downloads"></a>
  <a href="https://www.npmjs.com/package/ethbinary"><img src="https://img.shields.io/npm/v/ethbinary.svg" alt="Version"></a>
  <a href="https://www.npmjs.com/package/ethbinary"><img src="https://img.shields.io/npm/l/ethbinary.svg" alt="License"></a>
  <br>
</p>

# Ethereum Binaries

Fast, easy and secure Ethereum binary management.

- [X] 🎁 **Package Extraction**
- [x] 🔐 **Binary Verification**
- [x] ♨️ **Runtime Detection** 🐍
- [X] 🐳 **Docker Support** 
- [X] ⏰ **Lifecycle Events [ IPC_READY | SYNCED | STOPPED ... ]** 
- [x] ☁️ **Auto Update**
- [x] ⚡ **Caching**
- [x] 🐙 **Version Management**
- [x] 🌈 **Multi Client Support**

# Docs

Documentation is available at [github.io/ethereum-binaries](https://philipplgh.github.io/ethereum-binaries/#/)

# Supported Clients & Binaries

# Intro

Binaries are an integral part of the Ethereum ecosystem. There are many amazing tools (Clef, ZoKrates, Puppeth, ...) that go even beyond the different client implementations (Geth, Prysm, Besu, Nethermind, ...).
However, managing them can be a very complex task. There are no standards for how binaries are distributed and you might find Docker images, binaries hosted on (GitHub, Azure, Bintray, AWS), or even have to build them from source yourself by installing the respective toolchains first and learning about language specific details.
Moreover, important steps such as binary verification with e.g. GPG are often skipped because it is too complex or inconvenient.
Interacting with these binaries, e.g. from a script file when they are running inside a container creates a whole new set of challenges.
The goal of this library is to create a unified interface to download, configure and interact with Ethereum binaries so that it's more about the `what` and less about `how`.

# Installation
```shell
npm install -g ethbinary
```

# Quickstart 🚀

#### CLI Examples
```shell
ethbinary geth@latest --goerli
```

Will download the latest version of geth and start geth with a connection to the goerli testnet:

![Fast Start Gif](r./../img/fast_start.gif?raw=true "Title")

#### More CLI Examples
```shell
ethbinary list //example: returns [ 'besu', 'ewasm', 'geth', 'prysm' ]

ethbinary download geth // will display version selector
ethbinary download geth@1.9.10 // short-hand specifier
ethbinary download geth --clientVersion 1.9.10 // equivalent to above syntax

ethbinary exec geth@latest "version" // the command MUST be one string for the parser to work
ethbinary exec geth@latest "account new" // is auto-attached to terminal so that stdin for password works
ethbinary exec geth --clientVersion latest "account new" // verbose syntax

ethbinary start geth // will start latest geth version with mainnet connection (geth default)
ethbinary start geth --goerli
ethbinary start geth@1.9.10 --goerli
```


### Module Example: Use Geth for Testing

```javascript
const { default: cm } = require('ethbinary') // get the client manager instance
const clientId = await cm.getClient('geth')
const geth = await cm.setClient(clientId) // use singe-client api
await geth.start(['--goerli'])
await geth.stop()
```

### Module Example: Multi Client API

```javascript
const { default: cm } = require('ethbinary') // get the client manager instance
const clientId = await cm.getClient('geth')
await cm.startClient(clientId, 'latest', ['--goerli'])
await cm.stopClient(clientId)
```