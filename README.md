# Step-by-step hardhat config set up

First of all we need to install hardhat! (obviuslly lol)

```shell
npm i --dev hardhat
```

Then run hardhat, you could do with npx or `yarn add hardhat`

```shell
npx hardhat
```

Select `Create an empty hardhat.config.js` and paste this config into `hardhat.config.js` file.

```javascript
require("@nomiclabs/hardhat-waffle");
require("@nomiclabs/hardhat-etherscan");
require("hardhat-deploy");
require("solidity-coverage");
require("hardhat-gas-reporter");
require("hardhat-contract-sizer");
require("./tasks");
require("@appliedblockchain/chainlink-plugins-fund-link");
require("dotenv").config();

/**
 * @type import('hardhat/config').HardhatUserConfig
 */

const MAINNET_RPC_URL =
  process.env.MAINNET_RPC_URL ||
  process.env.ALCHEMY_MAINNET_RPC_URL ||
  "https://eth-mainnet.alchemyapi.io/v2/your-api-key";
const RINKEBY_RPC_URL =
  process.env.RINKEBY_RPC_URL ||
  "https://eth-rinkeby.alchemyapi.io/v2/your-api-key";
const KOVAN_RPC_URL =
  process.env.KOVAN_RPC_URL ||
  "https://eth-kovan.alchemyapi.io/v2/your-api-key";
const POLYGON_MAINNET_RPC_URL =
  process.env.POLYGON_MAINNET_RPC_URL ||
  "https://polygon-mainnet.alchemyapi.io/v2/your-api-key";
const PRIVATE_KEY = process.env.PRIVATE_KEY;
// optional
const MNEMONIC = process.env.MNEMONIC || "Your mnemonic";
const FORKING_BLOCK_NUMBER = process.env.FORKING_BLOCK_NUMBER;

// Your API key for Etherscan, obtain one at https://etherscan.io/
const ETHERSCAN_API_KEY =
  process.env.ETHERSCAN_API_KEY || "Your etherscan API key";
const POLYGONSCAN_API_KEY =
  process.env.POLYGONSCAN_API_KEY || "Your polygonscan API key";
const REPORT_GAS = process.env.REPORT_GAS || false;

module.exports = {
  defaultNetwork: "hardhat",
  networks: {
    hardhat: {
      // If you want to do some forking set `enabled` to true
      forking: {
        url: MAINNET_RPC_URL,
        blockNumber: FORKING_BLOCK_NUMBER,
        enabled: false,
      },
      chainId: 31337,
    },
    localhost: {
      chainId: 31337,
    },
    kovan: {
      url: KOVAN_RPC_URL,
      accounts: PRIVATE_KEY !== undefined ? [PRIVATE_KEY] : [],
      //accounts: {
      //     mnemonic: MNEMONIC,
      // },
      saveDeployments: true,
      chainId: 42,
    },
    rinkeby: {
      url: RINKEBY_RPC_URL,
      accounts: PRIVATE_KEY !== undefined ? [PRIVATE_KEY] : [],
      //   accounts: {
      //     mnemonic: MNEMONIC,
      //   },
      saveDeployments: true,
      chainId: 4,
    },
    mainnet: {
      url: MAINNET_RPC_URL,
      accounts: PRIVATE_KEY !== undefined ? [PRIVATE_KEY] : [],
      //   accounts: {
      //     mnemonic: MNEMONIC,
      //   },
      saveDeployments: true,
      chainId: 1,
    },
    polygon: {
      url: POLYGON_MAINNET_RPC_URL,
      accounts: PRIVATE_KEY !== undefined ? [PRIVATE_KEY] : [],
      saveDeployments: true,
      chainId: 137,
    },
  },
  etherscan: {
    // yarn hardhat verify --network <NETWORK> <CONTRACT_ADDRESS> <CONSTRUCTOR_PARAMETERS>
    apiKey: {
      rinkeby: ETHERSCAN_API_KEY,
      kovan: ETHERSCAN_API_KEY,
      polygon: POLYGONSCAN_API_KEY,
    },
  },
  gasReporter: {
    enabled: REPORT_GAS,
    currency: "USD",
    outputFile: "gas-report.txt",
    noColors: true,
    // coinmarketcap: process.env.COINMARKETCAP_API_KEY,
  },
  contractSizer: {
    runOnCompile: false,
    only: [
      "APIConsumer",
      "KeepersCounter",
      "PriceConsumerV3",
      "RandomNumberConsumerV2",
    ],
  },
  namedAccounts: {
    deployer: {
      default: 0, // here this will by default take the first account as deployer
      1: 0, // similarly on mainnet it will take the first account as deployer. Note though that depending on how hardhat network are configured, the account 0 on one network can be different than on another
    },
    feeCollector: {
      default: 1,
    },
  },
  solidity: {
    compilers: [
      {
        version: "0.8.7",
      },
      {
        version: "0.6.6",
      },
      {
        version: "0.4.24",
      },
    ],
  },
  mocha: {
    timeout: 200000, // 200 seconds max for running tests
  },
};
```

## Here is almost all the config you will need to have a fully basic set up in order to develope hardhat project.

---

Then run this on the shell.

```shell
npm i --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers @nomiclabs/hardhat-etherscan @nomiclabs/hardhat-waffle chai ethereum-waffle hardhat hardhat-contract-sizer hardhat-deploy hardhat-gas-reporter prettier prettier-plugin-solidity solhint solidity-coverage dotenv
```

If you notice, there's a lot of dotenv variables, so let create `.env` file.

```
KOVAN_RPC_URL='https://kovan.infura.io/v3/1234567890'
RINKEBY_RPC_URL='https://rinkeby.infura.io/v3/1234567890'
POLYGON_MAINNET_RPC_URL='https://rpc-mainnet.maticvigil.com'
ALCHEMY_MAINNET_RPC_URL="https://eth-mainnet.alchemyapi.io/v2/your-api-key"
PRIVATE_KEY='abcdefg'
MNEMONIC=
FORKING_BLOCK_NUMBER=
REPORT_GAS=true
COINMARKETCAP_API_KEY=https://coinmarketcap.com/api/documentation/v1/
ETHERSCAN_API_KEY=https://etherscan.io/apis
POLYGONSCAN_API_KEY=https://polygonscan.com/apis
```
I always like to have a `.prettierrc` config file. So let create it.

```prettier
{
    "tabWidth": 2,
    "useTabs": false,
    "semi": false,
    "singleQuote": true
}
```
If you gonna use prettier, is a good practice to have a `.prettierignore` file.

```
node_modules
artifacts
cache
coverage*
gasReporterOutput.json
package.json
img
.env
.*
README.md
coverage.json
deployments
```
DONT FORGET TO CREATE A `.GITIGNORE`!
---
```

```