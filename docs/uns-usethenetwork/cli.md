---
home: false
title: Using UNS network with the CLI 
---

# UNS CLI

[[toc]]

## Introduction
Universal Name System provides an interactive command line interface to create and manage your wallets ans UNIK tokens.
**Mainnet is not launched yet, please use devnet**.

## Requirements
UNS CLI installation requires some dependencies to be installed. It is expected to work on recent Linux and MacOS installations.

### Node.js

Install Node.js 10: [https://nodejs.org/en/download/](https://nodejs.org/en/download/).

**UNS CLI doesn't work on earlier (6, 8) or later versions of Node.js (11, 12+)**.

### Yarn

Install Yarn: [https://yarnpkg.com/en/docs/install](https://yarnpkg.com/en/docs/install).

**Don't forget to setup your $PATH to use UNS CLI as a global command.**

## Installation
Use yarn to install UNS CLI as global command

```bash
yarn global add @uns/uns-cli
```

Test your installation with

```bash
uns version
```

which should output something like that:

```
$ uns version
@uns/uns-cli/x.y.z linux-x64 node-v10.0.0
```

## UNS CLI Commands

### Get help

You can get help and list of commands with the following flags

```
-h, --help: Help
```

Example:

```bash
$ uns -h
uns CLI

VERSION
  @uns/uns-cli/0.1.0 linux-x64 node-v10.16.2

USAGE
  $ uns [COMMAND]

COMMANDS
  create-unik     Create UNIK token
  create-wallet   Create UNS wallet
  get-properties  Get properties of UNIK token.
  help            display help for uns
  read-unik       Display UNIK token informations
  read-wallet     Read current data of a specified wallet, ic. balance
  set-properties  Set (add or update) properties of UNIK token.
  status          Display blockchain status
  version         UNS CLI Version
```

You can get help on a specific command by using the `help` command, followed by your command name.

Example:

```bash
$ uns help create-wallet
Create UNS wallet

USAGE
  $ uns create-wallet

OPTIONS
  -h, --help              show CLI help
  --network=devnet|local  (required) Network used to create UNIK nft token (local are for development only)

EXAMPLE
  $ uns create-wallet --network [devnet|local]
```

### Global parameters

These parameters apply to all commands:
- `--network` (required): Network on which UNS CLI will create token [`devnet` or `local`] (`local` is for development only)
- `--verbose` (optional): Additionnal logs


### `version`

#### Introduction
Command used to prompt CLI version.

#### Usage

```bash
uns version
```

#### Output

Command prompts CLI version number, platform and node version

```
$ uns version
@uns/uns-cli/x.y.z linux-x64 node-v10.0.0
```

### `status`

#### Introduction
Command used to display blockchain status.

#### Parameters
- `-f --format` (optional): Specify how to format the output [json|table|yaml]. Default to Json.

Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns status
```

#### Examples
To display UNS devnet blockchain status

```bash
uns status --network devnet --format yaml
```

#### Output
Command displays some blockchain information

```bash
$ uns status --network devnet --format yaml

Height:  21600
Network:  devnet
Supply DUNS:  2197055800000000 DUNS
Supply UNIKs:  23 UNIKs
Active delegates:  2
Last block:  https://explorer.devnet.uns.network/block/7496261431277872677
```

### `create-unik`

#### Introduction
With UNS CLI you can create your own UNIK token using `create-unik` command.

#### Parameters
- `--explicitValue` (required): Chosen explicit value of your UNIK (255 characters max)
- `--type` (required): Type of your token [individual/corporate]
- `-f --format` (optional): Specify how to format the output [json|yaml]. Default to Json.

Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns create-unik --explicitValue {explicitValue} --type [individual|corporate]
```

#### Examples
As example, if you want to create UNIK `individual` token `bob` on `devnet` network:
```bash
uns create-unik --explicitValue bob --type individual --network devnet --verbose
```

Enter your passphrase:

```bash
$ uns create-unik --explicitValue bob --type individual --network devnet
Enter your wallet passphrase (12 words phrase):
```

Your passphrase will be hidden, no trace in your terminal history:
```
Enter your wallet passphrase (12 words phrase): ********************************************************************************
```


#### Output

If the creation succeed UNS CLI prompts yout UNIK token ID and links to see token and transaction in the UNS explorer.

```
Computing UNIK fingerprint... done
Creating transaction... done
Sending transaction... done
Waiting for transaction confirmation... done
UNIK nft created (1 confirmation(s)): bf21b5c7ae13a6892315aefcfa58ee1b1c470d011564f9f29c4f1941a2373956 [ https://explorer.devnet.uns.network/uniks/bf21b5c7ae13a6892315aefcfa58ee1b1c470d011564f9f29c4f1941a2373956 ]
See transaction in explorer: https://explorer.devnet.uns.network/transaction/a73f42691f2d076ba5a4e12c36f43ed8082cb8ae03c507d98305b8a08e6d4f03
{
  "data": {
    "id": "bf21b5c7ae13a6892315aefcfa58ee1b1c470d011564f9f29c4f1941a2373956",
    "transaction": "a73f42691f2d076ba5a4e12c36f43ed8082cb8ae03c507d98305b8a08e6d4f03",
    "confirmations": 1
  }
}

```

### `create-wallet`

#### Introduction
With UNS CLI you can create your UNS wallet using `create-wallet` command.

#### Parameters
- `-f --format` (optional): Specify how to format the output [json|yaml]. Default to Json.

Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns create-wallet
```

#### Examples
As example, if you want to create a UNS wallet on `devnet` network:
```bash
uns create-wallet --network devnet
```

#### Output

If the wallet creation succeed UNS CLI displays your wallet information.

```bash
$ uns create-wallet

⚠️  WARNING: this information is not saved anywhere. You need to copy and save it by your own. ⚠️

{
  "address": "D9B6HLADr1Fd6TXvEGCuNc3A9aHK9JzjYC",
  "publicKey": "03522706bd0b812faea10e92dc0400e37aba468f9df3e2f63570c11c2b66eadc22",
  "privateKey": "d7877b7867404cd35bf85ea7643ad23058f5af1262d11a389ffc429648f4abe7",
  "passphrase": "train drastic alley office seed glove cable fee firm during lottery cause",
  "network": "devnet"
}

```

Redirect stdout to file to create json file with wallet information:

```bash
uns create-wallet --network devnet >> ./myUNSWallet.json
```

### `read-wallet`

#### Introduction
Read current data of a specified wallet, ic. balance

#### Parameters

- `--idwallet` (required): the ID of the wallet. Can be either the publicKey or the address of the wallet.
- `--listunik` (optional): list UNIK tokens owned by the wallet, if any.
- `-f --format` (optional): Specify how to format the output [json|yaml]. Default to Json.

Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns read-wallet --idwallet {wallet identifier}] [--listunik]
```

#### Example

Display wallet information and list of UNIK token owned by this wallet
```bash
uns read-wallet --idwallet DB2cknUqNNoJgQ34nbnsJwsZi5h8TNsYKe --listunik --network devnet --format yaml
```

#### Output

```bash
$ uns read-wallet --idwallet DB2cknUqNNoJgQ34nbnsJwsZi5h8TNsYKe --listunik --network devnet --format yaml
data:
  address: DB2cknUqNNoJgQ34nbnsJwsZi5h8TNsYKe
  publicKey: 02cb4d32f1e69177bb428bf200b9c9dbf662826817f25fde2bf0bb17e28bd2292b
  username: null
  secondPublicKey: null
  balance: "99.9"
  isDelegate: false
  vote: null
  nbUnik: 1
  tokens:
    - 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f
chainmeta:
  network: devnet
  node: https://forger1.devnet.uns.network
  date: 2019-09-19T08:46:30.000Z
  height: 10610
```
For information: Wallet's balance is updated applying each transaction that engages the wallet (recipient or sender). Wallets are stored in-memory and are loaded by the node when it starts.


### `read-unik`

#### Introduction
Read current data of a specified UNIK token

#### Parameters

- `--unikid` (required): the ID of the UNIK token
- `-f --format` (optional): Specify how to format the output [json|yaml]. Default to Json.


Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns read-unik --unikid {UNIK token id}
```

#### Example

Display UNIK informations
```bash
uns read-unik --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet -f yaml
```

#### Output

```bash
$ uns read-unik --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet -f yaml
data:
  id: 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f
  ownerAddress: DB2cknUqNNoJgQ34nbnsJwsZi5h8TNsYKe
  creationBlock: "15218398688380119048"
  creationTransaction: 3831b7abdf68388c4f66663f6e655de996816e53314f1afe94a05f3da79f8d5e
  creationDate: 2019-09-19T06:53:48.000Z
  properties:
    - type: "1"
chainmeta:
  network: devnet
  node: https://forger1.devnet.uns.network
  date: 2019-09-19T09:02:00.000Z
  height: 10726

```

### `get-properties`

#### Introduction
Get properties of UNIK token. The command will fail if the minimum number of confirmations has not been reached yet.

#### Parameters

- `--unikid` (required): The UNIK token on which to get the properties.
- `--confirmed` (optional): Minimum number of confirmation since the last update of the UNIK required to return the value. Default value is 3
- `-f --format` (optional): Specify how to format the output [json|yaml|table|raw]. Default to Json.


Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns get-properties --unikid {UNIK token id}
```

#### Examples

##### Success example

Display UNIK properties
```bash
uns get-properties --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet
```

##### Success output example

```bash
$ uns get-properties --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet -f yaml
unikid: 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f
properties:
  - type: "1"
confirmations: 217
```

##### Failing example

Display UNIK properties with at least 300 confirmations since the last UNIK token update
```bash
uns get-properties --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet --confirmed 300
```

##### Failing output example

CLI throws error because of the actual number of confirmations of the last transaction that have updated UNIK token is lower than expected.
```bash
$ uns get-properties --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet --confirmed 300
›   Error: [get-properties] Not enough confirmations (expected: 300, actual: 217)
```

### `set-properties`

#### Introduction
Set (add or update) properties of UNIK token.

#### Parameters

- `--unikid` (required): the ID of the UNIK token
- `-p --properties` (required): Array of properties to set.
  
  `"key1:value1" "key3:"` Sets `value1` to `key1` and empty string to `key3`
- `--await` : Number of blocks to wait to get confirmed for the success. Default to `3`.
  
  `0` for immediate return.
  Needs to be strictly greater than `--confirmation` flag
- `--confirmations` : Number of confirmations to wait to get confirmed for the success. Default to `1`.

  Needs to be strictly lower than `--await` flag
- `--passphrase` : The passphrase of the owner of UNIK. If you do not enter a passphrase you will be prompted for it.
- `--fee` : Specify a dynamic fee in satoUNS. Defaults to `100000000 satoUNS = 1 UNS`.
- `-f --format` (optional): Specify how to format the output [json|yaml]. Default to Json.

It's impossible to ask more confirmations (with the --confirmations flag) than the number of blocks CLI waits (--await). Only 1 confirmation possible by block.

Some [global parameters](#global-parameters) may apply to this command.

#### Usage

```bash
uns set-properties --unikid {UNIK token id} --properties "key:value" "key3:" --network devnet
```

#### Example

Add property `key/value` to UNIK `2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f`
```bash
uns set-properties --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet --properties "key:value" --passphrase "train drastic alley office seed glove cable fee firm during lottery cause" -f yaml
```

#### Output

```bash
$ uns set-properties --unikid 2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f --network devnet --properties "key:value" --passphrase "train drastic alley office seed glove cable fee firm during lottery cause" -f yaml

unikid:  2145a1e84e8a54d066dbc535388898c56dae5d95e2c46a8c2e735dd3db97c03f
transaction:  5cb8c18b817f793eee58f4351426c2fe865d065d95667fcc8b23d8319afc0920
confirmations:  1

```
