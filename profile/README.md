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

By following the above steps, you will successfully deploy the governor contract to the linea testnet, goerli, and sepolia chains. Users will now be able to engage in cross-chain DAO activities, including creating proposals and voting without transferring their governance tokens between chains. If you need further assistance or have any questions, please contact contact@yashgoyal.dev.

---

## Deployed contract addresses

### Linea Goerli

- Hyperlane Mailbox: [`0xfa9057F23949A70d8c08d3bc7EaeBe6D593B7Ec3`](https://explorer.goerli.linea.build/address/0xfa9057F23949A70d8c08d3bc7EaeBe6D593B7Ec3)
- Hyperlane Interchain Gas Paymaster: [`0x8E93E48A05b21f9367e5a45fc037683040Fb81ed`](https://explorer.goerli.linea.build/address/0x8E93E48A05b21f9367e5a45fc037683040Fb81ed)
- Governor Contract: [`0x33d0360B653127424D4dEF3Aeeac16E30DE51050`](https://explorer.goerli.linea.build/address/0x33d0360B653127424D4dEF3Aeeac16E30DE51050)
- Governance Token: [`0xffF278B0c72AFa2ECC56efAba0E54Df8eA56965B`](https://explorer.goerli.linea.build/address/0xffF278B0c72AFa2ECC56efAba0E54Df8eA56965B)

### Goerli

- Governor Contract: [`0xc64d6cedC531da06F9688C11cF5288d8B0c19920`](https://goerli.etherscan.io/address/0xc64d6cedC531da06F9688C11cF5288d8B0c19920)
- Governance Token: [`0x3c9BA54eE1165e60830CA82d840A67D10E23FCD4`](https://goerli.etherscan.io/address/0x3c9BA54eE1165e60830CA82d840A67D10E23FCD4)

### Sepolia

- Governor Contract: [`0xc64d6cedC531da06F9688C11cF5288d8B0c19920`](https://sepolia.etherscan.io/address/0xc64d6cedC531da06F9688C11cF5288d8B0c19920)
- Governance Token: [`0x3c9BA54eE1165e60830CA82d840A67D10E23FCD4`](https://sepolia.etherscan.io/address/0x3c9BA54eE1165e60830CA82d840A67D10E23FCD4)

---

## Technologies Used

- **Linea**: I deployed the governor and the hyperlane contracts on linea.
- **Truffle**: I used truffle to develop, test, and deploy the governor and governance token contracts.
- **Infura**: I used infura to connect to linea goerli, goerli and sepolia networks to deploy and test the smart contracts.
- **Hyperlane**: I used hyperlane for cross chain communication between different governor contracts.

---
