# Voting Documentation

This document explains the methods in the NEO SDK that are related to voting.

Namespace: [Neo.SmartContract.Framework.Services.Neo](../neo.md)

Assembly: Neo.SmartContract.Framework

## Methods

| | Name | Description |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Wallet.Vote(NeoSystem, UInt160, byte[])](Voting/Wallet.Vote.md) | Votes for candidates|
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Snapshot.GetRegisteredVotes()](Voting/Snapshot.GetRegisteredVotes.md)  | Returns a dictionary with candidates and the number of votes they each have|
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Snapshot.GetSupporters()](Voting/Snapshot.GetSupporters.md) | Returns a dictionary with candidates and a list of addresses that voted for each one |
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Snapshot.GetSupporters(ECPoint)](Voting/Snapshot.GetSupporters(ECPoint).md) | Returns a list of addresses that voted for the specified public key |

# How Voting Works
See [How Voting Works in NEO](https://github.com/taomo-eo/docs/blob/master/Voting-Mechanism/How-Voting-Works-In-NEO.md)
for information that a NEO holder should know in order to vote. What follows is a more technical explanation oriented toward developers.

Because transactions are what are recorded on the NEO ledger, voting is implemented as a transaction. In particular, 
when [Vote(system, scriptHash, candidates)](Voting/Wallet.Vote.md) is called, a [StateTransaction](https://github.com/neo-project/neo/blob/master/neo/Network/P2P/Payloads/StateTransaction.cs) is created
with a [StateDescriptor](https://github.com/neo-project/neo/blob/master/neo/Network/P2P/Payloads/StateDescriptor.cs) containing a `Type`, `Key`, `Field`, and `Value`.
* The `Type` is `StateType.Account`, representing that the `Key` is that of a voter, not a candidate. 
* The `Key` is the byte array of `scriptHash`, which represents the voting account 
(it is the script hash of the address of the voting account). 
* The `Field` is set to "Votes"
* The `Value` is set to `candidates` (the public keys of the nodes being voted for).

After the transaction is made, it must be signed. Every transaction requires one or more signatures to prove consent by those involved, and voting is no different. Thus, this transaction is placed in a
[ContractParametersContext](https://github.com/neo-project/neo/blob/master/neo/SmartContract/ContractParametersContext.cs), which contains the field `Signatures`. In general, once all parties to a
transactions sign it (i.e., their signatures get added to `Signatures`), then the field `Completed` becomes true. Once that happens, the transaction is broadcast to be confirmed by the blockchain.
In `Vote(system, scriptHash, candidates)`, the transaction is signed by the account referenced by `scriptHash` and then broadcast to the NEO blockchain. (That blockchain is referenced in `system`, a [NeoSystem](https://github.com/neo-project/neo/blob/master/neo/NeoSystem.cs)).

Each account can vote for multiple candidates; i.e., the `candidates` parameter may contain multiple public keys. Each of those candidates receives votes equal to the amount of NEO owned by that account.

Votes are transferred when NEO is transferred. That is, when x amount of NEO is transferred from account A to account B, the candidates voted for by account A will lose x votes and the candidates voted for by account B will gain x votes.
This is updated in real time. The paragraph below explains how this works.

The top N candidates become consensus nodes, where N is determined by the algorithm described [here](https://neo-ngd.github.io/reference/How-To-Become-NEO-Consensus-Node.html#42-voting). 
The way this works is when `GetValidators(transactions)` is called, GetValidators first takes a [snapshot](https://github.com/neo-project/neo/blob/master/neo/Persistence/Snapshot.cs) of the blockchain
and updates the NEO for each account based on `transactions` while simultaneously modifying the votes of each candidate voted for by those accounts according to the NEO transfers in `transactions`.
Then the registered candidates with a positive number of votes are combined with the active standby validators, and this list is sorted to find the top N nodes, which are returned as the new validators (consensus nodes).
If this list has length less than N, standby validators are added (i.e., become active) to reach N. These are the new consensus nodes.

