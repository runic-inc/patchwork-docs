## Runic Protocol

### FragmentManager

The FragmentManager is a key component of the Runic Protocol that enforces data integrity between fragments and the NFTs they are assigned to. Some ways in which it does this include:

- A fragment can only ever be owned by one Meta NFT.
- Only the Meta NFT that owns the Fragment can perform operations on the Fragment NFT, unless the Meta NFT owner has delegated write/transfer access to another Meta NFT or application scope.
- The FragmentManager enforces whitelists for which other application scopes can modify fragment state.
- The FragmentManager enforces that some Fragments were specified as "Soulbound" to the Meta NFT, and thus transfers are always forbidden.

#### FragmentManager API

```solidity
// Claim a new applcation scope
function claimScope(string calldata scopeName);

// Give control of an application scope to a new owner
function transferScopeOwnership(string calldata scopeName, address newOwner);
function getScopeOwner(string calldata scopeName) public view returns (address owner);

// Add/remove approved operators
function addOperator(string calldata scopeName, address op);
function removeOperator(string calldata scopeName, address op);

// Mint a new meta NFT in a scope, soulbound to a non-Runic NFT
function createMetaNFT(string calldata scopeName, address owner, address originalNFTAddress, uint originalNFTTokenId, address metaAddress) public returns (uint256 tokenId);

// Assign fragment NFT to Meta NFT
function assignNFT(string calldata scopeName, address fragment, uint fragmentTokenId, address target, uint targetTokenId);
function unassignNFT(string calldata scopeName, address fragment, uint fragmentTokenId);
function bulkAssignNFT(string calldata scopeName, address[8] calldata artifacts, uint[8] calldata tokenIds, address target, uint targetTokenId);

// Check if nft is allowed to be transferred
function checkTransfer(address nft, uint256 tokenId);
```

#### Sample Code
```solidity
prot = new RunicProtocol();            // Fragment Manager
originalNFT = new OriginalNFT();       // e.g., BAYC
appMetaNFT = new ApplicationMetaNFT(); // rdk generated Meta NFT Contract
fragmentNFT = new FragmentNFT();       // rdk generated Fragment NFT Contract

scopeName = "NFT Fighter";

prot.claimScope(scopeName);
uint256 metaTokenId = prot.createMetaNFT(scopeName, originalNFT.ownerOf(1), address(originalNFT), 1, address(appMetaNFT));

// Register fragmentNFT to appMetaNFT
uint8 id = appMetaNFT.registerReferenceAddress(address(fragmentNFT));

// Init storage 
// TODO: This step goes away once we add intialization if it hasn't occured yet the first time assignNFT is called
AppMetaNFTMetadata memory data;
appMetaNFT.storeMetadata(1, data);

//Assign a fragment to a meta NFT
prot.assignNFT(scopeName, address(fragmenttNFT), 1, address(appMetaNFT), metaTokenId);

```


### ContractFactory

The ContractFactory is a tool that allows developers to automatically create Meta NFTs and Fragment NFTs for their applications. The workflow for using the ContractFactory is as follows:

1. The user expresses their application and fragment types and rules in JSON format.
2. The user uses the TypeScript Runic Developer Kit (rdk) to validate the schema and generate a bytecode payload that is compatible with the Runic ContractFactory.
3. The user calls a function `deployApplication` on the ContractFactory smart contract, providing an application scope name and the bytecode generated from the Runic Developer Kit.
4. The ContractFactory creates Meta NFT and Fragment NFT contracts as specified in the application schema, with rules that are enforced by the FragmentManager.

Here's an example of how to use the Runic Developer Kit (rdk) to generate bytecode that is compatible with the Runic ContractFactory:

```typescript
import { Runic } from "runic-sdk";


const rdk = new Runic();
rdk.validateSchema(applicationSchema);

const bytecode = rdk.generateBytecode(applicationSchema);
```

Here's an example of the `deployApplication` function prototype for the ContractFactory smart contract:

```solidity
function generateContract(RunicSchema schema, RunicContractOpts opts) public returns (bytes memory bytecode);
function deployApplication(bytes32 scopeName, bytes memory bytecode) public returns (address metaNFT);
```

The `deployApplication` function takes two arguments: the name of the application scope (`scopeName`) and the bytecode generated from the Runic Developer Kit or from the factory contract (`bytecode`). It returns the address of the newly-created Meta NFT contract (`metaNFT`) and adds default scoped permissions for the NFT contract.

By using the verified factory contract or Runic Developer Kit to validate the schema and generate bytecode, and then calling the `deployApplication` function on the ContractFactory smart contract, developers can easily create new Meta NFT and Fragment NFT contracts that are compatible with the Runic Protocol and enforced by the FragmentManager.


