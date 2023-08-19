Steps to setup hyperlane on linea:

- clone the hyperlane-deploy repo from github from `https://github.com/DAOBridger/hyperlane-deploy`
- install the dependencies using `yarn install`
- copy the .env.template file to .env file using `cp .env.template .env` and fill the infura api key in it.
- put all the address of the transaction validators in the `config/multisig_ism.ts` file in the lineagoerli section.
- in the `config/start_blocks.ts` file, update the current block height of linea goerli network.
- deploy the hyperlane contracts using `yarn ts-node scripts/deploy-hyperlane.ts --local lineagoerli --remotes ${put the chains where you want to support cross chain messages} --key ${deployer's private key}` in our case `yarn ts-node scripts/deploy-hyperlane.ts --local lineagoerli --remotes goerli sepolia --key ${deployer's private key}`
