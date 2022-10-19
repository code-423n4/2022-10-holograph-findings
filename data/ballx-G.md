# c4udit Report

## Files analyzed
- contracts/Holograph.sol
- contracts/HolographBridge.sol
- contracts/HolographFactory.sol
- contracts/HolographGenesis.sol
- contracts/HolographInterfaces.sol
- contracts/HolographOperator.sol
- contracts/HolographRegistry.sol
- contracts/HolographTreasury.sol
- contracts/abstract/Admin.sol
- contracts/abstract/EIP712.sol
- contracts/abstract/ERC1155H.sol
- contracts/abstract/ERC20H.sol
- contracts/abstract/ERC721H.sol
- contracts/abstract/Initializable.sol
- contracts/abstract/NonReentrant.sol
- contracts/abstract/Owner.sol
- contracts/abstract/StrictERC1155H.sol
- contracts/abstract/StrictERC20H.sol
- contracts/abstract/StrictERC721H.sol
- contracts/enforcer/HolographERC20.sol
- contracts/enforcer/HolographERC721.sol
- contracts/enforcer/Holographer.sol
- contracts/enforcer/PA1D.sol
- contracts/enum/ChainIdType.sol
- contracts/enum/HolographERC20Event.sol
- contracts/enum/HolographERC721Event.sol
- contracts/enum/InterfaceType.sol
- contracts/enum/TokenUriType.sol
- contracts/faucet/Faucet.sol
- contracts/interface/CollectionURI.sol
- contracts/interface/CrossChainMessageInterface.sol
- contracts/interface/ERC1271.sol
- contracts/interface/ERC165.sol
- contracts/interface/ERC20.sol
- contracts/interface/ERC20Burnable.sol
- contracts/interface/ERC20Metadata.sol
- contracts/interface/ERC20Permit.sol
- contracts/interface/ERC20Receiver.sol
- contracts/interface/ERC20Safer.sol
- contracts/interface/ERC721.sol
- contracts/interface/ERC721Enumerable.sol
- contracts/interface/ERC721Metadata.sol
- contracts/interface/ERC721TokenReceiver.sol
- contracts/interface/HolographBridgeInterface.sol
- contracts/interface/HolographERC20Interface.sol
- contracts/interface/HolographERC721Interface.sol
- contracts/interface/HolographFactoryInterface.sol
- contracts/interface/HolographInterface.sol
- contracts/interface/HolographInterfacesInterface.sol
- contracts/interface/HolographOperatorInterface.sol
- contracts/interface/HolographRegistryInterface.sol
- contracts/interface/HolographTreasuryInterface.sol
- contracts/interface/Holographable.sol
- contracts/interface/HolographedERC1155.sol
- contracts/interface/HolographedERC20.sol
- contracts/interface/HolographedERC721.sol
- contracts/interface/HolographerInterface.sol
- contracts/interface/InitializableInterface.sol
- contracts/interface/LayerZeroEndpointInterface.sol
- contracts/interface/LayerZeroModuleInterface.sol
- contracts/interface/LayerZeroOverrides.sol
- contracts/interface/LayerZeroReceiverInterface.sol
- contracts/interface/LayerZeroUserApplicationConfigInterface.sol
- contracts/interface/Ownable.sol
- contracts/interface/PA1DInterface.sol
- contracts/library/Base64.sol
- contracts/library/Bytes.sol
- contracts/library/ECDSA.sol
- contracts/library/Strings.sol
- contracts/mock/ERC20Mock.sol
- contracts/mock/LZEndpointMock.sol
- contracts/mock/Mock.sol
- contracts/mock/MockERC721Receiver.sol
- contracts/mock/MockExternalCall.sol
- contracts/mock/MockHolographChild.sol
- contracts/mock/MockHolographGenesisChild.sol
- contracts/mock/MockLZEndpoint.sol
- contracts/module/LayerZeroModule.sol
- contracts/proxy/CxipERC721Proxy.sol
- contracts/proxy/HolographBridgeProxy.sol
- contracts/proxy/HolographFactoryProxy.sol
- contracts/proxy/HolographOperatorProxy.sol
- contracts/proxy/HolographRegistryProxy.sol
- contracts/proxy/HolographTreasuryProxy.sol
- contracts/struct/DeploymentConfig.sol
- contracts/struct/OperatorJob.sol
- contracts/struct/Verification.sol
- contracts/struct/ZoraBidShares.sol
- contracts/struct/ZoraDecimal.sol
- contracts/token/CxipERC721.sol
- contracts/token/HolographUtilityToken.sol
- contracts/token/SampleERC20.sol
- contracts/token/SampleERC721.sol
- contracts/token/hToken.sol

## Issues found

### Don't Initialize Variables with Default Value

#### Impact
Issue Information: [G001](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g001---dont-initialize-variables-with-default-value)

#### Findings:
```
contracts/HolographBridge.sol::380 => uint256 fee = 0;
contracts/HolographInterfaces.sol::235 => for (uint256 i = 0; i < uriTypes.length; i++) {
contracts/HolographInterfaces.sol::264 => for (uint256 i = 0; i < length; i++) {
contracts/HolographInterfaces.sol::286 => for (uint256 i = 0; i < interfaceIds.length; i++) {
contracts/HolographOperator.sol::310 => uint256 gasLimit = 0;
contracts/HolographOperator.sol::311 => uint256 gasPrice = 0;
contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
contracts/HolographRegistry.sol::175 => for (uint256 i = 0; i < reservedTypes.length; i++) {
contracts/HolographRegistry.sol::255 => for (uint256 i = 0; i < length; i++) {
contracts/HolographRegistry.sol::322 => for (uint256 i = 0; i < hashes.length; i++) {
contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
contracts/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
contracts/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
contracts/library/Base64.sol::119 => uint256 i = 0;
contracts/library/Base64.sol::120 => uint256 j = 0;
contracts/library/Base64.sol::130 => uint8 la1 = 0;
contracts/library/Bytes.sol::159 => uint256 length = 0;
contracts/library/Strings.sol::114 => uint256 length = 0;
contracts/library/Strings.sol::136 => for (uint256 i = 0; i < 20; i++) {
contracts/mock/LZEndpointMock.sol::251 => for (uint256 i = 0; i < msgs.length - 1; i++) {
contracts/mock/Mock.sol::114 => bool shouldFail = false;
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Cache Array Length Outside of Loop

#### Impact
Issue Information: [G002](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g002---cache-array-length-outside-of-loop)

#### Findings:
```
contracts/HolographInterfaces.sol::235 => for (uint256 i = 0; i < uriTypes.length; i++) {
contracts/HolographInterfaces.sol::263 => uint256 length = fromChainType.length;
contracts/HolographInterfaces.sol::264 => for (uint256 i = 0; i < length; i++) {
contracts/HolographInterfaces.sol::286 => for (uint256 i = 0; i < interfaceIds.length; i++) {
contracts/HolographOperator.sol::316 => gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
contracts/HolographOperator.sol::320 => gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
contracts/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
contracts/HolographOperator.sol::391 => _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
contracts/HolographOperator.sol::408 => _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
contracts/HolographOperator.sol::451 => calldatacopy(0, payload.offset, sub(payload.length, 0x20))
contracts/HolographOperator.sol::461 => mload(sub(payload.length, 0x40)),
contracts/HolographOperator.sol::469 => sub(payload.length, 0x40),
contracts/HolographOperator.sol::503 => uint256 pod = random % _operatorPods.length;
contracts/HolographOperator.sol::507 => uint256 podSize = _operatorPods[pod].length;
contracts/HolographOperator.sol::551 => calldatacopy(0, bridgeInRequestPayload.offset, sub(bridgeInRequestPayload.length, 0x40))
contracts/HolographOperator.sol::556 => let result := call(gas(), sload(_bridgeSlot), callvalue(), 0, sub(bridgeInRequestPayload.length, 0x40), 0, 0)
contracts/HolographOperator.sol::718 => return _operatorPods.length;
contracts/HolographOperator.sol::728 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
contracts/HolographOperator.sol::729 => return _operatorPods[pod - 1].length;
contracts/HolographOperator.sol::739 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
contracts/HolographOperator.sol::745 => * @dev Use in conjunction with getPodOperatorsLength to know the total length of results
contracts/HolographOperator.sol::748 => * @param length the length of result set to be (will be shorter if reached end of array)
contracts/HolographOperator.sol::754 => uint256 length
contracts/HolographOperator.sol::756 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
contracts/HolographOperator.sol::762 => * @dev get total length of pod operators
contracts/HolographOperator.sol::764 => uint256 supply = _operatorPods[pod].length;
contracts/HolographOperator.sol::766 => * @dev check if length is out of bounds for this result set
contracts/HolographOperator.sol::768 => if (index + length > supply) {
contracts/HolographOperator.sol::770 => * @dev adjust length to return remainder of the results
contracts/HolographOperator.sol::772 => length = supply - index;
contracts/HolographOperator.sol::777 => operators = new address[](length);
contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
contracts/HolographOperator.sol::867 => if (_operatorPods.length < pod) {
contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
contracts/HolographOperator.sol::881 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
contracts/HolographOperator.sol::883 => _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;
contracts/HolographOperator.sol::1137 => uint256 lastIndex = _operatorPods[pod].length - 1;
contracts/HolographOperator.sol::1150 => * @dev shorten array length
contracts/HolographOperator.sol::1169 => if (pod >= _operatorPods.length) {
contracts/HolographOperator.sol::1173 => uint256 position = _operatorPods[pod].length;
contracts/HolographRegistry.sol::175 => for (uint256 i = 0; i < reservedTypes.length; i++) {
contracts/HolographRegistry.sol::244 => * @notice Get set length list, starting from index, for all holographable contracts
contracts/HolographRegistry.sol::246 => * @param length The length of returned results
contracts/HolographRegistry.sol::247 => * @return contracts address[] Returns a set length array of holographable contracts deployed
contracts/HolographRegistry.sol::249 => function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts) {
contracts/HolographRegistry.sol::250 => uint256 supply = _holographableContracts.length;
contracts/HolographRegistry.sol::251 => if (index + length > supply) {
contracts/HolographRegistry.sol::252 => length = supply - index;
contracts/HolographRegistry.sol::254 => contracts = new address[](length);
contracts/HolographRegistry.sol::255 => for (uint256 i = 0; i < length; i++) {
contracts/HolographRegistry.sol::264 => return _holographableContracts.length;
contracts/HolographRegistry.sol::322 => for (uint256 i = 0; i < hashes.length; i++) {
contracts/abstract/Admin.sol::135 => calldatacopy(0, data.offset, data.length)
contracts/abstract/Admin.sol::136 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
contracts/abstract/Owner.sol::149 => calldatacopy(0, data.offset, data.length)
contracts/abstract/Owner.sol::150 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
contracts/enforcer/HolographERC20.sol::652 => if (reason.length == 0) {
contracts/enforcer/HolographERC20.sol::664 => if (reason.length == 0) {
contracts/enforcer/HolographERC721.sol::341 => * @notice Get set length list, starting from index, for tokens owned by wallet.
contracts/enforcer/HolographERC721.sol::344 => * @param length The length of returned results.
contracts/enforcer/HolographERC721.sol::345 => * @return tokenIds uint256[] Returns a set length array of token ids owned by wallet.
contracts/enforcer/HolographERC721.sol::350 => uint256 length
contracts/enforcer/HolographERC721.sol::353 => if (index + length > supply) {
contracts/enforcer/HolographERC721.sol::354 => length = supply - index;
contracts/enforcer/HolographERC721.sol::356 => tokenIds = new uint256[](length);
contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::528 => //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
contracts/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
contracts/enforcer/HolographERC721.sol::543 => //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
contracts/enforcer/HolographERC721.sol::544 => //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
contracts/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
contracts/enforcer/HolographERC721.sol::560 => //     uint256 length
contracts/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::700 => require(index < _allTokens.length, "ERC721: index out of bounds");
contracts/enforcer/HolographERC721.sol::705 => * @notice Get set length list, starting from index, for all tokens.
contracts/enforcer/HolographERC721.sol::707 => * @param length The length of returned results.
contracts/enforcer/HolographERC721.sol::708 => * @return tokenIds uint256[] Returns a set length array of token ids minted.
contracts/enforcer/HolographERC721.sol::710 => function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {
contracts/enforcer/HolographERC721.sol::711 => uint256 supply = _allTokens.length;
contracts/enforcer/HolographERC721.sol::712 => if (index + length > supply) {
contracts/enforcer/HolographERC721.sol::713 => length = supply - index;
contracts/enforcer/HolographERC721.sol::715 => tokenIds = new uint256[](length);
contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::739 => return _allTokens.length;
contracts/enforcer/HolographERC721.sol::781 => _allTokensIndex[tokenId] = _allTokens.length;
contracts/enforcer/HolographERC721.sol::825 => uint256 lastTokenIndex = _allTokens.length - 1;
contracts/enforcer/PA1D.sol::301 => uint256 length;
contracts/enforcer/PA1D.sol::303 => length := sload(slot)
contracts/enforcer/PA1D.sol::305 => addresses = new address payable[](length);
contracts/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::318 => uint256 length = addresses.length;
contracts/enforcer/PA1D.sol::320 => sstore(slot, length)
contracts/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::334 => uint256 length;
contracts/enforcer/PA1D.sol::336 => length := sload(slot)
contracts/enforcer/PA1D.sol::338 => bps = new uint256[](length);
contracts/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::351 => uint256 length = bps.length;
contracts/enforcer/PA1D.sol::353 => sstore(slot, length)
contracts/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::385 => uint256 length = addresses.length;
contracts/enforcer/PA1D.sol::388 => uint256 gasCost = (23300 * length) + length;
contracts/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::408 => uint256 length = addresses.length;
contracts/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
contracts/enforcer/PA1D.sol::467 => * @dev Addresses and bps arrays must be equal length. Bps values added together must equal 10000 exactly.
contracts/enforcer/PA1D.sol::472 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
contracts/interface/HolographOperatorInterface.sol::216 => * @dev Use in conjunction with getPodOperatorsLength to know the total length of results
contracts/interface/HolographOperatorInterface.sol::219 => * @param length the length of result set to be (will be shorter if reached end of array)
contracts/interface/HolographOperatorInterface.sol::225 => uint256 length
contracts/interface/HolographRegistryInterface.sol::119 => function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts);
contracts/interface/LayerZeroEndpointInterface.sol::10 => // @param _destination - the address on destination chain (in bytes). address length/format may vary by chains
contracts/library/Base64.sol::114 => uint256 rem = _bs.length % 3;
contracts/library/Base64.sol::116 => uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
contracts/library/Base64.sol::117 => bytes memory res = new bytes(res_length);
contracts/library/Base64.sol::122 => for (; i + 3 <= _bs.length; i += 3) {
contracts/library/Base64.sol::129 => uint8 la0 = uint8(_bs[_bs.length - rem]);
contracts/library/Base64.sol::133 => la1 = uint8(_bs[_bs.length - 1]);
contracts/library/Bytes.sol::125 => uint256 _length
contracts/library/Bytes.sol::127 => require(_length + 31 >= _length, "slice_overflow");
contracts/library/Bytes.sol::128 => require(_bytes.length >= _start + _length, "slice_outOfBounds");
contracts/library/Bytes.sol::131 => switch iszero(_length)
contracts/library/Bytes.sol::134 => let lengthmod := and(_length, 31)
contracts/library/Bytes.sol::135 => let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
contracts/library/Bytes.sol::136 => let end := add(mc, _length)
contracts/library/Bytes.sol::138 => let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
contracts/library/Bytes.sol::145 => mstore(tempBytes, _length)
contracts/library/Bytes.sol::159 => uint256 length = 0;
contracts/library/Bytes.sol::161 => length++;
contracts/library/Bytes.sol::164 => return slice(abi.encodePacked(source), 32 - length, length);
contracts/library/ECDSA.sol::29 => revert("ECDSA: invalid signature length");
contracts/library/ECDSA.sol::58 => // Check the signature length
contracts/library/ECDSA.sol::61 => if (signature.length == 65) {
contracts/library/ECDSA.sol::73 => } else if (signature.length == 64) {
contracts/library/ECDSA.sol::201 => // 32 is the length in bytes of hash,
contracts/library/ECDSA.sol::215 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
contracts/library/Strings.sol::114 => uint256 length = 0;
contracts/library/Strings.sol::116 => length++;
contracts/library/Strings.sol::119 => return toHexString(value, length);
contracts/library/Strings.sol::122 => function toHexString(uint256 value, uint256 length) internal pure returns (string memory) {
contracts/library/Strings.sol::123 => bytes memory buffer = new bytes(2 * length + 2);
contracts/library/Strings.sol::126 => for (uint256 i = 2 * length + 1; i > 1; --i) {
contracts/library/Strings.sol::130 => require(value == 0, "Strings: hex length insufficient");
contracts/mock/ERC20Mock.sol::504 => if (reason.length == 0) {
contracts/mock/ERC20Mock.sol::516 => if (reason.length == 0) {
contracts/mock/LZEndpointMock.sol::246 => if (msgs.length > 0) {
contracts/mock/LZEndpointMock.sol::251 => for (uint256 i = 0; i < msgs.length - 1; i++) {
contracts/mock/LZEndpointMock.sol::262 => uint64(_payload.length),
contracts/mock/LZEndpointMock.sol::282 => return msgsToDeliver[_srcChainId][_srcAddress].length;
contracts/mock/LZEndpointMock.sol::307 => calldatacopy(ptr, sub(_b.offset, 2), add(_b.length, 2))
contracts/mock/LZEndpointMock.sol::368 => while (msgs.length > 0) {
contracts/mock/LZEndpointMock.sol::369 => QueuedPayload memory payload = msgs[msgs.length - 1];
contracts/mock/LZEndpointMock.sol::404 => require(_payload.length == sp.payloadLength && keccak256(_payload) == sp.payloadHash, "LayerZero: invalid payload");
contracts/mock/Mock.sol::145 => calldatacopy(0, data.offset, data.length)
contracts/mock/Mock.sol::146 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
contracts/mock/Mock.sol::160 => calldatacopy(0, data.offset, data.length)
contracts/mock/Mock.sol::161 => let result := staticcall(gas(), target, 0, data.length, 0, 0)
contracts/mock/Mock.sol::175 => calldatacopy(0, data.offset, data.length)
contracts/mock/Mock.sol::176 => let result := delegatecall(gas(), target, 0, data.length, 0, 0)
contracts/module/LayerZeroModule.sol::202 => calldatacopy(add(ptr, 0x0c), _srcAddress.offset, _srcAddress.length)
contracts/module/LayerZeroModule.sol::247 => abi.encodePacked(uint16(1), uint256(_baseGas() + (crossChainPayload.length * _gasPerByte())))
contracts/module/LayerZeroModule.sol::271 => uint256(_baseGas() + (crossChainPayload.length * _gasPerByte()))
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: [G003](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g003---use--0-instead-of--0-for-unsigned-integer-comparison)

#### Findings:
```
contracts/HolographBridge.sol::218 => if (hTokenValue > 0) {
contracts/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
contracts/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
contracts/HolographOperator.sol::398 => if (leftovers > 0) {
contracts/HolographOperator.sol::1126 => if (operatorIndex > 0) {
contracts/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
contracts/interface/ERC721.sol::46 => ///  checks if `_to` is a smart contract (code size > 0). If so, it calls
contracts/library/ECDSA.sol::161 => if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
contracts/mock/LZEndpointMock.sol::246 => if (msgs.length > 0) {
contracts/mock/LZEndpointMock.sol::368 => while (msgs.length > 0) {
contracts/token/hToken.sol::176 => require(msg.value > 0, "hToken: no value received");
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: [G006](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g006---use-immutable-for-openzeppelin-accesscontrols-roles-declarations)

#### Findings:
```
contracts/Holograph.sol::118 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
contracts/Holograph.sol::122 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.chainId')) - 1)
contracts/Holograph.sol::126 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
contracts/Holograph.sol::130 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographChainId')) - 1)
contracts/Holograph.sol::134 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
contracts/Holograph.sol::138 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/Holograph.sol::142 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/Holograph.sol::146 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.treasury')) - 1)
contracts/Holograph.sol::150 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
contracts/HolographBridge.sol::124 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
contracts/HolographBridge.sol::128 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/HolographBridge.sol::132 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.jobNonce')) - 1)
contracts/HolographBridge.sol::136 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/HolographBridge.sol::140 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/HolographFactory.sol::125 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/HolographFactory.sol::129 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/HolographFactory.sol::206 => bytes32 hash = keccak256(
contracts/HolographFactory.sol::211 => keccak256(config.byteCode),
contracts/HolographFactory.sol::212 => keccak256(config.initCode),
contracts/HolographFactory.sol::226 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), hash, keccak256(holographerBytecode)))))
contracts/HolographFactory.sol::333 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
contracts/HolographGenesis.sol::133 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, keccak256(sourceCode)))))
contracts/HolographOperator.sol::127 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
contracts/HolographOperator.sol::131 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/HolographOperator.sol::135 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
contracts/HolographOperator.sol::139 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.jobNonce')) - 1)
contracts/HolographOperator.sol::143 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.messagingModule')) - 1)
contracts/HolographOperator.sol::147 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/HolographOperator.sol::151 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
contracts/HolographOperator.sol::305 => bytes32 hash = keccak256(bridgeInRequestPayload);
contracts/HolographOperator.sol::491 => bytes32 jobHash = keccak256(bridgeInRequestPayload);
contracts/HolographOperator.sol::499 => uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
contracts/HolographOperator.sol::651 => emit CrossChainMessageSent(keccak256(encodedData));
contracts/HolographOperator.sol::686 => * @dev The job hash is a keccak256 hash of the entire job payload
contracts/HolographOperator.sol::687 => * @param jobHash keccak256 hash of the job
contracts/HolographRegistry.sol::119 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/HolographRegistry.sol::123 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
contracts/HolographTreasury.sol::127 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
contracts/HolographTreasury.sol::131 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/HolographTreasury.sol::135 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/HolographTreasury.sol::139 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/abstract/Admin.sol::106 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.admin')) - 1)
contracts/abstract/EIP712.sol::13 => * they need in their contracts using a combination of `abi.encode` and `keccak256`.
contracts/abstract/EIP712.sol::61 => bytes32 hashedName = keccak256(bytes(name));
contracts/abstract/EIP712.sol::62 => bytes32 hashedVersion = keccak256(bytes(version));
contracts/abstract/EIP712.sol::63 => bytes32 typeHash = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");
contracts/abstract/EIP712.sol::88 => return keccak256(abi.encode(typeHash, nameHash, versionHash, block.chainid, address(this)));
contracts/abstract/EIP712.sol::98 => * bytes32 digest = _hashTypedDataV4(keccak256(abi.encode(
contracts/abstract/EIP712.sol::99 => *     keccak256("Mail(address to,string contents)"),
contracts/abstract/EIP712.sol::101 => *     keccak256(bytes(mailContents))
contracts/abstract/ERC1155H.sol::108 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
contracts/abstract/ERC1155H.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
contracts/abstract/ERC20H.sol::108 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
contracts/abstract/ERC20H.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
contracts/abstract/ERC721H.sol::108 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
contracts/abstract/ERC721H.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
contracts/abstract/Initializable.sol::114 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.initialized')) - 1)
contracts/abstract/NonReentrant.sol::106 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.reentrant')) - 1)
contracts/abstract/Owner.sol::106 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
contracts/enforcer/HolographERC20.sol::140 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/enforcer/HolographERC20.sol::144 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
contracts/enforcer/HolographERC20.sol::470 => bytes32 structHash = keccak256(
contracts/enforcer/HolographERC721.sol::134 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/enforcer/HolographERC721.sol::138 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
contracts/enforcer/Holographer.sol::117 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.originChain')) - 1)
contracts/enforcer/Holographer.sol::121 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
contracts/enforcer/Holographer.sol::125 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.contractType')) - 1)
contracts/enforcer/Holographer.sol::129 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
contracts/enforcer/Holographer.sol::133 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.blockHeight')) - 1)
contracts/enforcer/PA1D.sol::122 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.defaultBp')) - 1)
contracts/enforcer/PA1D.sol::126 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.defaultReceiver')) - 1)
contracts/enforcer/PA1D.sol::130 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.initialized')) - 1)
contracts/enforcer/PA1D.sol::134 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.payout.addresses')) - 1)
contracts/enforcer/PA1D.sol::138 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.payout.bps')) - 1)
contracts/enforcer/PA1D.sol::257 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
contracts/enforcer/PA1D.sol::269 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
contracts/enforcer/PA1D.sol::280 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
contracts/enforcer/PA1D.sol::292 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
contracts/enforcer/PA1D.sol::308 => slot = keccak256(abi.encodePacked(i, slot));
contracts/enforcer/PA1D.sol::324 => slot = keccak256(abi.encodePacked(i, slot));
contracts/enforcer/PA1D.sol::341 => slot = keccak256(abi.encodePacked(i, slot));
contracts/enforcer/PA1D.sol::357 => slot = keccak256(abi.encodePacked(i, slot));
contracts/enforcer/PA1D.sol::366 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
contracts/enforcer/PA1D.sol::373 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
contracts/interface/ERC721.sol::48 => ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
contracts/interface/ERC721TokenReceiver.sol::116 => /// @return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
contracts/interface/HolographOperatorInterface.sol::186 => * @dev The job hash is a keccak256 hash of the entire job payload
contracts/interface/HolographOperatorInterface.sol::187 => * @param jobHash keccak256 hash of the job
contracts/library/ECDSA.sol::203 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
contracts/library/ECDSA.sol::215 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
contracts/library/ECDSA.sol::228 => return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
contracts/mock/ERC20Mock.sol::226 => // bytes4(keccak256(abi.encodePacked('safeTransfer(address,uint256)'))) == 0x423f6cef
contracts/mock/ERC20Mock.sol::228 => // bytes4(keccak256(abi.encodePacked('safeTransfer(address,uint256,bytes)'))) == 0xeb795549
contracts/mock/ERC20Mock.sol::230 => // bytes4(keccak256(abi.encodePacked('safeTransferFrom(address,address,uint256)'))) == 0x42842e0e
contracts/mock/ERC20Mock.sol::232 => // bytes4(keccak256(abi.encodePacked('safeTransferFrom(address,address,uint256,bytes)'))) == 0xb88d4fde
contracts/mock/ERC20Mock.sol::388 => // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
contracts/mock/ERC20Mock.sol::390 => bytes32 structHash = keccak256(
contracts/mock/LZEndpointMock.sol::264 => keccak256(_payload)
contracts/mock/LZEndpointMock.sol::404 => require(_payload.length == sp.payloadLength && keccak256(_payload) == sp.payloadHash, "LayerZero: invalid payload");
contracts/module/LayerZeroModule.sol::124 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
contracts/module/LayerZeroModule.sol::128 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
contracts/module/LayerZeroModule.sol::132 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.lZEndpoint')) - 1)
contracts/module/LayerZeroModule.sol::136 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/module/LayerZeroModule.sol::140 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/module/LayerZeroModule.sol::144 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/proxy/CxipERC721Proxy.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.contractType')) - 1)
contracts/proxy/CxipERC721Proxy.sol::116 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/proxy/HolographBridgeProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
contracts/proxy/HolographFactoryProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
contracts/proxy/HolographOperatorProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
contracts/proxy/HolographRegistryProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
contracts/proxy/HolographTreasuryProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.treasury')) - 1)
contracts/token/SampleERC20.sol::149 => _walletSalts[to] = keccak256(
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Long Revert Strings

#### Impact
Issue Information: [G007](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g007---long-revert-strings)

#### Findings:
```
contracts/Holograph.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/Holograph.sol::107 => import "./interface/InitializableInterface.sol";
contracts/Holograph.sol::108 => import "./interface/HolographInterface.sol";
contracts/HolographBridge.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographBridge.sol::107 => import "./interface/HolographERC20Interface.sol";
contracts/HolographBridge.sol::109 => import "./interface/HolographInterface.sol";
contracts/HolographBridge.sol::110 => import "./interface/HolographBridgeInterface.sol";
contracts/HolographBridge.sol::111 => import "./interface/HolographFactoryInterface.sol";
contracts/HolographBridge.sol::112 => import "./interface/HolographOperatorInterface.sol";
contracts/HolographBridge.sol::113 => import "./interface/HolographRegistryInterface.sol";
contracts/HolographBridge.sol::114 => import "./interface/InitializableInterface.sol";
contracts/HolographFactory.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographFactory.sol::110 => import "./interface/HolographFactoryInterface.sol";
contracts/HolographFactory.sol::111 => import "./interface/HolographRegistryInterface.sol";
contracts/HolographFactory.sol::112 => import "./interface/InitializableInterface.sol";
contracts/HolographGenesis.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographGenesis.sol::104 => import "./interface/InitializableInterface.sol";
contracts/HolographInterfaces.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographInterfaces.sol::121 => import "./interface/ERC721TokenReceiver.sol";
contracts/HolographInterfaces.sol::122 => import "./interface/InitializableInterface.sol";
contracts/HolographOperator.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographOperator.sol::107 => import "./interface/CrossChainMessageInterface.sol";
contracts/HolographOperator.sol::108 => import "./interface/HolographBridgeInterface.sol";
contracts/HolographOperator.sol::109 => import "./interface/HolographERC20Interface.sol";
contracts/HolographOperator.sol::110 => import "./interface/HolographInterface.sol";
contracts/HolographOperator.sol::111 => import "./interface/HolographOperatorInterface.sol";
contracts/HolographOperator.sol::112 => import "./interface/HolographRegistryInterface.sol";
contracts/HolographOperator.sol::113 => import "./interface/InitializableInterface.sol";
contracts/HolographOperator.sol::114 => import "./interface/HolographInterfacesInterface.sol";
contracts/HolographRegistry.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographRegistry.sol::107 => import "./interface/HolographInterface.sol";
contracts/HolographRegistry.sol::108 => import "./interface/HolographRegistryInterface.sol";
contracts/HolographRegistry.sol::109 => import "./interface/InitializableInterface.sol";
contracts/HolographTreasury.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/HolographTreasury.sol::107 => import "./interface/HolographERC20Interface.sol";
contracts/HolographTreasury.sol::108 => import "./interface/HolographERC721Interface.sol";
contracts/HolographTreasury.sol::109 => import "./interface/HolographInterface.sol";
contracts/HolographTreasury.sol::110 => import "./interface/HolographTreasuryInterface.sol";
contracts/HolographTreasury.sol::111 => import "./interface/HolographFactoryInterface.sol";
contracts/HolographTreasury.sol::112 => import "./interface/HolographOperatorInterface.sol";
contracts/HolographTreasury.sol::113 => import "./interface/HolographRegistryInterface.sol";
contracts/HolographTreasury.sol::114 => import "./interface/InitializableInterface.sol";
contracts/abstract/Admin.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/EIP712.sol::63 => bytes32 typeHash = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");
contracts/abstract/ERC1155H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/ERC20H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/ERC721H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/Initializable.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/Initializable.sol::104 => import "../interface/InitializableInterface.sol";
contracts/abstract/NonReentrant.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/Owner.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/StrictERC1155H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/StrictERC1155H.sol::104 => import "../interface/HolographedERC1155.sol";
contracts/abstract/StrictERC20H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/StrictERC20H.sol::104 => import "../interface/HolographedERC20.sol";
contracts/abstract/StrictERC721H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/abstract/StrictERC721H.sol::104 => import "../interface/HolographedERC721.sol";
contracts/enforcer/HolographERC20.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enforcer/HolographERC20.sol::115 => import "../interface/HolographERC20Interface.sol";
contracts/enforcer/HolographERC20.sol::122 => import "../interface/HolographedERC20.sol";
contracts/enforcer/HolographERC20.sol::123 => import "../interface/HolographInterface.sol";
contracts/enforcer/HolographERC20.sol::124 => import "../interface/HolographerInterface.sol";
contracts/enforcer/HolographERC20.sol::125 => import "../interface/HolographRegistryInterface.sol";
contracts/enforcer/HolographERC20.sol::126 => import "../interface/InitializableInterface.sol";
contracts/enforcer/HolographERC20.sol::127 => import "../interface/HolographInterfacesInterface.sol";
contracts/enforcer/HolographERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enforcer/HolographERC721.sol::113 => import "../interface/HolographERC721Interface.sol";
contracts/enforcer/HolographERC721.sol::115 => import "../interface/ERC721TokenReceiver.sol";
contracts/enforcer/HolographERC721.sol::117 => import "../interface/HolographedERC721.sol";
contracts/enforcer/HolographERC721.sol::118 => import "../interface/HolographInterface.sol";
contracts/enforcer/HolographERC721.sol::119 => import "../interface/HolographerInterface.sol";
contracts/enforcer/HolographERC721.sol::120 => import "../interface/HolographRegistryInterface.sol";
contracts/enforcer/HolographERC721.sol::121 => import "../interface/InitializableInterface.sol";
contracts/enforcer/HolographERC721.sol::122 => import "../interface/HolographInterfacesInterface.sol";
contracts/enforcer/Holographer.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enforcer/Holographer.sol::107 => import "../interface/HolographInterface.sol";
contracts/enforcer/Holographer.sol::108 => import "../interface/HolographerInterface.sol";
contracts/enforcer/Holographer.sol::109 => import "../interface/HolographRegistryInterface.sol";
contracts/enforcer/Holographer.sol::110 => import "../interface/InitializableInterface.sol";
contracts/enforcer/PA1D.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enforcer/PA1D.sol::109 => import "../interface/InitializableInterface.sol";
contracts/enforcer/PA1D.sol::144 => string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
contracts/enum/ChainIdType.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enum/HolographERC20Event.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enum/HolographERC721Event.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enum/InterfaceType.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/enum/TokenUriType.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/faucet/Faucet.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/faucet/Faucet.sol::105 => import "../interface/HolographERC20Interface.sol";
contracts/faucet/Faucet.sol::106 => import "../interface/InitializableInterface.sol";
contracts/faucet/Faucet.sol::128 => require(!_isInitialized(), "Faucet contract is already initialized");
contracts/interface/CollectionURI.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/CrossChainMessageInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/ERC721.sol::48 => ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
contracts/interface/ERC721TokenReceiver.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/ERC721TokenReceiver.sol::116 => /// @return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
contracts/interface/HolographBridgeInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographERC20Interface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographERC721Interface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographFactoryInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographInterfacesInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographOperatorInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographRegistryInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographTreasuryInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/Holographable.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographedERC1155.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographedERC20.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographedERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/HolographerInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/InitializableInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/LayerZeroEndpointInterface.sol::5 => import "./LayerZeroUserApplicationConfigInterface.sol";
contracts/interface/LayerZeroModuleInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/LayerZeroOverrides.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/Ownable.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/interface/PA1DInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/library/Base64.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/library/Base64.sol::105 => bytes private constant base64stdchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
contracts/library/Base64.sol::106 => bytes private constant base64urlchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_";
contracts/library/Bytes.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/library/ECDSA.sol::31 => revert("ECDSA: invalid signature 's' value");
contracts/library/ECDSA.sol::33 => revert("ECDSA: invalid signature 'v' value");
contracts/library/Strings.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/ERC20Mock.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/ERC20Mock.sol::109 => import "../interface/HolographERC20Interface.sol";
contracts/mock/ERC20Mock.sol::388 => // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
contracts/mock/LZEndpointMock.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/LZEndpointMock.sol::104 => import "../interface/LayerZeroReceiverInterface.sol";
contracts/mock/LZEndpointMock.sol::105 => import "../interface/LayerZeroEndpointInterface.sol";
contracts/mock/LZEndpointMock.sol::191 => require(lzEndpoint != address(0), "LayerZeroMock: destination LayerZero Endpoint not found");
contracts/mock/Mock.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/Mock.sol::106 => import "../interface/InitializableInterface.sol";
contracts/mock/MockERC721Receiver.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/MockERC721Receiver.sol::106 => import "../interface/ERC721TokenReceiver.sol";
contracts/mock/MockExternalCall.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/MockHolographChild.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/MockHolographGenesisChild.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/MockLZEndpoint.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/mock/MockLZEndpoint.sol::106 => import "../interface/LayerZeroOverrides.sol";
contracts/module/LayerZeroModule.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/module/LayerZeroModule.sol::109 => import "../interface/CrossChainMessageInterface.sol";
contracts/module/LayerZeroModule.sol::110 => import "../interface/HolographOperatorInterface.sol";
contracts/module/LayerZeroModule.sol::111 => import "../interface/InitializableInterface.sol";
contracts/module/LayerZeroModule.sol::112 => import "../interface/HolographInterfacesInterface.sol";
contracts/module/LayerZeroModule.sol::113 => import "../interface/LayerZeroModuleInterface.sol";
contracts/module/LayerZeroModule.sol::114 => import "../interface/LayerZeroOverrides.sol";
contracts/proxy/CxipERC721Proxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/proxy/CxipERC721Proxy.sol::107 => import "../interface/InitializableInterface.sol";
contracts/proxy/CxipERC721Proxy.sol::108 => import "../interface/HolographRegistryInterface.sol";
contracts/proxy/HolographBridgeProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/proxy/HolographBridgeProxy.sol::107 => import "../interface/InitializableInterface.sol";
contracts/proxy/HolographFactoryProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/proxy/HolographFactoryProxy.sol::107 => import "../interface/InitializableInterface.sol";
contracts/proxy/HolographOperatorProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/proxy/HolographOperatorProxy.sol::107 => import "../interface/InitializableInterface.sol";
contracts/proxy/HolographRegistryProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/proxy/HolographRegistryProxy.sol::107 => import "../interface/InitializableInterface.sol";
contracts/proxy/HolographTreasuryProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/proxy/HolographTreasuryProxy.sol::107 => import "../interface/InitializableInterface.sol";
contracts/struct/DeploymentConfig.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/struct/OperatorJob.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/struct/Verification.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/struct/ZoraBidShares.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/struct/ZoraDecimal.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/token/CxipERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/token/CxipERC721.sol::108 => import "../interface/HolographERC721Interface.sol";
contracts/token/CxipERC721.sol::109 => import "../interface/HolographInterfacesInterface.sol";
contracts/token/CxipERC721.sol::110 => import "../interface/HolographInterface.sol";
contracts/token/CxipERC721.sol::111 => import "../interface/HolographerInterface.sol";
contracts/token/HolographUtilityToken.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/token/HolographUtilityToken.sol::107 => import "../interface/HolographERC20Interface.sol";
contracts/token/HolographUtilityToken.sol::108 => import "../interface/HolographInterface.sol";
contracts/token/HolographUtilityToken.sol::109 => import "../interface/HolographerInterface.sol";
contracts/token/SampleERC20.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/token/SampleERC20.sol::106 => import "../interface/HolographERC20Interface.sol";
contracts/token/SampleERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/token/SampleERC721.sol::106 => import "../interface/HolographERC721Interface.sol";
contracts/token/hToken.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
contracts/token/hToken.sol::107 => import "../interface/HolographERC20Interface.sol";
contracts/token/hToken.sol::108 => import "../interface/HolographInterface.sol";
contracts/token/hToken.sol::109 => import "../interface/HolographerInterface.sol";
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: [G008](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)

#### Findings:
```
contracts/HolographOperator.sol::533 => ); // 80 next available bit position && so far 176 bits used with only 128 left
contracts/library/Base64.sol::116 => uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
contracts/library/ECDSA.sol::159 => // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)