## Patchwork Protocol Overview

The Patchwork Protocol provides a standardized way to create, manage, and manipulate complex relationships and data structures for NFTs. It ensures the integrity and security of NFT operations, including assignment and unassignment of fragments, batch assignments, transfers, lock states, and ownership updates.

### Scopes

A scope in the Patchwork Protocol is a robust data structure designed to manage access control and settings for various entities. A scope can correspond to a specific product, a group of entities within a product, a company, and more. By categorizing and managing NFTs under different scopes, the protocol provides fine-grained access control and ensures data integrity for various business scenarios.

A scope defines the owner and optional operators who have the permission to perform certain operations, such as assigning or unassigning NFT fragments. It can also be configured to allow users to assign their own NFTs within the scope. This user-centric design makes scopes a versatile tool for managing access rights in different contexts.

### Assigning and Unassigning NFT Fragments

The protocol provides mechanisms to assign an NFT fragment to a target NFT and to unassign it. These operations are highly secured, ensuring only authorized parties can perform them. The protocol also ensures the proper management of scope, which is essential to maintain the integrity of NFT relationships. In addition to single assignments, batch assignment operations are also supported for efficiency and convenience.

### Transfer and Ownership Operations

Transfers in the Patchwork Protocol are carried out in strict compliance with predefined rules and actions, preserving the integrity of NFT assignments. Before a transfer occurs, the protocol verifies whether the token is assignable and checks for any conditions that could block the transfer (like if the token is soulbound). If the checks pass, the protocol handles the transfer, ensuring that any effects of the transfer propagate correctly to assigned NFTs.

In the Patchwork Protocol, when an NFT is transferred, any NFTs assigned to it will be owned by the new owner. Although the ownership status of these assigned NFTs will not be immediately updated in storage due to gas efficiency considerations, events will be sent out to update any listening indexers, ensuring the system remains accurate and up-to-date. 

Additionally, the correct owner will always be returned in the `ownerOf()` call, even if the storage hasn't been updated yet.

### Ownership Updates

The protocol also supports operations to update the storage of the ownership tree of a specific Patchwork NFT. Whether the NFT supports the IPatchworkAssignable interface or the IPatchworkPatch interface, the ownership update is handled correctly, reflecting changes throughout the NFT's relationships and ensuring data integrity.

### Locks

To mitigate fraud risks and ensure the integrity of NFT relationships, the Patchwork Protocol utilizes a locking mechanism. Before certain operations like assignments and transfers, the protocol checks whether the involved NFTs are locked or not.

A lock uses a nonce to protect the NFT relationships from being maliciously altered. Specifically, it can prevent a seller from removing relationships from an NFT before a sale. In this way, locks are instrumental in maintaining the authenticity and trustworthiness of NFT transactions in the protocol.
