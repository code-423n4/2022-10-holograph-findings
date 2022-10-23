[1]  USAGE OF ``UINTS/INTS`` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

*There are 44 instances of this issue*

```
File: 2022-10-holograph/contracts/HolographBridge.sol

192: uint32 fromChain,

246:  uint32 toChain,

298:  uint32 toChain,

343:  uint32 toChain,

419:   uint32,
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L192](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L192)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L246](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L246)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L298](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L298)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L343](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L343)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L419](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L419)

```
File: 2022-10-holograph/contracts/HolographOperator.sol

698: uint16(_blockTime),

704-708:  uint16(packed >> 160),
          uint16(packed >> 144),
          uint16(packed >> 128),
          uint16(packed >> 112),
          uint16(packed >> 96)

881:  require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

208:  uint32 private _operatorTempStorageCounter;

585:  uint32 toChain,

665:  uint32

700:  uint40(packed >> 176),

702:  uint64(packed >> 16),

697:  uint8(packed >> 248),

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L698](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L698)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L704-L708](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L704-L708)

 [https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L881](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L881)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L208](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L208)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L585](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L585)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L665](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L665)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L700](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L700)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L702](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L702)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L697](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L697)

```
File: 2022-10-holograph/contracts/HolographFactory.sol

161:  uint32,

178:   uint32

323:   uint8 v,

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L161](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L161)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L178](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L178)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L323)

```
File:  2022-10-holograph/contracts/module/LayerZeroModule.sol

181: uint16,

242:  uint16(_interfaces().getChainId(ChainIdType.HOLOGRAPH, uint256(toChain), ChainIdType.LAYERZERO)),

257:  uint16 lzDestChain = uint16(

270:  uint16(1),

286:  uint16 lzDestChain = uint16(

296:  function _getPricing(LayerZeroOverrides lz, uint16 lzDestChain)

230:    uint32 toChain,

252:    uint32 toChain,

278:  uint32 toChain,

183:  uint64,
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L181](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L181)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L242](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L242)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L257](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L257)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L270](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L270)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L286](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L286)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L296](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L296)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L230](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L230)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L252](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L252)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L278](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L278)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L183](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L183)

```
File: 2022-10-holograph/contracts/enforcer/Holographer.sol

150: (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(

205:  function getOriginChain() external view returns (uint32 originChain) {
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L150](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L150)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L205](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L205)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC721.sol

160:  uint16 private _bps;

248:  uint16 contractBps,

252:    ) = abi.decode(initPayload, (string, string, uint16, uint256, bool, bytes));

399:    function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

414:   uint32 toChain,

878:   function _chain() private view returns (uint32) {

879:    uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())

```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L160](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L160)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L248](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L248)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L252](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L252)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L414)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L878](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L878)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L879](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L879)

```
File:2022-10-holograph/contracts/enforcer/HolographERC20.sol

380: function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

393:  uint32 toChain,

181:  uint8 private _decimals;

229:   uint8 contractDecimals,

465:  uint8 v,
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L380](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L380)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L393](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L393)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L181](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L181)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L229](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L229)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L465](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L465)

[2] USING  ``PRIVATE`` RATHER THAN ``PUBLIC`` FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

*There are 41 instances of this issue:*


```
File: 2022-10-holograph/contracts/HolographBridge.sol

126: bytes32 constant _factorySlot = 0xa49f20855ba576e09d13c8041c8039fa655356ea27f6c40f1ec46a4301cd5b23;

130: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;

134: bytes32 constant _jobNonceSlot = 0x1cda64803f3b43503042e00863791e8d996666552d5855a78d53ee1dd4b3286d;

138: bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;

142: bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L126](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L126)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L130](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L130)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L134](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L134)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L138](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L138)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L142](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L142)

```
File: 2022-10-holograph/contracts/HolographOperator.sol

129: bytes32 constant _bridgeSlot = 0xeb87cbb21687feb327e3d58c6c16d552231d12c7a0e8115042a4165fac8a77f9;

133: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;

137: bytes32 constant _interfacesSlot = 0xbd3084b8c09da87ad159c247a60e209784196be2530cecbbd8f337fdd1848827;

141: bytes32 constant _jobNonceSlot = 0x1cda64803f3b43503042e00863791e8d996666552d5855a78d53ee1dd4b3286d;

145: bytes32 constant _messagingModuleSlot = 0x54176250282e65985d205704ffce44a59efe61f7afd99e29fda50f55b48c061a;

149: bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;

153: bytes32 constant _utilityTokenSlot = 0xbf76518d46db472b71aa7677a0908b8016f3dee568415ffa24055f9a670f9c37;

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L129](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L129)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L133](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L133)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L137](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L137)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L141](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L141)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L145](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L145)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L149](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L149)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L153](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L153)

```
File: 2022-10-holograph/contracts/HolographFactory.sol

127: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;

131:  bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L127](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L127)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L131](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L131)

```
File:  2022-10-holograph/contracts/module/LayerZeroModule.sol

126: bytes32 constant _bridgeSlot = 0xeb87cbb21687feb327e3d58c6c16d552231d12c7a0e8115042a4165fac8a77f9;

130: bytes32 constant _interfacesSlot = 0xbd3084b8c09da87ad159c247a60e209784196be2530cecbbd8f337fdd1848827;

134: bytes32 constant _lZEndpointSlot = 0x56825e447adf54cdde5f04815fcf9b1dd26ef9d5c053625147c18b7c13091686;

138: bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;

142: bytes32 constant _baseGasSlot = 0x1eaa99919d5563fbfdd75d9d906ff8de8cf52beab1ed73875294c8a0c9e9d83a;

146: bytes32 constant _gasPerByteSlot = 0x99d8b07d37c89d4c4f4fa0fd9b7396caeb5d1d4e58b41c61c71e3cf7d424a625;

```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L126](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L126)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L130](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L130)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L134](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L134)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L138](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L138)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L142](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L142)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L146](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L146)

```
File: 2022-10-holograph/contracts/enforcer/Holographer.sol

119: bytes32 constant _originChainSlot = 0xd49ffd6af8249d6e6b5963d9d2b22c6db30ad594cb468453047a14e1c1bcde4d;

123: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;

127: bytes32 constant _contractTypeSlot = 0x0b671eb65810897366dd82c4cbb7d9dff8beda8484194956e81e89b8a361d9c7;

131: bytes32 constant _sourceContractSlot = 0x27d542086d1e831d40b749e7f5509a626c3047a36d160781c40d5acc83e5b074;

135: bytes32 constant _blockHeightSlot = 0x9172848b0f1df776dc924b58e7fa303087ae0409bbf611608529e7f747d55de3;

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L119](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L119)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L123](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L123)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L127](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L127)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L131](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L131)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L135](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L135)

```
File: 2022-10-holograph/contracts/enforcer/PA1D.sol

124:  bytes32 constant _defaultBpSlot = 0x3ab91e3c2ba71a57537d782545f8feb1d402b604f5e070fa6c3b911fc2f18f75;

128: bytes32 constant _defaultReceiverSlot = 0xfd430e1c7265cc31dbd9a10ce657e68878a41cfe179c80cd68c5edf961516848;

132: bytes32 constant _initializedPaidSlot = 0x33a44e907d5bf333e203bebc20bb8c91c00375213b80f466a908f3d50b337c6c;

136: bytes32 constant _payoutAddressesSlot = 0x700a541bc37f227b0d36d34e7b77cc0108bde768297c6f80f448f380387371df;

140:  bytes32 constant _payoutBpsSlot = 0x7a62e8104cd2cc2ef6bd3a26bcb71428108fbe0e0ead6a5bfb8676781e2ed28d;

142-144:  string constant _bpString = "eip1967.Holograph.PA1D.bp";
  string constant _receiverString = "eip1967.Holograph.PA1D.receiver";
  string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L124](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L124)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L128](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L128)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L132](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L132)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L136](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L136)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L140)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L142-L144](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L142-L144)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC721.sol

136: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;

140:  bytes32 constant _sourceContractSlot = 0x27d542086d1e831d40b749e7f5509a626c3047a36d160781c40d5acc83e5b074;
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L136](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L136)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L140)

```
File: 2022-10-holograph/contracts/enforcer/HolographERC20.sol

142:  bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;

146: bytes32 constant _sourceContractSlot = 0x27d542086d1e831d40b749e7f5509a626c3047a36d160781c40d5acc83e5b074;
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L142](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L142)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L146](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L146)

```
File:  2022-10-holograph/contracts/abstract/ERC721H.sol

110: bytes32 constant _holographerSlot = 0xe9fcff60011c1a99f7b7244d1f2d9da93d79ea8ef3654ce590d775575255b2bd;

114:  bytes32 constant _ownerSlot = 0xb56711ba6bd3ded7639fc335ee7524fe668a79d7558c85992e3f8494cf772777;
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L110](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L110)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L114](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L114)

```
File: 2022-10-holograph/contracts/abstract/ERC20H.sol

110: bytes32 constant _holographerSlot = 0xe9fcff60011c1a99f7b7244d1f2d9da93d79ea8ef3654ce590d775575255b2bd;

114:   bytes32 constant _ownerSlot = 0xb56711ba6bd3ded7639fc335ee7524fe668a79d7558c85992e3f8494cf772777;
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L110](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L110)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L114](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L114)


[3] DUPLICATED ``REQUIRE()``/``REVERT()`` CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

Saves deployment cost

*There are 22 instances of this issue:*

```
File: 2022-10-holograph/contracts/HolographBridge.sol

203-206: require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

255-258: require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

352-355:  require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L203-L206](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L203-L206)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L255-L258](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L255-L258)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L352-L355](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L352-L355)

```
File:  2022-10-holograph/contracts/HolographOperator.sol

728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

739:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

829: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

903:  require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

889:  require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L728](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L728).

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L756](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L756)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L829](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L829)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L903](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L903)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L839](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L839)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L889](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L889)

```
File: 2022-10-holograph/contracts/enforcer/PA1D.sol

411: require(balance > 10000, "PA1D: Not enough tokens to transfer");

435: require(balance > 10000, "PA1D: Not enough tokens to transfer");

416: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

439:  require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L416](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L416)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L439](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L439)

```
File: 2022-10-holograph/contracts/enforcer/HolographERC721.sol

458: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

622:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

371:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

388:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

817:  require(!_exists(tokenId), "ERC721: token already exists");

404:  require(!_exists(tokenId), "ERC721: token already exists");

323: require(_exists(tokenId), "ERC721: token does not exist");

906:  require(_exists(tokenId), "ERC721: token does not exist");
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L458](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L458)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L622](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L622)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L371](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L371)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L388](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L388)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L817](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L817)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L404](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L404)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L323)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L906](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L906)

[4] ``ABI.ENCODE()`` IS LESS EFFICIENT THAN ``ABI.ENCODEPACKED()``

*There are 4  instances of this issue:*
```
File: 2022-10-holograph/contracts/HolographFactory.sol

252: abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L252](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L252)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC721.sol

260:  abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))

426:  return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L260](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L260)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L426](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L426)

```
File: 2022-10-holograph/contracts/enforcer/HolographERC20.sol

471: abi.encode(
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L471](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L471)

[5] SPLITTING ``REQUIRE()`` STATEMENTS THAT USE ``&&`` SAVES GAS
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper.
*There are 4 instances of this issue:*

```
File:  2022-10-holograph/contracts/HolographOperator.sol

857:  require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857)

```
File: 2022-10-holograph/contracts/enforcer/Holographer.sol

166: require(success && selector == InitializableInterface.init.selector, "initialization failed");

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166)

```
File: 2022-10-holograph/contracts/enforcer/HolographERC721.sol

263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

464-470:  require(
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector),
        "ERC721: onERC721Received fail"
      );

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L470](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L470)


[6] ``<ARRAY>.LENGTH`` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A`` FOR``-LOOP

The overheads outlined below are *PER LOOP*, excluding the first loop

-storage arrays incur a Gwarmaccess (100 gas)
-memory arrays use MLOAD (3 gas)
-calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset.

*There are 6 instances of this issue:*

```

File: 2022-10-holograph/contracts/HolographOperator.sol

871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871)

```
File : 2022-10-holograph/contracts/enforcer/PA1D.sol

432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

437:  for (uint256 i = 0; i < addresses.length; i++) {

454:   for (uint256 i = 0; i < addresses.length; i++) {

474:  for (uint256 i = 0; i < addresses.length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC20.sol

564:  for (uint256 i = 0; i < wallets.length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

[7] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED ``PAYABLE``

If a function modifier such as ``onlyOwner`` is used, the function will revert if a normal user tries to pay the function. Marking the function as ``payable`` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are ``CALLVALUE``(2),``DUP1``(3),``ISZERO``(3),``PUSH``2(3),``JUMPI``(10),``PUSH1``(3),``DUP1``(3),``REVERT``(0),``JUMPDEST``(1),``POP``(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

*There are 15 instances of this issue:*

```
File:  2022-10-holograph/contracts/enforcer/PA1D.sol

471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

529-533:  function setRoyalties(
    uint256 tokenId,
    address payable receiver,
    uint256 bp
  ) public onlyOwner {
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L529-L533](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L529-L533)

```
File: 2022-10-holograph/contracts/enforcer/HolographERC721.sol

399:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

413-417:     function bridgeOut(
    uint32 toChain,
    address sender,
    bytes calldata payload
  ) external onlyBridge returns (bytes4 selector, bytes memory data) {

500:  function sourceBurn(uint256 tokenId) external onlySource {

508:  function sourceMint(address to, uint224 tokenId) external onlySource {

520:  function sourceGetChainPrepend() external view onlySource returns (uint256) {

577:  function sourceTransfer(address to, uint256 tokenId) external onlySource {

```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L413-L417](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L413-L417)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L500](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L500)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L508](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L508)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L520](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L520)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L577](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L577)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC20.sol

380:   function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

392-396:  function bridgeOut(
    uint32 toChain,
    address sender,
    bytes calldata payload
  ) external onlyBridge returns (bytes4 selector, bytes memory data) {

415:  function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {

549:  function sourceBurn(address from, uint256 amount) external onlySource {

556:  function sourceMint(address to, uint256 amount) external onlySource {

563:  function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {

572-576:   function sourceTransfer(
    address from,
    address to,
    uint256 amount
  ) external onlySource {
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L380](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L380)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L392-L396](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L392-L396)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L415](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L415)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L549](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L549)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L556](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L556)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L563](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L563)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L572-L576](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L572-L576)

[8] ++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--I/I-- TOO)

Saves 5 gas per loop

*There are 15 instances of this issue:*

```
File:  2022-10-holograph/contracts/HolographOperator.sol

781: for (uint256 i = 0; i < length; i++) {

871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871)

```
File:  2022-10-holograph/contracts/enforcer/PA1D.sol

307:  for (uint256 i = 0; i < length; i++) {

323:  for (uint256 i = 0; i < length; i++) {

340: for (uint256 i = 0; i < length; i++) {

356: for (uint256 i = 0; i < length; i++) {

394: for (uint256 i = 0; i < length; i++) {

414:  for (uint256 i = 0; i < length; i++) {

432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

437:  for (uint256 i = 0; i < addresses.length; i++) {

454:  for (uint256 i = 0; i < addresses.length; i++) {

474:  for (uint256 i = 0; i < addresses.length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC721.sol

357: for (uint256 i = 0; i < length; i++) {

716:  for (uint256 i = 0; i < length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC20.sol

564:   for (uint256 i = 0; i < wallets.length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)


[9] ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
 
The ``unchecked`` keyword is new in solidity 0.8.0,so this only applies to that version or higher, which these instances are. This saves 30-40 gas *[PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)*

*There are 15 instances of this issue:*

```
File:  2022-10-holograph/contracts/HolographOperator.sol

781: for (uint256 i = 0; i < length; i++) {

871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871)

```
File:  2022-10-holograph/contracts/enforcer/PA1D.sol

307:  for (uint256 i = 0; i < length; i++) {

323:  for (uint256 i = 0; i < length; i++) {

340: for (uint256 i = 0; i < length; i++) {

356: for (uint256 i = 0; i < length; i++) {

394: for (uint256 i = 0; i < length; i++) {

414:  for (uint256 i = 0; i < length; i++) {

432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

437:  for (uint256 i = 0; i < addresses.length; i++) {

454:  for (uint256 i = 0; i < addresses.length; i++) {

474:  for (uint256 i = 0; i < addresses.length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC721.sol

357: for (uint256 i = 0; i < length; i++) {

716:  for (uint256 i = 0; i < length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716)

```
File:  2022-10-holograph/contracts/enforcer/HolographERC20.sol

564:   for (uint256 i = 0; i < wallets.length; i++) {
```

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

[10] USE ASSEMBLY TO CHECK FOR ADDRESS(0) to save gas.

*There are 5 instances of this issue:*

```
File:  2022-10-holograph/contracts/enforcer/HolographERC721.sol

419:  require(to != address(0), "ERC721: zero address");

639:  require(wallet != address(0), "ERC721: zero address");

689:  require(tokenOwner != address(0), "ERC721: token does not exist");

816:  require(to != address(0), "ERC721: minting to burn address");

870:  require(to != address(0), "ERC721: use burn instead");
```
[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L419](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L419)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L639](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L639)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L689](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L689)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L816](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L816)

[https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L870](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L870)














