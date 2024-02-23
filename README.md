## Prerequisites

1. Python
2. [Node.js](https://nodejs.org/en/download)
3. Git
4. `curl https://get.starkli.sh | sh`
- Check `starkli --version`
5. `curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | sh -s -- -v 0.6.1`
- Check `scarb --version`


## Setup

1. Set up an ArgentX wallet, choose Goerli Testnet, fund with https://faucet.goerli.starknet.io/ and deploy account
- Note `ARGENTX_ADDRESS` and `ARGENTX_PRIVATE_KEY`
2. If working in another folder: run `scarb init`
- Make sure to update `.gitignore`
3. Change [package] name in `Scarb.toml`
4. `python3 -i utils.py`
- `str_to_felt("MEMES Token")` - referred to later as `TOKEN_NAME_FELT`
- `str_to_felt("MEMES")` - referred to later as `TOKEN_SYMBOL_FELT`
5. `scarb build`
6. `mkdir -p starkli-wallets/deployer`

## Deployment

Note: make sure to change variable placeholders to their actual values

1. `starkli signer keystore from-key starkli-wallets/deployer/keystore.json`, enter `ARGENTX_PRIVATE_KEY`, choose and enter a password (referred to later as `KEYSTORE_PWD`)
2. Find an RPC endpoint for Goerli Testnet, e.g. https://starknet-goerli.g.alchemy.com/v2/your-api-key and run `export STARKNET_RPC=<RPC_ENDPOINT>`
3. `starkli account fetch <ARGENTX_ADDRESS> --output ./starkli-wallets/deployer/account.json`
4. `export STARKNET_ACCOUNT=./starkli-wallets/deployer/account.json`
5. `export STARKNET_KEYSTORE=./starkli-wallets/deployer/keystore.json`
6. Find the `.sierra.json` file from `target/dev` and run `starkli declare target/dev/<FILE_NAME>.sierra.json --compiler-version=2.1.0`
- Note the `CLASS_HASH` from "Class hash declared"
7. Deploy your contract with `starkli deploy <CLASS_HASH> <ARGENTX_ADDRESS> <TOKEN_NAME_FELT> <TOKEN_SYMBOL_FELT> 18 1000000000000000 0`. Last 3 numbers account for decimals, initial supply and minting fee. Note `CONTRACT_ADDRESS` and `CONTRACT_DEPLOY_TX`
8. Have fun apeing!
