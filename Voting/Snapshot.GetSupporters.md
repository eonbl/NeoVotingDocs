# Snapshot.GetSupporters Method ()

Namespace: [Neo.SmartContract.Framework.Services.Neo](../../neo.md)

Assembly: Neo.SmartContract.Framework

## Syntax

```c#
public IEnumerable<UInt160> GetSupporters(ECPoint pubKey)
```

### Parameters

pubkey: the public key of a candidate

### Returns

a dictionary of the candidates' public keys and their supporters (nodes that voted for this candidate)

## Example

```c#
public class Contract1: FunctionCode
{
	public static void Main()
	{
		Dictionary<ECPoint, IEnumerable<UInt160>> d = Program.Snapshot.GetSupporters();
		foreach (KeyValuePair<ECPoint, IEnumerable<Fixed8>> kvp in d) {
			Console.WriteLine("Candidate = {0}, supporters' addresses = ", kvp.Key);
			foreach (Fixed8 address in kvp.Value) {
				Console.WriteLine("\t{0}", address);
			}
		}
	}
}


[Back](../Voting.md)