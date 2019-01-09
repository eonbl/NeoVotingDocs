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
See [How To Become NEO Consensus Node](https://neo-ngd.github.io/reference/How-To-Become-NEO-Consensus-Node.html#current-consensus-nodes)
for high-level information about election and voting.

When [Vote(system, address, candidates)](Voting/Vote.md) is called, a [StateTransaction](https://github.com/neo-project/neo/blob/master/neo/Network/P2P/Payloads/StateTransaction.cs) is created with empty inputs, empty outputs, and
a [StateDescriptor](https://github.com/neo-project/neo/blob/master/neo/Network/P2P/Payloads/StateDescriptor.cs) containing `candidates`. This transaction is placed in a [ContractParametersContext](https://github.com/neo-project/neo/blob/master/neo/SmartContract/ContractParametersContext.cs), 
which is then signed by 
