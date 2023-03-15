## Runic Developer Kit

RDK is a typescript library that makes it easy to generate and deploy Runic protocol compatible Meta NFTs, fragments and application rules.

`let contract = await makeRunicContract(scopeName, application.json);`

TODO
* Install via npm/yarn
* Validate a schema
* Generate a contract
* Create an application (Scaffold)
* Generate contract bindings
* Deploy a contract

Document the "Runic Node"
* Realtime data
* Leaderboards
* Achievements
* Caching / querying
* Connects to blockchain

The Runic Node is run by game operators. The way this works is that the RDK creates the basic app with server and client. Server is an extension of the runic node and can include any logic the developer wants to add. It should typically write access to the contracts. The client code should talk to the blockchain and to the server (runic node). 

For image generation, there are a few ways that we can suggest to go:
* Run a server that watches for mint and change events and publishes a new image at the URL (so the contract already knows where the image will be, no new store required)
* Run a network w/consensus using a hash of the image generated - obviously much more complex but allows for incentives and serverless arch



