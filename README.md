# Taiko audit details
- Total Prize Pool: $165,000 in USDC
  - HM awards: $128,900 in USDC
  - Analysis awards: $5,800 in USDC
  - QA awards: $2,900 in USDC
  - Gas awards: $2,900 in USDC
  - Judge awards: $15,000 in USDC
  - Lookout awards: 9,000 in USDC
  - Scout awards: $500 in USDC
 
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2024-03-taiko/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts March 6, 2024 20:00 UTC
- Ends March 27, 2024 20:00 UTC

## Publicly Known Issues

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-03-taiko/blob/main/4naly3er-report.md).

- All code inside [packages/protocol/contracts/team](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team) uses ERC20's `transferFrom` instead of `safeTransferFrom`. This is known and acceptable.
- All findings from our previous audit reports (see below) are ineligible for awards.
- [fix bridge prove message issue using staticcall](https://github.com/taikoxyz/taiko-mono/pull/16404)
- [fix a bug in changeBridgedToken](https://github.com/taikoxyz/taiko-mono/pull/16403)

# Overview

 Taiko is a Based rollup. You can learn about Based rollups by following the links below:

- [https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016](https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016)
- [https://taiko.mirror.xyz/7dfMydX1FqEx9_sOvhRt3V8hJksKSIWjzhCVu7FyMZU](https://taiko.mirror.xyz/7dfMydX1FqEx9_sOvhRt3V8hJksKSIWjzhCVu7FyMZU)

This version of the Taiko protocol is also known as Based Contestable Rollup, or BCR. You can learn about BCR design using these links:

- [https://taiko.mirror.xyz/Z4I5ZhreGkyfdaL5I9P0Rj0DNX4zaWFmcws-0CVMJ2A](https://taiko.mirror.xyz/Z4I5ZhreGkyfdaL5I9P0Rj0DNX4zaWFmcws-0CVMJ2A)
- [https://www.youtube.com/watch?v=A6ncZirXPfc](https://www.youtube.com/watch?v=A6ncZirXPfc)

There are also a few documents in [packages/protocol/docs](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/docs) that you can take a look at. We are working on converting them into our official documentation before the mainnet launch. Apologies that these files are not well-maintained, but I think they may provide some additional insights into BCR's design and/or implementation.

A built-in cross-layer communication mechanism is also included in the core protocol code to facilitate communication across multiple Taiko L2s and/or L3s. We call it multi-hop bridging. You can learn about the basic design [here](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/docs/multihop_bridging_deployment.md).

## Whitepapers

- Whitepaper Update Notice: The current version of the Taiko [whitepaper](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/docs/taiko-whitepaper.pdf)  outlines the fundamental principles of our Base rollup design. Please note, however, that this document has not been updated recently, and as such, some of the details may not accurately reflect the latest developments in our project. While the whitepaper does provide a valuable overview of Taiko's core concepts, it does not include information on our Contestable Rollup features, which are a significant part of our evolving architecture.

- Tokenomics Whitepaper Overview: For a straightforward explanation of how the Taiko token integrates within our protocol, refer to our [tokenomics whitepaper](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/docs/tokenomics-whitepaper.pdf). This document succinctly details the use of Taiko tokens as bonding mechanisms within the Taiko ecosystem, offering insight into our tokenomic strategy.

## Links


- **Previous audits:** An older version of the protocol has been audited by Sigma Prime ([report](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/audit/sigma_prime_taiko_smart_contract_security_assessment_report_v2_0.pdf)). The corresponding bridge code has been audited by QuillAudits ([report](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/audit/quill_audits_taiko_smart_contract_audit_report.pdf)). Please see [the list of changes](https://github.com/taikoxyz/taiko-mono/releases/tag/protocol-v1.0.0) between the audited version and the current v1.0.0 release.
- **Documentation:** [https://docs.taiko.xyz](https://docs.taiko.xyz)
- **Website:** [https://taiko.xyz](https://taiko.xyz)
- **Twitter:** [https://twitter.com/taikoxyz](https://twitter.com/taikoxyz)
- **Discord:** [https://discord.gg/taikoxyz](https://discord.gg/taikoxyz)


# Scope

*[See scope.txt](https://github.com/code-423n4/2024-03-taiko/blob/main/scope.txt)*

| Contract                                                    | SLOC | Purpose                                                           | Libraries used                                 | Priority |
|--------------------------------------------------------------|------|-------------------------------------------------------------------|--------------------------------------|----------|
| [contracts/common/IAddressManager.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/common/IAddressManager.sol)                                     | 4    | Manages registered addresses                                    |                                      |          |
| [contracts/common/AddressManager.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/common/AddressManager.sol)                                         | 35   | Manages registered addresses                                    |                                      |          |
| [contracts/common/IAddressResolver.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/common/IAddressResolver.sol)                                     | 18   | Resolve registered addresses                                     |                                      |          |
| [contracts/common/AddressResolver.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/common/AddressResolver.sol)                                       | 60   | Resolve registered addresses                                     | @openzeppelin                       |          |
| [contracts/common/EssentialContract.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/common/EssentialContract.sol)                                   | 91   | Base class for all proxied contracts (ERC1967Proxy)              | @openzeppelin                       | 🔥High     |
| [contracts/libs/Lib4844.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/libs/Lib4844.sol)                                                           | 38   | Helper function for validating a blob in ZK circuits            |                                      | Medium   |
| [contracts/libs/LibAddress.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/libs/LibAddress.sol)                                                     | 55   |                                                                   | /thirdparty/nomad-xyz, @openzeppelin | Medium   |
| [contracts/libs/LibMath.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/libs/LibMath.sol)                                                             | 9    |                                                                   |                                      |          |
| [contracts/libs/LibTrieProof.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/libs/LibTrieProof.sol)                                                   | 36   | Merkle Trie proof verification                                   | /thirdparty/optimism/               | 🔥High     |
| [contracts/L1/gov/TaikoGovernor.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)                                           | 121  |                                                                   | @openzeppelin                       | Medium   |
| [contracts/L1/gov/TaikoTimelockController.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/gov/TaikoTimelockController.sol)                       | 14   |                                                                   | @openzeppelin                       | Medium   |
| [contracts/L1/hooks/IHook.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/hooks/IHook.sol)                                                       | 11   | Hook interface                                                    |                                      |          |
| [contracts/L1/hooks/AssignmentHook.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)                                     | 119  | Proving job verification hook                                    | @openzeppelin                       | Medium   |
| [contracts/L1/ITaikoL1.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/ITaikoL1.sol)                                                             | 14   |                                                                   |                                      |          |
| [contracts/L1/libs/LibDepositing.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)                                         | 97   | Core logics for depositing Ether to L2                          |                                      | 🔥High     |
| [contracts/L1/libs/LibProposing.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/libs/LibProposing.sol)                                           | 188  | Core logics for proposing Taiko blocks                           |                                      | 🔥High     |
| [contracts/L1/libs/LibProving.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/libs/LibProving.sol)                                               | 243  | Core logics for proving Taiko blocks and contesting proofs       |                                      | 🔥High     |
| [contracts/L1/libs/LibUtils.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/libs/LibUtils.sol)                                                   | 61   |                                                                   |                                      | Medium   |
| [contracts/L1/libs/LibVerifying.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)                                           | 162  | Core logics for verifying Taiko blocks                           |                                      | 🔥High     |
| [contracts/L1/provers/GuardianProver.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)                                 | 32   | The top tier prover                                               |                                      | Medium   |
| [contracts/L1/provers/Guardians.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/provers/Guardians.sol)                                           | 79   | A multisig to authorize the Guardian prover                      |                                      | Medium   |
| [contracts/L1/TaikoData.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/TaikoData.sol)                                                           | 124  | Core Taiko protocol data, needs to support upgradeability        |                                      |          |
| [contracts/L1/TaikoErrors.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/TaikoErrors.sol)                                                       | 34   |                                                                   |                                      |          |
| [contracts/L1/TaikoEvents.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/TaikoEvents.sol)                                                       | 37   | Events whose definitions must match those in L1/libs/Lib***.sol. |                                      |          |
| [contracts/L1/TaikoL1.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/TaikoL1.sol)                                                               | 149  | Taiko BCR main contract on L1 (Ethereum)                         |                                      | Medium   |
| [contracts/L1/TaikoToken.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/TaikoToken.sol)                                                         | 85   | The protocol token. TaikoToken is on L1.                         |                                      | Medium   |
| [contracts/L1/tiers/ITierProvider.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/tiers/ITierProvider.sol)                                       | 21   |                                                       |                                      |          |
| [contracts/L1/tiers/DevnetTierProvider.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)                             | 40   |                                                       |                                      |          |
| [contracts/L1/tiers/MainnetTierProvider.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)                           | 52   |                                                       |                                      |          |
| [contracts/L1/tiers/TestnetTierProvider.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)                           | 52   |                                                       |                                      |          |
| [contracts/L2/CrossChainOwned.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L2/CrossChainOwned.sol)                                               | 45   |                                                                   |                                      |          |
| [contracts/L2/Lib1559Math.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L2/Lib1559Math.sol)                                                       | 33   | Math to calculate EIP-1559 base fee                              | /thirdparty/solmate                 | 🔥High     |
| [contracts/L2/TaikoL2.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L2/TaikoL2.sol)                                                               | 180  |                                                                   |                                      | 🔥High     |
| [contracts/L2/TaikoL2EIP1559Configurable.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol)                         | 25   |                                                                   |                                      | Medium   |
| [contracts/signal/ISignalService.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/signal/ISignalService.sol)                                         | 68   | Signal service interface                                          |                                      |          |
| [contracts/signal/LibSignals.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/signal/LibSignals.sol)                       | 5    |                                                                   |                                  |          |
| [contracts/signal/SignalService.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/signal/SignalService.sol)                | 246  | Signal service that supports app-agnostic, arbitrary message bridging cross multiple Taiko L2/L3s. |                                  | 🔥High     |
| [contracts/bridge/IBridge.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/bridge/IBridge.sol)                             | 63   | Bridge interface                                                  |                                  |          |
| [contracts/bridge/Bridge.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/bridge/Bridge.sol)                               | 404  | Contract that supports cross-layer message calls                  | /thirdparty/nomad-xyz, @openzeppelin | 🔥High     |
| [contracts/tokenvault/adapters/USDCAdapter.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol) | 22   | Native USDC contract adapter                                      | /thirdparty/nomad-xyz,@openzeppelin | Medium   |
| [contracts/tokenvault/BridgedERC20.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)           | 128  | Bridged ERC20 token                                               | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BridgedERC20Base.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)   | 56   | Bridged ERC20 token                                               | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BridgedERC721.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)         | 83   | Bridged ERC721 token                                              | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BridgedERC1155.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)       | 91   | Bridged ERC1155 token                                             | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BaseNFTVault.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/BaseNFTVault.sol)           | 79   | Base NFT vault                                                    | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/BaseVault.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/BaseVault.sol)                 | 46   | Base vault                                                        | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/ERC1155Vault.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)           | 230  | ERC1155 vault                                                     | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/ERC20Vault.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)               | 291  | ERC20 vault                                                       | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/ERC721Vault.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)             | 193  | ERC721 vault                                                      | @openzeppelin                   | 🔥High     |
| [contracts/tokenvault/IBridgedERC20.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/IBridgedERC20.sol)         | 7    |                                                                   |                                  |          |
| [contracts/tokenvault/LibBridgedToken.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)     | 52   |                                                                   |                                  | Medium   |
| [contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol) | 36   | Lib to avoid memory-copy attach            | nomad-xyz             |          |
| [contracts/thirdparty/optimism/Bytes.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/optimism/Bytes.sol)                       | 70   | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/rlp/RLPReader.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)       | 191  | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/rlp/RLPWriter.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)       | 44   | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/trie/MerkleTrie.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)    | 148  | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol) | 32   | Supporting library for Merkle proof verification |                        |          |
| [contracts/thirdparty/solmate/LibFixedPointMath.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/thirdparty/solmate/LibFixedPointMath.sol) | 35   |                                            |                        |          |
| [contracts/verifiers/IVerifier.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/verifiers/IVerifier.sol)                                   | 19   |                                            |                        | Medium   |
| [contracts/verifiers/GuardianVerifier.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)                       | 23   | Verifier for guardian prover              |                        | Medium   |
| [contracts/verifiers/SgxVerifier.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)                                 | 137  | Verifier for SXG prover                   |                        | 🔥High     |
| [contracts/team/airdrop/ERC20Airdrop.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol)                         | 40   | TKO airdrop contract                      |                        | Medium   |
| [contracts/team/airdrop/ERC20Airdrop2.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)                       | 61   |                                            |                        |          |
| [contracts/team/airdrop/ERC721Airdrop.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)                       | 37   |                                            |                        |          |
| [contracts/team/airdrop/MerkleClaimable.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)                   | 65   |                                            |                        | Medium   |
| [contracts/team/TimelockTokenPool.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team/TimelockTokenPool.sol)                         | 159  | Employee and investor unlock contract |      | 🔥High     |
| [contracts/automata-attestation/AutomataDcapV3Attestation.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol) | 360  | SGX supporting code |      |          |
| [contracts/automata-attestation/interfaces/IAttestation.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol) | 8    | SGX supporting code |      |          |
| [contracts/automata-attestation/interfaces/ISigVerifyLib.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/interfaces/ISigVerifyLib.sol) | 68   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/EnclaveIdStruct.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/lib/EnclaveIdStruct.sol) | 23   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/interfaces/IPEMCertChainLib.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/lib/interfaces/IPEMCertChainLib.sol) | 42   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/PEMCertChainLib.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol) | 279  | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol) | 246  | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol) | 48   | SGX supporting code |      |          |
| [contracts/automata-attestation/lib/TCBInfoStruct.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/lib/TCBInfoStruct.sol) | 23   | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/Asn1Decode.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol) | 107  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/BytesUtils.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol) | 225  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/RsaVerify.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol) | 208  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/SHA1.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/utils/SHA1.sol)       | 166  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/SigVerifyLib.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol) | 114  | SGX supporting code |      |          |
| [contracts/automata-attestation/utils/X509DateUtils.sol](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol) | 63   | SGX supporting code |      |          |


Here are the improved and corrected sentences:

## Out of Scope

All files outside of [packages/protocol/contracts](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts) are out of scope.

## Additional Context

- Our vaults are designed to work with all ERC20, ERC1155, and ERC721 tokens, respectively.
- The AssignmentHook contract shall support any ERC20 tokens and Ether as proving fees.
- The core protocol supports only TaikoToken as bonds.
- Contracts in [packages/protocol/contracts/team](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/contracts/team) are expected to be deployed on Taiko L2 and only work with the `BridgedERC20` token (not the `TaikoToken`) and a future Taiko NFT (ERC-721).
- `TaikoL1`, `TaikoGovernor`, `TaikoTimelockController`, and `TaikoToken` will be deployed on Ethereum; `TaikoL2` will be pre-deployed on Taiko L2 before genesis. All other contracts, including `AddressManager`, `SignalService`, `Bridge`, all vaults and bridged tokens, will be deployed on both Ethereum and Taiko L2.
- All contracts have an owner who can upgrade the contract code and perform certain special actions. There are also special named roles such as *proposer*, *proposer_one*, *bridge_pauser* that can call special functions. These named roles can be configured to be `address(0)` to disable these functions. Please search for `onlyFromNamed` to locate these special roles.
- `BridgedERC20` has a special role called *snapshooter*; once set, this role can take snapshots.

## Attack Ideas

- Launching a DoS attack on the core protocol.
- Exploiting bugs in Merkle proof verification logic.
- Exploiting potential bugs in multi-hop bridging, for example, use a multi-hop that contains a loop.
- Constructing bridge messages whose hashes collide.
- Continuously contesting valid proofs to delay block confirmation.
- Exhausting the L1 block proposing ring-buffer to halt the chain.
- Exploring bugs in L2's anchor transaction.

## Scoping Details 

- If you have a public code repo, please share it here: https://github.com/taikoxyz/taiko-mono/tree/v1.0.0
- How many contracts are in scope?: 81
- Total SLoC for these contracts?: 7611
- How many external imports are there?: 52
- How many separate interfaces and struct definitions are there for the contracts within scope?: 85
- Does most of your code generally use composition or inheritance?: Composition   
- How many external calls?: 17
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

```bash
cd packages/protocol/
pnpm install
pnpm compile
pnpm test
```

## Slither

*[See slither.txt](https://github.com/code-423n4/2024-03-taiko/blob/main/packages/protocol/slither.txt)*

Make sure you're using the latest version of Slither (0.10.1).

```bash
cd packages/protocol/
slither .
```

## Miscellaneous

Employees of Taiko and employees' family members are ineligible to participate in this audit.
