## FragmentManager

The FragmentManager is a key component of the Runic Protocol that enforces data integrity between fragments and the NFTs they are assigned to. Some ways in which it does this include:

- A fragment can only ever be owned by one Meta NFT.
- Only the Meta NFT that owns the Fragment can perform operations on the Fragment NFT, unless the Meta NFT owner has delegated write/transfer access to another Meta NFT or application scope.
- The FragmentManager enforces whitelists for which other application scopes can modify fragment state.
- The FragmentManager enforces that some Fragments were specified as "Soulbound" to the Meta NFT, and thus transfers are always forbidden.

## ContractFactory

The ContractFactory is a tool that allows developers to automatically create Meta NFTs and Fragment NFTs for their applications. The workflow for using the ContractFactory is as follows:

1. The user expresses their application and fragment types and rules in JSON format.
2. The user uses the TypeScript Runic Developer Kit (rdk) to validate the schema and generate a bytecode payload that is compatible with the Runic ContractFactory.
3. The user calls a function `addApplication` on the ContractFactory smart contract, providing an application scope name and the bytecode generated from the Runic Developer Kit.
4. The ContractFactory creates Meta NFT and Fragment NFT contracts as specified in the application schema, with rules that are enforced by the FragmentManager.

Here's an example of how to use the Runic Developer Kit (rdk) to generate bytecode that is compatible with the Runic ContractFactory:

```typescript
import { Runic } from "runic-sdk";


const rdk = new Runic();
rdk.validateSchema(applicationSchema);

const bytecode = rdk.generateBytecode(applicationSchema);
```

Here's an example of the `addApplication` function prototype for the ContractFactory smart contract:

```solidity
function addApplication(bytes32 scopeName, bytes memory bytecode) public returns (address metaNFT, address[] memory fragmentNFTs);
```

The `addApplication` function takes two arguments: the name of the application scope (`scopeName`) and the bytecode generated from the Runic Developer Kit (`bytecode`). It returns two values: the address of the newly-created Meta NFT contract (`metaNFT`) and an array of addresses for the Fragment NFT contracts that were created (`fragmentNFTs`).

By using the Runic Developer Kit to validate the schema and generate bytecode, and then calling the `addApplication` function on the ContractFactory smart contract, developers can easily create new Meta NFT and Fragment NFT contracts that are compatible with the Runic Protocol and enforced by the FragmentManager.


