# Snapshot.GetRegisteredVotes Method (), overloaded

Namespace: [Neo.SmartContract.Framework.Services.Neo](../../neo.md)

Assembly: Neo.SmartContract.Framework

## Syntax

```c#
public IEnumerable<UInt160> GetSupporters(ECPoint pubKey)
```

### Parameters

pubKey: the public key of a certain candidate

### Returns

a collection of addresses that are voting for the candidate

## Example

```c#
public class Contract1: FunctionCode
{
	public static void Main()
	{
		pubKey = "037ebe29fff57d8c177870e9d9eecb046b27fc290ccbac88a0e3da8bac5daa630d"
		IEnumerable<UInt160> addresses = Program.Snapshot.GetRegisteredVotes(pubKey);
		Console.WriteLine("Candidate = {0}, supporters' addresses =", pubKey);
		foreach (UInt160 address in addresses) {
			Console.WriteLine("\t{0}", address);
		}
	}
}
```


[Back](../Voting.md)