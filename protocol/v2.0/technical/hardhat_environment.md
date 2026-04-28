# hardhat_environment

This document details our current Hardhat development environment and explains why we continue to use Hardhat 2.x despite the availability of newer versions.

## 1. current_environment

We are currently using **Hardhat 2.22.x**. This version provides a stable foundation for our complex multi-chain DRM ecosystem.

- **Hardhat Version**: `^2.22.0`
- **Solidity Version**: `0.8.22`
- **EVM Version**: `paris`
- **Key Settings**:
    - `viaIR: true`: Enabled to handle complex contract inheritance and avoid "stack too deep" errors.
    - **Optimizer**: Enabled with 20 runs and aggressive Yul optimization steps (`dhfoDgvulfnTUtnIf`) to minimize contract size.
- **Key Plugins**:
    - `@nomicfoundation/hardhat-toolbox`: Standard toolkit for testing and deployment.
    - `@nomicfoundation/hardhat-ignition-ethers`: Used for declarative deployments.
    - `hardhat-contract-sizer`: Monitoring contract sizes against the 24KB limit.
    - `solidity-docgen`: Automated generation of technical documentation.

### 1.1 solidity_version_rationale

Our choice of **Solidity 0.8.22** and the **Paris** EVM version is deliberate:
1.  **EVM Compatibility**: We target the Paris EVM version (pre-Cancun) to ensure compatibility across all our target chains, including Elastos ESC and Arbitrum Sepolia, without relying on the `PUSH0` opcode (introduced in Shanghai/0.8.20 but often problematic in certain environments).
2.  **NatSpec Bug Avoidance**: Versions between 0.8.21 and 0.8.23 were selected to avoid specific NatSpec documentation bugs while maintaining modern language features.
3.  **Stability**: 0.8.22 has proven to be a highly stable version for our extensive use of `viaIR` and complex library integrations.

## 2. hardhat_3_experimentation_challenges

We recently attempted to migrate to **Hardhat 3 (pre-release/early versions)** to take advantage of new features like improved Ignition integration and EDR (Ethereum Development Runtime). However, the migration encountered several critical challenges that led us to revert to version 2.x.

### 2.1 esm_migration_friction

Hardhat 3 and its modern tooling strongly encourage or require a move to **ECMAScript Modules (ESM)** by setting `"type": "module"` in `package.json`.

- **Issue**: Our existing codebase, utility scripts, and many community plugins are built around **CommonJS**. Moving to ESM broke several of our custom internal scripts and required significant refactoring of our task definitions.
- **Decision**: The overhead of managing the ESM transition outweighed the immediate benefits of the version upgrade.

### 2.2 ignition_and_proxy_artifacts

While Hardhat Ignition is powerful, we found that in the Hardhat 3 environment, it had difficulty generating correct artifacts for **OpenZeppelin's Transparent Upgradeable Proxies**.

- **Workaround Attempted**: We had to create explicit wrapper contracts (e.g., `ProxyAdmin`, `TransparentUpgradeableProxy`, `UpgradeableBeacon`) within our own `contracts/proxy/` folder just to give the compiler/Ignition a concrete target to generate artifacts for.
- **Issue**: This added "dummy" code to our repository and increased the complexity of our inheritance tree just to satisfy tooling requirements.

### 2.3 plugin_ecosystem_stability

The plugin ecosystem for Hardhat 3 is still evolving. We found that shifting from the consolidated `@nomicfoundation/hardhat-toolbox` (v5) to separate plugins like `@nomicfoundation/hardhat-toolbox-mocha-ethers` introduced subtle configuration mismatches.

- **Example**: Network configurations in Hardhat 3 require new properties like `type: "http"` or `type: "edr-simulated"`, and access to environment variables changed to use `configVariable()`, which differed from our established `process.env` patterns.

## 3. summary_of_reversion

Due to the increased complexity in our core proxy infrastructure and the instability in our automation scripts, we made the strategic decision (in commit `cdcfe3a`) to **revert to Hardhat 2.x**. 

This allows us to:
1.  Maintain a clean contract structure without tooling-specific wrappers.
2.  Use a stable and mature plugin ecosystem.
3.  Focus on feature development rather than infrastructure refactoring.

