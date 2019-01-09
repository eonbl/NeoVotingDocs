# Snapshot.GetRegisteredVotes Method ()

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
		Dictionary<ECPoint, Fixed8> d = Program.Snapshot.GetRegisteredVotes();
		foreach (KeyValuePair<DateTime, string> kvp in d) {
			Console.WriteLine("Candidate = {0}, Votes = {1}", kvp.Key, kvp.Value);
		}
	}
}
```


[Back](../Voting.md)