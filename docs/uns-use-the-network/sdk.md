---
home: false
title: "uns.network SDK Overview"
---

# uns.network SDK overview

Using <uns/> SDKs, developers can employ the programming language of their choice to build applications utilizing the <uns/> blockchain.

The functionality of <uns/> API is split into two SDKs: Client and Cryptography.

**Client** SDKs help developers fetch information from the <uns/> blockchain about its current state: which delegates are currently forging, what transactions are associated with a given wallet, and so on.

**Cryptography** SDKs, by contrast, assist developers in working with transactions: signing, serializing, deserializing, etc.

If your application doesn't involve sending transactions, you can most likely build your application using the Client SDK alone. Otherwise, applications looking to leverage the full spectrum of <uns/> APIs should make use of both Client and Cryptography SDKs.

## Client SDK

Client SDK is a brand new product specifically developed for uns.network. 

SDK is compatible with node projects and browsers. 

### Installation

- In node : `yarn add @uns/ts-sdk`
- In browser : `<script src="https://cdn.jsdelivr.net/npm/@uns/ts-sdk/dist/index.umd.min.js"></script>`

### Setup

The SDK needs and initialization. You have to provide your current <uns/> configuration:

```typescript
const unsClient = new UNSClient();
unsClient.init({ network: Network.livenet });
```

Then you can use the `unsClient` to request and send transactions to <uns/>.

### Usage

To have a smooth and simple developer experience, SDK connects and query a <uns/> node. 
To replace this default behavior you can pass URL of your own <uns/> node as a parameter of `UNSClient`.

In some cases, functions return chain meta datas as `ChainMeta`. These meta datas represent the informations about the block used to read chain informations.

```
"height": 165009, // Block number
"timestamp": { // Date of reading
  "epoch": 2081533,
  "unix": 1570796439,
  "human": "2019-10-11T12:20:39.000Z"
}
```

### Key features

#### Get on-chain value UNIK types

```typescript
import { UNIKTypes } from "@uns/ts-sdk"

const unikTypeIndividual:number = UNIKTypes.INDIVIDUAL
```

See the [cheat sheet for the full list of available types](/uns-use-the-network/cheatsheet.html#types-of-unik)

#### Get status of uns.network node

```typescript
import { UNSClient, NodeStatus, Network } from "@uns/ts-sdk"

const nodeStatus:NodeStatus = (await unsClient.node.status()).data
const { synced, now, blocksCount } = nodeStatus;

```

#### Get UNIK token

```typescript
import { UNSClient, Unik, ResponseWithChainMeta, ChainMeta, Network } from "@uns/ts-sdk"

const response: ResponseWithChainMeta<Unik> = await unsClient.unik.get("unikId");
const unik: Unik = response.data;
const meta: ChainMeta = response.chainmeta;

```

#### Get UNIK property value

```typescript
import { getPropertyValue, ResponseWithChainMeta, PropertyValue, ChainMeta, Network } from "@uns/ts-sdk"

const options = {
  withChainmeta: true; // Retrieve ChainMeta object [default: true]
  confirmations: true; // Retrieve number of confirmations since last transaction on UNIK token [default: true]
  disableHtmlEscape: true; // Disable HTML escaping [default: false]
}

const response: ResponseWithChainMeta<PropertyValue> | PropertyValue = await getPropertyValue("unikId", "propertyKey", Network.livenet, options);

if( response instanceof PropertyValue ){
  const value: PropertyValue = response as PropertyValue;
} else {
  const resp: ResponseWithChainMeta<PropertyValue> = response as ResponseWithChainMeta<PropertyValue>
  const value: PropertyValue = resp.data
  const meta: ChainMeta = resp.chainmeta;
  const confirmations: number = resp.confirmations;
}

```

This function is protected by HTML escaping. When you use `getPropertyValue` function, the result is "HTML escaped" by default.

***Do not use `eval` function with a property retrieved by the chain. Javascript can be easily injected!***

#### Resolve Decentralized Identifier (DID)

```typescript
import { didResolve, DidResolution, ResolutionResult, Network } from "@uns/ts-sdk"
const unikType = "individual";
const unikName = "unikName";
const response = await didResolve(`@unik:${unikType}:${unikName}`, Network.livenet) as DidResolution<ResolutionResult>;
const { unikid, ownerAddress } = response.data;

```


#### Disclose

SDK contains some helpers for disclose feature.

##### build a disclose demand

To disclose explicit values of a given @unikname, you must create a demand first, then send this demand to an authorized certification service.
The demand builder requires :
- the unik-name fingerprint 
- a list of explicit values of your @unikname you want to disclose
- type of your @unikname
- passphrase of the wallet owner of the @unikname

```typescript
import { buildDiscloseDemand, DIDTypes, DiscloseDemand } from "@uns/ts-sdk"

const demand: DiscloseDemand = buildDiscloseDemand(
  "a242daa994cc5490020871731d34f7cd3c3993e0b30bac1233d7483001e96e77", // @unikname fingerprint 
  ["explicit1", "explicit2"], // explicit values to disclose
  DIDTypes.INDIVIDUAL, // type of the @unikname
  "this is my passphrase" // of the @unikname owner to sign demand
);
```

A disclose demand contains:

```typescript
{
  payload:{
    sub:string,
    iss:string,
    iat:number,
    explicitValue: string[]
    type: DIDTypes
  },
  signature: string
}
```

##### send a disclose demand

Once you have built a disclose demand through the builder above, you must submit it to an authorized certification service. 

```typescript
import { UNSClient, Response, DiscloseDemandCertification, FunctionalError } from "@uns/ts-sdk"

const certifiedDemandResponse: Response<DiscloseDemandCertification> = await unsClient.discloseDemandCertification.get(demand);
if( certifiedDemandResponse.data ){
  const certifiedDemand: DiscloseDemandCertification = certifiedDemandResponse.data
  console.log(certifiedDemand)
} else{
  const error: FunctionalError = certifiedDemandResponse.error 
  console.log(error.message)
}
```

A certified disclose demand contains:

```typescript
{
  payload:{
    sub: string;
    iss: string;
    iat: number;
  },
  signature: string
}
```

#### Get chain NFTs statuses

Retrieve all chain NFTs statuses (number of token for each types).

```typescript
import { getNftsStatuses, INftStatus, Network, ResponseWithChainMeta } from "@uns/ts-sdk";

const statuses: ResponseWithChainMeta<INftStatus[]> = await getNftsStatuses(Network.livenet);
const { data } = statuses;

```


## Cryptography SDK

Inherited from Ark crypto package, <uns/> crypto package is available as a typescript library, published on npm : [`@uns/crypto`](https://www.npmjs.com/package/@uns/crypto)

It can do more than Ark package : 

- mint UNIKs 
- add, delete or update UNIK properties
- transfer UNIKs to another wallet
- transfer UNS tokens
- create <uns/> wallets
- ...

For the full list of other available features of `crypto` package, please look at [Ark documentation](https://learn.ark.dev/concepts/cryptography).

<uns/> specific features are not documented yet. Feel free to explore and [contribute](https://github.com/unik-name/docs.uns.network) 😊.


