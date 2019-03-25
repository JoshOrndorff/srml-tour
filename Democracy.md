Democracy Module
================

IMPORTANT: You're Invited
----------------
Joshy Orndorff will host a live peer-learning sessions that covers and experiments with the democracy module on Friday March 29th for anyone interested in attending.

Overview
--------
The democracy module facilitates on-chain governance and makes up an important part of polkadot's own governance as explained by Gav in [these slides](https://www.slideshare.net/gavofyork/governance-in-polkadot-poc3).

In particular the democracy module handles this portion of polkadot governance.

TODO annotated screenshot.

The module allows anyone to make a proposal (mint some tokens, eject a validator, or change the runtime itself), and the most important proposals become referenda and are voted on.

To discourage short-sighted or spiteful voting and encourage thoughtful calculated voting, the module requires voters whose side won to lock some tokens for a period after the referendum in enacted. Thus if the result of a referenda hurts the value of the chain, those who are responsible for the decision must hold on to their tokens allowing their opposition to sell off before the bulk of the crash.

The module also includes interesting rules for turnout biasing which are described below.

Mechanics
---------
### Proposals
Anyone may make a proposal at any time. In order to ensure only the issues that people care about most, one must

### Seconding

### Referenda

### Votes and Locking
TODO Figure from Gav's slides

### Delegation
Recently added
UI not supported yet


Scenario 1 -- Give Alice some free coins
----------------------------------------
To get our feet wet, consider this scenario. Alice is a developer for our blockchain and she recently fixed a security-critical bug before it was exploited. Her fix prevented many grateful community members from being robbed. One such community member proposes that in appreciation Alice should be awarded 100 newly-minted tokens.

We'll explore several ways the scenario could unfold from here, but first a word about timing, experimentation, and patience. As we explore these scenarios, we will use a live development chain and the [Polkadot-JS UI](https://github.com/polkadot-js/apps). It is likely that you will occasionally issue the extrinsics out of order, too quickly, or too slowly. It is likely that you will need to restart the scenario a few times before getting it right. While this can sometimes be frustrating, remember that this is how you learn. The more times you practice, the more familiar you will become with the democracy module. Remember you can change the block time by modifying your chainspec file if you need to.

### Only Cheap Charlie Proposes
In the first scenario, Charlie proposes that Alice get the new tokens. Charlie, being a cheapskate, only supports the proposal with 5 tokens. This is less than the minimum

### TODO More variations of the scenario

### Automating Scenarios
For all the reasons we discussed earlier, always using UI interactively is tiresome and error-prone. We can automate scenarios like this by using the same polkadot-js api that we used in the introduction. While we won't dwell on this automation, here is an example of the previous scenario.

```javascript
//TODO Write this together during peer-learning session
```

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


Future Content
--------------
* Fix how the UI shows vote weights
* Add interface elements for delegation
* Demonstrate how delegation works more thoroughly