# Repo setup


# Taiko audit details
- Total Prize Pool: $140,000 in USDC
  - HM awards: $103,900 in USDC
  - Analysis awards: $5,800 in USDC
  - QA awards: $2,900 in USDC
  - Gas awards: $2,900 in USDC
  - Judge awards: $15,000 in USDC
  - Lookout awards: 9,000 in USDC
  - Scout awards: $500 in USDC
 
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2024-03-taiko/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts March 4, 2024 20:00 UTC
- Ends March 25, 2024 20:00 UTC

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-03-taiko/blob/main/4naly3er-report.md).



_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

- All code inside contracts/team/ uses ERC20's `transferFrom` instead of `safeTransferFrom`. This is known and acceptable.

# Overview

Taiko is a Based rollup. You can learn about Based rollup by following links below:

- https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016
- https://taiko.mirror.xyz/7dfMydX1FqEx9_sOvhRt3V8hJksKSIWjzhCVu7FyMZU

This version of the Taiko protocol is also known as Based Contestable Rollup, or BCR. You can learn about BCR design using these links:

- https://taiko.mirror.xyz/Z4I5ZhreGkyfdaL5I9P0Rj0DNX4zaWFmcws-0CVMJ2A
- https://www.youtube.com/watch?v=A6ncZirXPfc


There are also a few documents in [packages/protocol/docs](packages/protocol/docs) that you can take a look. We are working on making them into our official documentation before mainnet launch. Sorry that these files are not well maintained but I think they may give you some extra insights into BCR's design and/or implementation.

A built-in cross-layer communication machinmims is also included in the core protocol code to faciliate cross-multiple layer communcation. We call it multi-hop bridging. You can learn about the basic design in [`this file`](packages/protocol/docs/multihop_bridging_deployment.md).


## Links

- **Previous audits:** 

An [older version](https://github.com/taikoxyz/taiko-mono/releases/tag/protocol-v0.16.0%2Bbcr.locked) of the protocol has been audited Sigma Prime ([report](packages/protocol/audit/sigma_prime_taiko_smart_contract_security_assessment_report_v2_0.pdf)). The corresponding bridge code has been audited by QuillAudits ([report](packages/protocol/audit/quill_audits_taiko_smart_contract_audit_report.pdf)).

Please see [a list of changes](https://github.com/taikoxyz/taiko-mono/releases/tag/protocol-v1.0.0) between the audited version and the current v1.0.0 release.

- **Documentation:** https://docs.taiko.xyz
- **Website:** https://taiko.xyz
- **Twitter:** https://twitter.com/taikoxyz
- **Discord:** https://discord.gg/taikoxyz


# Scope

| Contract                                                    | SLOC | Purpose                                                           | Libraries used                                 | Priority |
|--------------------------------------------------------------|------|-------------------------------------------------------------------|--------------------------------------|----------|
| [contracts/common/IAddressManager.sol](packages/contracts/common/IAddressManager.sol)                                     | 4    | Mananges registered addresses                                    |                                      |          |
| [contracts/common/AddressManager.sol](packages/contracts/common/AddressManager.sol)                                         | 35   | Mananges registered addresses                                    |                                      |          |
| [contracts/common/IAddressResolver.sol](packages/contracts/common/IAddressResolver.sol)                                     | 18   | Resolve registered addresses                                     |                                      |          |
| [contracts/common/AddressResolver.sol](packages/contracts/common/AddressResolver.sol)                                       | 60   | Resolve registered addresses                                     | @openzeppelin                       |          |
| [contracts/common/EssentialContract.sol](packages/contracts/common/EssentialContract.sol)                                   | 91   | Base class for all proxied contracts (ERC1967Proxy)              | @openzeppelin                       | 🔥High     |
| [contracts/libs/Lib4844.sol](packages/contracts/libs/Lib4844.sol)                                                           | 38   | Helper function for validating a blob in ZK circuits            |                                      | Medium   |
| [contracts/libs/LibAddress.sol](packages/contracts/libs/LibAddress.sol)                                                     | 55   |                                                                   | /thirdparty/nomad-xyz, @openzeppelin | Medium   |
| [contracts/libs/LibMath.sol](packages/contracts/libs/LibMath.sol)                                                             | 9    |                                                                   |                                      |          |
| [contracts/libs/LibTrieProof.sol](packages/contracts/libs/LibTrieProof.sol)                                                   | 36   | Merkle Trie proof verification                                   | /thirdparty/optimism/               | 🔥High     |
| [contracts/L1/gov/TaikoGovernor.sol](packages/contracts/L1/gov/TaikoGovernor.sol)                                           | 121  |                                                                   | @openzeppelin                       | Medium   |
| [contracts/L1/gov/TaikoTimelockController.sol](packages/contracts/L1/gov/TaikoTimelockController.sol)                       | 14   |                                                                   | @openzeppelin                       | Medium   |
| [contracts/L1/hooks/IHook.sol](packages/contracts/L1/hooks/IHook.sol)                                                       | 11   | Hook interface                                                    |                                      |          |
| [contracts/L1/hooks/AssignmentHook.sol](packages/contracts/L1/hooks/AssignmentHook.sol)                                     | 119  | Proving job verification hook                                    | @openzeppelin                       | Medium   |
| [contracts/L1/ITaikoL1.sol](packages/contracts/L1/ITaikoL1.sol)                                                             | 14   |                                                                   |                                      |          |
| [contracts/L1/libs/LibDepositing.sol](packages/contracts/L1/libs/LibDepositing.sol)                                         | 97   | Core logics for depositing Ether to L2                          |                                      | 🔥High     |
| [contracts/L1/libs/LibProposing.sol](packages/contracts/L1/libs/LibProposing.sol)                                           | 188  | Core logics for proposing Taiko blocks                           |                                      | 🔥High     |
| [contracts/L1/libs/LibProving.sol](packages/contracts/L1/libs/LibProving.sol)                                               | 243  | Core logics for proving Taiko blocks and contesting proofs       |                                      | 🔥High     |
| [contracts/L1/libs/LibUtils.sol](packages/contracts/L1/libs/LibUtils.sol)                                                   | 61   |                                                                   |                                      | Medium   |
| [contracts/L1/libs/LibVerifying.sol](packages/contracts/L1/libs/LibVerifying.sol)                                           | 162  | Core logics for verifying Taiko blocks                           |                                      | 🔥High     |
| [contracts/L1/provers/GuardianProver.sol](packages/contracts/L1/provers/GuardianProver.sol)                                 | 32   | The top tier prover                                               |                                      | Medium   |
| [contracts/L1/provers/Guardians.sol](packages/contracts/L1/provers/Guardians.sol)                                           | 79   | A multisig to authorize the Guardian prover                      |                                      | Medium   |
| [contracts/L1/TaikoData.sol](packages/contracts/L1/TaikoData.sol)                                                           | 124  | Core Taiko protocol data, needs to support upgradeability        |                                      |          |
| [contracts/L1/TaikoErrors.sol](packages/contracts/L1/TaikoErrors.sol)                                                       | 34   |                                                                   |                                      |          |
| [contracts/L1/TaikoEvents.sol](packages/contracts/L1/TaikoEvents.sol)                                                       | 37   | Events whose definations must match those in L1/libs/Lib***.sol. |                                      |          |
| [contracts/L1/TaikoL1.sol](packages/contracts/L1/TaikoL1.sol)                                                               | 149  | Taiko BCR main contract on L1 (Ethereum)                         |                                      | Medium   |
| [contracts/L1/TaikoToken.sol](packages/contracts/L1/TaikoToken.sol)                                                         | 85   | The protocol token. TaikoToken is on L1.                         |                                      | Medium   |
| [contracts/L1/tiers/ITierProvider.sol](packages/contracts/L1/tiers/ITierProvider.sol)                                       | 21   |                                                       |                                      |          |
| [contracts/L1/tiers/DevnetTierProvider.sol](packages/contracts/L1/tiers/DevnetTierProvider.sol)                             | 40   |                                                       |                                      |          |
| [contracts/L1/tiers/MainnetTierProvider.sol](packages/contracts/L1/tiers/MainnetTierProvider.sol)                           | 52   |                                                       |                                      |          |
| [contracts/L1/tiers/TestnetTierProvider.sol](packages/contracts/L1/tiers/TestnetTierProvider.sol)                           | 52   |                                                       |                                      |          |
| [contracts/L2/CrossChainOwned.sol](packages/contracts/L2/CrossChainOwned.sol)                                               | 45   |                                                                   |                                      |          |
| [contracts/L2/Lib1559Math.sol](packages/contracts/L2/Lib1559Math.sol)                                                       | 33   | Math to calculate EIP-1559 base fee                              | /thirdparty/solmate                 | 🔥High     |
| [contracts/L2/TaikoL2.sol](packages/contracts/L2/TaikoL2.sol)                                                               | 180  |                                                                   |                                      | 🔥High     |
| [contracts/L2/TaikoL2EIP1559Configurable.sol](packages/contracts/L2/TaikoL2EIP1559Configurable.sol)                         | 25   |                                                                   |                                      | Medium   |
| [contracts/signal/ISignalService.sol](packages/contracts/signal/ISignalService.sol)                                         | 68   | Signal service interface                                          |                                      |          |
| [contracts/signal/ISignalService.sol](packages/contracts/signal/ISignalService.sol)             | 68   | Signal service interface                                          |                                  |          |
| [contracts/signal/LibSignals.sol](packages/contracts/signal/LibSignals.sol)                       | 5    |                                                                   |                                  |          |
| [contracts/signal/SignalService.sol](packages/contracts/signal/SignalService.sol)                | 246  | Signal service that supports app-agnostic, arbitrary message bridging cross multiple Taiko L2/L3s. |                                  | 🔥High     |
| [contracts/bridge/IBridge.sol](packages/contracts/bridge/IBridge.sol)                             | 63   | Bridge interface                                                  |                                  |          |
| [contracts/bridge/Bridge.sol](packages/contracts/bridge/Bridge.sol)                               | 404  | Contract that supports cross-layer message calls                  | /thirdparty/nomad-xyz, @openzeppelin | 🔥High     |
| [contracts/tokenvault/adapters/USDCAdapter.sol](packages/contracts/tokenvault/adapters/USDCAdapter.sol) | 22   | Native USDC contract adapter                                      | /thirdparty/nomad-xyz,@openzeppelin | Medium   |
| [contracts/tokenvault/BridgedERC20.sol](packages/contracts/tokenvault/BridgedERC20.sol)           | 128  | Bridged ERC20 token                                               | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BridgedERC20Base.sol](packages/contracts/tokenvault/BridgedERC20Base.sol)   | 56   | Bridged ERC20 token                                               | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BridgedERC721.sol](packages/contracts/tokenvault/BridgedERC721.sol)         | 83   | Bridged ERC721 token                                              | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BridgedERC1155.sol](packages/contracts/tokenvault/BridgedERC1155.sol)       | 91   | Bridged ERC1155 token                                             | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BaseNFTVault.sol](packages/contracts/tokenvault/BaseNFTVault.sol)           | 79   | Base NFT vault                                                    | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BaseVault.sol](packages/contracts/tokenvault/BaseVault.sol)                 | 46   | Base vault                                                        | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/ERC1155Vault.sol](packages/contracts/tokenvault/ERC1155Vault.sol)           | 230  | ERC1155 vault                                                     | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/ERC20Vault.sol](packages/contracts/tokenvault/ERC20Vault.sol)               | 291  | ERC20 vault                                                       | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/ERC721Vault.sol](packages/contracts/tokenvault/ERC721Vault.sol)             | 193  | ERC721 vault                                                      | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/IBridgedERC20.sol](packages/contracts/tokenvault/IBridgedERC20.sol)         | 7    |                                                                   |                                  |          |
| [contracts/tokenvault/LibBridgedToken.sol](packages/contracts/tokenvault/LibBridgedToken.sol)     | 52   |                                                                   |                                  | Medium   |
| [contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol](packages/contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol) | 36   | Lib to avoid memory-copy attach            | nomad-xyz             |          |
| [contracts/thirdparty/optimism/Bytes.sol](packages/contracts/thirdparty/optimism/Bytes.sol)                       | 70   | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/rlp/RLPReader.sol](packages/contracts/thirdparty/optimism/rlp/RLPReader.sol)       | 191  | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/rlp/RLPWriter.sol](packages/contracts/thirdparty/optimism/rlp/RLPWriter.sol)       | 44   | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/trie/MerkleTrie.sol](packages/contracts/thirdparty/optimism/trie/MerkleTrie.sol)    | 148  | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol](packages/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol) | 32   | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/solmate/LibFixedPointMath.sol](packages/contracts/thirdparty/solmate/LibFixedPointMath.sol) | 35   |                                            |                        |          |
| [contracts/verifiers/IVerifier.sol](packages/contracts/verifiers/IVerifier.sol)                                   | 19   |                                            |                        | Medium   |
| [contracts/verifiers/GuardianVerifier.sol](packages/contracts/verifiers/GuardianVerifier.sol)                       | 23   | Verifier for guardian prover              |                        | Medium   |
| [contracts/verifiers/SgxVerifier.sol](packages/contracts/verifiers/SgxVerifier.sol)                                 | 137  | Verifier for SXG prover                   |                        | 🔥High     |
| [contracts/team/airdrop/ERC20Airdrop.sol](packages/contracts/team/airdrop/ERC20Airdrop.sol)                         | 40   | TKO airdrop contract                      |                        | Medium   |
| [contracts/team/airdrop/ERC20Airdrop2.sol](packages/contracts/team/airdrop/ERC20Airdrop2.sol)                       | 61   |                                            |                        |          |
| [contracts/team/airdrop/ERC721Airdrop.sol](packages/contracts/team/airdrop/ERC721Airdrop.sol)                       | 37   |                                            |                        |          |
| [contracts/team/airdrop/MerkleClaimable.sol](packages/contracts/team/airdrop/MerkleClaimable.sol)                   | 65   |                                            |                        | Medium   |
| [contracts/team/TimelockTokenPool.sol](packages/contracts/team/TimelockTokenPool.sol)                         | 159  | Employeee and investor unlock contract |      | 🔥High     |
| [contracts/automata-attestation/AutomataDcapV3Attestation.sol](packages/contracts/automata-attestation/AutomataDcapV3Attestation.sol) | 360  | SGX supporting code |      |          |
| [contracts/automata-attestation/interfaces/IAttestation.sol](packages/contracts/automata-attestation/interfaces/IAttestation.sol) | 8    | SGX supporting code |      |          |
| [contracts/automata-attestation/interfaces/ISigVerifyLib.sol](packages/contracts/automata-attestation/interfaces/ISigVerifyLib.sol) | 68   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/EnclaveIdStruct.sol](packages/contracts/automata-attestation/lib/EnclaveIdStruct.sol) | 23   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/interfaces/IPEMCertChainLib.sol](packages/contracts/automata-attestation/lib/interfaces/IPEMCertChainLib.sol) | 42   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/PEMCertChainLib.sol](packages/contracts/automata-attestation/lib/PEMCertChainLib.sol) | 279  | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol](packages/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol) | 246  | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol](packages/contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol) | 48   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/TCBInfoStruct.sol](packages/contracts/automata-attestation/lib/TCBInfoStruct.sol) | 23   | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/Asn1Decode.sol](packages/contracts/automata-attestation/utils/Asn1Decode.sol) | 107  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/BytesUtils.sol](packages/contracts/automata-attestation/utils/BytesUtils.sol) | 225  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/RsaVerify.sol](packages/contracts/automata-attestation/utils/RsaVerify.sol) | 208  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/SHA1.sol](packages/contracts/automata-attestation/utils/SHA1.sol)       | 166  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/SigVerifyLib.sol](packages/contracts/automata-attestation/utils/SigVerifyLib.sol) | 114  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/X509DateUtils.sol](packages/contracts/automata-attestation/utils/X509DateUtils.sol) | 63   | SGX supporting code |      |          |
```

## Out of scope

The following files are out of scope:

- genesis/*
- test/*
- simulation/*
- script/*
- deployments/*
- utils/*


# Additional Context

- Our vaults shall work with all ERC20, ERC1155, and ERC721 tokens, respectively.
- The AssignmentHook contract shall support any ERC20 tokens and Ether as proving fees.
- The core protocol only supports TaikoToken as bonds.
- Contracts in `packages/contracts/team` are expect with work the TaikoToken.sol and a future (ERC-721) Taiko NFT only .
- TaikoL1, TaikoGovernor, TaikoTimelockController, and TaikoToken will be deployed on Ethereum; TaikoL2 will be deployed on Taiko L2. All other contracts including AddressManager, SignalService, Bridge, Vaults, and Bridged tokens will be deployed on both Ethereum and Taiko L2.
- All contracts have an owner who can upgrade contract code and perform certain special actions. There are also special named roles such as "proposer", "proposer_one", "bridge_pauser" that can call special functions, these named roles can be configured to be address(0) to disable these functions. Please search for `onlyFromNamed` to locate these special roles.
- `BridgedERC20` has a special role called snapshooter, once set, this role can take snapshots.


## Attack ideas

- DoS attacking the core protocol.
- Exploit bugs in Merkle proof verification logics.
- Exploit potential bugs in multi-hop bridging, for example, a multi-hop may contains a loop.
- Construct bridge messages whose hash collide.
- Keep contesting valid proofs to delay block confirmation.
- Exhaust the L1 block proposing ring-buffer to hault the chain.
- Explore bugs in L2's anchor transaction.


## Scoping Details 


- If you have a public code repo, please share it here: https://github.com/taikoxyz/taiko-mono/tree/main/packages/protocol 
- How many contracts are in scope?: 81
- Total SLoC for these contracts?: 7611
- How many external imports are there?: 52
- How many separate interfaces and struct definitions are there for the contracts within scope?: 85
- Does most of your code generally use composition or inheritance?: Composition   
- How many external calls?: 0   
- What is the overall line coverage percentage provided by your tests?: 79
- Is this an upgrade of an existing system?: False
- Check all that apply (e.g. timelock, NFT, AMM, ERC20, rollups, etc.): ERC20, ERC721, ERC1155, Rollup, SGX, Multi-Chain, Bridging
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?: False   
- Please describe required context: you must understand how rollup works.
- Does it use an oracle?:  No
- Describe any novel or unique curve logic or mathematical models your code uses: None 
- Is this either a fork of or an alternate implementation of another project?: False  
- Does it use a side-chain?: No


# Tests

```
cd packages/protocol/
pnpm install
pnpm compile
pnpm test
```

## Miscellaneous

Employees of Taiko and employees' family members are ineligible to participate in this audit.
