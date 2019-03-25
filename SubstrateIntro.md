Using Substrate
===============

Substrate can be [used in many ways](https://github.com/paritytech/substrate#3-usage). This guide will cover blockchain nodes that are based on runtime modules.

Install Substrate
-----------------

Use the one-liner if you dare.

What you just installed.

User Interfaces
--------------
We'll be using the [polkadot-js interface](https://github.com/polkadot-js/apps). The simplest way to start is the [hosted interface](https://polkadot.js.org/apps). It is also possible to run this interface locally by following the instructions in the github linked above. In some parts of the tour we will inspect the interface's code, and even make improvements or modifications to the interface, so eventually you will want to run it locally, but for now the hosted version is sufficient.

When the interface first loads, you are connected to Alexander Polkadot network, a public testnet for polkadot. On the explore tab you can see information about recent blocks, and blocks being produced.

TODO Insert screenshot.

The interface has several other tabs that correspond to block exploring, key/account management, and runtime modules. For example you can see tabs for Staking and Democracy, both of which are covered in this tutorial. The user interface is designed to be intentionally extensible so that developers can quickly prototype interfaces for their runtime modules. Different blockchains will have different tabs supported in the UI.

Other options for interacting with nodes
* http RPC (default port 9933)
* [javascript library](https://github.com/polkadot-js/api)
* oo7 ui

Starting a Development Chain
----------------------------
`substrate --dev`

watch blocks getting formed

connect to it with the UI.

`--dev` is short for ...

In addition to the dev chain, there is the staging chain. Let's try that out. Notice the different tabs (I think, right?)

Transferring tokens
-------------------

Adding Accounts
--------------
Through the polkadot interface
With `subkey`

Executing Javascript Queries
-----------------------------
The javascript tab provides a convenient interface for quick queries to the node. Let's run a few of the examples.

Coding in the browser is not suitable for larger tasks, or frequently run queries. For those situations, you can (with a little more preamble code) execute the same kind of javascript using the [polkadot-js API module](https://github.com/polkadot-js/api) for node.js. Check out some of their excellent [examples](https://polkadot.js.org/api/examples/promise/).

Chain Specifications
--------------------
There aren't just `dev` and `staging` there are infinitely many possibilities.

We can change properties of the blockchain by changing things in the chain specification file or "chainspec" for short. Rather than write one ourselves, let's get a copy of the spec for the dev chain we've been running and make some modifications to it.

Protip about the big wasm blob

can change starting balances and block period among other things.

Custom Runtimes
---------------
Changing settings in the chainspec will be sufficient for some applications. Others will need to write their own runtime modules and recompile the node. This process is well-documented in Shawn Tabrizi's excellent [Crypto Collectibles Workshop](https://shawntabrizi.github.io/substrate-collectables-workshop/).

If this guide becomes sufficiently popular that material may be covered here in detail as well. For now you're all set to continue on to the [democracy module tour](Democracy.md).

Outline of ideas:
* `substrate-node-new JoshyChain "Joshy Orndorff"`
* build process `./build.sh` `cargo build`
* Add and SRML module to your custom build -- this is why it would be helpful to cover.
