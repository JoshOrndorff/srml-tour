Democracy Module
================

This content was covered in a peer learning session on 29 March 2019 ([video recording](https://youtu.be/1xw_4R_HmCU)).

Overview
--------
The democracy module facilitates on-chain governance and makes up an important part of polkadot's own governance as explained by Gav in [these slides](https://www.slideshare.net/gavofyork/governance-in-polkadot-poc3).

In particular the democracy module handles the circled portion of polkadot governance.

![Illustration of polkadot governance with council and democracy](democracyDiagram.png)

Democracy allows anyone who is willing to lock some tokens to make a proposal (mint some tokens, eject a validator, even change the runtime itself), and the most important proposals become referenda and are voted on.

To discourage short-sighted or spiteful voting and encourage thoughtful calculated voting, the module requires voters whose side won to lock some tokens for a period after the referendum in enacted. Thus if the result of a referenda hurts the value of the chain, those who are responsible for the decision must hold on to their tokens allowing their opposition to sell off before the bulk of the crash.

The module's rust code lives at: [https://github.com/paritytech/substrate/tree/master/srml/democracy](https://github.com/paritytech/substrate/tree/master/srml/democracy)

Mechanics
---------
### Proposals
Anyone may make a proposal at any time. So that voters don't need to worry about a thousand trivial proposals all at once, only one proposal (the one with the highest deposit) is tabled every period (Default 9 blocks). If you're curious, check out the [rust code](https://github.com/paritytech/substrate/blob/master/srml/democracy/src/lib.rs#L86-L107).

### Seconding
When someone makes a proposal that is important to you, you may add to its deposit to help get it tables faster by seconding it. Seconding simply means locking up the same amount of tokens as the original staker. If you're curious, check out the [rust code](https://github.com/paritytech/substrate/blob/master/srml/democracy/src/lib.rs#L109-L118).

Tokens locked when proposing and seconding are [released](https://github.com/paritytech/substrate/blob/master/srml/democracy/src/lib.rs#L401) as soon as (and only when) the proposal is tabled.

In my opinion this is not ideal because it means someone who supported an unpopular proposal in a busy democracy, will never be able to "give up" on their pet issue and get their tokens back.

### Voting on Referenda
Once a proposal has been tabled it is open for voting for a predetermined number of blocks (18 by default). Any token holder is eligible to vote on the referendum. The token holder's vote is proportional to the number of tokens they chose to lock and the amount of time they chose to lock the tokens for.

![Votes are proportional to tokens locked and lockup period](weightedVotes.png)

After the voting closes, any voter who "didn't get her way" receives their locked tokens back immediately. That is to say if a referendum passes, all voters who voted against it, or if a referendum fails, all voters who voted in favor.

### Turnout Biasing
Real-world democracies often require a simple majority of the voters who bothered voting to support a referendum before it is ultimately accepted. Such a system has the drawback that when voter turnout is low, a vocal minority can affect the system against those who didn't turn out. Thus one must always keep an eye on governance even when they are happy with the status quo.

The SRML's democracy module solves this problem by varying the threshold of votes to pass a referendum as the voter turnout varies. In short, lower voter turnout requires a higher margin of support to pass a motion. (When the council proposes a motion, the turnout biasing can be set manually).

Gav shows this dynamic effectively in this slide.
![Turnout biasing in SRML Democracy](turnoutBias.png)

### Directly Creating and Canceling Referenda
Looking back at the figure illustrating polkadot's governance, you'll notice that the council can also create and cancel referenda directly. While such features are not exposed directly to end users, they are included in the democracy module's code so it can [work with the council module](https://github.com/paritytech/substrate/blob/master/srml/democracy/src/lib.rs#L341-L352).

### Delegation
The ability to delegate ones vote was to the democracy module recently. Imagine that Alice cares about some issue, but is unable to vote on it herself (maybe she doesn't have time or ability the nuanced arguments around the issue), but she trusts her friend Bob to vote on her behalf. With delegation, Alice can explicitly attach her tokens to the vote that Bob casts.

Because delegation was added to the module recently, it is not yet supported by the polkadot UI. This means that, for one thing, users cannot delegate their votes through that interface. But it also means that the interface does not correctly count votes that were delegated.

I'm planning to add [such functionality](https://github.com/polkadot-js/apps/issues/836) to the interface as soon.

Starting a Development Chain
---------------
Luckily the pre-baked substrate node comes with the democracy module already installed, so we will not need to compile our own node for this portion of the tour. You can start a node with `substrate --dev` and connect to it with the polkadot UI just as we did in the intro.

Scenario 1 -- Give Alice free tokens
----------------------------------------
To get our feet wet, consider this scenario. Alice is a developer for our blockchain and she recently fixed a security-critical bug before it was exploited. Her fix prevented many grateful community members from being robbed. One such community member proposes that in appreciation Alice should be awarded 100 newly-minted tokens.

As we explore these scenarios, we will use a live development chain and the [Polkadot-JS UI](https://github.com/polkadot-js/apps). It is likely that you will occasionally issue the extrinsics out of order, too quickly, or too slowly. It is likely that you will need to restart the scenario a few times before getting it right. While this can sometimes be frustrating, remember that this is how you learn. The more times you practice, the more familiar you will become with the democracy module. Remember you can change the block time by modifying your chainspec file if you need to.

### Charlie proposes, Bob seconds, Referendum Passes
This timing diagram is my attempt at illustrating that Bob makes the proposal, charlie seconds it, increasing it's weight. Once it becomes a referendum, Bob and Charlie both vote Aye, but Alice, humble as she is, votes Nay. Accordingly the motion passes.

![timing diagram](timing/1.png)

Begin by going to the extrinsics tab in the UI, we can have charlie submit a proposal. ([Extrinsics](https://docs.substrate.dev/docs/glossary#section-extrinsic) are transactions and things like blocktime. In this case, just think transactions.) In my screenshot, Charlie has chosen to lock up 1000 tokens to support this proposal.

TODO screenshot

On the democracy tab you can see the proposal now. Wait a few blocks and (as long as nobody else adds a more popular proposal!) it will be tabled as a referendum. You could go back to the extrinsics tab to cast your votes, but you've probably noticed there is a nice voting interface right here on the democracy tab. Go ahead and use it.

Confirm for yourself that Alice has received the tokens once the referendum passes.

### Only Cheap Charlie Proposes
Let's see how the same situation unfolds when Carlie locks fewer tokens behind his proposal. Try the same scenario using only 5 tokens this time.

TODO Rejected screenshot

When the next block is added to the chain, we see that the extrinsic failed because Charlie didn't deposit enough tokens.

### Exercise: Charlie proposes, No seconds, Referendum Fails
Based on the title and the timing diagram, execute this scenario through the Polkadot UI.

![timing diagram](timing/2.png)

At any time you can query the blockchain and observe properties of the runtime. For example we can see how many proposals have been made so far. On the "Chain State" tab .....
TODO Screenshot

### Exercise: Charlie Proposes, Other proposal more popular
![timing daigram](timing/3.png)

Execute this scenario with the Polkadot UI.


Democracy Settings in the Chainspec
-----------------------------------
Just like we changed blocktime, and initial token balances by modifying the chainspec, we can change several properties of the democracy module. The settings are found at the bottom of the file after the large wasm blob.

```json
"democracy": {
  "launchPeriod": 9,
  "minimumDeposit": 10,
  "publicDelay": 7,
  "maxLockPeriods": 6,
  "votingPeriod": 18
}
```

For starters, let's change the minimum deposit to 3 tokens. This will allow cheapskate Charlie's proposal to go through. While we're at it let's change the public delay. This property determines how many blocks must elapse after a referendum is passed before it is enacted.

Navigating back to the interface, and executing the very first scenario above, we see that now Charlie's proposal is accepted because the minimum deposit has been lowered.


Scenario 2 -- Bob wants to be super user
----------------------------------------
The dev chain (and the slightly customized chain we just created) includes the sudo module. sudo allows a single user, the "super user" to execute extrinsics with higher authority than usual. Essentially whoever is the super-user is the king of the blockchain. This module is super useful for building and debugging a blockchain, but not a great governance strategy in a public chain. Since we have sudo, let's use it in an example.

remember to purge your chain, generate the raw chainspec, and start with the proper flags. Review the [intro](SubstrateIntro.md) if necessary.

### Referendum Passes
When the chain is launched, Alice is the super user. Bob, thinking he would be a better super user than Alice, proposes that he be the new super user. Some of his friends think that's a good idea. The timing diagram is similar to above, but because we've changed our the public delay, the passed referendum is not enacted immediately.

![timing diagram](timing/4.png)

If you have the patience, it is usually insightful to play with the ui, and running scenarios is a good way to do it. If you're bored with that, then read on.

### Exercise: Alice cancels referendum
by using her existing sudo capabilities

![timing diagram](timing/5.png)

Automating Scenarios
--------------------
For all the reasons we discussed earlier, always using the UI interactively is tiresome and error-prone. We can automate scenarios like these by using the same polkadot-js api that we used in the introduction. While we won't dwell on this automation, here is an example automated scenario.

```javascript
#! /usr/bin/env node

const { ApiPromise } = require('@polkadot/api');
const { Keyring } = require('@polkadot/keyring');

// https://polkadot.js.org/api/METHODS_STORAGE.html#democracy
// https://polkadot.js.org/api/METHODS_EXTRINSICS.html#democracy
// https://polkadot.js.org/api/METHODS_EVENTS.html#democracy

// Main function necessary so we can use `await` inside
async function main() {

  // Create instances of the API (connected to local node) and keyring.
  const api = await ApiPromise.create();
  const keyring = new Keyring();

  // Identities we'll use
  // ATM this gives the old ED25519 keys without 2nd and 3rd parameters.


  const alice = keyring.addFromUri('//Alice', {}, 'sr25519');
  const bob   = keyring.addFromUri('//Bob', {}, 'sr25519');
  const charlie = keyring.addFromUri('//Charlie', {}, 'sr25519');
  console.log("Alice's address is " + alice.address());
  console.log("Bob's address is " + bob.address());
  console.log("Charlie's address is " + charlie.address());

  // ISSUE: copying Charlie's address from the ui didn't work. see https://github.com/polkadot-js/apps/issues/872
  // ExtError: Decoding 5FLSigC9HGRKVhB9FiEo4Y3koPsNmBmLJbpXg2mp1hXcS3gV: Invalid decoded address checksum
  //const charlieAddr = '5FLSigC9HGRKVhB9FiEo4Y3koPsNmBmLJbpXg2mp1hXcS3gV';

  // Make sure both alice and bob have funds
  //TODO set balances

  // Alice makes a proposal to burn Charlie's funds,
  // and supports it with 2000 tokens.
  const proposal = api.tx.balances.setBalance(charlie.address(), 0, 0);
  const aliceMotion = api.tx.democracy.propose(proposal, 2000);
  const motionHash = await aliceMotion.signAndSend(alice);
  console.log(`Receipt for Alice's motion: ${motionHash.toHex()}`);

  // Bob seconds
  // TODO How do I get the index of my proposal after the fact?
  // Maybe record what it is right before I submit the transaction?
  // But that doesn't guarantee anything
  const secondHash = await api.tx.democracy.second(0).signAndSend(bob);
  console.log(`Receipt for Bob's second: ${secondHash.toHex()}`);

  // Bob delegates to Alice
  //TODO

  // Wait for proposal to be tabled
  //TODO

  // Alice votes Aye
  //await api.tx.democracy.vote(0, true).signAndSend(alice);

  // Charlie votes Nay
  //TODO

  // Wait for referendum to close
  //TODO

  // Tally votes? Check vote tally?
  //TODO

  // Check whether referendum passed
  //TODO
}

// Call main
main().catch(console.error).finally(() => process.exit());
```


### Exercise: Low turnout
In this final exercise, you should construct a scenario, using the interface or javascript, in which a referendum has (just) over 50% voter support, but does not pas due to low voter turnout. It may help to check out the rust code that [implements turnout biasing](https://github.com/paritytech/substrate/blob/master/srml/democracy/src/vote_threshold.rs#L79-L90).

Future Content / Blockers to Fix
--------------
* Fix how the UI shows vote weights
* Add interface elements for delegation
* Demonstrate how delegation works more thoroughly
