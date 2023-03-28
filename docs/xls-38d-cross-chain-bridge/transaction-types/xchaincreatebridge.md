---
html: xchaincreatebridge.html 
parent: transaction-types.html
blurb: Create a bridge between two chains.
labels:
  - Interoperability
status: not_enabled
---
# XChainCreateBridge

<embed src="/snippets/_xchain-bridges-disclaimer.md" />

The `XChainCreateBridge` transaction creates a new `Bridge` ledger object and defines a new cross-chain bridge entrance on the chain that the transaction is submitted on. It includes information about door accounts and assets for the bridge. 

The transaction must be submitted by the door account. To set up a valid bridge, door accounts on both chains must submit this transaction, in addition to setting up witness servers.

The complete production-grade setup would also include a `SignerListSet` transaction on the two door accounts for the witnesses’ signing keys, as well as disabling the door accounts’ master key. This ensures that the witness servers are truly in control of the funds.

**Caution:** Ensure that you do not create a duplicate bridge with different set of door accounts for an existing asset. Doing so can cause an imbalance of available wrapped XRP on the issuing chain. 

To mitigate the possibility of creating a duplicate bridge, ensure the following:

* The issuing chain door account should only issue tokens for a single bridge. Do not create two(2) bridges with the same issuing asset on the same issuing chain. 
* Do not create two (2) bridges with the same locking asset in the same door account. You can create them with different door accounts. 


## Example XChainCreateBridge JSON

```json
{
  "TransactionType": "XChainCreateBridge",
  "Account": "rhWQzvdmhf5vFS35vtKUSUwNZHGT53qQsg",
  "XChainBridge": {
    "LockingChainDoor": "rhWQzvdmhf5vFS35vtKUSUwNZHGT53qQsg",
    "LockingChainIssue": {
      "currency": "XRP"
    },
    "IssuingChainDoor": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
    "IssuingChainIssue": {
      "currency": "XRP"
    }
  },
  "SignatureReward": 200,
  "MinAccountCreateAmount": 1000000
}
```


## XChainCreateBridge Fields

| Field                    | JSON Type         | [Internal Type][] | Required? | Description |
|:-------------------------|:------------------|:------------------|:----------------|:------|
| `MinAccountCreateAmount` | `Currency Amount` | `AMOUNT`          | No        | The minimum amount, in XRP, required for a `XChainAccountCreateCommit` transaction. If this isn't present, the `XChainAccountCreateCommit` transaction will fail. This field can only be present on XRP-XRP bridges. |
| `SignatureReward`        | `Currency Amount` | `AMOUNT`          | Yes       | The total amount to pay the witness servers for their signatures. This amount will be split among the signers. |
| `XChainBridge`           | `XChainBridge`    | `XCHAIN_BRIDGE`   | Yes       | The bridge (door accounts and assets) to create. |


<embed src="/docs/xls-38d-cross-chain-bridge/snippets/_xchainbridge-serialization.md" />


<!-- ## Error Cases

In addition to errors that can occur for all transactions, {{currentpage.name}} transactions can result in the following [transaction result codes](transaction-results.html):

| Error Code                    | Description                                  |
|:------------------------------|:---------------------------------------------|
| `temDISABLED`                 | The [NonFungibleTokensV1 amendment][] is not enabled. |
-->