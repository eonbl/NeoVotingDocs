# Snapshot.GetSupporters Method ()

Gets each candidate's supporters (the accounts that voted for each candidate)

Namespace: [Neo.SmartContract.Framework.Services.Neo](../../neo.md)

Assembly: Neo.SmartContract.Framework

[Code](https://github.com/eonbl/neo/blob/votingSDK/neo/Persistence/Snapshot.cs)

## Syntax

```c#
public Dictionary<ECPoint, IEnumerable<UInt160>> GetSupporters()
```

### Parameters

(none)

### Returns

a dictionary of the candidates' public keys and their supporters's script hashes (accounts that voted for the candidate)

## Example

```c#
public class Contract1: FunctionCode
{
	public static void Main()
	{
		Snapshot snapshot = Blockchain.Singleton.GetSnapshot();
		Dictionary<ECPoint, IEnumerable<UInt160>> d = snapshot.GetSupporters();
		foreach (KeyValuePair<ECPoint, IEnumerable<UInt160>> kvp in d) {
			Console.WriteLine("Candidate = {0}, supporters' script hash = ", kvp.Key);
			foreach (UInt160 scriptHash in kvp.Value) {
				Console.WriteLine("\t{0}", scriptHash);
			}
		}
	}
}


[Back](../Voting.md)