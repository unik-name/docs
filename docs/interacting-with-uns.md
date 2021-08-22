---
home: false
tags:
 - network
 - livenet
 - sandbox
 - devnet
 - faucet
 - api
 - sdk
---

# Interacting with the unikname.network

Users can interact or let say 'use' the <brand name="uns"/> in multiple ways depending on user's needs.

If you're familiar with the <brand name="uns"/> environement, you may be interested to use the [cheat sheet](cheatsheet) to get a quick overview of usefull commands and APIs.

### Use the CLI, to go fast with basic actions

The CLI (Command Line Interface) has been designed to facilitate basic actions and testing. With the CLI you can create a cryptoaccount, mint a @unikname into the chain, update user's properties, list existing tokens or query the status of the network, and many other things. 

The CLI runs with the LIVENET but also with the SANDBOX for integration testing. 

[Read more &rightarrow;](cli)

### Use the APIs, to consume network services

All network services can be used via the <brand name="uns"/> APIs. This is the best way for developers to integrate <brand name="uns"/> services into their own developments and APPs. 

[Read more &rightarrow;](api)

### Use the SDKs, to quickly embed <brand name="uns"/> features, and Unikname solutions, withing APPs

SDKs have been developped to simplify developer work when they integrate Unikname solutions within their plateforms or their APPs. 

[Read more &rightarrow;](sdk)

### Use Wallets and Unikname APP

Wallets are end user tools, ready to use to transfer and to manage tokens. 

Unikname mobile APP is the preferred user APP within our ecosystem. 

First of all Unikname mobile APP is used to: 
1. to authenticate connection to any web platform, 
1. to manage his/her @unikname,
1. to manage his/her personal data linked with @unikname,

but Unikname mobile APP is also used : 
- to handle and transfer UNIK tokens, 
- to handle and transfer UNIKNAME tokens, 
- to trigger transactions over the network and requiring tokens,
- to consume network services 

[Read more &rightarrow;](wallet)

### Use the SANDBOX, to test your developments, or to test new features

The SANDBOX is a unikname.network dedicated for testing, it's operated by some delegates of the community.

You can connect a node to the SANDBOX by configuring the CLI, SDK, and even a dedicated chain explorer.

#### What can you do with the SANDBOX ?

- Creating test accounts
- Developing and testing applications for free (use the faucet to get SUNIK tokens)
- Testing existing applications against new releases or release candidates of <brand name="uns"/> Core

Please note that **this network is ephemeral**, and may be reset anytime. Resets will be announced several days before the effective operation, so follow [announcements published on the unikname forum](https://forum.unikname.com/c/uns-network).

#### SANDBOX faucet

If you need SUNIK tokens to test the <brand name="uns"/> features (creating your UNIKNAME for example), you can ask for SUNIK on [the unikname forum](https://forum.unikname.com/c/uns-network/faucet).
Don't forget to provide your cryptoaccount address, created with [the <brand name="uns"/> CLI](cli.html#cryptoaccount-create) or [the desktop wallet](wallet) (it must start with a `S` letter, such as `SHxZ4PSU9uaDeEhsfinn8jCempbLwwyFQB`).

You can also use [a community-managed faucet to receive SUNIK](https://unikname-faucet.rigwild.dev/).

### Legacy DEVNET network

Previous testing network used to be called "DEVNET". It is retired and is now replaced by SANDBOX network (see above).
