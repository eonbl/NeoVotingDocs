# Wallet.GetRegisteredVotes Method ()

Votes for 

Namespace: [Neo.SmartContract.Framework.Services.Neo](../../neo.md)

Assembly: Neo.SmartContract.Framework

## Syntax

```c#
public Dictionary<ECPoint, Fixed8> GetRegisteredVotes()
```

### Parameters

(none)

### Returns

a dictionary containing the pairs of public keys with their corresponding vote counts

## Example

```c#
public class Contract1: FunctionCode
{
	public static void Main()
	{
		return Program.Snapshot.GetRegisteredVotes();
	}


}
```


[Back](../Voting.md)