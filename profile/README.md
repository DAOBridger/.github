# DAOBridger: Cross Chain DAO Solution

DAOBridger is a robust solution designed to support cross-chain DAO functionalities. It allows users to create and vote on proposals across multiple chains without transferring their governance tokens. Follow the steps below to set up the necessary components.

## Setting up Hyperlane on Linea

### Step 1: Hyperlane Deployment

1. **Clone the Repository:** Clone the hyperlane-deploy repo from GitHub:
   ```bash
   git clone https://github.com/DAOBridger/hyperlane-deploy
   ```
2. **Install Dependencies:** Use Yarn to install dependencies:
   ```bash
   yarn install
   ```
3. **Configure Environment Variables:** Copy the .env.template file to .env and fill the Infura API key:
   ```bash
   cp .env.template .env
   ```
4. **Configure Validators:** Update the transaction validators' addresses in `config/multisig_ism.ts` within the lineagoerli section.
5. **Update Block Height:** Update the current block height of the linea goerli network in the `config/start_blocks.ts` file.
6. **Deploy Hyperlane Contracts:** Deploy with the following command (customize as needed):
   ```bash
   yarn ts-node scripts/deploy-hyperlane.ts --local lineagoerli --remotes goerli sepolia --key ${deployer's private key}
   ```

### Step 2: Hyperlane Validator and Relayer

1. **Clone the Binaries Repo:** Clone the hyperlane-binaries repo from GitHub:
   ```bash
   git clone https://github.com/DAOBridger/hyperlane-binaries
   ```
2. **Build Binaries:** Build the necessary binaries:
   ```bash
   cargo build --release --bin validator
   cargo build --release --bin relayer
   ```
3. **Configure Environment Files:** Copy and update the environment files:
   ```bash
   cp validator.env.template validator.env
   cp relayer.env.template relayer.env
   ```
4. **Run Validator and Relayer:** Run the following in separate terminals:
   ```bash
   env $(cat validator.env | grep -v "#" | xargs) ./target/release/validator
   env $(cat relayer.env | grep -v "#" | xargs) ./target/release/relayer
   ```

### Step 3: Deploying Governor Contracts

1. **Clone Contracts Repo:** Clone the contracts repository:
   ```bash
   git clone https://github.com/DAOBridger/contracts
   ```
2. **Install Dependencies:**
   ```bash
   yarn install
   ```
3. **Prepare Hyperlane Artifacts:** Delete the existing folder if needed and copy the artifacts:
   ```bash
   cp -r ../hyperlane-deploy/artifacts ./hyperlane-artifacts
   ```
4. **Configure Environment Variables:** Copy the .env.template file to .env and fill the Infura API key and the Mnemonic:
   ```bash
   cp .env.template .env
   ```
5. **Run Migration Scripts:** Perform the migration steps across the different networks as follows:
   ```bash
   npx truffle migrate --network lineagoerli --tokenAddress $tokenAddress
   npx truffle migrate --network goerli --tokenAddress $tokenAddress
   npx truffle migrate --network sepolia --tokenAddress $tokenAddress
   npx truffle migrate --network lineagoerli
   npx truffle migrate --network goerli
   ```

## Conclusion

By following the above steps, you will successfully deploy the governor contract to the linea testnet, goerli, and sepolia chains. Users will now be able to engage in cross-chain DAO activities, including creating proposals and voting without transferring their governance tokens between chains. If you need further assistance or have any questions, please refer to the documentation or contact support.

---
