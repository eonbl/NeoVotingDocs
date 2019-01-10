# Voting Documentation

This document explains the methods in the NEO SDK that are related to voting.

Namespace: [Neo.SmartContract.Framework.Services.Neo](../neo.md)

Assembly: Neo.SmartContract.Framework

## Methods

| | Name | Description |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Wallet.Vote(NeoSystem, UInt160, byte[])](Voting/Vote.md) | Votes for candidates|
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Snapshot.GetRegisteredVotes()](Voting/GetRegisteredVotes.md)  | Returns a dictionary with candidates and the number of votes they each have|
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Snapshot.GetSupporters()](Voting/GetSupporters.md) | Returns a dictionary with candidates and a list of addresses that voted for each one |
| ![](https://i-msdn.sec.s-msft.com/dynimg/IC91302.jpeg) | [Snapshot.GetSupporters(ECPoint)](Voting/GetSupporters.md) | Returns a list of addresses that voted for the specified public key |

# How Voting Works
See [How To Become NEO Consensus Node](https://neo-ngd.github.io/reference/How-To-Become-NEO-Consensus-Node.html)
for high-level information about election and voting.

When [Vote(system, address, candidates)](Voting/Vote.md) is called, a [StateTransaction](https://github.com/neo-project/neo/blob/master/neo/Network/P2P/Payloads/StateTransaction.cs) is created with empty inputs, empty outputs, and
a [StateDescriptor](https://github.com/neo-project/neo/blob/master/neo/Network/P2P/Payloads/StateDescriptor.cs) containing `candidates`. This transaction is placed in a [ContractParametersContext](https://github.com/neo-project/neo/blob/master/neo/SmartContract/ContractParametersContext.cs), 
which is then signed by `address` and broadcast to the blockchain referenced in `system`. 

Each account can vote for multiple candidates (`candidates` may contain multiple candidate public keys), and each of those candidates receives votes equal to the amount of NEO owned by that account.

Votes are transferred when NEO is transferred. That is, when x amount of NEO is transferred from account A to account B, the candidates voted for by account A will lose x votes and the candidates voted for by account B will gain x votes. This is updated in real time.

The top N candidates become consensus nodes, where N is determined by the algorithm described [here](https://neo-ngd.github.io/reference/How-To-Become-NEO-Consensus-Node.html#42-voting). 
The way this works is when `GetValidators(transactions)` is called, GetValidators first takes a [snapshot](https://github.com/neo-project/neo/blob/master/neo/Persistence/Snapshot.cs) of the blockchain
and updates the NEO for each account based on `transactions` while simultaneously modifying the votes of each candidate voted for by those accounts according to the NEO transfers.
Then the registered candidates with a positive number of votes are combined with the active standby validators, and this list is sorted to find the top N nodes, which are returned as the current validators (consensus nodes).
If this list has length less than N, standby validators are added (i.e., become active) to reach N.