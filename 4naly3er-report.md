# Report


## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use ERC721A instead ERC721 | 2 |
| [GAS-2](#GAS-2) | `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings) | 40 |
| [GAS-3](#GAS-3) | Using bools for storage incurs overhead | 10 |
| [GAS-4](#GAS-4) | Cache array length outside of loop | 29 |
| [GAS-5](#GAS-5) | For Operations that will not overflow, you could use unchecked | 612 |
| [GAS-6](#GAS-6) | Use Custom Errors instead of Revert Strings to save Gas | 34 |
| [GAS-7](#GAS-7) | Avoid contract existence checks by using low level calls | 8 |
| [GAS-8](#GAS-8) | Functions guaranteed to revert when called by normal users can be marked `payable` | 12 |
| [GAS-9](#GAS-9) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 13 |
| [GAS-10](#GAS-10) | Using `private` rather than `public` for constants, saves gas | 21 |
| [GAS-11](#GAS-11) | Use shift right/left instead of division/multiplication if possible | 7 |
| [GAS-12](#GAS-12) | Splitting require() statements that use && saves gas | 1 |
| [GAS-13](#GAS-13) | Use of `this` instead of marking as `public` an `external` function | 3 |
| [GAS-14](#GAS-14) | Increments/decrements can be unchecked in for-loops | 45 |
| [GAS-15](#GAS-15) | Use != 0 instead of > 0 for unsigned integer comparison | 16 |
### <a name="GAS-1"></a>[GAS-1] Use ERC721A instead ERC721
ERC721A standard, ERC721A is an improvement standard for ERC721 tokens. It was proposed by the Azuki team and used for developing their NFT collection. Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

    Reference: https://nextrope.com/erc721-vs-erc721a-2/

*Instances (2)*:
```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

4: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

4: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

### <a name="GAS-2"></a>[GAS-2] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)
This saves **16 gas per instance.**

*Instances (40)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

102:                     totalFee += _fee;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

252:                 ts.contestations += 1;

377:             _ts.contestations += 1;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

68:             index += increment;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

159:             acc += upperDigit * (16 ** ((2 * i) + 1));

161:             decoded += acc;

221:         offset += 2;

223:         offset += 4;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

80:         for (uint256 idx = 0; idx < shortest; idx += 32) {

100:             selfptr += 32;

101:             otherptr += 32;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

18:             if (uint8(x509Time[0]) - 48 < 5) yrs += 2000;

19:             else yrs += 1900;

21:             yrs += (uint8(x509Time[0]) - 48) * 1000 + (uint8(x509Time[1]) - 48) * 100;

24:         yrs += (uint8(x509Time[offset + 0]) - 48) * 10 + uint8(x509Time[offset + 1]) - 48;

26:         dys += (uint8(x509Time[offset + 4]) - 48) * 10 + uint8(x509Time[offset + 5]) - 48;

27:         hrs += (uint8(x509Time[offset + 6]) - 48) * 10 + uint8(x509Time[offset + 7]) - 48;

28:         mins += (uint8(x509Time[offset + 8]) - 48) * 10 + uint8(x509Time[offset + 9]) - 48;

29:         secs += (uint8(x509Time[offset + 10]) - 48) * 10 + uint8(x509Time[offset + 11]) - 48;

50:                 timestamp += 31_622_400; // Leap year in seconds

52:                 timestamp += 31_536_000; // Normal year in seconds

60:             timestamp += uint256(monthDays[i - 1]) * 86_400; // Days in seconds

63:         timestamp += uint256(day - 1) * 86_400; // Days in seconds

64:         timestamp += uint256(hour) * 3600; // Hours in seconds

65:         timestamp += uint256(minute) * 60; // Minutes in seconds

66:         timestamp += second;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

254:                 invocationDelay += invocationExtraDelay;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

141:         totalAmountGranted += _grant.amount;

156:         totalAmountVoided += amountVoided;

213:         r.amountWithdrawn += amountToWithdraw;

214:         r.costPaid += costToWithdraw;

216:         totalAmountWithdrawn += amountToWithdraw;

217:         totalCostPaid += costToWithdraw;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

83:         claimedAmount[user] += amount;

90:         withdrawnAmount[user] += amount;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

89:             itemCount += 1;

90:             offset += itemOffset + itemLength;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

137:                     currentKeyIndex += 1;

189:                     currentKeyIndex += sharedNibbleLength;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

207:             validSince += INSTANCE_VALIDITY_DELAY;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-3"></a>[GAS-3] Using bools for storage incurs overhead
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (10)*:
```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

38:     bool private _checkLocalEnclaveReport;

39:     mapping(bytes32 enclave => bool trusted) private _trustedUserMrEnclave;

40:     mapping(bytes32 signer => bool trusted) private _trustedUserMrSigner;

47:     mapping(uint256 idx => mapping(bytes serialNum => bool revoked)) private _serialNumIsRevoked;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

42:     mapping(address addr => bool banned) public addressBanned;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

21:     mapping(address addr => bool authorized) public isAuthorized;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

12:     mapping(bytes32 hash => bool claimed) public isClaimed;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

14:     bool public migratingInbound;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

52:     mapping(address btoken => bool blacklisted) public btokenBlacklist;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

55:     mapping(address instanceAddress => bool alreadyAttested) public addressRegistered;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-4"></a>[GAS-4] Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (29)*:
```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

172:         for (uint256 i; i < _tierFees.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

86:             for (uint256 i; i < deposits_.length;) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

244:             for (uint256 i; i < params.hookCalls.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

74:         for (uint256 i; i < guardians.length; ++i) {

80:         for (uint256 i = 0; i < _newGuardians.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

80:         for (uint256 i; i < serialNumBatch.length; ++i) {

95:         for (uint256 i; i < serialNumBatch.length; ++i) {

191:         for (uint256 i; i < enclaveId.tcbLevels.length; ++i) {

214:         for (uint256 i; i < tcb.tcbLevels.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

244:         for (uint256 i; i < split.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

153:         for (uint256 i; i < encoded.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol

174:         for (uint256 i; i < _sha256.length; ++i) {

283:         for (uint256 i; i < sha1Prefix.length; ++i) {

290:         for (uint256 i; i < _sha1.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

90:         for (uint256 i; i < _msgHashes.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

104:         for (uint256 i; i < hopProofs.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

59:         for (uint256 i; i < tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

66:         for (uint256 j = 0; j < out_.length; j++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

85:         for (uint256 i = 0; i < proof.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

47:         for (uint256 i; i < _op.amounts.length; ++i) {

251:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

269:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

34:         for (uint256 i; i < _op.tokenIds.length; ++i) {

170:             for (uint256 i; i < _tokenIds.length; ++i) {

175:             for (uint256 i; i < _tokenIds.length; ++i) {

197:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

210:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

104:         for (uint256 i; i < _ids.length; ++i) {

210:         for (uint256 i; i < _instances.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-5"></a>[GAS-5] For Operations that will not overflow, you could use unchecked

*Instances (612)*:
```solidity
File: packages/protocol/contracts/L1/ITaikoL1.sol

4: import "./TaikoData.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/ITaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoData.sol

95:         bytes32 l1Hash; // slot 1

96:         bytes32 difficulty; // slot 2

97:         bytes32 blobHash; //or txListHash (if Blob not yet supported), // slot 3

98:         bytes32 extraData; // slot 4

99:         bytes32 depositsHash; // slot 5

100:         address coinbase; // L2 coinbase, // slot 6

103:         uint64 timestamp; // slot 7

109:         bytes32 parentMetaHash; // slot 8

123:         bytes32 key; // slot 1, only written/read for the 1st state transition.

124:         bytes32 blockHash; // slot 2

125:         bytes32 stateRoot; // slot 3

126:         address prover; // slot 4

128:         address contester; // slot 5

130:         uint64 timestamp; // slot 6 (90 bits)

138:         bytes32 metaHash; // slot 1

139:         address assignedProver; // slot 2

141:         uint64 blockId; // slot 3

142:         uint64 proposedAt; // timestamp

143:         uint64 proposedIn; // L1 block number

193:         SlotA slotA; // slot 6

194:         SlotB slotB; // slot 7

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoData.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoEvents.sol

4: import "./TaikoData.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoEvents.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

4: import "../common/EssentialContract.sol";

5: import "./libs/LibDepositing.sol";

6: import "./libs/LibProposing.sol";

7: import "./libs/LibProving.sol";

8: import "./libs/LibVerifying.sol";

9: import "./ITaikoL1.sol";

10: import "./TaikoErrors.sol";

11: import "./TaikoEvents.sol";

125:         super.unpause(); // permission checked inside

207:             livenessBond: 250e18, // 250 Taiko token

215:             ethDepositMaxFee: 1 ether / 10,

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20SnapshotUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

7: import "../common/EssentialContract.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

4: import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";

6:     "@openzeppelin/contracts-upgradeable/governance/compatibility/GovernorCompatibilityBravoUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesUpgradeable.sol";

9:     "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";

11:     "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorTimelockControlUpgradeable.sol";

12: import "../../common/EssentialContract.sol";

112:         return 7200; // 1 day

118:         return 50_400; // 1 week

124:         return 1_000_000_000 ether / 10_000; // 0.01% of Taiko Token

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoTimelockController.sol

4: import "@openzeppelin/contracts-upgradeable/governance/TimelockControllerUpgradeable.sol";

5: import "../../common/EssentialContract.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoTimelockController.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import "../../common/EssentialContract.sol";

7: import "../../libs/LibAddress.sol";

8: import "../ITaikoL1.sol";

9: import "./IHook.sol";

31:         uint256 tip; // A tip to L1 block builder

172:         for (uint256 i; i < _tierFees.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/IHook.sol

4: import "../TaikoData.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/IHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

4: import "../../common/IAddressResolver.sol";

5: import "../../libs/LibAddress.sol";

6: import "../../libs/LibMath.sol";

7: import "../TaikoData.sol";

62:             _state.slotA.numEthDeposits++;

76:         uint256 numPending = _state.slotA.numEthDeposits - _state.slotA.nextEthDepositToProcess;

83:             uint96 fee = uint96(_config.ethDepositMaxFee.min(block.basefee * _config.ethDepositGas));

101:                     deposits_[i].amount -= _fee;

102:                     totalFee += _fee;

103:                     ++i;

104:                     ++j;

116:                 _state.slotA.numEthDeposits++;

140:                 && _state.slotA.numEthDeposits - _state.slotA.nextEthDepositToProcess

141:                     < _config.ethDepositRingBufferSize - 1;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "../../common/IAddressResolver.sol";

6: import "../../libs/LibAddress.sol";

7: import "../hooks/IHook.sol";

8: import "../tiers/ITierProvider.sol";

9: import "../TaikoData.sol";

10: import "./LibDepositing.sol";

21:     uint256 public constant MAX_BYTES_PER_BLOB = 4096 * 32;

98:         if (b.numBlocks >= b.lastVerifiedBlockId + _config.blockMaxProposals + 1) {

103:             _state.blocks[(b.numBlocks - 1) % _config.blockRingBufferSize].metaHash;

122:                 l1Hash: blockhash(block.number - 1),

123:                 difficulty: 0, // to be initialized below

124:                 blobHash: 0, // to be initialized below

131:                 l1Height: uint64(block.number - 1),

132:                 txListByteOffset: 0, // to be initialized below

133:                 txListByteSize: 0, // to be initialized below

134:                 minTier: 0, // to be initialized below

171:             if (uint256(params.txListByteOffset) + params.txListByteSize > MAX_BYTES_PER_BLOB) {

233:             ++_state.slotB.numBlocks;

244:             for (uint256 i; i < params.hookCalls.length; ++i) {

268:             if (tko.balanceOf(address(this)) != tkoBalance + _config.livenessBond) {

296:         return _state.reusableBlobs[_blobHash] + _config.blobExpiry > block.timestamp;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "../../common/IAddressResolver.sol";

6: import "../../libs/LibMath.sol";

7: import "../../verifiers/IVerifier.sol";

8: import "../tiers/ITierProvider.sol";

9: import "../TaikoData.sol";

10: import "./LibUtils.sol";

252:                 ts.contestations += 1;

293:                 tid_ = _blk.nextTransitionId++;

304:             ts_.contestBond = 1; // to save gas

367:                 _tko.transfer(_ts.prover, _ts.validityBond + reward);

371:                 _tko.transfer(_ts.contester, _ts.contestBond + reward);

377:             _ts.contestations += 1;

382:                 _tko.transfer(msg.sender, reward - _tier.validityBond);

384:                 _tko.transferFrom(msg.sender, address(this), _tier.validityBond - reward);

389:         _ts.contestBond = 1; // to save gas

415:             + _tier.provingWindow * 60 >= block.timestamp;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibUtils.sol

4: import "../TaikoData.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibUtils.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "../../common/IAddressResolver.sol";

6: import "../../libs/LibMath.sol";

7: import "../../signal/ISignalService.sol";

8: import "../../signal/LibSignals.sol";

9: import "../tiers/ITierProvider.sol";

10: import "../TaikoData.sol";

11: import "./LibUtils.sol";

125:             ++blockId;

152:                         uint256(ITierProvider(tierProvider).getTier(ts.tier).cooldownWindow) * 60

153:                             + uint256(ts.timestamp).max(_state.slotB.lastUnpausedAt) > block.timestamp

178:                 uint256 bondToReturn = uint256(ts.validityBond) + blk.livenessBond;

185:                     bondToReturn -= blk.livenessBond >> 1;

208:                 ++blockId;

209:                 ++numBlocksVerified;

213:                 uint64 lastVerifiedBlockId = b.lastVerifiedBlockId + numBlocksVerified;

235:             _config.chainId, LibSignals.STATE_ROOT, 0 /* latest block Id*/

238:         if (_lastVerifiedBlockId > lastSyncedBlock + _config.blockSyncThreshold) {

247:             _config.chainId <= 1 || _config.chainId == block.chainid //

249:                 || _config.blockRingBufferSize <= _config.blockMaxProposals + 1

251:                 || _config.blockMaxTxListBytes > 128 * 1024 // calldata up to 128K

262:                 || _config.ethDepositMaxFee > type(uint96).max / _config.ethDepositMaxCountPerBlock

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/provers/GuardianProver.sol

4: import "../tiers/ITierProvider.sol";

5: import "../ITaikoL1.sol";

6: import "./Guardians.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

4: import "../../common/EssentialContract.sol";

68:         if (_minGuardians < (_newGuardians.length + 1) >> 1 || _minGuardians > _newGuardians.length)

74:         for (uint256 i; i < guardians.length; ++i) {

80:         for (uint256 i = 0; i < _newGuardians.length; ++i) {

92:         ++version;

116:             _approvals[version][_hash] |= 1 << (id - 1);

133:             for (uint256 i; i < guardiansLength; ++i) {

134:                 if (bits & 1 == 1) ++count;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

4: import "../../common/EssentialContract.sol";

5: import "./ITierProvider.sol";

24:                 validityBond: 250 ether, // TKO

25:                 contestBond: 500 ether, // TKO

26:                 cooldownWindow: 1440, //24 hours

27:                 provingWindow: 120, // 2 hours

35:                 validityBond: 0, // must be 0 for top tier

36:                 contestBond: 0, // must be 0 for top tier

37:                 cooldownWindow: 60, //1 hours

38:                 provingWindow: 2880, // 48 hours

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/ITierProvider.sol

12:         uint24 cooldownWindow; // in minutes

13:         uint16 provingWindow; // in minutes

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/ITierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

4: import "../../common/EssentialContract.sol";

5: import "./ITierProvider.sol";

24:                 validityBond: 250 ether, // TKO

25:                 contestBond: 500 ether, // TKO

26:                 cooldownWindow: 1440, //24 hours

27:                 provingWindow: 60, // 1 hours

35:                 validityBond: 500 ether, // TKO

36:                 contestBond: 1000 ether, // TKO

37:                 cooldownWindow: 1440, //24 hours

38:                 provingWindow: 240, // 4 hours

46:                 validityBond: 0, // must be 0 for top tier

47:                 contestBond: 0, // must be 0 for top tier

48:                 cooldownWindow: 60, //1 hours

49:                 provingWindow: 2880, // 48 hours

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

4: import "../../common/EssentialContract.sol";

5: import "./ITierProvider.sol";

24:                 validityBond: 250 ether, // TKO

25:                 contestBond: 500 ether, // TKO

26:                 cooldownWindow: 1440, //24 hours

27:                 provingWindow: 30, // 0.5 hours

35:                 validityBond: 500 ether, // TKO

36:                 contestBond: 1000 ether, // TKO

37:                 cooldownWindow: 1440, //24 hours

38:                 provingWindow: 60, // 1 hours

46:                 validityBond: 0, // must be 0 for top tier

47:                 contestBond: 0, // must be 0 for top tier

48:                 cooldownWindow: 60, //1 hours

49:                 provingWindow: 2880, // 48 hours

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "../common/EssentialContract.sol";

6: import "../bridge/IBridge.sol";

53:         emit TransactionExecuted(nextTxId++, bytes4(txdata));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/Lib1559Math.sol

4: import "../thirdparty/solmate/LibFixedPointMath.sol";

28:         return _ethQty(_gasExcess, _adjustmentFactor) / LibFixedPointMath.SCALING_FACTOR

29:             / _adjustmentFactor;

41:         uint256 input = _gasExcess * LibFixedPointMath.SCALING_FACTOR / _adjustmentFactor;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/Lib1559Math.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import "../libs/LibAddress.sol";

8: import "../libs/LibMath.sol";

9: import "../signal/ISignalService.sol";

10: import "../signal/LibSignals.sol";

11: import "./Lib1559Math.sol";

12: import "./CrossChainOwned.sol";

90:             uint256 parentHeight = block.number - 1;

127:             parentId = block.number - 1;

145:         if (_l1BlockId > lastSyncedBlock + BLOCK_SYNC_THRESHOLD) {

202:         if (_blockId + 256 >= block.number) return blockhash(_blockId);

213:         config_.gasTargetPerL1Block = 15 * 1e6 * 4;

234:             for (uint256 i; i < 255 && _blockId >= i + 1; ++i) {

235:                 uint256 j = _blockId - i - 1;

243:             publicInputHashOld := keccak256(inputs, 8192 /*mul(256, 32)*/ )

248:             publicInputHashNew := keccak256(inputs, 8192 /*mul(256, 32)*/ )

265:             uint256 excess = uint256(gasExcess) + _parentGasUsed;

276:                 numL1Blocks = _l1BlockId - lastSyncedBlock;

280:                 uint256 issuance = numL1Blocks * _config.gasTargetPerL1Block;

281:                 excess = excess > issuance ? excess - issuance : 1;

291:                 gasExcess_, uint256(_config.basefeeAdjustmentQuotient) * _config.gasTargetPerL1Block

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol

4: import "./TaikoL2.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

4: import { V3Struct } from "./lib/QuoteV3Auth/V3Struct.sol";

5: import { V3Parser } from "./lib/QuoteV3Auth/V3Parser.sol";

6: import { IPEMCertChainLib } from "./lib/interfaces/IPEMCertChainLib.sol";

7: import { PEMCertChainLib } from "./lib/PEMCertChainLib.sol";

8: import { TCBInfoStruct } from "./lib/TCBInfoStruct.sol";

9: import { EnclaveIdStruct } from "./lib/EnclaveIdStruct.sol";

10: import { IAttestation } from "./interfaces/IAttestation.sol";

13: import { Base64 } from "solady/src/utils/Base64.sol";

14: import { LibString } from "solady/src/utils/LibString.sol";

15: import { BytesUtils } from "./utils/BytesUtils.sol";

18: import { ISigVerifyLib } from "./interfaces/ISigVerifyLib.sol";

80:         for (uint256 i; i < serialNumBatch.length; ++i) {

95:         for (uint256 i; i < serialNumBatch.length; ++i) {

191:         for (uint256 i; i < enclaveId.tcbLevels.length; ++i) {

214:         for (uint256 i; i < tcb.tcbLevels.length; ++i) {

240:         for (uint256 i; i < CPUSVN_LENGTH; ++i) {

259:         for (uint256 i; i < n; ++i) {

261:             if (i == n - 1) {

265:                 issuer = certs[i + 1];

266:                 if (i == n - 2) {

420:             for (uint256 i; i < 3; ++i) {

421:                 bool isPckCert = i == 0; // additional parsing for PCKCert

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol

4: import { V3Struct } from "../lib/QuoteV3Auth/V3Struct.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

4: import { LibString } from "solady/src/utils/LibString.sol";

5: import { Asn1Decode, NodePtr } from "../utils/Asn1Decode.sol";

6: import { BytesUtils } from "../utils/BytesUtils.sol";

7: import { X509DateUtils } from "../utils/X509DateUtils.sol";

8: import { IPEMCertChainLib } from "./interfaces/IPEMCertChainLib.sol";

17:     string internal constant HEADER = "-----BEGIN CERTIFICATE-----";

18:     string internal constant FOOTER = "-----END CERTIFICATE-----";

54:         for (uint256 i; i < size; ++i) {

57:                 input = LibString.slice(pemChainStr, index, index + len);

68:             index += increment;

171:             sigPtr = NodePtr.getPtr(sigPtr.ixs() + 3, sigPtr.ixf() + 3, sigPtr.ixl());

233:         uint256 contentStart = beginPos + HEADER_LENGTH;

244:         for (uint256 i; i < split.length; ++i) {

249:         return (true, contentBytes, endPos + FOOTER_LENGTH);

265:         uint256 lengthDiff = n - expectedLength;

336:                 tbsPtr = 0; // exit

354:         for (uint256 i; i < SGX_TCB_CPUSVN_SIZE + 1; ++i) {

355:             uint256 svnPtr = der.firstChildOf(svnParentPtr); // OID

356:             uint256 svnValuePtr = der.nextSiblingOf(svnPtr); // value

359:                 ? uint16(bytes2(svnValueBytes)) / 256

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

4: import { Base64 } from "solady/src/utils/Base64.sol";

5: import { BytesUtils } from "../../utils/BytesUtils.sol";

6: import { IPEMCertChainLib, PEMCertChainLib } from "../../lib/PEMCertChainLib.sol";

7: import { V3Struct } from "./V3Struct.sol";

34:         if (quote.length - 436 != localAuthDataSize) {

69:             bytes memory signedQuoteData, // concatenation of header and local enclave report bytes

106:         uint32 totalQuoteSize = 48 // header

107:             + 384 // local QE report

108:             + 64 // ecdsa256BitSignature

109:             + 64 // ecdsaAttestationKey

110:             + 384 // QE report

111:             + 64 // qeReportSignature

112:             + 2 // sizeof(v3Quote.v3AuthData.qeAuthData.parsedDataSize)

113:             + v3Quote.v3AuthData.qeAuthData.parsedDataSize + 2 // sizeof(v3Quote.v3AuthData.certification.certType)

114:             + 4 // sizeof(v3Quote.v3AuthData.certification.certDataSize)

115:             + v3Quote.v3AuthData.certification.certDataSize;

153:         for (uint256 i; i < encoded.length; ++i) {

155:             uint256 upperDigit = digits / 16;

158:             uint256 acc = lowerDigit * (16 ** (2 * i));

159:             acc += upperDigit * (16 ** ((2 * i) + 1));

161:             decoded += acc;

215:         uint256 offset = 578 + qeAuthData.parsedDataSize;

221:         offset += 2;

223:         offset += 4;

281:         for (uint256 i; i < 3; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol

25:         bytes reserved3; // 96 bytes

28:         bytes reserved4; // 60 bytes

29:         bytes reportData; // 64 bytes - For QEReports, this contains the hash of the concatenation

44:         bytes[3] decodedCertDataArray; // base64 decoded cert bytes array

48:         bytes ecdsa256BitSignature; // 64 bytes

49:         bytes ecdsaAttestationKey; // 64 bytes

50:         EnclaveReport pckSignedQeReport; // 384 bytes

51:         bytes qeReportSignature; // 64 bytes

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol

8: import "./BytesUtils.sol";

58:         return _readNodeLength(der, ptr.ixf() + 1);

78:         return _readNodeLength(der, ptr.ixl() + 1);

112:         return der.substring(ptr.ixf(), ptr.ixl() + 1 - ptr.ixf());

122:         return der.substring(ptr.ixs(), ptr.ixl() + 1 - ptr.ixs());

132:         return der.readBytesN(ptr.ixf(), ptr.ixl() + 1 - ptr.ixf());

144:         uint256 len = ptr.ixl() + 1 - ptr.ixf();

145:         return uint256(der.readBytesN(ptr.ixf(), len) >> (32 - len) * 8);

157:         uint256 valueLength = ptr.ixl() + 1 - ptr.ixf();

159:             return der.substring(ptr.ixf() + 1, valueLength - 1);

166:         return der.keccak(ptr.ixf(), ptr.ixl() + 1 - ptr.ixf());

170:         return der.keccak(ptr.ixs(), ptr.ixl() + 1 - ptr.ixs());

183:         uint256 valueLength = ptr.ixl() + 1 - ptr.ixf();

184:         return der.substring(ptr.ixf() + 1, valueLength - 1);

191:         if ((der[ix + 1] & 0x80) == 0) {

192:             length = uint8(der[ix + 1]);

193:             ixFirstContentByte = uint80(ix + 2);

194:             ixLastContentByte = uint80(ixFirstContentByte + length - 1);

196:             uint8 lengthbytesLength = uint8(der[ix + 1] & 0x7F);

198:                 length = der.readUint8(ix + 2);

200:                 length = der.readUint16(ix + 2);

203:                     der.readBytesN(ix + 2, lengthbytesLength) >> (32 - lengthbytesLength) * 8

206:             ixFirstContentByte = uint80(ix + 2 + lengthbytesLength);

207:             ixLastContentByte = uint80(ixFirstContentByte + length - 1);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

25:         require(offset + len <= self.length, "invalid offset");

80:         for (uint256 idx = 0; idx < shortest; idx += 32) {

91:                     mask = type(uint256).max; // aka 0xffffff....

93:                     mask = ~(2 ** (8 * (32 - shortest + idx)) - 1);

95:                 uint256 diff = (a & mask) - (b & mask);

100:             selfptr += 32;

101:             otherptr += 32;

104:         return int256(len) - int256(otherlen);

148:         return keccak(self, offset, self.length - offset)

149:             == keccak(other, otherOffset, other.length - otherOffset);

169:         return self.length >= offset + other.length && equals(self, offset, other, 0, other.length);

199:         require(idx + 2 <= self.length, "invalid idx");

212:         require(idx + 4 <= self.length, "unexpected idx");

225:         require(idx + 32 <= self.length, "unexpected idx");

238:         require(idx + 20 <= self.length, "unexpected idx");

265:         require(idx + len <= self.length, "unexpected idx");

293:         require(offset + len <= self.length, "unexpected offset");

333:         for (uint256 i; i < len; ++i) {

334:             bytes1 char = self[off + i];

336:             decoded = uint8(BASE32_HEX_TABLE[uint256(uint8(char)) - 0x30]);

338:             if (i == len - 1) {

344:         uint256 bitlen = len * 5;

351:             bitlen -= 2;

355:             bitlen -= 4;

359:             bitlen -= 1;

363:             bitlen -= 3;

368:         return bytes32(ret << (256 - bitlen));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol

5: import "./SHA1.sol";

13:     This program is free software: you can redistribute it and/or modify

24:     along with this program.  If not, see <http://www.gnu.org/licenses/>.

27: https://csrc.nist.gov/CSRC/media/Projects/Cryptographic-Algorithm-Validation-Program/documents/dss/186-2rsatestvectors.zip

28:     file SigVer15_186-3.rsp

125:         if (uint8(decipher[decipherlen - 50]) == 0x31) {

128:         } else if (uint8(decipher[decipherlen - 48]) == 0x2f) {

135:         uint256 paddingLen = decipherlen - 5 - digestAlgoWithParamLen - 32;

140:         for (uint256 i = 2; i < 2 + paddingLen; ++i) {

145:         if (decipher[2 + paddingLen] != 0) {

152:             for (uint256 i; i < digestAlgoWithParamLen; ++i) {

153:                 if (decipher[3 + paddingLen + i] != bytes1(sha256ExplicitNullParam[i])) {

158:             for (uint256 i; i < digestAlgoWithParamLen; ++i) {

159:                 if (decipher[3 + paddingLen + i] != bytes1(sha256ImplicitNullParam[i])) {

168:             decipher[3 + paddingLen + digestAlgoWithParamLen] != 0x04

169:                 || decipher[4 + paddingLen + digestAlgoWithParamLen] != 0x20

174:         for (uint256 i; i < _sha256.length; ++i) {

175:             if (decipher[5 + paddingLen + digestAlgoWithParamLen + i] != _sha256[i]) {

268:         uint256 paddingLen = decipherlen - 3 - sha1Prefix.length - 20;

273:         for (uint256 i = 2; i < 2 + paddingLen; ++i) {

278:         if (decipher[2 + paddingLen] != 0) {

283:         for (uint256 i; i < sha1Prefix.length; ++i) {

284:             if (decipher[3 + paddingLen + i] != bytes1(sha1Prefix[i])) {

290:         for (uint256 i; i < _sha1.length; ++i) {

291:             if (decipher[3 + paddingLen + sha1Prefix.length + i] != _sha1[i]) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SHA1.sol

27:             function readword(ptr, off, count) -> result {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SHA1.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol

4: import "../interfaces/ISigVerifyLib.sol";

5: import "./RsaVerify.sol";

6: import "./BytesUtils.sol";

90:         bytes memory modulus = publicKey.substring(3, publicKey.length - 3);

107:         bytes memory modulus = publicKey.substring(3, publicKey.length - 3);

138:         assert(success); // never reverts, always returns 0 or 1

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

18:             if (uint8(x509Time[0]) - 48 < 5) yrs += 2000;

19:             else yrs += 1900;

21:             yrs += (uint8(x509Time[0]) - 48) * 1000 + (uint8(x509Time[1]) - 48) * 100;

24:         yrs += (uint8(x509Time[offset + 0]) - 48) * 10 + uint8(x509Time[offset + 1]) - 48;

25:         mnths = (uint8(x509Time[offset + 2]) - 48) * 10 + uint8(x509Time[offset + 3]) - 48;

26:         dys += (uint8(x509Time[offset + 4]) - 48) * 10 + uint8(x509Time[offset + 5]) - 48;

27:         hrs += (uint8(x509Time[offset + 6]) - 48) * 10 + uint8(x509Time[offset + 7]) - 48;

28:         mins += (uint8(x509Time[offset + 8]) - 48) * 10 + uint8(x509Time[offset + 9]) - 48;

29:         secs += (uint8(x509Time[offset + 10]) - 48) * 10 + uint8(x509Time[offset + 11]) - 48;

48:         for (uint16 i = 1970; i < year; ++i) {

50:                 timestamp += 31_622_400; // Leap year in seconds

52:                 timestamp += 31_536_000; // Normal year in seconds

59:         for (uint8 i = 1; i < month; ++i) {

60:             timestamp += uint256(monthDays[i - 1]) * 86_400; // Days in seconds

63:         timestamp += uint256(day - 1) * 86_400; // Days in seconds

64:         timestamp += uint256(hour) * 3600; // Hours in seconds

65:         timestamp += uint256(minute) * 60; // Minutes in seconds

66:         timestamp += second;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

4: import "@openzeppelin/contracts/utils/Address.sol";

5: import "../common/EssentialContract.sol";

6: import "../libs/LibAddress.sol";

7: import "../signal/ISignalService.sol";

8: import "../thirdparty/nomad-xyz/ExcessivelySafeCall.sol";

9: import "./IBridge.sol";

90:         for (uint256 i; i < _msgHashes.length; ++i) {

138:         uint256 expectedAmount = _message.value + _message.fee;

144:         message_.id = nextMessageId++;

189:         if (block.timestamp >= invocationDelay + receivedAt) {

254:                 invocationDelay += invocationExtraDelay;

258:         if (block.timestamp >= invocationDelay + receivedAt) {

295:                 refundTo.sendEther(_message.fee + refundAmount);

424:             block.chainid == 1 // Ethereum mainnet

430:             block.chainid == 2 // Ropsten

431:                 || block.chainid == 4 // Rinkeby

432:                 || block.chainid == 5 // Goerli

433:                 || block.chainid == 42 // Kovan

434:                 || block.chainid == 17_000 // Holesky

435:                 || block.chainid == 11_155_111 // Sepolia

491:             _message.data.length >= 4 // msg can be empty

501:                 64, // return max 64 bytes

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/bridge/IBridge.sol

61:         bytes32 msgHash; // Message hash.

62:         address from; // Sender's address.

63:         uint64 srcChainId; // Source chain ID.

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/IBridge.sol)

```solidity
File: packages/protocol/contracts/common/AddressManager.sol

4: import "./IAddressManager.sol";

5: import "./EssentialContract.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/AddressManager.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

4: import "@openzeppelin/contracts/proxy/utils/UUPSUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol";

6: import "./AddressResolver.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

4: import "@openzeppelin/contracts/utils/Address.sol";

5: import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

6: import "@openzeppelin/contracts/utils/introspection/IERC165.sol";

7: import "@openzeppelin/contracts/interfaces/IERC1271.sol";

8: import "../thirdparty/nomad-xyz/ExcessivelySafeCall.sol";

31:             64, // return max 64 bytes

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/libs/LibTrieProof.sol

9: import "../thirdparty/optimism/rlp/RLPReader.sol";

10: import "../thirdparty/optimism/rlp/RLPWriter.sol";

11: import "../thirdparty/optimism/trie/SecureMerkleTrie.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibTrieProof.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

4: import "@openzeppelin/contracts/utils/math/SafeCast.sol";

5: import "../common/EssentialContract.sol";

6: import "../libs/LibTrieProof.sol";

7: import "./ISignalService.sol";

8: import "./LibSignals.sol";

104:         for (uint256 i; i < hopProofs.length; ++i) {

108:             bool isLastHop = i == hopProofs.length - 1;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

4: import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import "../common/EssentialContract.sol";

141:         totalAmountGranted += _grant.amount;

156:         totalAmountVoided += amountVoided;

193:         amountToWithdraw = amountUnlocked - amountWithdrawn;

197:         uint128 _amountUnlocked = amountUnlocked / 1e18; // divide first

198:         costToWithdraw = _amountUnlocked * r.grant.costPerToken - r.costPaid;

213:         r.amountWithdrawn += amountToWithdraw;

214:         r.costPaid += costToWithdraw;

216:         totalAmountWithdrawn += amountToWithdraw;

217:         totalCostPaid += costToWithdraw;

228:         amountVoided = _grant.amount - amountOwned;

260:         if (block.timestamp >= _start + _period) return _amount;

264:         return _amount * uint64(block.timestamp - _start) / _period;

278:             if (_cliff >= _start + _period) revert INVALID_GRANT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/governance/utils/IVotes.sol";

6: import "./MerkleClaimable.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "../../libs/LibMath.sol";

6: import "./MerkleClaimable.sol";

40:         if (claimEnd > block.timestamp || claimEnd + withdrawalWindow < block.timestamp) {

83:         claimedAmount[user] += amount;

90:         withdrawnAmount[user] += amount;

120:         withdrawableAmount = timeBasedAllowance - withdrawnAmount[user];

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

4: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

5: import "./MerkleClaimable.sol";

59:         for (uint256 i; i < tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

4: import "@openzeppelin/contracts/utils/cryptography/MerkleProof.sol";

5: import "../../common/EssentialContract.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol

46:                     _gas, // gas

47:                     _target, // recipient

48:                     _value, // ether value

49:                     add(_calldata, 0x20), // inloc

50:                     mload(_calldata), // inlen

51:                     0, // outloc

52:                     0 // outlen

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/Bytes.sol

25:             require(_length + 31 >= _length, "slice_overflow");

26:             require(_start + _length >= _start, "slice_overflow");

27:             require(_bytes.length >= _start + _length, "slice_outOfBounds");

95:         return slice(_bytes, _start, _bytes.length - _start);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/Bytes.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

62:             listOffset + listLength == _in.length,

77:                     length: _in.length - offset,

78:                     ptr: MemoryPointer.wrap(MemoryPointer.unwrap(_in.ptr) + offset)

85:                 length: itemLength + itemOffset,

86:                 ptr: MemoryPointer.wrap(MemoryPointer.unwrap(_in.ptr) + offset)

89:             itemCount += 1;

90:             offset += itemOffset + itemLength;

118:             _in.length == itemOffset + itemLength,

170:             uint256 strLen = prefix - 0x80;

190:             uint256 lenOfStrLen = prefix - 0xb7;

218:                 _in.length > lenOfStrLen + strLen,

222:             return (1 + lenOfStrLen, strLen, RLPItemType.DATA_ITEM);

226:             uint256 listLen = prefix - 0xc0;

236:             uint256 lenOfListLen = prefix - 0xf7;

264:                 _in.length > lenOfListLen + listLen,

268:             return (1 + lenOfListLen, listLen, RLPItemType.LIST_ITEM);

294:         uint256 src = MemoryPointer.unwrap(_src) + _offset;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

35:             out_[0] = bytes1(uint8(_len) + uint8(_offset));

39:             while (_len / i != 0) {

40:                 lenLen++;

41:                 i *= 256;

44:             out_ = new bytes(lenLen + 1);

45:             out_[0] = bytes1(uint8(lenLen) + uint8(_offset) + 55);

46:             for (i = 1; i <= lenLen; i++) {

47:                 out_[i] = bytes1(uint8((_len / (256 ** (lenLen - i))) % 256));

59:         for (; i < 32; i++) {

65:         out_ = new bytes(32 - i);

66:         for (uint256 j = 0; j < out_.length; j++) {

67:             out_[j] = b[i++];

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

4: import { Bytes } from "../Bytes.sol";

5: import { RLPReader } from "../rlp/RLPReader.sol";

24:     uint256 internal constant BRANCH_NODE_LENGTH = TREE_RADIX + 1;

85:         for (uint256 i = 0; i < proof.length; i++) {

126:                         i == proof.length - 1,

137:                     currentKeyIndex += 1;

142:                 uint8 offset = 2 - (prefix % 2);

179:                         i == proof.length - 1,

189:                     currentKeyIndex += sharedNibbleLength;

211:                 ++i;

246:                 ++shared_;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol

4: import { MerkleTrie } from "./MerkleTrie.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseNFTVault.sol

4: import "./BaseVault.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseNFTVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

4: import "@openzeppelin/contracts-upgradeable/utils/introspection/IERC165Upgradeable.sol";

5: import "@openzeppelin/contracts/proxy/ERC1967/ERC1967Proxy.sol";

6: import "../bridge/IBridge.sol";

7: import "../common/EssentialContract.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

4: import "@openzeppelin/contracts/utils/Strings.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC1155/ERC1155Upgradeable.sol";

7:     "@openzeppelin/contracts-upgradeable/token/ERC1155/extensions/IERC1155MetadataURIUpgradeable.sol";

8: import "../common/EssentialContract.sol";

9: import "./LibBridgedToken.sol";

126:         address, /*_operator*/

127:         address, /*_from*/

129:         uint256[] memory, /*_ids*/

130:         uint256[] memory, /*_amounts*/

131:         bytes memory /*_data*/

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/IERC20MetadataUpgradeable.sol";

5: import "@openzeppelin/contracts/utils/Strings.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20SnapshotUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

8: import "./LibBridgedToken.sol";

9: import "./BridgedERC20Base.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

4: import "../common/EssentialContract.sol";

5: import "./IBridgedERC20.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

5: import "@openzeppelin/contracts/utils/Strings.sol";

6: import "../common/EssentialContract.sol";

7: import "./LibBridgedToken.sol";

116:         address, /*_from*/

118:         uint256, /*_firstTokenId*/

119:         uint256 /*_batchSize*/

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

4: import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC1155/utils/ERC1155ReceiverUpgradeable.sol";

6: import "../bridge/IBridge.sol";

7: import "../libs/LibAddress.sol";

8: import "./BaseNFTVault.sol";

9: import "./BridgedERC1155.sol";

47:         for (uint256 i; i < _op.amounts.length; ++i) {

59:             id: 0, // will receive a new value

60:             from: address(0), // will receive a new value

61:             srcChainId: 0, // will receive a new value

67:             value: msg.value - _op.fee,

251:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

269:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import "../bridge/IBridge.sol";

8: import "../libs/LibAddress.sol";

9: import "./BridgedERC20.sol";

10: import "./BaseVault.sol";

222:             id: 0, // will receive a new value

223:             from: address(0), // will receive a new value

224:             srcChainId: 0, // will receive a new value

230:             value: msg.value - _op.fee,

380:             balanceChange_ = t.balanceOf(address(this)) - _balance;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

4: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

5: import "@openzeppelin/contracts/token/ERC721/IERC721Receiver.sol";

6: import "../bridge/IBridge.sol";

7: import "../libs/LibAddress.sol";

8: import "./BaseNFTVault.sol";

9: import "./BridgedERC721.sol";

34:         for (uint256 i; i < _op.tokenIds.length; ++i) {

45:             id: 0, // will receive a new value

46:             from: address(0), // will receive a new value

47:             srcChainId: 0, // will receive a new value

53:             value: msg.value - _op.fee,

170:             for (uint256 i; i < _tokenIds.length; ++i) {

175:             for (uint256 i; i < _tokenIds.length; ++i) {

197:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

210:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/LibBridgedToken.sol

4: import "@openzeppelin/contracts/utils/Strings.sol";

59:                 "/tokenURI?uint256="

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)

```solidity
File: packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol

4: import "../BridgedERC20Base.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol)

```solidity
File: packages/protocol/contracts/verifiers/GuardianVerifier.sol

4: import "../common/EssentialContract.sol";

5: import "../L1/TaikoData.sol";

6: import "./IVerifier.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/IVerifier.sol

4: import "../L1/TaikoData.sol";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/IVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

4: import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

5: import "../L1/ITaikoL1.sol";

6: import "../common/EssentialContract.sol";

7: import "../thirdparty/optimism/Bytes.sol";

8: import "../automata-attestation/interfaces/IAttestation.sol";

9: import "../automata-attestation/lib/QuoteV3Auth/V3Struct.sol";

10: import "./IVerifier.sol";

104:         for (uint256 i; i < _ids.length; ++i) {

207:             validSince += INSTANCE_VALIDITY_DELAY;

210:         for (uint256 i; i < _instances.length; ++i) {

222:             nextInstanceId++;

237:             && block.timestamp <= instances[id].validSince + INSTANCE_EXPIRY;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-6"></a>[GAS-6] Use Custom Errors instead of Revert Strings to save Gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

Additionally, custom errors can be used inside and outside of contracts (including interfaces and libraries).

Source: <https://blog.soliditylang.org/2021/04/21/custom-errors/>:

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Consider replacing **all revert strings** with custom errors in the solution, and particularly those that have multiple occurrences:

*Instances (34)*:
```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

61:         require(msg.sender == owner, "onlyOwner");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

116:         require(totalQuoteSize >= MINIMUM_QUOTE_LENGTH, "Invalid quote size");

279:         require(certParsedSuccessfully, "splitCertificateChain failed");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol

57:         require(der[ptr.ixs()] == 0x03, "Not type BIT STRING");

67:         require(der[ptr.ixs()] == 0x04, "Not type OCTET STRING");

88:         require(der[ptr.ixs()] & 0x20 == 0x20, "Not a constructed type");

142:         require(der[ptr.ixs()] == 0x02, "Not type INTEGER");

143:         require(der[ptr.ixf()] & 0x80 == 0, "Not positive");

155:         require(der[ptr.ixs()] == 0x02, "Not type INTEGER");

156:         require(der[ptr.ixf()] & 0x80 == 0, "Not positive");

180:         require(der[ptr.ixs()] == 0x03, "ixs Not type BIT STRING 0x03");

182:         require(der[ptr.ixf()] == 0x00, "ixf Not 0");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

25:         require(offset + len <= self.length, "invalid offset");

199:         require(idx + 2 <= self.length, "invalid idx");

212:         require(idx + 4 <= self.length, "unexpected idx");

225:         require(idx + 32 <= self.length, "unexpected idx");

238:         require(idx + 20 <= self.length, "unexpected idx");

264:         require(len <= 32, "unexpected len");

265:         require(idx + len <= self.length, "unexpected idx");

293:         require(offset + len <= self.length, "unexpected offset");

329:         require(len <= 52, "unexpected len");

335:             require(char >= 0x30 && char <= 0x7A, "invalid char");

337:             require(decoded <= 0x20, "invalid decoded");

365:             revert("unexpected len");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol

50:             revert("Unsupported algorithm");

75:             revert("Unsupported algorithm");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/Bytes.sol

25:             require(_length + 31 >= _length, "slice_overflow");

26:             require(_start + _length >= _start, "slice_overflow");

27:             require(_bytes.length >= _start + _length, "slice_outOfBounds");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/Bytes.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

77:         require(_key.length > 0, "MerkleTrie: empty key");

89:             require(currentKeyIndex <= key.length, "MerkleTrie: key index exceeds total key length");

191:                     revert("MerkleTrie: received a node with an unknown prefix");

194:                 revert("MerkleTrie: received an unparseable node");

198:         revert("MerkleTrie: ran out of proof elements");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

### <a name="GAS-7"></a>[GAS-7] Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*Instances (8)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

238:             uint256 tkoBalance = tko.balanceOf(address(this));

268:             if (tko.balanceOf(address(this)) != tkoBalance + _config.livenessBond) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

176:             IERC20(_token).safeTransfer(_to, IERC20(_token).balanceOf(address(this)));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

73:             return ECDSA.recover(_hash, _sig) == _addr;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

171:         address recipient = ECDSA.recover(hash, _sig);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

378:             uint256 _balance = t.balanceOf(address(this));

380:             balanceChange_ = t.balanceOf(address(this)) - _balance;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

159:             ECDSA.recover(getSignedHash(_tran, newInstance, _ctx.prover, _ctx.metaHash), signature);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-8"></a>[GAS-8] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (12)*:
```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

47:     function burn(address _from, uint256 _amount) public onlyOwner {

52:     function snapshot() public onlyFromOwnerOrNamed("snapshooter") {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

65:     function setMrSigner(bytes32 _mrSigner, bool _trusted) external onlyOwner {

69:     function setMrEnclave(bytes32 _mrEnclave, bool _trusted) external onlyOwner {

122:     function toggleLocalReportCheck() external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

114:     function _authorizeUpgrade(address) internal virtual override onlyOwner { }

116:     function _authorizePause(address) internal virtual onlyOwner { }

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

56:     function authorize(address _addr, bool _authorize) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

135:     function grant(address _recipient, Grant memory _grant) external onlyOwner {

150:     function void(address _recipient) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

80:     function setSnapshoter(address _snapshooter) external onlyOwner {

85:     function snapshot() external onlyOwnerOrSnapshooter {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

### <a name="GAS-9"></a>[GAS-9] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)
Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (13)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

62:             _state.slotA.numEthDeposits++;

116:                 _state.slotA.numEthDeposits++;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

293:                 tid_ = _blk.nextTransitionId++;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

53:         emit TransactionExecuted(nextTxId++, bytes4(txdata));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

17:     string internal constant HEADER = "-----BEGIN CERTIFICATE-----";

18:     string internal constant FOOTER = "-----END CERTIFICATE-----";

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

144:         message_.id = nextMessageId++;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

40:                 lenLen++;

46:             for (i = 1; i <= lenLen; i++) {

59:         for (; i < 32; i++) {

66:         for (uint256 j = 0; j < out_.length; j++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

85:         for (uint256 i = 0; i < proof.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

222:             nextInstanceId++;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-10"></a>[GAS-10] Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (21)*:
```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

38:     uint256 public constant MAX_GAS_PAYING_PROVER = 50_000;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

21:     uint256 public constant MAX_BYTES_PER_BLOB = 4096 * 32;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

20:     bytes32 public constant RETURN_LIVENESS_BOND = keccak256("RETURN_LIVENESS_BOND");

23:     bytes32 public constant TIER_OP = bytes32("tier_optimistic");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

11:     uint256 public constant MIN_NUM_GUARDIANS = 5;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/ITierProvider.sol

39:     uint16 public constant TIER_OPTIMISTIC = 100;

42:     uint16 public constant TIER_SGX = 200;

45:     uint16 public constant TIER_SGX_ZKVM = 300;

48:     uint16 public constant TIER_GUARDIAN = 1000;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/ITierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

32:     address public constant GOLDEN_TOUCH_ADDRESS = 0x0000777735367b36bC9B61C50022d9D0700dB4Ec;

35:     uint8 public constant BLOCK_SYNC_THRESHOLD = 5;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/libs/Lib4844.sol

10:     address public constant POINT_EVALUATION_PRECOMPILE_ADDRESS = address(0x0A);

13:     uint32 public constant FIELD_ELEMENTS_PER_BLOB = 4096;

16:     uint256 public constant BLS_MODULUS =

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/Lib4844.sol)

```solidity
File: packages/protocol/contracts/signal/LibSignals.sol

8:     bytes32 public constant STATE_ROOT = keccak256("STATE_ROOT");

11:     bytes32 public constant SIGNAL_ROOT = keccak256("SIGNAL_ROOT");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/LibSignals.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseNFTVault.sol

47:     bytes4 public constant ERC1155_INTERFACE_ID = 0xd9b67a26;

50:     bytes4 public constant ERC721_INTERFACE_ID = 0x80ac58cd;

53:     uint256 public constant MAX_TOKEN_PER_TXN = 10;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseNFTVault.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

30:     uint64 public constant INSTANCE_EXPIRY = 180 days;

34:     uint64 public constant INSTANCE_VALIDITY_DELAY = 1 days;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-11"></a>[GAS-11] Use shift right/left instead of division/multiplication if possible
While the `DIV` / `MUL` opcode uses 5 gas, the `SHR` / `SHL` opcode only uses 3 gas. Furthermore, beware that Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting. Eventually, overflow checks are never performed for shift operations as they are done for arithmetic operations. Instead, the result is always truncated, so the calculation can be unchecked in Solidity version `0.8+`
- Use `>> 1` instead of `/ 2`
- Use `>> 2` instead of `/ 4`
- Use `<< 3` instead of `* 8`
- ...
- Use `>> 5` instead of `/ 2^5 == / 32`
- Use `<< 6` instead of `* 2^6 == * 64`

TL;DR:
- Shifting left by N is like multiplying by 2^N (Each bits to the left is an increased power of 2)
- Shifting right by N is like dividing by 2^N (Each bits to the right is a decreased power of 2)

*Saves around 2 gas + 20 for unchecked per instance*

*Instances (7)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

21:     uint256 public constant MAX_BYTES_PER_BLOB = 4096 * 32;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

251:                 || _config.blockMaxTxListBytes > 128 * 1024 // calldata up to 128K

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

213:         config_.gasTargetPerL1Block = 15 * 1e6 * 4;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

359:                 ? uint16(bytes2(svnValueBytes)) / 256

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

155:             uint256 upperDigit = digits / 16;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol

145:         return uint256(der.readBytesN(ptr.ixf(), len) >> (32 - len) * 8);

203:                     der.readBytesN(ix + 2, lengthbytesLength) >> (32 - lengthbytesLength) * 8

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol)

### <a name="GAS-12"></a>[GAS-12] Splitting require() statements that use && saves gas

*Instances (1)*:
```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

335:             require(char >= 0x30 && char <= 0x7A, "invalid char");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

### <a name="GAS-13"></a>[GAS-13] Use of `this` instead of marking as `public` an `external` function
Using `this.` is like making an expensive external call. Consider marking the called function as public

*Saves around 2000 gas per instance*

*Instances (3)*:
```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

281:             this.onMessageInvocation, abi.encode(ctoken_, _user, _op.to, _op.tokenIds, _op.amounts)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

384:             this.onMessageInvocation, abi.encode(ctoken_, _user, _to, balanceChange_)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

217:             this.onMessageInvocation, abi.encode(ctoken_, _user, _op.to, _op.tokenIds)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

### <a name="GAS-14"></a>[GAS-14] Increments/decrements can be unchecked in for-loops
In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (45)*:
```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

172:         for (uint256 i; i < _tierFees.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

244:             for (uint256 i; i < params.hookCalls.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

74:         for (uint256 i; i < guardians.length; ++i) {

80:         for (uint256 i = 0; i < _newGuardians.length; ++i) {

133:             for (uint256 i; i < guardiansLength; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

234:             for (uint256 i; i < 255 && _blockId >= i + 1; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

80:         for (uint256 i; i < serialNumBatch.length; ++i) {

95:         for (uint256 i; i < serialNumBatch.length; ++i) {

191:         for (uint256 i; i < enclaveId.tcbLevels.length; ++i) {

214:         for (uint256 i; i < tcb.tcbLevels.length; ++i) {

240:         for (uint256 i; i < CPUSVN_LENGTH; ++i) {

259:         for (uint256 i; i < n; ++i) {

420:             for (uint256 i; i < 3; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

54:         for (uint256 i; i < size; ++i) {

244:         for (uint256 i; i < split.length; ++i) {

354:         for (uint256 i; i < SGX_TCB_CPUSVN_SIZE + 1; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

153:         for (uint256 i; i < encoded.length; ++i) {

281:         for (uint256 i; i < 3; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

333:         for (uint256 i; i < len; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol

140:         for (uint256 i = 2; i < 2 + paddingLen; ++i) {

152:             for (uint256 i; i < digestAlgoWithParamLen; ++i) {

158:             for (uint256 i; i < digestAlgoWithParamLen; ++i) {

174:         for (uint256 i; i < _sha256.length; ++i) {

273:         for (uint256 i = 2; i < 2 + paddingLen; ++i) {

283:         for (uint256 i; i < sha1Prefix.length; ++i) {

290:         for (uint256 i; i < _sha1.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

48:         for (uint16 i = 1970; i < year; ++i) {

59:         for (uint8 i = 1; i < month; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

90:         for (uint256 i; i < _msgHashes.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

104:         for (uint256 i; i < hopProofs.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

59:         for (uint256 i; i < tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

46:             for (i = 1; i <= lenLen; i++) {

59:         for (; i < 32; i++) {

66:         for (uint256 j = 0; j < out_.length; j++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

85:         for (uint256 i = 0; i < proof.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

47:         for (uint256 i; i < _op.amounts.length; ++i) {

251:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

269:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

34:         for (uint256 i; i < _op.tokenIds.length; ++i) {

170:             for (uint256 i; i < _tokenIds.length; ++i) {

175:             for (uint256 i; i < _tokenIds.length; ++i) {

197:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

210:                 for (uint256 i; i < _op.tokenIds.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

104:         for (uint256 i; i < _ids.length; ++i) {

210:         for (uint256 i; i < _instances.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="GAS-15"></a>[GAS-15] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (16)*:
```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

125:         if (address(this).balance > 0) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

192:             bool returnLivenessBond = blk.livenessBond > 0 && _proof.data.length == 32

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

212:             if (numBlocksVerified > 0) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

262:         if (gasExcess > 0) {

275:             if (lastSyncedBlock > 0 && _l1BlockId > lastSyncedBlock) {

279:             if (numL1Blocks > 0) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

56:             if (i > 0) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

238:         require(idx + 20 <= self.length, "unexpected idx");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

120:             bool isFullProof = hop.accountProof.length > 0;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

275:             if (_cliff > 0) revert INVALID_GRANT();

277:             if (_cliff > 0 && _cliff <= _start) revert INVALID_GRANT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

38:             _in.length > 0,

153:             _in.length > 0,

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

77:         require(_key.length > 0, "MerkleTrie: empty key");

120:                         value_.length > 0,

173:                         value_.length > 0,

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)


## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | `require()` should be used instead of `assert()` | 4 |
| [NC-2](#NC-2) | Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked` | 21 |
| [NC-3](#NC-3) | `constant`s should be defined rather than using magic numbers | 298 |
| [NC-4](#NC-4) | Control structures do not follow the Solidity Style Guide | 329 |
| [NC-5](#NC-5) | Default Visibility for constants | 3 |
| [NC-6](#NC-6) | Consider disabling `renounceOwnership()` | 1 |
| [NC-7](#NC-7) | Functions should not be longer than 50 lines | 213 |
| [NC-8](#NC-8) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 16 |
| [NC-9](#NC-9) | `address`s shouldn't be hard-coded | 2 |
| [NC-10](#NC-10) | Numeric values having to do with time should use time units for readability | 1 |
| [NC-11](#NC-11) | Owner can renounce while system is paused | 1 |
| [NC-12](#NC-12) | Take advantage of Custom Error's return value property | 170 |
| [NC-13](#NC-13) | Avoid the use of sensitive terms | 5 |
| [NC-14](#NC-14) | Use Underscores for Number Literals (add an underscore every 3 digits) | 28 |
| [NC-15](#NC-15) | Constants should be defined rather than using magic numbers | 25 |
| [NC-16](#NC-16) | Variables need not be initialized to zero | 11 |
### <a name="NC-1"></a>[NC-1] `require()` should be used instead of `assert()`
Prior to solidity version 0.8.0, hitting an assert consumes the **remainder of the transaction's available gas** rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that "The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix. Additionally, a require statement (or a custom error) are more friendly in terms of understanding what happened."

*Instances (4)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

223:                 assert(tier.validityBond == 0);

224:                 assert(ts.validityBond == 0 && ts.contestBond == 0 && ts.contester == address(0));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol

138:         assert(success); // never reverts, always returns 0 or 1

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

486:         assert(_message.from != address(this));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

### <a name="NC-2"></a>[NC-2] Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked`
Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

Solidity version 0.8.12 introduces `string.concat()` (vs `abi.encodePacked(<str>,<str>), which catches concatenation errors (in the event of a `bytes` data mixed in the concatenation)`)

*Instances (21)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

204:         meta_.difficulty = keccak256(abi.encodePacked(block.prevrandao, b.numBlocks, block.number));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

163:         bytes memory retData = abi.encodePacked(INVALID_EXIT_CODE);

315:             abi.encodePacked(authDataV3.ecdsaAttestationKey, authDataV3.qeAuthData.data);

369:         bytes memory retData = abi.encodePacked(INVALID_EXIT_CODE);

482:         retData = abi.encodePacked(sha256(abi.encode(v3quote)), tcbStatus);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

181:             cert.signature = abi.encodePacked(sigX, sigY);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

119:         bytes memory headerBytes = abi.encodePacked(

129:         signedQuoteData = abi.encodePacked(headerBytes, V3Parser.packQEReport(localEnclaveReport));

251:         packedQEReport = abi.encodePacked(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/libs/Lib4844.sol

44:             abi.encodePacked(_blobHash, _x, _y, _commitment, _pointProof)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/Lib4844.sol)

```solidity
File: packages/protocol/contracts/libs/LibTrieProof.sol

48:                 SecureMerkleTrie.get(abi.encodePacked(_addr), _accountProof, _rootHash);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibTrieProof.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

203:         return keccak256(abi.encodePacked("SIGNAL", _chainId, _app, _signal));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

170:         bytes32 hash = keccak256(abi.encodePacked("Withdraw unlocked Taiko token to: ", _to));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

17:             out_ = abi.encodePacked(_writeLength(_in.length, 128), _in);

56:         bytes memory b = abi.encodePacked(_x);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

81:         bytes memory currentNodeID = abi.encodePacked(_root);

94:                     Bytes.equal(abi.encodePacked(keccak256(currentNode.encoded)), currentNodeID),

100:                     Bytes.equal(abi.encodePacked(keccak256(currentNode.encoded)), currentNodeID),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol

55:         hash_ = abi.encodePacked(keccak256(_key));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

109:             abi.encodePacked(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/LibBridgedToken.sol

54:             abi.encodePacked(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)

### <a name="NC-3"></a>[NC-3] `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (298)*:
```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

192:             chainId: 167_008,

195:             blockMaxProposals: 864_000,

196:             blockRingBufferSize: 864_100,

198:             maxBlocksToVerifyPerProposal: 10,

199:             blockMaxGasLimit: 15_000_000,

203:             blockMaxTxListBytes: 120_000,

204:             blobExpiry: 24 hours,

207:             livenessBond: 250e18, // 250 Taiko token

209:             ethDepositRingBufferSize: 1024,

210:             ethDepositMinCountPerBlock: 8,

211:             ethDepositMaxCountPerBlock: 32,

213:             ethDepositMaxAmount: 10_000 ether,

214:             ethDepositGas: 21_000,

215:             ethDepositMaxFee: 1 ether / 10,

216:             blockSyncThreshold: 16

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

112:         return 7200; // 1 day

118:         return 50_400; // 1 week

124:         return 1_000_000_000 ether / 10_000; // 0.01% of Taiko Token

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

89:                     recipient: address(uint160(data >> 96)),

151:         return (uint256(uint160(_addr)) << 96) | _amount;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

192:             bool returnLivenessBond = blk.livenessBond > 0 && _proof.data.length == 32

366:                 reward = _ts.contestBond >> 2;

370:                 reward = _ts.validityBond >> 2;

415:             + _tier.provingWindow * 60 >= block.timestamp;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

63:         blk.nextTransitionId = 2;

152:                         uint256(ITierProvider(tierProvider).getTier(ts.tier).cooldownWindow) * 60

251:                 || _config.blockMaxTxListBytes > 128 * 1024 // calldata up to 128K

256:             || _config.ethDepositMaxCountPerBlock > 32

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

24:                 validityBond: 250 ether, // TKO

25:                 contestBond: 500 ether, // TKO

26:                 cooldownWindow: 1440, //24 hours

27:                 provingWindow: 120, // 2 hours

28:                 maxBlocksToVerifyPerProof: 16

37:                 cooldownWindow: 60, //1 hours

38:                 provingWindow: 2880, // 48 hours

39:                 maxBlocksToVerifyPerProof: 16

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

24:                 validityBond: 250 ether, // TKO

25:                 contestBond: 500 ether, // TKO

26:                 cooldownWindow: 1440, //24 hours

27:                 provingWindow: 60, // 1 hours

28:                 maxBlocksToVerifyPerProof: 8

35:                 validityBond: 500 ether, // TKO

36:                 contestBond: 1000 ether, // TKO

37:                 cooldownWindow: 1440, //24 hours

38:                 provingWindow: 240, // 4 hours

39:                 maxBlocksToVerifyPerProof: 4

48:                 cooldownWindow: 60, //1 hours

49:                 provingWindow: 2880, // 48 hours

50:                 maxBlocksToVerifyPerProof: 16

68:         if (_rand % 1000 == 0) return LibTiers.TIER_SGX_ZKVM;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

24:                 validityBond: 250 ether, // TKO

25:                 contestBond: 500 ether, // TKO

26:                 cooldownWindow: 1440, //24 hours

27:                 provingWindow: 30, // 0.5 hours

28:                 maxBlocksToVerifyPerProof: 12

35:                 validityBond: 500 ether, // TKO

36:                 contestBond: 1000 ether, // TKO

37:                 cooldownWindow: 1440, //24 hours

38:                 provingWindow: 60, // 1 hours

39:                 maxBlocksToVerifyPerProof: 8

48:                 cooldownWindow: 60, //1 hours

49:                 provingWindow: 2880, // 48 hours

50:                 maxBlocksToVerifyPerProof: 16

68:         if (_rand % 10 == 0) return LibTiers.TIER_SGX;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

202:         if (_blockId + 256 >= block.number) return blockhash(_blockId);

213:         config_.gasTargetPerL1Block = 15 * 1e6 * 4;

214:         config_.basefeeAdjustmentQuotient = 8;

234:             for (uint256 i; i < 255 && _blockId >= i + 1; ++i) {

236:                 inputs[j % 255] = blockhash(j);

243:             publicInputHashOld := keccak256(inputs, 8192 /*mul(256, 32)*/ )

246:         inputs[_blockId % 255] = blockhash(_blockId);

248:             publicInputHashNew := keccak256(inputs, 8192 /*mul(256, 32)*/ )

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

266:                 if (i == n - 2) {

313:         bytes32 expectedAuthDataHash = bytes32(qeEnclaveReport.reportData.substring(0, 32));

420:             for (uint256 i; i < 3; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

171:             sigPtr = NodePtr.getPtr(sigPtr.ixs() + 3, sigPtr.ixf() + 3, sigPtr.ixl());

174:             bytes memory sigX = _trimBytes(der.bytesAt(sigPtr), 32);

177:             bytes memory sigY = _trimBytes(der.bytesAt(sigPtr), 32);

180:             cert.pubKey = _trimBytes(der.bytesAt(subjectPublicKeyInfoPtr), 64);

358:             uint16 svnValue = svnValueBytes.length < 2

359:                 ? uint16(bytes2(svnValueBytes)) / 256

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

33:         uint256 localAuthDataSize = littleEndianDecode(quote.substring(432, 4));

34:         if (quote.length - 436 != localAuthDataSize) {

38:         bytes memory rawHeader = quote.substring(0, 48);

51:         bytes memory rawLocalEnclaveReport = quote.substring(48, 384);

78:             localEnclaveReport.reserved3.length == 96 && localEnclaveReport.reserved4.length == 60

79:                 && localEnclaveReport.reportData.length == 64,

83:             pckSignedQeReport.reserved3.length == 96 && pckSignedQeReport.reserved4.length == 60

84:                 && pckSignedQeReport.reportData.length == 64,

88:             v3Quote.v3AuthData.certification.certType == 5,

89:             "certType must be 5: Concatenated PCK Cert Chain (PEM formatted)"

92:             v3Quote.v3AuthData.certification.decodedCertDataArray.length == 3, "3 certs in chain"

95:             v3Quote.v3AuthData.ecdsa256BitSignature.length == 64

96:                 && v3Quote.v3AuthData.ecdsaAttestationKey.length == 64

97:                 && v3Quote.v3AuthData.qeReportSignature.length == 64,

106:         uint32 totalQuoteSize = 48 // header

107:             + 384 // local QE report

108:             + 64 // ecdsa256BitSignature

109:             + 64 // ecdsaAttestationKey

110:             + 384 // QE report

111:             + 64 // qeReportSignature

112:             + 2 // sizeof(v3Quote.v3AuthData.qeAuthData.parsedDataSize)

113:             + v3Quote.v3AuthData.qeAuthData.parsedDataSize + 2 // sizeof(v3Quote.v3AuthData.certification.certType)

114:             + 4 // sizeof(v3Quote.v3AuthData.certification.certDataSize)

138:         enclaveReport.cpuSvn = bytes16(rawEnclaveReport.substring(0, 16));

139:         enclaveReport.miscSelect = bytes4(rawEnclaveReport.substring(16, 4));

140:         enclaveReport.reserved1 = bytes28(rawEnclaveReport.substring(20, 28));

141:         enclaveReport.attributes = bytes16(rawEnclaveReport.substring(48, 16));

142:         enclaveReport.mrEnclave = bytes32(rawEnclaveReport.substring(64, 32));

143:         enclaveReport.reserved2 = bytes32(rawEnclaveReport.substring(96, 32));

144:         enclaveReport.mrSigner = bytes32(rawEnclaveReport.substring(128, 32));

145:         enclaveReport.reserved3 = rawEnclaveReport.substring(160, 96);

146:         enclaveReport.isvProdId = uint16(littleEndianDecode(rawEnclaveReport.substring(256, 2)));

147:         enclaveReport.isvSvn = uint16(littleEndianDecode(rawEnclaveReport.substring(258, 2)));

148:         enclaveReport.reserved4 = rawEnclaveReport.substring(260, 60);

149:         enclaveReport.reportData = rawEnclaveReport.substring(320, 64);

155:             uint256 upperDigit = digits / 16;

156:             uint256 lowerDigit = digits % 16;

170:         bytes2 version = bytes2(rawHeader.substring(0, 2));

175:         bytes2 attestationKeyType = bytes2(rawHeader.substring(2, 2));

180:         bytes4 teeType = bytes4(rawHeader.substring(4, 4));

185:         bytes16 qeVendorId = bytes16(rawHeader.substring(12, 16));

194:             qeSvn: bytes2(rawHeader.substring(8, 2)),

195:             pceSvn: bytes2(rawHeader.substring(10, 2)),

197:             userData: bytes20(rawHeader.substring(28, 20))

212:         qeAuthData.parsedDataSize = uint16(littleEndianDecode(rawAuthData.substring(576, 2)));

215:         uint256 offset = 578 + qeAuthData.parsedDataSize;

217:         cert.certType = uint16(littleEndianDecode(rawAuthData.substring(offset, 2)));

218:         if (cert.certType < 1 || cert.certType > 5) {

221:         offset += 2;

222:         cert.certDataSize = uint32(littleEndianDecode(rawAuthData.substring(offset, 4)));

223:         offset += 4;

227:         authDataV3.ecdsa256BitSignature = rawAuthData.substring(0, 64);

228:         authDataV3.ecdsaAttestationKey = rawAuthData.substring(64, 64);

229:         bytes memory rawQeReport = rawAuthData.substring(128, 384);

231:         authDataV3.qeReportSignature = rawAuthData.substring(512, 64);

249:         uint16 isvProdIdPackBE = (enclaveReport.isvProdId >> 8) | (enclaveReport.isvProdId << 8);

250:         uint16 isvSvnPackBE = (enclaveReport.isvSvn >> 8) | (enclaveReport.isvSvn << 8);

278:             pemCertLib.splitCertificateChain(certBytes, 3);

281:         for (uint256 i; i < 3; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol

20:         return uint80(self >> 80);

25:         return uint80(self >> 160);

30:         _ixs |= _ixf << 80;

31:         _ixs |= _ixl << 160;

145:         return uint256(der.readBytesN(ptr.ixf(), len) >> (32 - len) * 8);

193:             ixFirstContentByte = uint80(ix + 2);

198:                 length = der.readUint8(ix + 2);

199:             } else if (lengthbytesLength == 2) {

200:                 length = der.readUint16(ix + 2);

203:                     der.readBytesN(ix + 2, lengthbytesLength) >> (32 - lengthbytesLength) * 8

206:             ixFirstContentByte = uint80(ix + 2 + lengthbytesLength);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

27:             ret := keccak256(add(add(self, 32), offset), len)

77:             selfptr := add(self, add(offset, 32))

78:             otherptr := add(other, add(otheroffset, 32))

80:         for (uint256 idx = 0; idx < shortest; idx += 32) {

90:                 if (shortest > 32) {

100:             selfptr += 32;

101:             otherptr += 32;

199:         require(idx + 2 <= self.length, "invalid idx");

201:             ret := and(mload(add(add(self, 2), idx)), 0xFFFF)

212:         require(idx + 4 <= self.length, "unexpected idx");

214:             ret := and(mload(add(add(self, 4), idx)), 0xFFFFFFFF)

225:         require(idx + 32 <= self.length, "unexpected idx");

227:             ret := mload(add(add(self, 32), idx))

238:         require(idx + 20 <= self.length, "unexpected idx");

242:                     mload(add(add(self, 32), idx)),

264:         require(len <= 32, "unexpected len");

268:             ret := and(mload(add(add(self, 32), idx)), mask)

300:             dest := add(ret, 32)

301:             src := add(add(self, 32), offset)

329:         require(len <= 52, "unexpected len");

341:             ret = (ret << 5) | decoded;

344:         uint256 bitlen = len * 5;

345:         if (len % 8 == 0) {

347:             ret = (ret << 5) | decoded;

348:         } else if (len % 8 == 2) {

350:             ret = (ret << 3) | (decoded >> 2);

351:             bitlen -= 2;

352:         } else if (len % 8 == 4) {

354:             ret = (ret << 1) | (decoded >> 4);

355:             bitlen -= 4;

356:         } else if (len % 8 == 5) {

358:             ret = (ret << 4) | (decoded >> 1);

360:         } else if (len % 8 == 7) {

362:             ret = (ret << 2) | (decoded >> 3);

363:             bitlen -= 3;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol

11:     Copyright 2016, Adrià Massanet

15:     the Free Software Foundation, either version 3 of the License, or

102:                     sub(gas(), 2000),

103:                     5,

125:         if (uint8(decipher[decipherlen - 50]) == 0x31) {

128:         } else if (uint8(decipher[decipherlen - 48]) == 0x2f) {

135:         uint256 paddingLen = decipherlen - 5 - digestAlgoWithParamLen - 32;

140:         for (uint256 i = 2; i < 2 + paddingLen; ++i) {

250:                     sub(gas(), 2000),

251:                     5,

268:         uint256 paddingLen = decipherlen - 3 - sha1Prefix.length - 20;

273:         for (uint256 i = 2; i < 2 + paddingLen; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SHA1.sol

18:             data := add(data, 32)

21:             let totallen := add(and(add(len, 1), 0xFFFFFFFFFFFFFFC0), 64)

22:             switch lt(sub(totallen, len), 9)

23:             case 1 { totallen := add(totallen, 64) }

32:                     if lt(count, 32) {

39:             for { let i := 0 } lt(i, totallen) { i := add(i, 64) } {

41:                 mstore(add(scratch, 32), readword(data, add(i, 32), len))

44:                 switch lt(sub(len, i), 64)

48:                 switch eq(i, sub(totallen, 64))

49:                 case 1 { mstore(add(scratch, 32), or(mload(add(scratch, 32)), mul(len, 8))) }

52:                 for { let j := 64 } lt(j, 128) { j := add(j, 12) } {

55:                             xor(mload(add(scratch, sub(j, 12))), mload(add(scratch, sub(j, 32)))),

56:                             xor(mload(add(scratch, sub(j, 56))), mload(add(scratch, sub(j, 64))))

61:                                 mul(temp, 2),

71:                 for { let j := 128 } lt(j, 320) { j := add(j, 24) } {

74:                             xor(mload(add(scratch, sub(j, 24))), mload(add(scratch, sub(j, 64)))),

75:                             xor(mload(add(scratch, sub(j, 112))), mload(add(scratch, sub(j, 128))))

80:                                 mul(temp, 4),

94:                 for { let j := 0 } lt(j, 80) { j := add(j, 1) } {

95:                     switch div(j, 20)

113:                     case 2 {

131:                     case 3 {

151:                                 mload(add(scratch, mul(j, 4))),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SHA1.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol

89:         bytes memory exponent = publicKey.substring(0, 3);

90:         bytes memory modulus = publicKey.substring(3, publicKey.length - 3);

106:         bytes memory exponent = publicKey.substring(0, 3);

107:         bytes memory modulus = publicKey.substring(3, publicKey.length - 3);

123:         if (signature.length != 64) {

126:         uint256 r = uint256(bytes32(signature.substring(0, 32)));

127:         uint256 s = uint256(bytes32(signature.substring(32, 32)));

129:         if (publicKey.length != 64) {

132:         uint256 gx = uint256(bytes32(publicKey.substring(0, 32)));

133:         uint256 gy = uint256(bytes32(publicKey.substring(32, 32)));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

17:         if (x509Time.length == 13) {

18:             if (uint8(x509Time[0]) - 48 < 5) yrs += 2000;

19:             else yrs += 1900;

21:             yrs += (uint8(x509Time[0]) - 48) * 1000 + (uint8(x509Time[1]) - 48) * 100;

22:             offset = 2;

24:         yrs += (uint8(x509Time[offset + 0]) - 48) * 10 + uint8(x509Time[offset + 1]) - 48;

25:         mnths = (uint8(x509Time[offset + 2]) - 48) * 10 + uint8(x509Time[offset + 3]) - 48;

26:         dys += (uint8(x509Time[offset + 4]) - 48) * 10 + uint8(x509Time[offset + 5]) - 48;

27:         hrs += (uint8(x509Time[offset + 6]) - 48) * 10 + uint8(x509Time[offset + 7]) - 48;

28:         mins += (uint8(x509Time[offset + 8]) - 48) * 10 + uint8(x509Time[offset + 9]) - 48;

29:         secs += (uint8(x509Time[offset + 10]) - 48) * 10 + uint8(x509Time[offset + 11]) - 48;

48:         for (uint16 i = 1970; i < year; ++i) {

50:                 timestamp += 31_622_400; // Leap year in seconds

52:                 timestamp += 31_536_000; // Normal year in seconds

56:         uint8[12] memory monthDays = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];

57:         if (isLeapYear(year)) monthDays[1] = 29;

60:             timestamp += uint256(monthDays[i - 1]) * 86_400; // Days in seconds

63:         timestamp += uint256(day - 1) * 86_400; // Days in seconds

64:         timestamp += uint256(hour) * 3600; // Hours in seconds

65:         timestamp += uint256(minute) * 60; // Minutes in seconds

72:         if (year % 4 != 0) return false;

73:         if (year % 100 != 0) return true;

74:         if (year % 400 != 0) return false;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

428:             return (1 hours, 384 seconds);

430:             block.chainid == 2 // Ropsten

431:                 || block.chainid == 4 // Rinkeby

432:                 || block.chainid == 5 // Goerli

433:                 || block.chainid == 42 // Kovan

434:                 || block.chainid == 17_000 // Holesky

435:                 || block.chainid == 11_155_111 // Sepolia

438:             return (30 minutes, 384 seconds);

439:         } else if (block.chainid >= 32_300 && block.chainid <= 32_400) {

441:             return (5 minutes, 384 seconds);

491:             _message.data.length >= 4 // msg can be empty

501:                 64, // return max 64 bytes

546:                 tstore(add(_CTX_SLOT, 2), _srcChainId)

563:                 srcChainId := tload(add(_CTX_SLOT, 2))

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/libs/Lib4844.sol

17:         52_435_875_175_126_190_479_447_740_508_185_965_837_690_552_500_527_637_822_603_658_699_938_581_184_513;

49:         if (ret.length != 64) revert EVAL_FAILED_2();

54:             first := mload(add(ret, 32))

55:             second := mload(add(ret, 64))

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/Lib4844.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

31:             64, // return max 64 bytes

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/Bytes.sol

25:             require(_length + 31 >= _length, "slice_overflow");

47:                 let lengthmod := and(_length, 31)

69:                 mstore(0x40, and(add(mc, 31), not(31)))

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/Bytes.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

44:             ptr := add(_in, 32)

213:                 strLen > 55,

214:                 "RLPReader: length of content must be greater than 55 bytes (long string)"

259:                 listLen > 55,

260:                 "RLPReader: length of content must be greater than 55 bytes (long list)"

296:             let dest := add(out_, 32)

298:             for { } lt(i, _length) { i := add(i, 32) } { mstore(add(dest, i), mload(add(src, i))) }

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

14:         if (_in.length == 1 && uint8(_in[0]) < 128) {

17:             out_ = abi.encodePacked(_writeLength(_in.length, 128), _in);

33:         if (_len < 56) {

41:                 i *= 256;

45:             out_[0] = bytes1(uint8(lenLen) + uint8(_offset) + 55);

47:                 out_[i] = bytes1(uint8((_len / (256 ** (lenLen - i))) % 256));

59:         for (; i < 32; i++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

97:             } else if (currentNode.encoded.length >= 32) {

142:                 uint8 offset = 2 - (prefix % 2);

221:         id_ = _node.length < 32 ? RLPReader.readRawBytes(_node) : RLPReader.readBytes(_node);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/LibBridgedToken.sol

56:                 Strings.toHexString(uint160(_srcToken), 20),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

152:         if (_proof.data.length != 89) revert SGX_INVALID_PROOF();

154:         uint32 id = uint32(bytes4(Bytes.slice(_proof.data, 0, 4)));

155:         address newInstance = address(bytes20(Bytes.slice(_proof.data, 4, 20)));

156:         bytes memory signature = Bytes.slice(_proof.data, 24);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="NC-4"></a>[NC-4] Control structures do not follow the Solidity Style Guide
See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (329)*:
```solidity
File: packages/protocol/contracts/L1/ITaikoL1.sol

31:     function verifyBlocks(uint64 _maxBlocksToVerify) external;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/ITaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoData.sol

24:         uint64 maxBlocksToVerifyPerProposal;

96:         bytes32 difficulty; // slot 2

97:         bytes32 blobHash; //or txListHash (if Blob not yet supported), // slot 3

145:         uint32 verifiedTransitionId;

170:         uint64 lastVerifiedBlockId;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoData.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoErrors.sol

34:     error L1_MISSING_VERIFIER();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoErrors.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoEvents.sol

37:     event BlockVerified(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoEvents.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

8: import "./libs/LibVerifying.sol";

29:         if (state.slotB.provingPaused) revert L1_PROVING_PAUSED();

35:         if (!_inNonReentrant()) revert L1_RECEIVE_DISABLED();

51:         LibVerifying.init(state, getConfig(), _genesisBlockHash);

70:             LibVerifying.verifyBlocks(state, config, this, config.maxBlocksToVerifyPerProposal);

90:         if (_blockId != meta.id) revert L1_INVALID_BLOCK_ID();

94:         uint8 maxBlocksToVerify = LibProving.proveBlock(state, config, this, meta, tran, proof);

96:         LibVerifying.verifyBlocks(state, config, this, maxBlocksToVerify);

100:     function verifyBlocks(uint64 _maxBlocksToVerify)

106:         LibVerifying.verifyBlocks(state, getConfig(), this, _maxBlocksToVerify);

154:             ts_ = state.transitions[slot][blk_.verifiedTransitionId];

198:             maxBlocksToVerifyPerProposal: 10,

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

61:         if (_to == address(this)) revert TKO_INVALID_ADDR();

79:         if (_to == address(this)) revert TKO_INVALID_ADDR();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

81:         if (_signatures.length != _calldatas.length) revert TG_INVALID_SIGNATURES_LENGTH();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

81:         if (

173:             if (_tierFees[i].tier == _tierId) return _tierFees[i].fee;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

150:         if (_amount > type(uint96).max) revert L1_INVALID_ETH_DEPOSIT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

94:         if (!_isProposerPermitted(b, _resolver)) revert L1_UNAUTHORIZED();

123:                 difficulty: 0, // to be initialized below

142:             if (!_config.blobAllowedForDA) revert L1_BLOB_FOR_DA_DISABLED();

145:                 if (!_config.blobReuseEnabled) revert L1_BLOB_REUSE_DISABLED();

159:                 if (meta_.blobHash == 0) revert L1_BLOB_NOT_FOUND();

182:             if (!LibAddress.isSenderEOA()) revert L1_PROPOSER_NOT_EOA();

204:         meta_.difficulty = keccak256(abi.encodePacked(block.prevrandao, b.numBlocks, block.number));

208:             uint256(meta_.difficulty)

224:             verifiedTransitionId: 0,

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

7: import "../../verifiers/IVerifier.sol";

67:     error L1_MISSING_VERIFIER();

74:         if (_state.slotB.provingPaused == _pause) revert L1_INVALID_PAUSE_STATUS();

100:         returns (uint8 maxBlocksToVerify_)

161:             address verifier = _resolver.resolve(tier.verifierName, true);

177:                 IVerifier(verifier).verifyProof(ctx, _tran, _proof);

182:                 revert L1_MISSING_VERIFIER();

219:             if (sameTransition) revert L1_ALREADY_PROVED();

239:                 if (ts.contester != address(0)) revert L1_ALREADY_CONTESTED();

265:         return tier.maxBlocksToVerifyPerProof;

374:             if (_sameTransition) revert L1_ALREADY_PROVED();

412:         if (_tier.contestBond == 0) return;

420:             if (!isAssignedPover) revert L1_NOT_ASSIGNED_PROVER();

424:             if (isAssignedPover) revert L1_ASSIGNED_PROVER_NOT_ALLOWED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibUtils.sol

40:         if (blk.blockId != _blockId) revert L1_BLOCK_MISMATCH();

43:         if (tid == 0) revert L1_TRANSITION_NOT_FOUND();

86:         if (tid_ >= _blk.nextTransitionId) revert L1_UNEXPECTED_TRANSITION_ID();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibUtils.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

28:     event BlockVerified(

54:         if (!_isConfigValid(_config)) revert L1_INVALID_CONFIG();

65:         blk.verifiedTransitionId = 1;

85:     function verifyBlocks(

89:         uint64 _maxBlocksToVerify

100:         uint64 blockId = b.lastVerifiedBlockId;

105:         if (blk.blockId != blockId) revert L1_BLOCK_MISMATCH();

107:         uint32 tid = blk.verifiedTransitionId;

111:         if (tid == 0) revert L1_TRANSITION_ID_ZERO();

117:         uint64 numBlocksVerified;

131:                 if (blk.blockId != blockId) revert L1_BLOCK_MISMATCH();

137:                 if (tid == 0) break;

151:                     if (

162:                 blk.verifiedTransitionId = tid;

209:                 ++numBlocksVerified;

213:                 uint64 lastVerifiedBlockId = b.lastVerifiedBlockId + numBlocksVerified;

216:                 _state.slotB.lastVerifiedBlockId = lastVerifiedBlockId;

219:                 _syncChainData(_config, _resolver, lastVerifiedBlockId, stateRoot);

227:         uint64 _lastVerifiedBlockId,

240:                 _config.chainId, LibSignals.STATE_ROOT, _lastVerifiedBlockId, _stateRoot

246:         if (

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/provers/GuardianProver.sol

45:         if (_proof.tier != LibTiers.TIER_GUARDIAN) revert INVALID_PROOF();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

68:         if (_minGuardians < (_newGuardians.length + 1) >> 1 || _minGuardians > _newGuardians.length)

82:             if (guardian == address(0)) revert INVALID_GUARDIAN();

84:             if (guardianIds[guardian] != 0) revert INVALID_GUARDIAN_SET();

113:         if (id == 0) revert INVALID_GUARDIAN();

134:                 if (bits & 1 == 1) ++count;

135:                 if (count == minGuardians) return true;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

23:                 verifierName: "tier_optimistic",

28:                 maxBlocksToVerifyPerProof: 16

34:                 verifierName: "tier_guardian",

39:                 maxBlocksToVerifyPerProof: 16

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/ITierProvider.sol

9:         bytes32 verifierName;

14:         uint8 maxBlocksToVerifyPerProof;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/ITierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

23:                 verifierName: "tier_sgx",

28:                 maxBlocksToVerifyPerProof: 8

34:                 verifierName: "tier_sgx_zkvm",

39:                 maxBlocksToVerifyPerProof: 4

45:                 verifierName: "tier_guardian",

50:                 maxBlocksToVerifyPerProof: 16

68:         if (_rand % 1000 == 0) return LibTiers.TIER_SGX_ZKVM;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

23:                 verifierName: "tier_optimistic",

28:                 maxBlocksToVerifyPerProof: 12

34:                 verifierName: "tier_sgx",

39:                 maxBlocksToVerifyPerProof: 8

45:                 verifierName: "tier_guardian",

50:                 maxBlocksToVerifyPerProof: 16

68:         if (_rand % 10 == 0) return LibTiers.TIER_SGX;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

43:         if (txId != nextTxId) revert XCO_INVALID_TX_ID();

51:         if (!success) revert XCO_TX_REVERTED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

116:         if (

123:         if (msg.sender != GOLDEN_TOUCH_ADDRESS) revert L2_INVALID_SENDER();

172:         if (_to == address(0)) revert L2_INVALID_PARAM();

201:         if (_blockId >= block.number) return 0;

202:         if (_blockId + 256 >= block.number) return blockhash(_blockId);

296:         if (basefee_ == 0) basefee_ = 1;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol

33:         if (_newConfig.gasTargetPerL1Block == 0) revert L2_INVALID_CONFIG();

34:         if (_newConfig.basefeeAdjustmentQuotient == 0) revert L2_INVALID_CONFIG();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

18: import { ISigVerifyLib } from "./interfaces/ISigVerifyLib.sol";

25:     ISigVerifyLib public immutable sigVerifyLib;

55:         sigVerifyLib = ISigVerifyLib(sigVerifyLibAddr);

139:         (bool success,) = _verify(data);

172:         return _verifyParsedQuote(parsedV3Quote);

175:     function _verifyQEReportWithIdentity(V3Struct.EnclaveReport memory quoteEnclaveReport)

207:         IPEMCertChainLib.PCKCertificateField memory pck,

248:     function _verifyCertChain(IPEMCertChainLib.ECSha256Certificate[] memory certs)

256:         bool verified;

260:             IPEMCertChainLib.ECSha256Certificate memory issuer;

285:             verified = sigVerifyLib.verifyES256Signature(

286:                 certs[i].tbsCertificate, certs[i].signature, issuer.pubKey

300:         return !certRevoked && certNotExpired && verified && certChainCanBeTrusted;

303:     function _enclaveReportSigVerification(

322:             bool qeSigVerified = sigVerifyLib.verifyES256Signature(

325:             bool quoteSigVerified = sigVerifyLib.verifyES256Signature(

328:             return qeSigVerified && quoteSigVerified;

355:     function verifyParsedQuote(V3Struct.ParsedV3QuoteStruct calldata v3quote)

361:         return _verifyParsedQuote(v3quote);

364:     function _verifyParsedQuote(V3Struct.ParsedV3QuoteStruct memory v3quote)

400:             bool verifiedEnclaveIdSuccessfully;

401:             (verifiedEnclaveIdSuccessfully, qeTcbStatus) =

402:                 _verifyQEReportWithIdentity(v3quote.v3AuthData.pckSignedQeReport);

406:             if (

407:                 !verifiedEnclaveIdSuccessfully

415:         IPEMCertChainLib.ECSha256Certificate[] memory parsedQuoteCerts;

419:             parsedQuoteCerts = new IPEMCertChainLib.ECSha256Certificate[](3);

425:                     authDataV3.certification.decodedCertDataArray[i], isPckCert

442:             IPEMCertChainLib.ECSha256Certificate memory pckCert = parsedQuoteCerts[0];

453:             bool tcbVerified;

454:             (tcbVerified, tcbStatus) = _checkTcbLevels(parsedQuoteCerts[0].pck, fetchedTcbInfo);

463:             bool pckCertChainVerified = _verifyCertChain(parsedQuoteCerts);

471:             bool enclaveReportSigsVerified = _enclaveReportSigVerification(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol

9:     function verifyAttestation(bytes calldata data) external returns (bool);

10:     function verifyParsedQuote(V3Struct.ParsedV3QuoteStruct calldata v3quote)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/interfaces/ISigVerifyLib.sol

26:         bytes tbsCertificate;

38:     function verifyAttStmtSignature(

48:     function verifyCertificateSignature(

58:     function verifyRS256Signature(

67:     function verifyRS1Signature(

76:     function verifyES256Signature(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/interfaces/ISigVerifyLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

17:     string internal constant HEADER = "-----BEGIN CERTIFICATE-----";

18:     string internal constant FOOTER = "-----END CERTIFICATE-----";

22:     string internal constant PCK_COMMON_NAME = "Intel SGX PCK Certificate";

40:     function splitCertificateChain(

80:         returns (bool success, ECSha256Certificate memory cert)

134:             if (

179:             cert.tbsCertificate = der.allBytesAt(tbsParentPtr);

265:         uint256 lengthDiff = n - expectedLength;

266:         output = input.substring(lengthDiff, expectedLength);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

39:         (bool headerVerifiedSuccessfully, V3Struct.Header memory header) =

40:             parseAndVerifyHeader(rawHeader);

45:         (bool authDataVerifiedSuccessfully, V3Struct.ECDSAQuoteV3AuthData memory authDataV3) =

46:             parseAuthDataAndVerifyCertType(quote.substring(436, localAuthDataSize), pemCertLibAddr);

88:             v3Quote.v3AuthData.certification.certType == 5,

92:             v3Quote.v3AuthData.certification.decodedCertDataArray.length == 3, "3 certs in chain"

113:             + v3Quote.v3AuthData.qeAuthData.parsedDataSize + 2 // sizeof(v3Quote.v3AuthData.certification.certType)

114:             + 4 // sizeof(v3Quote.v3AuthData.certification.certDataSize)

115:             + v3Quote.v3AuthData.certification.certDataSize;

165:     function parseAndVerifyHeader(bytes memory rawHeader)

203:     function parseAuthDataAndVerifyCertType(

216:         V3Struct.CertificationData memory cert;

225:         cert.decodedCertDataArray = parseCerificationChainBytes(certData, pemCertLibAddr);

233:         authDataV3.certification = cert;

267:     function parseCerificationChainBytes(

276:         IPEMCertChainLib.ECSha256Certificate[] memory parsedQuoteCerts;

278:             pemCertLib.splitCertificateChain(certBytes, 3);

279:         require(certParsedSuccessfully, "splitCertificateChain failed");

280:         parsedQuoteCerts = new IPEMCertChainLib.ECSha256Certificate[](3);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol

53:         CertificationData certification;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Struct.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/interfaces/IPEMCertChainLib.sol

11:         bytes tbsCertificate;

15:         PCKCertificateField pck;

36:     function splitCertificateChain(

50:         returns (bool success, ECSha256Certificate memory cert);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/interfaces/IPEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

95:                 uint256 diff = (a & mask) - (b & mask);

97:                     return int256(diff);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol

13:     This program is free software: you can redistribute it and/or modify

24:     along with this program.  If not, see <http://www.gnu.org/licenses/>.

167:         if (

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol

4: import "../interfaces/ISigVerifyLib.sol";

5: import "./RsaVerify.sol";

18:     address private ES256VERIFIER;

21:         ES256VERIFIER = es256Verifier;

24:     function verifyAttStmtSignature(

38:             return verifyRS256Signature(tbs, signature, publicKey.pubKey);

43:             return verifyES256Signature(tbs, signature, publicKey.pubKey);

48:             return verifyRS1Signature(tbs, signature, publicKey.pubKey);

54:     function verifyCertificateSignature(

68:             return verifyRS256Signature(tbs, signature, publicKey.pubKey);

73:             return verifyRS1Signature(tbs, signature, publicKey.pubKey);

79:     function verifyRS256Signature(

93:         sigValid = RsaVerify.pkcs1Sha256Raw(tbs, signature, exponent, modulus);

96:     function verifyRS1Signature(

110:         sigValid = RsaVerify.pkcs1Sha1Raw(tbs, signature, exponent, modulus);

113:     function verifyES256Signature(

137:         (bool success, bytes memory ret) = ES256VERIFIER.staticcall(args);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SigVerifyLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

18:             if (uint8(x509Time[0]) - 48 < 5) yrs += 2000;

57:         if (isLeapYear(year)) monthDays[1] = 29;

72:         if (year % 4 != 0) return false;

73:         if (year % 100 != 0) return true;

74:         if (year % 400 != 0) return false;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

65:         if (_chainId != block.chainid) revert B_INVALID_CHAINID();

109:         if (addressBanned[_addr] == _ban) revert B_INVALID_STATUS();

132:         if (!destChainEnabled) revert B_INVALID_CHAINID();

139:         if (expectedAmount != msg.value) revert B_INVALID_VALUE();

166:         if (messageStatus[msgHash] != Status.NEW) revert B_STATUS_MISMATCH();

227:         if (messageStatus[msgHash] != Status.NEW) revert B_STATUS_MISMATCH();

269:             if (

322:             if (msg.sender != _message.destOwner) revert B_PERMISSION_DENIED();

341:         if (_message.srcChainId != block.chainid) return false;

360:         if (_message.srcChainId != block.chainid) return false;

382:         if (_message.destChainId != block.chainid) return false;

423:         if (

429:         } else if (

485:         if (_gasLimit == 0) revert B_INVALID_GAS_LIMIT();

490:         if (

516:         if (messageStatus[_msgHash] == _status) return;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/common/AddressManager.sol

48:         if (_newAddress == oldAddress) revert AM_INVALID_PARAMS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/AddressManager.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

42:         if (msg.sender != owner() && msg.sender != resolve(_name, true)) revert RESOLVER_DENIED();

47:         if (_loadReentryLock() == _TRUE) revert REENTRANT_CALL();

54:         if (!paused()) revert INVALID_PAUSE_STATUS();

59:         if (paused()) revert INVALID_PAUSE_STATUS();

105:         if (_addressManager == address(0)) revert ZERO_ADDR_MANAGER();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/libs/Lib4844.sol

40:         if (_x >= BLS_MODULUS) revert POINT_X_TOO_LARGE();

41:         if (_y >= BLS_MODULUS) revert POINT_Y_TOO_LARGE();

47:         if (!ok) revert EVAL_FAILED_1();

49:         if (ret.length != 64) revert EVAL_FAILED_2();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/Lib4844.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

24:         if (_to == address(0)) revert ETH_TRANSFER_FAILED();

36:         if (!success) revert ETH_TRANSFER_FAILED();

54:         if (!Address.isContract(_addr)) return false;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/libs/LibTrieProof.sol

34:     function verifyMerkleProof(

50:             if (rlpAccount.length == 0) revert LTP_INVALID_ACCOUNT_PROOF();

60:         bool verified = SecureMerkleTrie.verifyInclusionProof(

64:         if (!verified) revert LTP_INVALID_INCLUSION_PROOF();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibTrieProof.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

36:         if (_app == address(0)) revert SS_INVALID_SENDER();

41:         if (_input == 0) revert SS_INVALID_VALUE();

57:         if (isAuthorized[_addr] == _authorize) revert SS_INVALID_STATE();

77:         if (!isAuthorized[msg.sender]) revert SS_UNAUTHORIZED();

95:         if (hopProofs.length == 0) revert SS_EMPTY_PROOF();

107:             bytes32 signalRoot = _verifyHopProof(chainId, app, signal, value, hop, signalService);

111:                 if (hop.chainId != block.chainid) revert SS_INVALID_LAST_HOP_CHAINID();

172:             if (chainData_ == 0) revert SS_SIGNAL_NOT_FOUND();

206:     function _verifyHopProof(

221:         return LibTrieProof.verifyMerkleProof(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

37:         uint64 grantCliff;

46:         uint64 unlockCliff;

121:         if (_taikoToken == address(0)) revert INVALID_PARAM();

124:         if (_costToken == address(0)) revert INVALID_PARAM();

127:         if (_sharedVault == address(0)) revert INVALID_PARAM();

136:         if (_recipient == address(0)) revert INVALID_PARAM();

137:         if (recipients[_recipient].grant.amount != 0) revert ALREADY_GRANTED();

154:         if (amountVoided == 0) revert NOTHING_TO_VOID();

169:         if (_to == address(0)) revert INVALID_PARAM();

236:         return _calcAmount(_grant.amount, _grant.grantStart, _grant.grantCliff, _grant.grantPeriod);

241:             _getAmountOwned(_grant), _grant.unlockStart, _grant.unlockCliff, _grant.unlockPeriod

248:         uint64 _cliff,

255:         if (_amount == 0) return 0;

256:         if (_start == 0) return _amount;

257:         if (block.timestamp <= _start) return 0;

259:         if (_period == 0) return _amount;

260:         if (block.timestamp >= _start + _period) return _amount;

262:         if (block.timestamp <= _cliff) return 0;

268:         if (_grant.amount == 0) revert INVALID_GRANT();

269:         _validateCliff(_grant.grantStart, _grant.grantCliff, _grant.grantPeriod);

270:         _validateCliff(_grant.unlockStart, _grant.unlockCliff, _grant.unlockPeriod);

275:             if (_cliff > 0) revert INVALID_GRANT();

277:             if (_cliff > 0 && _cliff <= _start) revert INVALID_GRANT();

278:             if (_cliff >= _start + _period) revert INVALID_GRANT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol

60:         _verifyClaim(abi.encode(user, amount), proof);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

80:         _verifyClaim(abi.encode(user, amount), proof);

111:         if (balance == 0) return (0, 0);

114:         if (block.timestamp < claimEnd) return (balance, 0);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

56:         _verifyClaim(abi.encode(user, tokenIds), proof);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

34:         if (

70:         if (isClaimed[hash]) revert CLAIMED_ALREADY();

71:         if (!_verifyMerkleProof(proof, merkleRoot, hash)) revert INVALID_PROOF();

77:     function _verifyMerkleProof(

87:         return MerkleProof.verify(_proof, _merkleRoot, _value);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

50:     function verifyInclusionProof(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol

19:     function verifyInclusionProof(

30:         valid_ = MerkleTrie.verifyInclusionProof(key, _value, _proof, _root);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseNFTVault.sol

149:         if (_op.token == address(0)) revert VAULT_INVALID_TOKEN();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseNFTVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

55:         if (ctx_.from != selfOnSourceChain) revert VAULT_PERMISSION_DENIED();

65:         if (ctx_.from != msg.sender) revert VAULT_PERMISSION_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

137:         if (_to == address(this)) revert BTOKEN_CANNOT_RECEIVE();

138:         if (paused()) revert INVALID_PAUSE_STATUS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

147:         if (_to == address(this)) revert BTOKEN_CANNOT_RECEIVE();

148:         if (paused()) revert INVALID_PAUSE_STATUS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

59:         if (_isMigratingOut()) revert BB_MINT_DISALLOWED();

78:             if (msg.sender != _account) revert BB_PERMISSION_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

125:         if (_to == address(this)) revert BTOKEN_CANNOT_RECEIVE();

126:         if (paused()) revert INVALID_PAUSE_STATUS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

48:             if (_op.amounts[i] == 0) revert VAULT_INVALID_AMOUNT();

108:         if (to == address(0) || to == address(this)) revert VAULT_INVALID_TO();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

162:         if (btokenBlacklist[_btokenNew]) revert VAULT_BTOKEN_BLACKLISTED();

174:             if (

214:         if (_op.amount == 0) revert VAULT_INVALID_AMOUNT();

215:         if (_op.token == address(0)) revert VAULT_INVALID_TOKEN();

216:         if (btokenBlacklist[_op.token]) revert VAULT_BTOKEN_BLACKLISTED();

267:         if (to == address(0) || to == address(this)) revert VAULT_INVALID_TO();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

35:             if (_op.amounts[i] != 0) revert VAULT_INVALID_AMOUNT();

91:         if (to == address(0) || to == address(this)) revert VAULT_INVALID_TO();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/LibBridgedToken.sol

20:         if (

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)

```solidity
File: packages/protocol/contracts/verifiers/GuardianVerifier.sol

6: import "./IVerifier.sol";

23:     function verifyProof(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/IVerifier.sol

24:     function verifyProof(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/IVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

10: import "./IVerifier.sol";

107:             if (instances[idx].addr == address(0)) revert SGX_INVALID_INSTANCE();

128:         (bool verified,) = IAttestation(automataDcapAttestation).verifyParsedQuote(_attestation);

130:         if (!verified) revert SGX_INVALID_ATTESTATION();

139:     function verifyProof(

148:         if (_ctx.isContesting) return;

152:         if (_proof.data.length != 89) revert SGX_INVALID_PROOF();

161:         if (!_isInstanceValid(id, oldInstance)) revert SGX_INVALID_INSTANCE();

184:                 "VERIFY_PROOF",

211:             if (addressRegistered[_instances[i]]) revert SGX_ALREADY_ATTESTED();

215:             if (_instances[i] == address(0)) revert SGX_INVALID_INSTANCE();

234:         if (instance == address(0)) return false;

235:         if (instance != instances[id].addr) return false;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="NC-5"></a>[NC-5] Default Visibility for constants
Some constants are using the default visibility. For readability, consider explicitly declaring them as `internal`.

*Instances (3)*:
```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

32:     uint256 constant SGX_TCB_CPUSVN_SIZE = 16;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

310:     bytes constant BASE32_HEX_TABLE =

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol

6:     uint256 constant LOW_28_MASK =

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/nomad-xyz/ExcessivelySafeCall.sol)

### <a name="NC-6"></a>[NC-6] Consider disabling `renounceOwnership()`
If the plan for your project does not include eventually giving up all ownership control, consider overwriting OpenZeppelin's `Ownable`'s `renounceOwnership()` function in order to disable it.

*Instances (1)*:
```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

10: abstract contract EssentialContract is UUPSUpgradeable, Ownable2StepUpgradeable, AddressResolver {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

### <a name="NC-7"></a>[NC-7] Functions should not be longer than 50 lines
Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability 

*Instances (213)*:
```solidity
File: packages/protocol/contracts/L1/ITaikoL1.sol

27:     function proveBlock(uint64 _blockId, bytes calldata _input) external;

31:     function verifyBlocks(uint64 _maxBlocksToVerify) external;

35:     function getConfig() external view returns (TaikoData.Config memory);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/ITaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

119:     function depositEtherToL2(address _recipient) external payable nonReentrant whenNotPaused {

132:     function canDepositEthToL2(uint256 _amount) public view returns (bool) {

137:     function isBlobReusable(bytes32 _blobHash) public view returns (bool) {

186:     function getConfig() public view virtual override returns (TaikoData.Config memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

47:     function burn(address _from, uint256 _amount) public onlyOwner {

52:     function snapshot() public onlyFromOwnerOrNamed("snapshooter") {

60:     function transfer(address _to, uint256 _amount) public override returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

111:     function votingDelay() public pure override returns (uint256) {

117:     function votingPeriod() public pure override returns (uint256) {

123:     function proposalThreshold() public pure override returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoTimelockController.sol

15:     function init(address _owner, uint256 _minDelay) external initializer {

24:     function getMinDelay() public view override returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoTimelockController.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

57:     function init(address _owner, address _addressManager) external initializer {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

149:     function _encodeEthDeposit(address _addr, uint256 _amount) private pure returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

73:     function pauseProving(TaikoData.State storage _state, bool _pause) external {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

245:     function _isConfigValid(TaikoData.Config memory _config) private view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/provers/GuardianProver.sol

25:     function init(address _owner, address _addressManager) external initializer {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

101:     function isApproved(bytes32 _hash) public view returns (bool) {

107:     function numGuardians() public view returns (uint256) {

111:     function approve(uint256 _operationId, bytes32 _hash) internal returns (bool approved_) {

128:     function isApproved(uint256 _approvalBits) internal view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

15:     function init(address _owner) external initializer {

20:     function getTier(uint16 _tierId) public pure override returns (ITierProvider.Tier memory) {

47:     function getTierIds() public pure override returns (uint16[] memory tiers_) {

54:     function getMinTier(uint256) public pure override returns (uint16) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/ITierProvider.sol

22:     function getTier(uint16 tierId) external view returns (Tier memory);

28:     function getTierIds() external view returns (uint16[] memory);

33:     function getMinTier(uint256 rand) external view returns (uint16);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/ITierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

15:     function init(address _owner) external initializer {

20:     function getTier(uint16 _tierId) public pure override returns (ITierProvider.Tier memory) {

58:     function getTierIds() public pure override returns (uint16[] memory tiers_) {

66:     function getMinTier(uint256 _rand) public pure override returns (uint16) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

15:     function init(address _owner) external initializer {

20:     function getTier(uint16 _tierId) public pure override returns (ITierProvider.Tier memory) {

58:     function getTierIds() public pure override returns (uint16[] memory tiers_) {

66:     function getMinTier(uint256 _rand) public pure override returns (uint16) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

36:     function onMessageInvocation(bytes calldata _data)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

200:     function getBlockHash(uint64 _blockId) public view returns (bytes32) {

208:     function getConfig() public view virtual returns (Config memory config_) {

219:     function skipFeeCheck() public pure virtual returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol

43:     function getConfig() public view override returns (Config memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

65:     function setMrSigner(bytes32 _mrSigner, bool _trusted) external onlyOwner {

69:     function setMrEnclave(bytes32 _mrEnclave, bool _trusted) external onlyOwner {

114:     function configureQeIdentityJson(EnclaveIdStruct.EnclaveId calldata qeIdentityInput)

122:     function toggleLocalReportCheck() external onlyOwner {

126:     function _attestationTcbIsValid(TCBInfoStruct.TCBStatus status)

138:     function verifyAttestation(bytes calldata data) external view override returns (bool) {

162:     function _verify(bytes calldata quote) private view returns (bool, bytes memory) {

175:     function _verifyQEReportWithIdentity(V3Struct.EnclaveReport memory quoteEnclaveReport)

248:     function _verifyCertChain(IPEMCertChainLib.ECSha256Certificate[] memory certs)

355:     function verifyParsedQuote(V3Struct.ParsedV3QuoteStruct calldata v3quote)

364:     function _verifyParsedQuote(V3Struct.ParsedV3QuoteStruct memory v3quote)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol

9:     function verifyAttestation(bytes calldata data) external returns (bool);

10:     function verifyParsedQuote(V3Struct.ParsedV3QuoteStruct calldata v3quote)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/interfaces/IAttestation.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

216:     function _removeHeadersAndFooters(string memory pemData)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

62:     function validateParsedInput(V3Struct.ParsedV3QuoteStruct memory v3Quote)

133:     function parseEnclaveReport(bytes memory rawEnclaveReport)

152:     function littleEndianDecode(bytes memory encoded) private pure returns (uint256 decoded) {

165:     function parseAndVerifyHeader(bytes memory rawHeader)

244:     function packQEReport(V3Struct.EnclaveReport memory enclaveReport)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol

14:     function ixs(uint256 self) internal pure returns (uint256) {

19:     function ixf(uint256 self) internal pure returns (uint256) {

24:     function ixl(uint256 self) internal pure returns (uint256) {

29:     function getPtr(uint256 _ixs, uint256 _ixf, uint256 _ixl) internal pure returns (uint256) {

47:     function root(bytes memory der) internal pure returns (uint256) {

56:     function rootOfBitStringAt(bytes memory der, uint256 ptr) internal pure returns (uint256) {

66:     function rootOfOctetStringAt(bytes memory der, uint256 ptr) internal pure returns (uint256) {

77:     function nextSiblingOf(bytes memory der, uint256 ptr) internal pure returns (uint256) {

87:     function firstChildOf(bytes memory der, uint256 ptr) internal pure returns (uint256) {

98:     function isChildOf(uint256 i, uint256 j) internal pure returns (bool) {

111:     function bytesAt(bytes memory der, uint256 ptr) internal pure returns (bytes memory) {

121:     function allBytesAt(bytes memory der, uint256 ptr) internal pure returns (bytes memory) {

131:     function bytes32At(bytes memory der, uint256 ptr) internal pure returns (bytes32) {

141:     function uintAt(bytes memory der, uint256 ptr) internal pure returns (uint256) {

154:     function uintBytesAt(bytes memory der, uint256 ptr) internal pure returns (bytes memory) {

165:     function keccakOfBytesAt(bytes memory der, uint256 ptr) internal pure returns (bytes32) {

169:     function keccakOfAllBytesAt(bytes memory der, uint256 ptr) internal pure returns (bytes32) {

179:     function bitstringAt(bytes memory der, uint256 ptr) internal pure returns (bytes memory) {

187:     function _readNodeLength(bytes memory der, uint256 ix) private pure returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/Asn1Decode.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

39:     function compare(bytes memory self, bytes memory other) internal pure returns (int256) {

178:     function equals(bytes memory self, bytes memory other) internal pure returns (bool) {

188:     function readUint8(bytes memory self, uint256 idx) internal pure returns (uint8 ret) {

198:     function readUint16(bytes memory self, uint256 idx) internal pure returns (uint16 ret) {

211:     function readUint32(bytes memory self, uint256 idx) internal pure returns (uint32 ret) {

224:     function readBytes32(bytes memory self, uint256 idx) internal pure returns (bytes32 ret) {

237:     function readBytes20(bytes memory self, uint256 idx) internal pure returns (bytes20 ret) {

272:     function memcpy(uint256 dest, uint256 src, uint256 len) private pure {

371:     function compareBytes(bytes memory a, bytes memory b) internal pure returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SHA1.sol

11:     function sha1(bytes memory data) internal pure returns (bytes20 ret) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SHA1.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

8:     function toTimestamp(bytes memory x509Time) internal pure returns (uint256) {

71:     function isLeapYear(uint16 year) internal pure returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

75:     function init(address _owner, address _addressManager) external initializer {

340:     function isMessageSent(Message calldata _message) public view returns (bool) {

403:     function context() public view returns (Context memory ctx_) {

449:     function hashMessage(Message memory _message) public pure returns (bytes32) {

456:     function signalForFailedMessage(bytes32 _msgHash) public pure returns (bytes32) {

515:     function _updateMessageStatus(bytes32 _msgHash, Status _status) private {

541:     function _storeContext(bytes32 _msgHash, address _from, uint64 _srcChainId) private {

555:     function _loadContext() private view returns (Context memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/bridge/IBridge.sol

120:     function recallMessage(Message calldata _message, bytes calldata _proof) external;

130:     function processMessage(Message calldata _message, bytes calldata _proof) external;

141:     function retryMessage(Message calldata _message, bool _isLastAttempt) external;

145:     function context() external view returns (Context memory ctx_);

150:     function isMessageSent(Message calldata _message) external view returns (bool);

155:     function hashMessage(Message memory _message) external pure returns (bytes32);

178:     function onMessageInvocation(bytes calldata _data) external payable;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/IBridge.sol)

```solidity
File: packages/protocol/contracts/common/AddressManager.sol

30:     function init(address _owner) external initializer {

54:     function getAddress(uint64 _chainId, bytes32 _name) public view override returns (address) {

58:     function _authorizePause(address) internal pure override {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/AddressManager.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

109:     function __Essential_init(address _owner) internal virtual {

114:     function _authorizeUpgrade(address) internal virtual override onlyOwner { }

116:     function _authorizePause(address) internal virtual onlyOwner { }

119:     function _storeReentryLock(uint8 _reentry) internal virtual {

130:     function _loadReentryLock() internal view virtual returns (uint8 reentry_) {

140:     function _inNonReentrant() internal view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/common/IAddressManager.sol

14:     function getAddress(uint64 _chainId, bytes32 _name) external view returns (address);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/IAddressManager.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

22:     function sendEther(address _to, uint256 _amount, uint256 _gasLimit) internal {

42:     function sendEther(address _to, uint256 _amount) internal {

77:     function isSenderEOA() internal view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/libs/LibMath.sol

12:     function min(uint256 _a, uint256 _b) internal pure returns (uint256) {

20:     function max(uint256 _a, uint256 _b) internal pure returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibMath.sol)

```solidity
File: packages/protocol/contracts/signal/ISignalService.sol

59:     function sendSignal(bytes32 _signal) external returns (bytes32 slot_);

59:     function sendSignal(bytes32 _signal) external returns (bytes32 slot_);

96:     function isSignalSent(address _app, bytes32 _signal) external view returns (bool);

96:     function isSignalSent(address _app, bytes32 _signal) external view returns (bool);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/ISignalService.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

48:     function init(address _owner, address _addressManager) external initializer {

56:     function authorize(address _addr, bool _authorize) external onlyOwner {

63:     function sendSignal(bytes32 _signal) external returns (bytes32) {

153:     function isSignalSent(address _app, bytes32 _signal) public view returns (bool) {

231:     function _authorizePause(address) internal pure override {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

135:     function grant(address _recipient, Grant memory _grant) external onlyOwner {

150:     function void(address _recipient) external onlyOwner {

168:     function withdraw(address _to, bytes memory _sig) external {

204:     function getMyGrant(address _recipient) public view returns (Grant memory) {

208:     function _withdraw(address _recipient, address _to) private {

225:     function _voidGrant(Grant storage _grant) private returns (uint128 amountVoided) {

235:     function _getAmountOwned(Grant memory _grant) private view returns (uint128) {

239:     function _getAmountUnlocked(Grant memory _grant) private view returns (uint128) {

267:     function _validateGrant(Grant memory _grant) private pure {

273:     function _validateCliff(uint64 _start, uint64 _cliff, uint32 _period) private pure {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

78:     function claim(address user, uint256 amount, bytes32[] calldata proof) external nonReentrant {

88:     function withdraw(address user) external ongoingWithdrawals {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

67:     function _verifyClaim(bytes memory data, bytes32[] calldata proof) internal ongoingClaim {

90:     function _setConfig(uint64 _claimStart, uint64 _claimEnd, bytes32 _merkleRoot) private {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/Bytes.sol

91:     function slice(bytes memory _bytes, uint256 _start) internal pure returns (bytes memory) {

102:     function toNibbles(bytes memory _bytes) internal pure returns (bytes memory) {

149:     function equal(bytes memory _bytes, bytes memory _other) internal pure returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/Bytes.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

35:     function toRLPItem(bytes memory _in) internal pure returns (RLPItem memory out_) {

53:     function readList(RLPItem memory _in) internal pure returns (RLPItem[] memory out_) {

102:     function readList(bytes memory _in) internal pure returns (RLPItem[] memory out_) {

109:     function readBytes(RLPItem memory _in) internal pure returns (bytes memory out_) {

128:     function readBytes(bytes memory _in) internal pure returns (bytes memory out_) {

135:     function readRawBytes(RLPItem memory _in) internal pure returns (bytes memory out_) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

13:     function writeBytes(bytes memory _in) internal pure returns (bytes memory out_) {

24:     function writeUint(uint256 _in) internal pure returns (bytes memory out_) {

32:     function _writeLength(uint256 _len, uint256 _offset) private pure returns (bytes memory out_) {

55:     function _toBinary(uint256 _x) private pure returns (bytes memory out_) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

205:     function _parseProof(bytes[] memory _proof) private pure returns (TrieNode[] memory proof_) {

220:     function _getNodeID(RLPReader.RLPItem memory _node) private pure returns (bytes memory id_) {

227:     function _getNodePath(TrieNode memory _node) private pure returns (bytes memory nibbles_) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol

54:     function _getSecureKey(bytes memory _key) private pure returns (bytes memory hash_) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/SecureMerkleTrie.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

32:     function init(address _owner, address _addressManager) external initializer {

39:     function supportsInterface(bytes4 _interfaceId) public view virtual override returns (bool) {

45:     function name() public pure virtual returns (bytes32);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

115:     function name() public view returns (string memory) {

121:     function symbol() public view returns (string memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

80:     function setSnapshoter(address _snapshooter) external onlyOwner {

85:     function snapshot() external onlyOwnerOrSnapshooter {

125:     function canonical() public view returns (address, uint256) {

129:     function _mintToken(address _account, uint256 _amount) internal override {

133:     function _burnToken(address _from, uint256 _amount) internal override {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

57:     function mint(address _account, uint256 _amount) public nonReentrant whenNotPaused {

75:     function burn(address _account, uint256 _amount) public nonReentrant whenNotPaused {

93:     function owner() public view override(IBridgedERC20, OwnableUpgradeable) returns (address) {

97:     function _mintToken(address _account, uint256 _amount) internal virtual;

99:     function _burnToken(address _from, uint256 _amount) internal virtual;

101:     function _isMigratingOut() internal view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

87:     function name() public view override(ERC721Upgradeable) returns (string memory) {

93:     function symbol() public view override(ERC721Upgradeable) returns (string memory) {

100:     function source() public view returns (address, uint256) {

107:     function tokenURI(uint256 _tokenId) public view virtual override returns (string memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

18:     function name() external view returns (string memory);

21:     function symbol() external view returns (string memory);

93:     function onMessageInvocation(bytes calldata data) external payable nonReentrant whenNotPaused {

204:     function name() public pure override returns (bytes32) {

288:     function _getOrDeployBridgedToken(CanonicalNFT memory _ctoken)

303:     function _deployBridgedToken(CanonicalNFT memory _ctoken) private returns (address btoken_) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

253:     function onMessageInvocation(bytes calldata _data)

316:     function name() public pure override returns (bytes32) {

391:     function _getOrDeployBridgedToken(CanonicalERC20 memory ctoken)

407:     function _deployBridgedToken(CanonicalERC20 memory ctoken) private returns (address btoken) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

77:     function onMessageInvocation(bytes calldata _data)

156:     function name() public pure override returns (bytes32) {

224:     function _getOrDeployBridgedToken(CanonicalNFT memory _ctoken)

240:     function _deployBridgedToken(CanonicalNFT memory _ctoken) private returns (address btoken_) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/IBridgedERC20.sol

14:     function mint(address _account, uint256 _amount) external;

19:     function burn(address _from, uint256 _amount) external;

24:     function changeMigrationStatus(address _addr, bool _inbound) external;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/IBridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/LibBridgedToken.sol

39:     function buildSymbol(string memory _symbol) internal pure returns (string memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)

```solidity
File: packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol

16:     function mint(address _to, uint256 _amount) external;

23:     function transferFrom(address from, address _to, uint256 _amount) external returns (bool);

38:     function init(address _owner, address _addressManager, IUSDC _usdc) external initializer {

43:     function _mintToken(address _account, uint256 _amount) internal override {

47:     function _burnToken(address _from, uint256 _amount) internal override {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol)

```solidity
File: packages/protocol/contracts/verifiers/GuardianVerifier.sol

18:     function init(address _owner, address _addressManager) external initializer {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

83:     function init(address _owner, address _addressManager) external initializer {

90:     function addInstances(address[] calldata _instances)

118:     function registerInstance(V3Struct.ParsedV3QuoteStruct calldata _attestation)

226:     function _replaceInstance(uint256 id, address oldInstance, address newInstance) private {

233:     function _isInstanceValid(uint256 id, address instance) private view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="NC-8"></a>[NC-8] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor
If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (16)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

310:             if (proposerOne != address(0) && msg.sender != proposerOne) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

123:         if (msg.sender != GOLDEN_TOUCH_ADDRESS) revert L2_INVALID_SENDER();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

61:         require(msg.sender == owner, "onlyOwner");

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

250:         if (invocationDelay != 0 && msg.sender != proofReceipt[msgHash].preferredExecutor) {

260:             if (_message.gasLimit == 0 && msg.sender != _message.destOwner) {

294:             if (msg.sender == refundTo) {

322:             if (msg.sender != _message.destOwner) revert B_PERMISSION_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

42:         if (msg.sender != owner() && msg.sender != resolve(_name, true)) revert RESOLVER_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

77:         if (!isAuthorized[msg.sender]) revert SS_UNAUTHORIZED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

23:         if (msg.sender != resolve("bridge", false)) {

65:         if (ctx_.from != msg.sender) revert VAULT_PERMISSION_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

38:         if (msg.sender != owner() && msg.sender != snapshooter) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

61:         if (msg.sender == migratingAddress) {

64:         } else if (msg.sender != resolve("erc20_vault", true)) {

78:             if (msg.sender != _account) revert BB_PERMISSION_DENIED();

83:         } else if (msg.sender != resolve("erc20_vault", true)) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

### <a name="NC-9"></a>[NC-9] `address`s shouldn't be hard-coded
It is often better to declare `address`es as `immutable`, and assign them via constructor arguments. This allows the code to remain the same across deployments on different networks, and avoids recompilation when addresses need to change.

*Instances (2)*:
```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

32:     address public constant GOLDEN_TOUCH_ADDRESS = 0x0000777735367b36bC9B61C50022d9D0700dB4Ec;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/SHA1.sol

182:                                     and(div(h, 0x100000000), 0xFFFFFFFF00000000000000000000000000000000),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/SHA1.sol)

### <a name="NC-10"></a>[NC-10] Numeric values having to do with time should use time units for readability
There are [units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units) for seconds, minutes, hours, days, and weeks, and since they're defined, they should be used

*Instances (1)*:
```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

46:         uint256 timestamp = 0;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

### <a name="NC-11"></a>[NC-11] Owner can renounce while system is paused
The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely.

*Instances (1)*:
```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

116:     function _authorizePause(address) internal virtual onlyOwner { }

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

### <a name="NC-12"></a>[NC-12] Take advantage of Custom Error's return value property
An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, this kind of approach provides a serious advantage in debugging and examining the revert details of dapps such as tenderly.

*Instances (170)*:
```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

29:         if (state.slotB.provingPaused) revert L1_PROVING_PAUSED();

35:         if (!_inNonReentrant()) revert L1_RECEIVE_DISABLED();

90:         if (_blockId != meta.id) revert L1_INVALID_BLOCK_ID();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

61:         if (_to == address(this)) revert TKO_INVALID_ADDR();

79:         if (_to == address(this)) revert TKO_INVALID_ADDR();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

81:         if (_signatures.length != _calldatas.length) revert TG_INVALID_SIGNATURES_LENGTH();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

88:             revert HOOK_ASSIGNMENT_EXPIRED();

97:             revert HOOK_ASSIGNMENT_INVALID_SIG();

175:         revert HOOK_TIER_NOT_FOUND();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibDepositing.sol

38:             revert L1_INVALID_ETH_DEPOSIT();

150:         if (_amount > type(uint96).max) revert L1_INVALID_ETH_DEPOSIT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibDepositing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

82:             revert L1_INVALID_PROVER();

94:         if (!_isProposerPermitted(b, _resolver)) revert L1_UNAUTHORIZED();

99:             revert L1_TOO_MANY_BLOCKS();

109:             revert L1_UNEXPECTED_PARENT();

142:             if (!_config.blobAllowedForDA) revert L1_BLOB_FOR_DA_DISABLED();

145:                 if (!_config.blobReuseEnabled) revert L1_BLOB_REUSE_DISABLED();

149:                     revert L1_BLOB_NOT_REUSABLE();

159:                 if (meta_.blobHash == 0) revert L1_BLOB_NOT_FOUND();

172:                 revert L1_TXLIST_OFFSET();

182:             if (!LibAddress.isSenderEOA()) revert L1_PROPOSER_NOT_EOA();

186:                 revert L1_INVALID_PARAM();

196:             revert L1_TXLIST_SIZE();

246:                     revert L1_INVALID_HOOK();

269:                 revert L1_LIVENESS_BOND_NOT_RECEIVED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

74:         if (_state.slotB.provingPaused == _pause) revert L1_INVALID_PAUSE_STATUS();

106:             revert L1_INVALID_TRANSITION();

112:             revert L1_INVALID_BLOCK_ID();

122:             revert L1_BLOCK_MISMATCH();

135:             revert L1_INVALID_TIER();

182:                 revert L1_MISSING_VERIFIER();

219:             if (sameTransition) revert L1_ALREADY_PROVED();

239:                 if (ts.contester != address(0)) revert L1_ALREADY_CONTESTED();

374:             if (_sameTransition) revert L1_ALREADY_PROVED();

420:             if (!isAssignedPover) revert L1_NOT_ASSIGNED_PROVER();

424:             if (isAssignedPover) revert L1_ASSIGNED_PROVER_NOT_ALLOWED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibUtils.sol

35:             revert L1_INVALID_BLOCK_ID();

40:         if (blk.blockId != _blockId) revert L1_BLOCK_MISMATCH();

43:         if (tid == 0) revert L1_TRANSITION_NOT_FOUND();

64:             revert L1_INVALID_BLOCK_ID();

86:         if (tid_ >= _blk.nextTransitionId) revert L1_UNEXPECTED_TRANSITION_ID();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibUtils.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

54:         if (!_isConfigValid(_config)) revert L1_INVALID_CONFIG();

105:         if (blk.blockId != blockId) revert L1_BLOCK_MISMATCH();

111:         if (tid == 0) revert L1_TRANSITION_ID_ZERO();

131:                 if (blk.blockId != blockId) revert L1_BLOCK_MISMATCH();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/provers/GuardianProver.sol

45:         if (_proof.tier != LibTiers.TIER_GUARDIAN) revert INVALID_PROOF();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)

```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

64:             revert INVALID_GUARDIAN_SET();

70:             revert INVALID_MIN_GUARDIANS();

82:             if (guardian == address(0)) revert INVALID_GUARDIAN();

84:             if (guardianIds[guardian] != 0) revert INVALID_GUARDIAN_SET();

113:         if (id == 0) revert INVALID_GUARDIAN();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

43:         revert TIER_NOT_FOUND();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

54:         revert TIER_NOT_FOUND();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

54:         revert TIER_NOT_FOUND();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

31:     error XCO_TX_REVERTED();

43:         if (txId != nextTxId) revert XCO_INVALID_TX_ID();

47:             revert XCO_PERMISSION_DENIED();

51:         if (!success) revert XCO_TX_REVERTED();

71:             revert XCO_INVALID_OWNER_CHAINID();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/Lib1559Math.sol

25:             revert EIP1559_INVALID_PARAMS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/Lib1559Math.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

83:             revert L2_INVALID_CHAIN_ID();

93:             revert L2_TOO_LATE();

120:             revert L2_INVALID_PARAM();

123:         if (msg.sender != GOLDEN_TOUCH_ADDRESS) revert L2_INVALID_SENDER();

133:             revert L2_PUBLIC_INPUT_HASH_MISMATCH();

142:             revert L2_BASEFEE_MISMATCH();

172:         if (_to == address(0)) revert L2_INVALID_PARAM();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol

33:         if (_newConfig.gasTargetPerL1Block == 0) revert L2_INVALID_CONFIG();

34:         if (_newConfig.basefeeAdjustmentQuotient == 0) revert L2_INVALID_CONFIG();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2EIP1559Configurable.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

65:         if (_chainId != block.chainid) revert B_INVALID_CHAINID();

109:         if (addressBanned[_addr] == _ban) revert B_INVALID_STATUS();

125:             revert B_INVALID_USER();

132:         if (!destChainEnabled) revert B_INVALID_CHAINID();

134:             revert B_INVALID_CHAINID();

139:         if (expectedAmount != msg.value) revert B_INVALID_VALUE();

166:         if (messageStatus[msgHash] != Status.NEW) revert B_STATUS_MISMATCH();

175:                 revert B_MESSAGE_NOT_SENT();

180:                 revert B_NOT_FAILED();

212:             revert B_INVOCATION_TOO_EARLY();

227:         if (messageStatus[msgHash] != Status.NEW) revert B_STATUS_MISMATCH();

237:                 revert B_NOT_RECEIVED();

261:                 revert B_PERMISSION_DENIED();

305:             revert B_INVOCATION_TOO_EARLY();

322:             if (msg.sender != _message.destOwner) revert B_PERMISSION_DENIED();

327:             revert B_NON_RETRIABLE();

406:             revert B_INVALID_CONTEXT();

485:         if (_gasLimit == 0) revert B_INVALID_GAS_LIMIT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/common/AddressManager.sol

48:         if (_newAddress == oldAddress) revert AM_INVALID_PARAMS();

59:         revert AM_UNSUPPORTED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/AddressManager.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

42:         if (msg.sender != owner() && msg.sender != resolve(_name, true)) revert RESOLVER_DENIED();

47:         if (_loadReentryLock() == _TRUE) revert REENTRANT_CALL();

54:         if (!paused()) revert INVALID_PAUSE_STATUS();

59:         if (paused()) revert INVALID_PAUSE_STATUS();

105:         if (_addressManager == address(0)) revert ZERO_ADDR_MANAGER();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/libs/Lib4844.sol

40:         if (_x >= BLS_MODULUS) revert POINT_X_TOO_LARGE();

41:         if (_y >= BLS_MODULUS) revert POINT_Y_TOO_LARGE();

47:         if (!ok) revert EVAL_FAILED_1();

49:         if (ret.length != 64) revert EVAL_FAILED_2();

58:             revert EVAL_FAILED_2();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/Lib4844.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

24:         if (_to == address(0)) revert ETH_TRANSFER_FAILED();

36:         if (!success) revert ETH_TRANSFER_FAILED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/libs/LibTrieProof.sol

50:             if (rlpAccount.length == 0) revert LTP_INVALID_ACCOUNT_PROOF();

64:         if (!verified) revert LTP_INVALID_INCLUSION_PROOF();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibTrieProof.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

36:         if (_app == address(0)) revert SS_INVALID_SENDER();

41:         if (_input == 0) revert SS_INVALID_VALUE();

57:         if (isAuthorized[_addr] == _authorize) revert SS_INVALID_STATE();

77:         if (!isAuthorized[msg.sender]) revert SS_UNAUTHORIZED();

95:         if (hopProofs.length == 0) revert SS_EMPTY_PROOF();

111:                 if (hop.chainId != block.chainid) revert SS_INVALID_LAST_HOP_CHAINID();

115:                     revert SS_INVALID_MID_HOP_CHAINID();

132:             revert SS_SIGNAL_NOT_FOUND();

172:             if (chainData_ == 0) revert SS_SIGNAL_NOT_FOUND();

232:         revert SS_UNSUPPORTED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

121:         if (_taikoToken == address(0)) revert INVALID_PARAM();

124:         if (_costToken == address(0)) revert INVALID_PARAM();

127:         if (_sharedVault == address(0)) revert INVALID_PARAM();

136:         if (_recipient == address(0)) revert INVALID_PARAM();

137:         if (recipients[_recipient].grant.amount != 0) revert ALREADY_GRANTED();

154:         if (amountVoided == 0) revert NOTHING_TO_VOID();

169:         if (_to == address(0)) revert INVALID_PARAM();

268:         if (_grant.amount == 0) revert INVALID_GRANT();

275:             if (_cliff > 0) revert INVALID_GRANT();

277:             if (_cliff > 0 && _cliff <= _start) revert INVALID_GRANT();

278:             if (_cliff >= _start + _period) revert INVALID_GRANT();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

41:             revert WITHDRAWALS_NOT_ONGOING();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

37:         ) revert CLAIM_NOT_ONGOING();

70:         if (isClaimed[hash]) revert CLAIMED_ALREADY();

71:         if (!_verifyMerkleProof(proof, merkleRoot, hash)) revert INVALID_PROOF();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseNFTVault.sol

142:             revert VAULT_TOKEN_ARRAY_MISMATCH();

146:             revert VAULT_MAX_TOKEN_PER_TXN_EXCEEDED();

149:         if (_op.token == address(0)) revert VAULT_INVALID_TOKEN();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseNFTVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

24:             revert VAULT_PERMISSION_DENIED();

55:         if (ctx_.from != selfOnSourceChain) revert VAULT_PERMISSION_DENIED();

65:         if (ctx_.from != msg.sender) revert VAULT_PERMISSION_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

137:         if (_to == address(this)) revert BTOKEN_CANNOT_RECEIVE();

138:         if (paused()) revert INVALID_PAUSE_STATUS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

39:             revert BTOKEN_UNAUTHORIZED();

147:         if (_to == address(this)) revert BTOKEN_CANNOT_RECEIVE();

148:         if (paused()) revert INVALID_PAUSE_STATUS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

46:             revert BB_INVALID_PARAMS();

59:         if (_isMigratingOut()) revert BB_MINT_DISALLOWED();

66:             revert BB_PERMISSION_DENIED();

78:             if (msg.sender != _account) revert BB_PERMISSION_DENIED();

85:             revert RESOLVER_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

80:             revert BTOKEN_INVALID_BURN();

125:         if (_to == address(this)) revert BTOKEN_CANNOT_RECEIVE();

126:         if (paused()) revert INVALID_PAUSE_STATUS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

48:             if (_op.amounts[i] == 0) revert VAULT_INVALID_AMOUNT();

52:             revert VAULT_INTERFACE_NOT_SUPPORTED();

108:         if (to == address(0) || to == address(this)) revert VAULT_INVALID_TO();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

159:             revert VAULT_INVALID_NEW_BTOKEN();

162:         if (btokenBlacklist[_btokenNew]) revert VAULT_BTOKEN_BLACKLISTED();

165:             revert VAULT_NOT_SAME_OWNER();

178:             ) revert VAULT_CTOKEN_MISMATCH();

214:         if (_op.amount == 0) revert VAULT_INVALID_AMOUNT();

215:         if (_op.token == address(0)) revert VAULT_INVALID_TOKEN();

216:         if (btokenBlacklist[_op.token]) revert VAULT_BTOKEN_BLACKLISTED();

267:         if (to == address(0) || to == address(this)) revert VAULT_INVALID_TO();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

35:             if (_op.amounts[i] != 0) revert VAULT_INVALID_AMOUNT();

39:             revert VAULT_INTERFACE_NOT_SUPPORTED();

91:         if (to == address(0) || to == address(this)) revert VAULT_INVALID_TO();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/LibBridgedToken.sol

24:             revert BTOKEN_INVALID_PARAMS();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/LibBridgedToken.sol)

```solidity
File: packages/protocol/contracts/verifiers/GuardianVerifier.sol

32:             revert PERMISSION_DENIED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

107:             if (instances[idx].addr == address(0)) revert SGX_INVALID_INSTANCE();

125:             revert SGX_RA_NOT_SUPPORTED();

130:         if (!verified) revert SGX_INVALID_ATTESTATION();

152:         if (_proof.data.length != 89) revert SGX_INVALID_PROOF();

161:         if (!_isInstanceValid(id, oldInstance)) revert SGX_INVALID_INSTANCE();

211:             if (addressRegistered[_instances[i]]) revert SGX_ALREADY_ATTESTED();

215:             if (_instances[i] == address(0)) revert SGX_INVALID_INSTANCE();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="NC-13"></a>[NC-13] Avoid the use of sensitive terms
Use [alternative variants](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/), e.g. allowlist/denylist instead of whitelist/blacklist

*Instances (5)*:
```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

52:     mapping(address btoken => bool blacklisted) public btokenBlacklist;

136:     error VAULT_BTOKEN_BLACKLISTED();

162:         if (btokenBlacklist[_btokenNew]) revert VAULT_BTOKEN_BLACKLISTED();

181:             btokenBlacklist[btokenOld_] = true;

216:         if (btokenBlacklist[_op.token]) revert VAULT_BTOKEN_BLACKLISTED();

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

### <a name="NC-14"></a>[NC-14] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (28)*:
```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

209:             ethDepositRingBufferSize: 1024,

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

112:         return 7200; // 1 day

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

21:     uint256 public constant MAX_BYTES_PER_BLOB = 4096 * 32;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

251:                 || _config.blockMaxTxListBytes > 128 * 1024 // calldata up to 128K

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

26:                 cooldownWindow: 1440, //24 hours

38:                 provingWindow: 2880, // 48 hours

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/ITierProvider.sol

48:     uint16 public constant TIER_GUARDIAN = 1000;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/ITierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

26:                 cooldownWindow: 1440, //24 hours

36:                 contestBond: 1000 ether, // TKO

37:                 cooldownWindow: 1440, //24 hours

49:                 provingWindow: 2880, // 48 hours

68:         if (_rand % 1000 == 0) return LibTiers.TIER_SGX_ZKVM;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

26:                 cooldownWindow: 1440, //24 hours

36:                 contestBond: 1000 ether, // TKO

37:                 cooldownWindow: 1440, //24 hours

49:                 provingWindow: 2880, // 48 hours

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

243:             publicInputHashOld := keccak256(inputs, 8192 /*mul(256, 32)*/ )

248:             publicInputHashNew := keccak256(inputs, 8192 /*mul(256, 32)*/ )

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

14:     uint256 internal constant MINIMUM_QUOTE_LENGTH = 1020;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol

11:     Copyright 2016, Adrià Massanet

102:                     sub(gas(), 2000),

250:                     sub(gas(), 2000),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/RsaVerify.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

18:             if (uint8(x509Time[0]) - 48 < 5) yrs += 2000;

19:             else yrs += 1900;

21:             yrs += (uint8(x509Time[0]) - 48) * 1000 + (uint8(x509Time[1]) - 48) * 100;

48:         for (uint16 i = 1970; i < year; ++i) {

64:         timestamp += uint256(hour) * 3600; // Hours in seconds

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/libs/Lib4844.sol

13:     uint32 public constant FIELD_ELEMENTS_PER_BLOB = 4096;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/Lib4844.sol)

### <a name="NC-15"></a>[NC-15] Constants should be defined rather than using magic numbers

*Instances (25)*:
```solidity
File: packages/protocol/contracts/L1/TaikoData.sol

130:         uint64 timestamp; // slot 6 (90 bits)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoData.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol

33:         uint256 localAuthDataSize = littleEndianDecode(quote.substring(432, 4));

46:             parseAuthDataAndVerifyCertType(quote.substring(436, localAuthDataSize), pemCertLibAddr);

51:         bytes memory rawLocalEnclaveReport = quote.substring(48, 384);

139:         enclaveReport.miscSelect = bytes4(rawEnclaveReport.substring(16, 4));

140:         enclaveReport.reserved1 = bytes28(rawEnclaveReport.substring(20, 28));

141:         enclaveReport.attributes = bytes16(rawEnclaveReport.substring(48, 16));

142:         enclaveReport.mrEnclave = bytes32(rawEnclaveReport.substring(64, 32));

143:         enclaveReport.reserved2 = bytes32(rawEnclaveReport.substring(96, 32));

145:         enclaveReport.reserved3 = rawEnclaveReport.substring(160, 96);

147:         enclaveReport.isvSvn = uint16(littleEndianDecode(rawEnclaveReport.substring(258, 2)));

148:         enclaveReport.reserved4 = rawEnclaveReport.substring(260, 60);

158:             uint256 acc = lowerDigit * (16 ** (2 * i));

159:             acc += upperDigit * (16 ** ((2 * i) + 1));

185:         bytes16 qeVendorId = bytes16(rawHeader.substring(12, 16));

197:             userData: bytes20(rawHeader.substring(28, 20))

212:         qeAuthData.parsedDataSize = uint16(littleEndianDecode(rawAuthData.substring(576, 2)));

213:         qeAuthData.data = rawAuthData.substring(578, qeAuthData.parsedDataSize);

228:         authDataV3.ecdsaAttestationKey = rawAuthData.substring(64, 64);

231:         authDataV3.qeReportSignature = rawAuthData.substring(512, 64);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/QuoteV3Auth/V3Parser.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

438:             return (30 minutes, 384 seconds);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/Bytes.sol

69:                 mstore(0x40, and(add(mc, 31), not(31)))

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/Bytes.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

179:                 firstByteOfContent := and(mload(add(ptr, 1)), shl(248, 0xff))

199:                 firstByteOfContent := and(mload(add(ptr, 1)), shl(248, 0xff))

245:                 firstByteOfContent := and(mload(add(ptr, 1)), shl(248, 0xff))

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

### <a name="NC-16"></a>[NC-16] Variables need not be initialized to zero
The default value for variables is zero, so initializing them to zero is superfluous.

*Instances (11)*:
```solidity
File: packages/protocol/contracts/L1/provers/Guardians.sol

80:         for (uint256 i = 0; i < _newGuardians.length; ++i) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/Guardians.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol

51:         uint256 index = 0;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/lib/PEMCertChainLib.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol

80:         for (uint256 idx = 0; idx < shortest; idx += 32) {

331:         uint256 ret = 0;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/BytesUtils.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol

46:         uint256 timestamp = 0;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/utils/X509DateUtils.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol

72:         uint256 itemCount = 0;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPReader.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

58:         uint256 i = 0;

66:         for (uint256 j = 0; j < out_.length; j++) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol

82:         uint256 currentKeyIndex = 0;

85:         for (uint256 i = 0; i < proof.length; i++) {

208:         for (uint256 i = 0; i < length;) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/trie/MerkleTrie.sol)


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Use of `tx.origin` is unsafe in almost every context | 1 |
| [L-2](#L-2) | Use of `tx.origin` is unsafe in almost every context | 1 |
| [L-3](#L-3) | `decimals()` is not a part of the ERC-20 standard | 1 |
| [L-4](#L-4) | Division by zero not prevented | 6 |
| [L-5](#L-5) | Initializers could be front-run | 87 |
| [L-6](#L-6) | Signature use at deadlines should be allowed | 7 |
| [L-7](#L-7) | Owner can renounce while system is paused | 1 |
| [L-8](#L-8) | Loss of precision | 1 |
| [L-9](#L-9) | Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership` | 1 |
| [L-10](#L-10) | `symbol()` is not a part of the ERC-20 standard | 5 |
| [L-11](#L-11) | Unsafe ERC20 operation(s) | 14 |
| [L-12](#L-12) | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 62 |
| [L-13](#L-13) | Upgradeable contract not initialized | 142 |
### <a name="L-1"></a>[L-1] Use of `tx.origin` is unsafe in almost every context
According to [Vitalik Buterin](https://ethereum.stackexchange.com/questions/196/how-do-i-make-my-dapp-serenity-proof), contracts should _not_ `assume that tx.origin will continue to be usable or meaningful`. An example of this is [EIP-3074](https://eips.ethereum.org/EIPS/eip-3074#allowing-txorigin-as-signer-1) which explicitly mentions the intention to change its semantics when it's used with new op codes. There have also been calls to [remove](https://github.com/ethereum/solidity/issues/683) `tx.origin`, and there are [security issues](solidity.readthedocs.io/en/v0.4.24/security-considerations.html#tx-origin) associated with using it for authorization. For these reasons, it's best to completely avoid the feature.

*Instances (1)*:
```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

78:         return msg.sender == tx.origin;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

### <a name="L-2"></a>[L-2] Use of `tx.origin` is unsafe in almost every context
According to [Vitalik Buterin](https://ethereum.stackexchange.com/questions/196/how-do-i-make-my-dapp-serenity-proof), contracts should _not_ `assume that tx.origin will continue to be usable or meaningful`. An example of this is [EIP-3074](https://eips.ethereum.org/EIPS/eip-3074#allowing-txorigin-as-signer-1) which explicitly mentions the intention to change its semantics when it's used with new op codes. There have also been calls to [remove](https://github.com/ethereum/solidity/issues/683) `tx.origin`, and there are [security issues](solidity.readthedocs.io/en/v0.4.24/security-considerations.html#tx-origin) associated with using it for authorization. For these reasons, it's best to completely avoid the feature.

*Instances (1)*:
```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

78:         return msg.sender == tx.origin;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

### <a name="L-3"></a>[L-3] `decimals()` is not a part of the ERC-20 standard
The `decimals()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (1)*:
```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

368:                 decimals: meta.decimals(),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

### <a name="L-4"></a>[L-4] Division by zero not prevented
The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.

*Instances (6)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

262:                 || _config.ethDepositMaxFee > type(uint96).max / _config.ethDepositMaxCountPerBlock

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L2/Lib1559Math.sol

28:         return _ethQty(_gasExcess, _adjustmentFactor) / LibFixedPointMath.SCALING_FACTOR

41:         uint256 input = _gasExcess * LibFixedPointMath.SCALING_FACTOR / _adjustmentFactor;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/Lib1559Math.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

264:         return _amount * uint64(block.timestamp - _start) / _period;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol

39:             while (_len / i != 0) {

47:                 out_[i] = bytes1(uint8((_len / (256 ** (lenLen - i))) % 256));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/thirdparty/optimism/rlp/RLPWriter.sol)

### <a name="L-5"></a>[L-5] Initializers could be front-run
Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (87)*:
```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

42:     function init(

48:         initializer

50:         __Essential_init(_owner, _addressManager);

51:         LibVerifying.init(state, getConfig(), _genesisBlockHash);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

25:     function init(

32:         initializer

34:         __Essential_init(_owner);

35:         __ERC20_init(_name, _symbol);

36:         __ERC20Snapshot_init();

37:         __ERC20Votes_init();

38:         __ERC20Permit_init(_name);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

31:     function init(

37:         initializer

39:         __Essential_init(_owner);

40:         __Governor_init("TaikoGovernor");

41:         __GovernorCompatibilityBravo_init();

42:         __GovernorVotes_init(_token);

43:         __GovernorVotesQuorumFraction_init(4);

44:         __GovernorTimelockControl_init(_timelock);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoTimelockController.sol

15:     function init(address _owner, uint256 _minDelay) external initializer {

16:         __Essential_init(_owner);

18:         __TimelockController_init(_minDelay, nil, nil, owner());

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoTimelockController.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

57:     function init(address _owner, address _addressManager) external initializer {

58:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

47:     function init(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L1/provers/GuardianProver.sol

25:     function init(address _owner, address _addressManager) external initializer {

26:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

15:     function init(address _owner) external initializer {

16:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

15:     function init(address _owner) external initializer {

16:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

15:     function init(address _owner) external initializer {

16:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

60:     function __CrossChainOwned_init(

69:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

71:     function init(

78:         initializer

80:         __CrossChainOwned_init(_owner, _addressManager, _l1ChainId);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

75:     function init(address _owner, address _addressManager) external initializer {

76:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/common/AddressManager.sol

30:     function init(address _owner) external initializer {

31:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/AddressManager.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

95:     function __Essential_init(

103:         __Essential_init(_owner);

106:         __AddressResolver_init(_addressManager);

109:     function __Essential_init(address _owner) internal virtual {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

48:     function init(address _owner, address _addressManager) external initializer {

49:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

111:     function init(

118:         initializer

120:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol

27:     function init(

36:         initializer

38:         __Essential_init(_owner);

39:         __MerkleClaimable_init(_claimStart, _claimEnd, _merkleRoot);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

54:     function init(

64:         initializer

66:         __Essential_init(_owner);

67:         __MerkleClaimable_init(_claimStart, _claimEnd, _merkleRoot);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

25:     function init(

34:         initializer

36:         __Essential_init(_owner);

37:         __MerkleClaimable_init(_claimStart, _claimEnd, _merkleRoot);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

56:     function __MerkleClaimable_init(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

32:     function init(address _owner, address _addressManager) external initializer {

33:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

38:     function init(

47:         initializer

53:         __Essential_init(_owner, _addressManager);

54:         __ERC1155_init(LibBridgedToken.buildURI(_srcToken, _srcChainId));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

52:     function init(

62:         initializer

66:         __Essential_init(_owner, _addressManager);

67:         __ERC20_init(_name, _symbol);

68:         __ERC20Snapshot_init();

69:         __ERC20Votes_init();

70:         __ERC20Permit_init(_name);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

31:     function init(

40:         initializer

44:         __Essential_init(_owner, _addressManager);

45:         __ERC721_init(_name, _symbol);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol

38:     function init(address _owner, address _addressManager, IUSDC _usdc) external initializer {

39:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol)

```solidity
File: packages/protocol/contracts/verifiers/GuardianVerifier.sol

18:     function init(address _owner, address _addressManager) external initializer {

19:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

83:     function init(address _owner, address _addressManager) external initializer {

84:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="L-6"></a>[L-6] Signature use at deadlines should be allowed
According to [EIP-2612](https://github.com/ethereum/EIPs/blob/71dc97318013bf2ac572ab63fab530ac9ef419ca/EIPS/eip-2612.md?plain=1#L58), signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.

*Instances (7)*:
```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

296:         return _state.reusableBlobs[_blobHash] + _config.blobExpiry > block.timestamp;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

415:             + _tier.provingWindow * 60 >= block.timestamp;

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

153:                             + uint256(ts.timestamp).max(_state.slotB.lastUnpausedAt) > block.timestamp

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

40:         if (claimEnd > block.timestamp || claimEnd + withdrawalWindow < block.timestamp) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

35:             merkleRoot == 0x0 || claimStart == 0 || claimEnd == 0 || claimStart > block.timestamp

36:                 || claimEnd < block.timestamp

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

236:         return instances[id].validSince <= block.timestamp

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)

### <a name="L-7"></a>[L-7] Owner can renounce while system is paused
The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely.

*Instances (1)*:
```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

116:     function _authorizePause(address) internal virtual onlyOwner { }

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

### <a name="L-8"></a>[L-8] Loss of precision
Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*Instances (1)*:
```solidity
File: packages/protocol/contracts/L2/Lib1559Math.sol

28:         return _ethQty(_gasExcess, _adjustmentFactor) / LibFixedPointMath.SCALING_FACTOR

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/Lib1559Math.sol)

### <a name="L-9"></a>[L-9] Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership`
Use [Ownable2Step.transferOwnership](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol) which is safer. Use it as it is more secure due to 2-stage ownership transfer.

**Recommended Mitigation Steps**

Use <a href="https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol">Ownable2Step.sol</a>
  
  ```solidity
      function acceptOwnership() external {
          address sender = _msgSender();
          require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
          _transferOwnership(sender);
      }
```

*Instances (1)*:
```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

110:         _transferOwnership(_owner == address(0) ? msg.sender : _owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

### <a name="L-10"></a>[L-10] `symbol()` is not a part of the ERC-20 standard
The `symbol()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (5)*:
```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

108:         return LibBridgedToken.buildSymbol(super.symbol());

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

94:         return LibBridgedToken.buildSymbol(super.symbol());

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

266:                 try t.symbol() returns (string memory _symbol) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC20Vault.sol

369:                 symbol: meta.symbol(),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC20Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

206:                     symbol: t.symbol(),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

### <a name="L-11"></a>[L-11] Unsafe ERC20 operation(s)

*Instances (14)*:
```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

62:         return super.transfer(_to, _amount);

80:         return super.transferFrom(_from, _to, _amount);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

102:         tko.transferFrom(_blk.assignedProver, taikoL1Address, _blk.livenessBond);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProving.sol

196:                 tko.transfer(blk.assignedProver, blk.livenessBond);

242:                 tko.transferFrom(msg.sender, address(this), tier.contestBond);

367:                 _tko.transfer(_ts.prover, _ts.validityBond + reward);

371:                 _tko.transfer(_ts.contester, _ts.contestBond + reward);

382:                 _tko.transfer(msg.sender, reward - _tier.validityBond);

384:                 _tko.transferFrom(msg.sender, address(this), _tier.validityBond - reward);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProving.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

189:                 tko.transfer(ts.prover, bondToReturn);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

219:         IERC20(taikoToken).transferFrom(sharedVault, _to, amountToWithdraw);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol

63:         IERC20(token).transferFrom(vault, user, amount);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

91:         IERC20(token).transferFrom(vault, user, amount);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol

48:         usdc.transferFrom(_from, address(this), _amount);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol)

### <a name="L-12"></a>[L-12] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions
See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

*Instances (62)*:
```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20SnapshotUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

15: contract TaikoToken is EssentialContract, ERC20SnapshotUpgradeable, ERC20VotesUpgradeable {

89:         override(ERC20Upgradeable, ERC20SnapshotUpgradeable)

100:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

110:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

120:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

4: import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";

6:     "@openzeppelin/contracts-upgradeable/governance/compatibility/GovernorCompatibilityBravoUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesUpgradeable.sol";

9:     "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";

11:     "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorTimelockControlUpgradeable.sol";

18:     GovernorCompatibilityBravoUpgradeable,

19:     GovernorVotesUpgradeable,

20:     GovernorVotesQuorumFractionUpgradeable,

21:     GovernorTimelockControlUpgradeable

33:         IVotesUpgradeable _token,

34:         TimelockControllerUpgradeable _timelock

55:         override(IGovernorUpgradeable, GovernorUpgradeable, GovernorCompatibilityBravoUpgradeable)

78:         override(GovernorCompatibilityBravoUpgradeable)

83:         return GovernorCompatibilityBravoUpgradeable.propose(

92:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable, IERC165Upgradeable)

102:         override(IGovernorUpgradeable, GovernorUpgradeable, GovernorTimelockControlUpgradeable)

135:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)

147:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)

156:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoTimelockController.sol

4: import "@openzeppelin/contracts-upgradeable/governance/TimelockControllerUpgradeable.sol";

9: contract TaikoTimelockController is EssentialContract, TimelockControllerUpgradeable {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoTimelockController.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

4: import "@openzeppelin/contracts/proxy/utils/UUPSUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol";

10: abstract contract EssentialContract is UUPSUpgradeable, Ownable2StepUpgradeable, AddressResolver {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

4: import "@openzeppelin/contracts-upgradeable/utils/introspection/IERC165Upgradeable.sol";

16:     IERC165Upgradeable

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC1155/ERC1155Upgradeable.sol";

7:     "@openzeppelin/contracts-upgradeable/token/ERC1155/extensions/IERC1155MetadataURIUpgradeable.sol";

14: contract BridgedERC1155 is EssentialContract, IERC1155MetadataURIUpgradeable, ERC1155Upgradeable {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/IERC20MetadataUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20SnapshotUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

17:     IERC20MetadataUpgradeable,

18:     ERC20SnapshotUpgradeable,

19:     ERC20VotesUpgradeable

94:         override(ERC20Upgradeable, IERC20MetadataUpgradeable)

105:         override(ERC20Upgradeable, IERC20MetadataUpgradeable)

116:         override(ERC20Upgradeable, IERC20MetadataUpgradeable)

145:         override(ERC20Upgradeable, ERC20SnapshotUpgradeable)

158:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

168:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

178:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

93:     function owner() public view override(IBridgedERC20, OwnableUpgradeable) returns (address) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

12: contract BridgedERC721 is EssentialContract, ERC721Upgradeable {

87:     function name() public view override(ERC721Upgradeable) returns (string memory) {

93:     function symbol() public view override(ERC721Upgradeable) returns (string memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC1155/utils/ERC1155ReceiverUpgradeable.sol";

29: contract ERC1155Vault is BaseNFTVault, ERC1155ReceiverUpgradeable {

171:         return IERC1155ReceiverUpgradeable.onERC1155BatchReceived.selector;

186:         return IERC1155ReceiverUpgradeable.onERC1155Received.selector;

196:         override(BaseVault, ERC1155ReceiverUpgradeable)

199:         return interfaceId == type(ERC1155ReceiverUpgradeable).interfaceId

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

201:                 ERC721Upgradeable t = ERC721Upgradeable(_op.token);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

### <a name="L-13"></a>[L-13] Upgradeable contract not initialized
Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (142)*:
```solidity
File: packages/protocol/contracts/L1/TaikoL1.sol

48:         initializer

50:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoL1.sol)

```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20SnapshotUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

15: contract TaikoToken is EssentialContract, ERC20SnapshotUpgradeable, ERC20VotesUpgradeable {

32:         initializer

34:         __Essential_init(_owner);

35:         __ERC20_init(_name, _symbol);

36:         __ERC20Snapshot_init();

37:         __ERC20Votes_init();

38:         __ERC20Permit_init(_name);

89:         override(ERC20Upgradeable, ERC20SnapshotUpgradeable)

100:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

110:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

120:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

4: import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";

6:     "@openzeppelin/contracts-upgradeable/governance/compatibility/GovernorCompatibilityBravoUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesUpgradeable.sol";

9:     "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";

11:     "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorTimelockControlUpgradeable.sol";

18:     GovernorCompatibilityBravoUpgradeable,

19:     GovernorVotesUpgradeable,

20:     GovernorVotesQuorumFractionUpgradeable,

21:     GovernorTimelockControlUpgradeable

33:         IVotesUpgradeable _token,

34:         TimelockControllerUpgradeable _timelock

37:         initializer

39:         __Essential_init(_owner);

40:         __Governor_init("TaikoGovernor");

41:         __GovernorCompatibilityBravo_init();

42:         __GovernorVotes_init(_token);

43:         __GovernorVotesQuorumFraction_init(4);

44:         __GovernorTimelockControl_init(_timelock);

55:         override(IGovernorUpgradeable, GovernorUpgradeable, GovernorCompatibilityBravoUpgradeable)

78:         override(GovernorCompatibilityBravoUpgradeable)

83:         return GovernorCompatibilityBravoUpgradeable.propose(

92:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable, IERC165Upgradeable)

102:         override(IGovernorUpgradeable, GovernorUpgradeable, GovernorTimelockControlUpgradeable)

135:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)

147:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)

156:         override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/L1/gov/TaikoTimelockController.sol

4: import "@openzeppelin/contracts-upgradeable/governance/TimelockControllerUpgradeable.sol";

9: contract TaikoTimelockController is EssentialContract, TimelockControllerUpgradeable {

15:     function init(address _owner, uint256 _minDelay) external initializer {

16:         __Essential_init(_owner);

18:         __TimelockController_init(_minDelay, nil, nil, owner());

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoTimelockController.sol)

```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

57:     function init(address _owner, address _addressManager) external initializer {

58:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

123:                 difficulty: 0, // to be initialized below

124:                 blobHash: 0, // to be initialized below

132:                 txListByteOffset: 0, // to be initialized below

133:                 txListByteSize: 0, // to be initialized below

134:                 minTier: 0, // to be initialized below

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/provers/GuardianProver.sol

25:     function init(address _owner, address _addressManager) external initializer {

26:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/provers/GuardianProver.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol

15:     function init(address _owner) external initializer {

16:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/DevnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol

15:     function init(address _owner) external initializer {

16:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/MainnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol

15:     function init(address _owner) external initializer {

16:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/tiers/TestnetTierProvider.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

60:     function __CrossChainOwned_init(

69:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

78:         initializer

80:         __CrossChainOwned_init(_owner, _addressManager, _l1ChainId);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

75:     function init(address _owner, address _addressManager) external initializer {

76:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/common/AddressManager.sol

30:     function init(address _owner) external initializer {

31:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/AddressManager.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

4: import "@openzeppelin/contracts/proxy/utils/UUPSUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol";

10: abstract contract EssentialContract is UUPSUpgradeable, Ownable2StepUpgradeable, AddressResolver {

65:         _disableInitializers();

95:     function __Essential_init(

103:         __Essential_init(_owner);

106:         __AddressResolver_init(_addressManager);

109:     function __Essential_init(address _owner) internal virtual {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

48:     function init(address _owner, address _addressManager) external initializer {

49:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

118:         initializer

120:         __Essential_init(_owner);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol

36:         initializer

38:         __Essential_init(_owner);

39:         __MerkleClaimable_init(_claimStart, _claimEnd, _merkleRoot);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol

64:         initializer

66:         __Essential_init(_owner);

67:         __MerkleClaimable_init(_claimStart, _claimEnd, _merkleRoot);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC20Airdrop2.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol

34:         initializer

36:         __Essential_init(_owner);

37:         __MerkleClaimable_init(_claimStart, _claimEnd, _merkleRoot);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/ERC721Airdrop.sol)

```solidity
File: packages/protocol/contracts/team/airdrop/MerkleClaimable.sol

56:     function __MerkleClaimable_init(

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/airdrop/MerkleClaimable.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BaseVault.sol

4: import "@openzeppelin/contracts-upgradeable/utils/introspection/IERC165Upgradeable.sol";

16:     IERC165Upgradeable

32:     function init(address _owner, address _addressManager) external initializer {

33:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BaseVault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC1155.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC1155/ERC1155Upgradeable.sol";

7:     "@openzeppelin/contracts-upgradeable/token/ERC1155/extensions/IERC1155MetadataURIUpgradeable.sol";

14: contract BridgedERC1155 is EssentialContract, IERC1155MetadataURIUpgradeable, ERC1155Upgradeable {

47:         initializer

53:         __Essential_init(_owner, _addressManager);

54:         __ERC1155_init(LibBridgedToken.buildURI(_srcToken, _srcChainId));

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC1155.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/IERC20MetadataUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20SnapshotUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

17:     IERC20MetadataUpgradeable,

18:     ERC20SnapshotUpgradeable,

19:     ERC20VotesUpgradeable

62:         initializer

66:         __Essential_init(_owner, _addressManager);

67:         __ERC20_init(_name, _symbol);

68:         __ERC20Snapshot_init();

69:         __ERC20Votes_init();

70:         __ERC20Permit_init(_name);

94:         override(ERC20Upgradeable, IERC20MetadataUpgradeable)

105:         override(ERC20Upgradeable, IERC20MetadataUpgradeable)

116:         override(ERC20Upgradeable, IERC20MetadataUpgradeable)

145:         override(ERC20Upgradeable, ERC20SnapshotUpgradeable)

158:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

168:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

178:         override(ERC20Upgradeable, ERC20VotesUpgradeable)

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20Base.sol

93:     function owner() public view override(IBridgedERC20, OwnableUpgradeable) returns (address) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20Base.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC721.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

12: contract BridgedERC721 is EssentialContract, ERC721Upgradeable {

40:         initializer

44:         __Essential_init(_owner, _addressManager);

45:         __ERC721_init(_name, _symbol);

87:     function name() public view override(ERC721Upgradeable) returns (string memory) {

93:     function symbol() public view override(ERC721Upgradeable) returns (string memory) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC721.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC1155/utils/ERC1155ReceiverUpgradeable.sol";

29: contract ERC1155Vault is BaseNFTVault, ERC1155ReceiverUpgradeable {

171:         return IERC1155ReceiverUpgradeable.onERC1155BatchReceived.selector;

186:         return IERC1155ReceiverUpgradeable.onERC1155Received.selector;

196:         override(BaseVault, ERC1155ReceiverUpgradeable)

199:         return interfaceId == type(ERC1155ReceiverUpgradeable).interfaceId

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

201:                 ERC721Upgradeable t = ERC721Upgradeable(_op.token);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol

38:     function init(address _owner, address _addressManager, IUSDC _usdc) external initializer {

39:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/adapters/USDCAdapter.sol)

```solidity
File: packages/protocol/contracts/verifiers/GuardianVerifier.sol

18:     function init(address _owner, address _addressManager) external initializer {

19:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/GuardianVerifier.sol)

```solidity
File: packages/protocol/contracts/verifiers/SgxVerifier.sol

83:     function init(address _owner, address _addressManager) external initializer {

84:         __Essential_init(_owner, _addressManager);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/verifiers/SgxVerifier.sol)


## Medium Issues


| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | `block.number` means different things on different L2s | 14 |
| [M-2](#M-2) | Centralization Risk for trusted owners | 17 |
| [M-3](#M-3) | Direct `supportsInterface()` calls may cause caller to revert | 6 |
### <a name="M-1"></a>[M-1] `block.number` means different things on different L2s
On Optimism, `block.number` is the L2 block number, but on Arbitrum, it's the L1 block number, and `ArbSys(address(100)).arbBlockNumber()` must be used. Furthermore, L2 block numbers often occur much more frequently than L1 block numbers (any may even occur on a per-transaction basis), so using block numbers for timing results in inconsistencies, especially when voting is involved across multiple chains. As of version 4.9, OpenZeppelin has [modified](https://blog.openzeppelin.com/introducing-openzeppelin-contracts-v4.9#governor) their governor code to use a clock rather than block numbers, to avoid these sorts of issues, but this still requires that the project [implement](https://docs.openzeppelin.com/contracts/4.x/governance#token_2) a [clock](https://eips.ethereum.org/EIPS/eip-6372) for each L2.

*Instances (14)*:
```solidity
File: packages/protocol/contracts/L1/hooks/AssignmentHook.sol

86:                 || assignment.maxProposedIn != 0 && block.number > assignment.maxProposedIn

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/hooks/AssignmentHook.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibProposing.sol

122:                 l1Hash: blockhash(block.number - 1),

131:                 l1Height: uint64(block.number - 1),

204:         meta_.difficulty = keccak256(abi.encodePacked(block.prevrandao, b.numBlocks, block.number));

220:             proposedIn: uint64(block.number),

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibProposing.sol)

```solidity
File: packages/protocol/contracts/L1/libs/LibVerifying.sol

57:         _state.slotA.genesisHeight = uint64(block.number);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/libs/LibVerifying.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

86:         if (block.number == 0) {

88:         } else if (block.number == 1) {

90:             uint256 parentHeight = block.number - 1;

97:         (publicInputHash,) = _calcPublicInputHash(block.number);

118:                 || (block.number != 1 && _parentGasUsed == 0)

127:             parentId = block.number - 1;

201:         if (_blockId >= block.number) return 0;

202:         if (_blockId + 256 >= block.number) return blockhash(_blockId);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

### <a name="M-2"></a>[M-2] Centralization Risk for trusted owners

#### Impact:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (17)*:
```solidity
File: packages/protocol/contracts/L1/TaikoToken.sol

47:     function burn(address _from, uint256 _amount) public onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/TaikoToken.sol)

```solidity
File: packages/protocol/contracts/L2/CrossChainOwned.sol

14: abstract contract CrossChainOwned is EssentialContract, IMessageInvocable {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/CrossChainOwned.sol)

```solidity
File: packages/protocol/contracts/L2/TaikoL2.sol

21: contract TaikoL2 is CrossChainOwned {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L2/TaikoL2.sol)

```solidity
File: packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol

65:     function setMrSigner(bytes32 _mrSigner, bool _trusted) external onlyOwner {

69:     function setMrEnclave(bytes32 _mrEnclave, bool _trusted) external onlyOwner {

122:     function toggleLocalReportCheck() external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/automata-attestation/AutomataDcapV3Attestation.sol)

```solidity
File: packages/protocol/contracts/common/EssentialContract.sol

114:     function _authorizeUpgrade(address) internal virtual override onlyOwner { }

116:     function _authorizePause(address) internal virtual onlyOwner { }

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/common/EssentialContract.sol)

```solidity
File: packages/protocol/contracts/signal/SignalService.sol

56:     function authorize(address _addr, bool _authorize) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/signal/SignalService.sol)

```solidity
File: packages/protocol/contracts/team/TimelockTokenPool.sol

135:     function grant(address _recipient, Grant memory _grant) external onlyOwner {

150:     function void(address _recipient) external onlyOwner {

180:             uint128 amountOwned,

189:         amountOwned = _getAmountOwned(r.grant);

226:         uint128 amountOwned = _getAmountOwned(_grant);

235:     function _getAmountOwned(Grant memory _grant) private view returns (uint128) {

241:             _getAmountOwned(_grant), _grant.unlockStart, _grant.unlockCliff, _grant.unlockPeriod

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/team/TimelockTokenPool.sol)

```solidity
File: packages/protocol/contracts/tokenvault/BridgedERC20.sol

80:     function setSnapshoter(address _snapshooter) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/BridgedERC20.sol)

### <a name="M-3"></a>[M-3] Direct `supportsInterface()` calls may cause caller to revert
Calling `supportsInterface()` on a contract that doesn't implement the ERC-165 standard will result in the call reverting. Even if the caller does support the function, the contract may be malicious and consume all of the transaction's available gas. Call it via a low-level [staticcall()](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L119), with a fixed amount of gas, and check the return code, or use OpenZeppelin's [`ERC165Checker.supportsInterface()`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L36-L39).

*Instances (6)*:
```solidity
File: packages/protocol/contracts/L1/gov/TaikoGovernor.sol

95:         return super.supportsInterface(_interfaceId);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/L1/gov/TaikoGovernor.sol)

```solidity
File: packages/protocol/contracts/bridge/Bridge.sol

195:             if (_message.from.supportsInterface(type(IRecallableSender).interfaceId)) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/bridge/Bridge.sol)

```solidity
File: packages/protocol/contracts/libs/LibAddress.sol

56:         try IERC165(_addr).supportsInterface(_interfaceId) returns (bool _result) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/libs/LibAddress.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC1155Vault.sol

51:         if (!_op.token.supportsInterface(ERC1155_INTERFACE_ID)) {

200:             || BaseVault.supportsInterface(interfaceId);

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC1155Vault.sol)

```solidity
File: packages/protocol/contracts/tokenvault/ERC721Vault.sol

38:         if (!_op.token.supportsInterface(ERC721_INTERFACE_ID)) {

```
[Link to code](https://github.com/code-423n4/2024-03-taiko/tree/main/packages/protocol/contracts/tokenvault/ERC721Vault.sol)
