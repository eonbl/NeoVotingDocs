# Wallet.Vote Method (NeoSystem, UInt160, byte[])

Votes for candidates

Namespace: [Neo.SmartContract.Framework.Services.Neo](../../neo.md)

Assembly: Neo.SmartContract.Framework

[Code](https://github.com/eonbl/neo/blob/votingSDK/neo/Wallets/Wallet.cs)

## Syntax

```c#
public extern bool Vote(NeoSystem system, UInt160 scriptHash, byte[] candidates)
```

### Parameters

system: NeoSystem containing reference to the blockchain and your local node

scriptHash: script hash (decoded address) of account that is voting. You can call [address.ToScriptHash()](https://github.com/neo-project/neo/blob/master/neo/Wallets/Helper.cs) to get a script hash from a string address

candidates: list of candidates' public keys you wish to vote for, converted to a byte array

### Returns

True if executed with no runtime errors, False otherwise

## Example

```c#
public class Contract1: FunctionCode
{
     public static void Main()
     {
        string address = "AU8fBXPDFzZR8VfXzN3ZPGoXqcz9sE68vV";
        string[] candidates = { "03b34a4be80db4a38f62bb41d63f9b1cb664e5e0416c1ac39db605a8e30ef270cc", "0395929b852d79d7d8c0ea4055b2861c0cfd668717e947f79ebba20a845bb0b4a4" };
        byte[] byteArray = new List<string>(candidates).Select(p => ECPoint.Parse(p, ECCurve.Secp256r1)).ToArray().ToByteArray();
        return Program.Wallet.Vote(system, address.ToScriptHash(), byteArray);
     }
}
```



[Back](../Voting.md)