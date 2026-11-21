# Foundry Fund Me

![Smart Contract Development](https://img.shields.io/badge/Focus-Smart%20Contract%20Development-blue)
![Education](https://img.shields.io/badge/Learning-Cyfrin%20Updraft-orange)
![License](https://img.shields.io/badge/License-MIT%20License-yellow)

A Foundry smart contract project that accepts ETH funding from users and allows the contract owner to withdraw the collected balance.

The project includes unit and integration tests, a Chainlink price-feed mock for local development, deployment scripts, and funding/withdrawal interaction scripts.

Built following the [Cyfrin Updraft](https://updraft.cyfrin.io/) Foundry DeFi course by Patrick Collins.

## Contracts

| File | Description |
| --- | --- |
| `src/FundMe.sol` | Accepts ETH only when it meets the $5 USD minimum and lets the owner withdraw funds |
| `src/PriceConverter.sol` | Converts an ETH amount to USD using a Chainlink `AggregatorV3Interface` |
| `script/DeployFundMe.s.sol` | Deploys `FundMe` with the appropriate price feed |
| `script/HelperConfig.s.sol` | Uses Sepolia/mainnet feeds or deploys a mock feed on local networks |
| `script/Interactions.s.sol` | Funds and withdraws from the most recently deployed `FundMe` |

## Requirements

- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- Git

## Installation

```shell
git clone <repository-url>
cd foundry-fund-me
make install
```

`make install` adds Forge Std, Chainlink contracts, and Foundry DevOps to `lib/`.

## Build

```shell
forge build
```

## Test

Run the complete test suite:

```shell
forge test
```

Run a specific test file:

```shell
forge test --match-path test/unit/FundMeTest.t.sol
```

Format Solidity files:

```shell
forge fmt
```

Create a gas snapshot:

```shell
forge snapshot
```

## Local Deployment

Start Anvil:

```shell
make anvil
```

In another terminal, deploy the contract:

```shell
make deploy
```

The local deployment automatically uses `MockV3Aggregator` with 8 decimals and an initial ETH/USD price of $2000.

Fund and withdraw from the deployed contract:

```shell
make fund
make withdraw
```

Each script funds or withdraws 0.1 ETH. `make fund` and `make withdraw` broadcast from Anvil's default account.

## Sepolia Deployment

Create a `.env` file:

```dotenv
SEPOLIA_RPC_URL=<sepolia_rpc_url>
PRIVATE_KEY=<deployer_private_key>
ETHERSCAN_API_KEY=<etherscan_api_key>
```

Load and deploy:

```shell
source .env
make deploy ARGS="--network sepolia"
```

Sepolia uses the configured ETH/USD Chainlink feed. Verification requires a valid Etherscan API key.

## Network Configuration

`HelperConfig` selects a price feed by chain ID:

- Anvil/local networks: deploys `MockV3Aggregator`
- Sepolia (`11155111`): Chainlink ETH/USD feed
- Mainnet (`1`): Chainlink ETH/USD feed

## Make Targets

```shell
make build
make test
make snapshot
make format
make anvil
make deploy
make deploy ARGS="--network sepolia"
make fund
make withdraw
make clean
make help
```

## Further Reading

- [Foundry Book](https://book.getfoundry.sh/)

---

## License

[MIT](https://opensource.org/licenses/MIT)
