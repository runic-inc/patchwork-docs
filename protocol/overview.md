## Patchwork Protocol

The heart of the protocol is made up primarily of two contracts: The Fragment Manager for orchestrating fragments, and the Contract Factory for generating Patchwork compatible Patches and Fragment NFTS.

### FragmentManager

The FragmentManager is a key component of the Patchwork Protocol that enforces data integrity between fragments and the NFTs they are assigned to. Some ways in which it does this include:

- A fragment can only ever be owned by one Patch.
- Only the Patch that owns the Fragment can perform operations on the Fragment NFT, unless the Patch owner has delegated write/transfer access to another Patch or application scope.
- The FragmentManager enforces whitelists for which other application scopes can modify fragment state.
- The FragmentManager enforces that some Fragments were specified as "Soulbound" to the Patch, and thus transfers are always forbidden.

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

// Mint a new patch in a scope, soulbound to a non-Patchwork NFT
function createPatchNFT(string calldata scopeName, address owner, address originalNFTAddress, uint originalNFTTokenId, address patchAddress) public returns (uint256 tokenId);

// Assign fragment NFT to Patch
function assignNFT(string calldata scopeName, address fragment, uint fragmentTokenId, address target, uint targetTokenId);
function unassignNFT(string calldata scopeName, address fragment, uint fragmentTokenId);
function bulkAssignNFT(string calldata scopeName, address[8] calldata artifacts, uint[8] calldata tokenIds, address target, uint targetTokenId);

// Check if nft is allowed to be transferred
function checkTransfer(address nft, uint256 tokenId);
```

#### Sample Code
```solidity
prot = new PatchworkProtocol();          // Fragment Manager
originalNFT = new OriginalNFT();         // e.g., BAYC
appPatchNFT = new ApplicationPatchNFT(); // pdk generated Patch Contract
fragmentNFT = new FragmentNFT();         // pdk generated Fragment NFT Contract

scopeName = "NFT Fighter";

prot.claimScope(scopeName);
uint256 patchTokenId = prot.createPatchNFT(scopeName, originalNFT.ownerOf(1), address(originalNFT), 1, address(appPatchNFT));

// Register fragmentNFT to appPatchNFT
uint8 id = appPatchNFT.registerReferenceAddress(address(fragmentNFT));

// Init storage 
// TODO: This step goes away once we add intialization if it hasn't occured yet the first time assignNFT is called
AppPatchNFTMetadata memory data;
appPatchNFT.storeMetadata(1, data);

//Assign a fragment to a Patch
prot.assignNFT(scopeName, address(fragmenttNFT), 1, address(appPatchNFT), patchTokenId);

```


### ContractFactory

The ContractFactory is a tool that allows developers to automatically create Patches and Fragment NFTs for their applications. The workflow for using the ContractFactory is as follows:

1. The user expresses their application and fragment types and rules in JSON format.
2. The user uses the [Patchwork Developer Kit (PDK)](pdk/pdk.md?id=patchwork-developer-kit-pdk-technical-document) to [validate the schema](pdk/pdk.md??id=schema-validation) and [generate a Solidity contract](pdk/pdk.md?id=contract-generation) that can be deployed with the Patchwork ContractFactory.
3. The user calls a function `deployApplication` on the ContractFactory smart contract, providing an application scope name and the bytecode generated from the Patchwork Developer Kit.
4. The ContractFactory creates Patch and Fragment NFT contracts as specified in the application schema, with rules that are enforced by the FragmentManager.

Here's an example of how to use the Patchwork Developer Kit (pdk) to generate a contract that is compatible with the Patchwork ContractFactory that can then be compiled to bytecode using solc:

```bash
pdk generate-contract schema.json -o contract.sol
```

Here's an example of the `deployApplication` function prototype for the ContractFactory smart contract:

```solidity
function generateContract(PatchworkSchema schema, PatchworkContractOpts opts) public returns (bytes memory bytecode);
function deployApplication(bytes32 scopeName, bytes memory bytecode) public returns (address patch);
```

The `deployApplication` function takes two arguments: the name of the application scope (`scopeName`) and the bytecode generated from the Patchwork Developer Kit or from the factory contract (`bytecode`). It returns the address of the newly-created Patch contract and adds default scoped permissions for the NFT contract.

By using the verified factory contract or Patchwork Developer Kit to validate the schema and generate bytecode, and then calling the `deployApplication` function on the ContractFactory smart contract, developers can easily create new Patch and Fragment NFT contracts that are compatible with the Patchwork Protocol and enforced by the FragmentManager.


