## Runic Developer Kit (RDK) Technical Document

The Runic Developer Kit (RDK) is a powerful tool for developers who want to create and deploy applications using the Runic Protocol. This document outlines the process for installing the RDK, validating a schema, generating a contract, creating an application, generating contract bindings, and deploying a contract.

### Table of Contents

1. [Installation](#installation)
2. [Schema Validation](#schema-validation)
3. [Contract Generation](#contract-generation)
4. [Application Creation (Scaffolding)](#application-creation)
5. [Generating Contract Bindings](#generating-contract-bindings)
6. [Contract Deployment](#contract-deployment)
7. [Runic Node](#runic-node)

### Installation

To install the Runic Developer Kit (RDK) using npm or yarn, follow these steps:

#### Using npm

```bash
npm install -g runic-developer-kit
```

#### Using yarn

```bash
yarn global add runic-developer-kit
```

### Schema Validation

To validate a schema using RDK, follow these steps:

1. Create a JSON file containing your schema, e.g., `schema.json`.
2. Use the RDK to validate the schema:

```bash
rdk validate-schema schema.json
```

If the schema is valid, the RDK will output a confirmation message.

### Contract Generation

To generate a contract using RDK, follow these steps:

1. Ensure you have a valid schema JSON file.
2. Use the RDK to generate a contract:

```bash
rdk generate-contract schema.json -o contract.sol
```

The RDK will output the generated contract in Solidity format.

### Application Creation (Scaffolding)

To create a new application using RDK, follow these steps:

1. Run the following command:

```bash
rdk create-app myApp
```

2. This command will scaffold a new application in a directory called `myApp`, including the necessary server and client code.

### Generating Contract Bindings

To generate contract bindings using RDK, follow these steps:

1. Ensure you have a Solidity contract file (e.g., `contract.sol`).
2. Use the RDK to generate contract bindings:

```bash
rdk generate-bindings contract.sol -o contractBindings.ts
```

The RDK will output TypeScript bindings for your contract.

### Contract Deployment

To deploy a contract using RDK, follow these steps:

1. Ensure you have a valid Solidity contract file and corresponding contract bindings.
2. Configure your deployment settings in the `runic.config.js` file:

```javascript
module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*",
    },
    mainnet: {
      provider: () => new HDWalletProvider(mnemonic, infuraApiKey),
      network_id: 1,
      gas: 4500000,
      gasPrice: 10000000000,
    },
  },
};
```

3. Deploy your contract using the RDK:

```bash
rdk deploy contract.sol --network mainnet
```

The RDK will deploy your contract to the specified network and output the contract address.

### Runic Node

The Runic Node is a powerful Node.js module run by game operators, designed to enable features such as real-time data, caching/querying of blockchain data, leaderboards, achievements, and image generation for Runic Protocol applications.

#### Server and Client

The RDK creates the basic app with server and client components. The server is an extension of the Runic Node and can include any logic the developer wants to add. It should typically have write access to the contracts. The client code should communicate with the blockchain and the server (Runic Node).

#### Image Generation

There are a few ways to handle image generation:

1. **Server-side image generation:** Run a server that watches for mint and change events and publishes a new image at a predefined URL. The contract already knows where the image will be, so no additional storage is required.

```javascript
// Server-side code to watch for mint and change events
contract.events.TokenURIUpdated()
  .on('data', (event) => {
    // Generate and publish the new image at the predefined URL
  });
```

2. **Network with consensus:** Run a network with consensus using a hash of the image generated. This approach is more complex but allows for incentives and a serverless architecture.

Assuming that the leaderboard and achievement systems are already implemented in the Runic Node, this section demonstrates how to use the APIs to create a leaderboard, add scores, query the leaderboard, create an achievement, and unlock an achievement using server and client-side code.

#### Leaderboards API

##### Server-side: Add a Score to the Leaderboard

To add a score to the leaderboard, use the `addScore` function provided by the Runic Node leaderboard API.

```javascript
// Add a score to the leaderboard
runicNode.leaderboards.addScore(leaderboardId, playerAddress, score);
```

##### Server-side: Update leaderboard based on blockchain events

Runic Node allows developers to create leaderboards that rank players based on various criteria, such as scores or achievements. By listening to events emitted by the Runic Protocol smart contracts, the Runic Node can update the leaderboard data in real-time and serve it to the clients.

```javascript
// Server-side code to listen for score updates and update the leaderboard
contract.events.ScoreUpdated()
  .on('data', (event) => {
    // Update the leaderboard data based on the new score
  });
```

##### Client-side: Query the Leaderboard

To query the leaderboard from the client-side, use the `getLeaderboard` function provided by the Runic Node leaderboard API.

```javascript
// Fetch the leaderboard data from the server
async function fetchLeaderboard(leaderboardId) {
  const response = await fetch(`/api/leaderboards/${leaderboardId}`);
  const leaderboardData = await response.json();
  return leaderboardData;
}

// Usage example
fetchLeaderboard(1).then(leaderboardData => {
  console.log(leaderboardData);
});
```

#### Achievements API

##### Server-side: Create an Achievement

To create an achievement, use the `createAchievement` function provided by the Runic Node achievement API.

```javascript
// Define an achievement
const achievement = {
  id: 1,
  name: 'First Achievement',
  description: 'Complete the first level',
  requiredScore: 10,
};

// Create the achievement
runicNode.achievements.createAchievement(achievement);
```

##### Server-side: Unlock an Achievement

To unlock an achievement for a user, use the `unlockAchievement` function provided by the Runic Node achievement API.

```javascript
// Unlock an achievement for a user
runicNode.achievements.unlockAchievement(achievementId, playerAddress);
```

##### Server-side: Unlock an Achievement based on blockchain events

Achievements can be integrated into Runic Protocol applications using the Runic Node. Developers can define custom achievements and the criteria for earning them. The Runic Node listens to events emitted by the Runic Protocol smart contracts, updates the players' achievements, and notifies the clients when they have earned new achievements.

```javascript
// Server-side code to listen for achievement-related events
contract.events.AchievementEarned()
  .on('data', (event) => {
    // Update the player's achievements and notify the client
    runicNode.achievements.unlockAchievement(event.achievementId, event.playerAddress);
  });
```

##### Client-side: Query Achievements

To query the achievements of a user from the client-side, use the `getAchievements` function provided by the Runic Node achievement API.

```javascript
// Fetch the achievements data from the server
async function fetchAchievements(playerAddress) {
  const response = await fetch(`/api/achievements/${playerAddress}`);
  const achievementsData = await response.json();
  return achievementsData;
}

// Usage example
fetchAchievements('0x123...').then(achievementsData => {
  console.log(achievementsData);
});
```


