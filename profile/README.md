Steps to setup hyperlane on linea:

- clone the hyperlane-deploy repo from github from `https://github.com/DAOBridger/hyperlane-deploy`
- install the dependencies using `yarn install`
- copy the .env.template file to .env file using `cp .env.template .env` and fill the infura api key in it.
- put all the address of the transaction validators in the `config/multisig_ism.ts` file in the lineagoerli section.
- in the `config/start_blocks.ts` file, update the current block height of linea goerli network.
- deploy the hyperlane contracts using `yarn ts-node scripts/deploy-hyperlane.ts --local lineagoerli --remotes ${put the chains where you want to support cross chain messages} --key ${deployer's private key}` in our case `yarn ts-node scripts/deploy-hyperlane.ts --local lineagoerli --remotes goerli sepolia --key ${deployer's private key}`

Steps to run the hyperlane validator and relayer:

- clone the hyperlane-binaries repo from github from `https://github.com/DAOBridger/hyperlane-binaries`
- build the binaries using `cargo build --release --bin validator` & `cargo build --release --bin relayer`
- copy the validator.env.template file to validator.env file using `cp validator.env.template validator.env` and fill/update all env variables.
- copy the relayer.env.template file to relayer.env file using `cp relayer.env.template relayer.env` and fill/update all env variables.
- run the validator in terminal 1 using `env $(cat validator.env | grep -v "#" | xargs) ./target/release/validator` and relayer in terminal 2 using `env $(cat relayer.env | grep -v "#" | xargs) ./target/release/relayer`

Steps to deploy the governor contracts:

- clone the contracts repo from github from `https://github.com/DAOBridger/contracts`
- install the dependencies using `yarn install`
- delete the `hyperlane-artifacts` folder if already exists and then copy the artificats folder generated in the hyperlane-deploy repo after running the above steps using `cp -r ../hyperlane-deploy/artifacts ./hyperlane-artifacts`
- run the migration script step 1 using `npx truffle migrate --network lineagoerli --tokenAddress $tokenAddress` by putting the governance token contract address on the lineagoerli network 
- run the migration script step 2 using `npx truffle migrate --network goerli --tokenAddress $tokenAddress` by putting the governance token contract address on the goerli network
- run the migration script step 3 using `npx truffle migrate --network sepolia --tokenAddress $tokenAddress` by putting the governance token contract address on the sepolia network
- run the migration script step 4 using `npx truffle migrate --network lineagoerli`
- run the migration script step 5 using `npx truffle migrate --network goerli`

After running the above setup, the governor contract will be deployed to linea testnet, goerli and sepolia chains, and users will be able to create any proposal on any one of these chains, and all the governance token holders will be able to vote on the proposals without transferring their governance tokens from one chains to another. 
