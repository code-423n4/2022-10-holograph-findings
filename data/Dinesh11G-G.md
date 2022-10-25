### Don't Initialize Variables with Default Value

#### Impact
Issue Information: Uninitialized variables are assigned with the type's default value.

Explicitly initializing a variable with its default value costs unnecessary gas.

#### Findings:
```
2022-10-holograph/contracts/HolographBridge.sol::380 => uint256 fee = 0;
2022-10-holograph/contracts/HolographInterfaces.sol::235 => for (uint256 i = 0; i < uriTypes.length; i++) {
2022-10-holograph/contracts/HolographInterfaces.sol::264 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographInterfaces.sol::286 => for (uint256 i = 0; i < interfaceIds.length; i++) {
2022-10-holograph/contracts/HolographOperator.sol::310 => uint256 gasLimit = 0;
2022-10-holograph/contracts/HolographOperator.sol::311 => uint256 gasPrice = 0;
2022-10-holograph/contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::175 => for (uint256 i = 0; i < reservedTypes.length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::255 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::322 => for (uint256 i = 0; i < hashes.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/library/Base64.sol::119 => uint256 i = 0;
2022-10-holograph/contracts/library/Base64.sol::120 => uint256 j = 0;
2022-10-holograph/contracts/library/Base64.sol::130 => uint8 la1 = 0;
2022-10-holograph/contracts/library/Bytes.sol::159 => uint256 length = 0;
2022-10-holograph/contracts/library/Strings.sol::114 => uint256 length = 0;
2022-10-holograph/contracts/library/Strings.sol::136 => for (uint256 i = 0; i < 20; i++) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::251 => for (uint256 i = 0; i < msgs.length - 1; i++) {
2022-10-holograph/contracts/mock/Mock.sol::114 => bool shouldFail = false;
2022-10-holograph/src/HolographBridge.sol::281 => uint256 fee = 0;
2022-10-holograph/src/HolographInterfaces.sol::136 => for (uint256 i = 0; i < uriTypes.length; i++) {
2022-10-holograph/src/HolographInterfaces.sol::165 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographInterfaces.sol::187 => for (uint256 i = 0; i < interfaceIds.length; i++) {
2022-10-holograph/src/HolographOperator.sol::211 => uint256 gasLimit = 0;
2022-10-holograph/src/HolographOperator.sol::212 => uint256 gasPrice = 0;
2022-10-holograph/src/HolographOperator.sol::682 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographRegistry.sol::76 => for (uint256 i = 0; i < reservedTypes.length; i++) {
2022-10-holograph/src/HolographRegistry.sol::153 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographRegistry.sol::220 => for (uint256 i = 0; i < hashes.length; i++) {
2022-10-holograph/src/enforcer/HolographERC20.sol::465 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::258 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::432 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::448 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::465 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::617 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::208 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::224 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::241 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::257 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::295 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::315 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::333 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/src/enforcer/PA1D.sol::338 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::355 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::375 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/library/Base64.sol::20 => uint256 i = 0;
2022-10-holograph/src/library/Base64.sol::21 => uint256 j = 0;
2022-10-holograph/src/library/Base64.sol::31 => uint8 la1 = 0;
2022-10-holograph/src/library/Bytes.sol::60 => uint256 length = 0;
2022-10-holograph/src/library/Strings.sol::15 => uint256 length = 0;
2022-10-holograph/src/library/Strings.sol::37 => for (uint256 i = 0; i < 20; i++) {
2022-10-holograph/src/mock/LZEndpointMock.sol::152 => for (uint256 i = 0; i < msgs.length - 1; i++) {
2022-10-holograph/src/mock/Mock.sol::15 => bool shouldFail = false;
```
#### Tools used
Manual

### Cache Array Length Outside of Loop

#### Impact
Issue Information: Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

Example
🤦 Bad:

for (uint256 i = 0; i < array.length; i++) {
    // invariant: array's length is not changed
}
🚀 Good:

uint256 len = array.length
for (uint256 i = 0; i < len; i++) {
    // invariant: array's length is not changed
}

#### Findings:
```
2022-10-holograph/contracts/HolographInterfaces.sol::235 => for (uint256 i = 0; i < uriTypes.length; i++) {
2022-10-holograph/contracts/HolographInterfaces.sol::263 => uint256 length = fromChainType.length;
2022-10-holograph/contracts/HolographInterfaces.sol::264 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographInterfaces.sol::286 => for (uint256 i = 0; i < interfaceIds.length; i++) {
2022-10-holograph/contracts/HolographOperator.sol::316 => gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
2022-10-holograph/contracts/HolographOperator.sol::320 => gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
2022-10-holograph/contracts/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
2022-10-holograph/contracts/HolographOperator.sol::391 => _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
2022-10-holograph/contracts/HolographOperator.sol::408 => _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
2022-10-holograph/contracts/HolographOperator.sol::451 => calldatacopy(0, payload.offset, sub(payload.length, 0x20))
2022-10-holograph/contracts/HolographOperator.sol::461 => mload(sub(payload.length, 0x40)),
2022-10-holograph/contracts/HolographOperator.sol::469 => sub(payload.length, 0x40),
2022-10-holograph/contracts/HolographOperator.sol::503 => uint256 pod = random % _operatorPods.length;
2022-10-holograph/contracts/HolographOperator.sol::507 => uint256 podSize = _operatorPods[pod].length;
2022-10-holograph/contracts/HolographOperator.sol::551 => calldatacopy(0, bridgeInRequestPayload.offset, sub(bridgeInRequestPayload.length, 0x40))
2022-10-holograph/contracts/HolographOperator.sol::556 => let result := call(gas(), sload(_bridgeSlot), callvalue(), 0, sub(bridgeInRequestPayload.length, 0x40), 0, 0)
2022-10-holograph/contracts/HolographOperator.sol::718 => return _operatorPods.length;
2022-10-holograph/contracts/HolographOperator.sol::728 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/contracts/HolographOperator.sol::729 => return _operatorPods[pod - 1].length;
2022-10-holograph/contracts/HolographOperator.sol::739 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/contracts/HolographOperator.sol::745 => * @dev Use in conjunction with getPodOperatorsLength to know the total length of results
2022-10-holograph/contracts/HolographOperator.sol::748 => * @param length the length of result set to be (will be shorter if reached end of array)
2022-10-holograph/contracts/HolographOperator.sol::754 => uint256 length
2022-10-holograph/contracts/HolographOperator.sol::756 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/contracts/HolographOperator.sol::762 => * @dev get total length of pod operators
2022-10-holograph/contracts/HolographOperator.sol::764 => uint256 supply = _operatorPods[pod].length;
2022-10-holograph/contracts/HolographOperator.sol::766 => * @dev check if length is out of bounds for this result set
2022-10-holograph/contracts/HolographOperator.sol::768 => if (index + length > supply) {
2022-10-holograph/contracts/HolographOperator.sol::770 => * @dev adjust length to return remainder of the results
2022-10-holograph/contracts/HolographOperator.sol::772 => length = supply - index;
2022-10-holograph/contracts/HolographOperator.sol::777 => operators = new address[](length);
2022-10-holograph/contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographOperator.sol::867 => if (_operatorPods.length < pod) {
2022-10-holograph/contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/contracts/HolographOperator.sol::881 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
2022-10-holograph/contracts/HolographOperator.sol::883 => _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;
2022-10-holograph/contracts/HolographOperator.sol::1137 => uint256 lastIndex = _operatorPods[pod].length - 1;
2022-10-holograph/contracts/HolographOperator.sol::1150 => * @dev shorten array length
2022-10-holograph/contracts/HolographOperator.sol::1169 => if (pod >= _operatorPods.length) {
2022-10-holograph/contracts/HolographOperator.sol::1173 => uint256 position = _operatorPods[pod].length;
2022-10-holograph/contracts/HolographRegistry.sol::175 => for (uint256 i = 0; i < reservedTypes.length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::244 => * @notice Get set length list, starting from index, for all holographable contracts
2022-10-holograph/contracts/HolographRegistry.sol::246 => * @param length The length of returned results
2022-10-holograph/contracts/HolographRegistry.sol::247 => * @return contracts address[] Returns a set length array of holographable contracts deployed
2022-10-holograph/contracts/HolographRegistry.sol::249 => function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts) {
2022-10-holograph/contracts/HolographRegistry.sol::250 => uint256 supply = _holographableContracts.length;
2022-10-holograph/contracts/HolographRegistry.sol::251 => if (index + length > supply) {
2022-10-holograph/contracts/HolographRegistry.sol::252 => length = supply - index;
2022-10-holograph/contracts/HolographRegistry.sol::254 => contracts = new address[](length);
2022-10-holograph/contracts/HolographRegistry.sol::255 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::264 => return _holographableContracts.length;
2022-10-holograph/contracts/HolographRegistry.sol::322 => for (uint256 i = 0; i < hashes.length; i++) {
2022-10-holograph/contracts/abstract/Admin.sol::135 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/contracts/abstract/Admin.sol::136 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
2022-10-holograph/contracts/abstract/Owner.sol::149 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/contracts/abstract/Owner.sol::150 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::652 => if (reason.length == 0) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::664 => if (reason.length == 0) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::341 => * @notice Get set length list, starting from index, for tokens owned by wallet.
2022-10-holograph/contracts/enforcer/HolographERC721.sol::344 => * @param length The length of returned results.
2022-10-holograph/contracts/enforcer/HolographERC721.sol::345 => * @return tokenIds uint256[] Returns a set length array of token ids owned by wallet.
2022-10-holograph/contracts/enforcer/HolographERC721.sol::350 => uint256 length
2022-10-holograph/contracts/enforcer/HolographERC721.sol::353 => if (index + length > supply) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::354 => length = supply - index;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::356 => tokenIds = new uint256[](length);
2022-10-holograph/contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::528 => //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::543 => //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::544 => //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::560 => //     uint256 length
2022-10-holograph/contracts/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::700 => require(index < _allTokens.length, "ERC721: index out of bounds");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::705 => * @notice Get set length list, starting from index, for all tokens.
2022-10-holograph/contracts/enforcer/HolographERC721.sol::707 => * @param length The length of returned results.
2022-10-holograph/contracts/enforcer/HolographERC721.sol::708 => * @return tokenIds uint256[] Returns a set length array of token ids minted.
2022-10-holograph/contracts/enforcer/HolographERC721.sol::710 => function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::711 => uint256 supply = _allTokens.length;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::712 => if (index + length > supply) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::713 => length = supply - index;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::715 => tokenIds = new uint256[](length);
2022-10-holograph/contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::739 => return _allTokens.length;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::781 => _allTokensIndex[tokenId] = _allTokens.length;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::825 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-10-holograph/contracts/enforcer/PA1D.sol::301 => uint256 length;
2022-10-holograph/contracts/enforcer/PA1D.sol::303 => length := sload(slot)
2022-10-holograph/contracts/enforcer/PA1D.sol::305 => addresses = new address payable[](length);
2022-10-holograph/contracts/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::318 => uint256 length = addresses.length;
2022-10-holograph/contracts/enforcer/PA1D.sol::320 => sstore(slot, length)
2022-10-holograph/contracts/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::334 => uint256 length;
2022-10-holograph/contracts/enforcer/PA1D.sol::336 => length := sload(slot)
2022-10-holograph/contracts/enforcer/PA1D.sol::338 => bps = new uint256[](length);
2022-10-holograph/contracts/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::351 => uint256 length = bps.length;
2022-10-holograph/contracts/enforcer/PA1D.sol::353 => sstore(slot, length)
2022-10-holograph/contracts/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::385 => uint256 length = addresses.length;
2022-10-holograph/contracts/enforcer/PA1D.sol::388 => uint256 gasCost = (23300 * length) + length;
2022-10-holograph/contracts/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::408 => uint256 length = addresses.length;
2022-10-holograph/contracts/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::467 => * @dev Addresses and bps arrays must be equal length. Bps values added together must equal 10000 exactly.
2022-10-holograph/contracts/enforcer/PA1D.sol::472 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
2022-10-holograph/contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/interface/HolographOperatorInterface.sol::216 => * @dev Use in conjunction with getPodOperatorsLength to know the total length of results
2022-10-holograph/contracts/interface/HolographOperatorInterface.sol::219 => * @param length the length of result set to be (will be shorter if reached end of array)
2022-10-holograph/contracts/interface/HolographOperatorInterface.sol::225 => uint256 length
2022-10-holograph/contracts/interface/HolographRegistryInterface.sol::119 => function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts);
2022-10-holograph/contracts/interface/LayerZeroEndpointInterface.sol::10 => // @param _destination - the address on destination chain (in bytes). address length/format may vary by chains
2022-10-holograph/contracts/library/Base64.sol::114 => uint256 rem = _bs.length % 3;
2022-10-holograph/contracts/library/Base64.sol::116 => uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
2022-10-holograph/contracts/library/Base64.sol::117 => bytes memory res = new bytes(res_length);
2022-10-holograph/contracts/library/Base64.sol::122 => for (; i + 3 <= _bs.length; i += 3) {
2022-10-holograph/contracts/library/Base64.sol::129 => uint8 la0 = uint8(_bs[_bs.length - rem]);
2022-10-holograph/contracts/library/Base64.sol::133 => la1 = uint8(_bs[_bs.length - 1]);
2022-10-holograph/contracts/library/Bytes.sol::125 => uint256 _length
2022-10-holograph/contracts/library/Bytes.sol::127 => require(_length + 31 >= _length, "slice_overflow");
2022-10-holograph/contracts/library/Bytes.sol::128 => require(_bytes.length >= _start + _length, "slice_outOfBounds");
2022-10-holograph/contracts/library/Bytes.sol::131 => switch iszero(_length)
2022-10-holograph/contracts/library/Bytes.sol::134 => let lengthmod := and(_length, 31)
2022-10-holograph/contracts/library/Bytes.sol::135 => let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
2022-10-holograph/contracts/library/Bytes.sol::136 => let end := add(mc, _length)
2022-10-holograph/contracts/library/Bytes.sol::138 => let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
2022-10-holograph/contracts/library/Bytes.sol::145 => mstore(tempBytes, _length)
2022-10-holograph/contracts/library/Bytes.sol::159 => uint256 length = 0;
2022-10-holograph/contracts/library/Bytes.sol::161 => length++;
2022-10-holograph/contracts/library/Bytes.sol::164 => return slice(abi.encodePacked(source), 32 - length, length);
2022-10-holograph/contracts/library/ECDSA.sol::29 => revert("ECDSA: invalid signature length");
2022-10-holograph/contracts/library/ECDSA.sol::58 => // Check the signature length
2022-10-holograph/contracts/library/ECDSA.sol::61 => if (signature.length == 65) {
2022-10-holograph/contracts/library/ECDSA.sol::73 => } else if (signature.length == 64) {
2022-10-holograph/contracts/library/ECDSA.sol::201 => // 32 is the length in bytes of hash,
2022-10-holograph/contracts/library/ECDSA.sol::215 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
2022-10-holograph/contracts/library/Strings.sol::114 => uint256 length = 0;
2022-10-holograph/contracts/library/Strings.sol::116 => length++;
2022-10-holograph/contracts/library/Strings.sol::119 => return toHexString(value, length);
2022-10-holograph/contracts/library/Strings.sol::122 => function toHexString(uint256 value, uint256 length) internal pure returns (string memory) {
2022-10-holograph/contracts/library/Strings.sol::123 => bytes memory buffer = new bytes(2 * length + 2);
2022-10-holograph/contracts/library/Strings.sol::126 => for (uint256 i = 2 * length + 1; i > 1; --i) {
2022-10-holograph/contracts/library/Strings.sol::130 => require(value == 0, "Strings: hex length insufficient");
2022-10-holograph/contracts/mock/ERC20Mock.sol::504 => if (reason.length == 0) {
2022-10-holograph/contracts/mock/ERC20Mock.sol::516 => if (reason.length == 0) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::246 => if (msgs.length > 0) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::251 => for (uint256 i = 0; i < msgs.length - 1; i++) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::262 => uint64(_payload.length),
2022-10-holograph/contracts/mock/LZEndpointMock.sol::282 => return msgsToDeliver[_srcChainId][_srcAddress].length;
2022-10-holograph/contracts/mock/LZEndpointMock.sol::307 => calldatacopy(ptr, sub(_b.offset, 2), add(_b.length, 2))
2022-10-holograph/contracts/mock/LZEndpointMock.sol::368 => while (msgs.length > 0) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::369 => QueuedPayload memory payload = msgs[msgs.length - 1];
2022-10-holograph/contracts/mock/LZEndpointMock.sol::404 => require(_payload.length == sp.payloadLength && keccak256(_payload) == sp.payloadHash, "LayerZero: invalid payload");
2022-10-holograph/contracts/mock/Mock.sol::145 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/contracts/mock/Mock.sol::146 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
2022-10-holograph/contracts/mock/Mock.sol::160 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/contracts/mock/Mock.sol::161 => let result := staticcall(gas(), target, 0, data.length, 0, 0)
2022-10-holograph/contracts/mock/Mock.sol::175 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/contracts/mock/Mock.sol::176 => let result := delegatecall(gas(), target, 0, data.length, 0, 0)
2022-10-holograph/contracts/module/LayerZeroModule.sol::202 => calldatacopy(add(ptr, 0x0c), _srcAddress.offset, _srcAddress.length)
2022-10-holograph/contracts/module/LayerZeroModule.sol::247 => abi.encodePacked(uint16(1), uint256(_baseGas() + (crossChainPayload.length * _gasPerByte())))
2022-10-holograph/contracts/module/LayerZeroModule.sol::271 => uint256(_baseGas() + (crossChainPayload.length * _gasPerByte()))
2022-10-holograph/src/HolographInterfaces.sol::136 => for (uint256 i = 0; i < uriTypes.length; i++) {
2022-10-holograph/src/HolographInterfaces.sol::164 => uint256 length = fromChainType.length;
2022-10-holograph/src/HolographInterfaces.sol::165 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographInterfaces.sol::187 => for (uint256 i = 0; i < interfaceIds.length; i++) {
2022-10-holograph/src/HolographOperator.sol::217 => gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
2022-10-holograph/src/HolographOperator.sol::221 => gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
2022-10-holograph/src/HolographOperator.sol::264 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
2022-10-holograph/src/HolographOperator.sol::292 => _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
2022-10-holograph/src/HolographOperator.sol::309 => _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
2022-10-holograph/src/HolographOperator.sol::352 => calldatacopy(0, payload.offset, sub(payload.length, 0x20))
2022-10-holograph/src/HolographOperator.sol::362 => mload(sub(payload.length, 0x40)),
2022-10-holograph/src/HolographOperator.sol::370 => sub(payload.length, 0x40),
2022-10-holograph/src/HolographOperator.sol::404 => uint256 pod = random % _operatorPods.length;
2022-10-holograph/src/HolographOperator.sol::408 => uint256 podSize = _operatorPods[pod].length;
2022-10-holograph/src/HolographOperator.sol::452 => calldatacopy(0, bridgeInRequestPayload.offset, sub(bridgeInRequestPayload.length, 0x40))
2022-10-holograph/src/HolographOperator.sol::457 => let result := call(gas(), sload(_bridgeSlot), callvalue(), 0, sub(bridgeInRequestPayload.length, 0x40), 0, 0)
2022-10-holograph/src/HolographOperator.sol::619 => return _operatorPods.length;
2022-10-holograph/src/HolographOperator.sol::629 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/src/HolographOperator.sol::630 => return _operatorPods[pod - 1].length;
2022-10-holograph/src/HolographOperator.sol::640 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/src/HolographOperator.sol::646 => * @dev Use in conjunction with getPodOperatorsLength to know the total length of results
2022-10-holograph/src/HolographOperator.sol::649 => * @param length the length of result set to be (will be shorter if reached end of array)
2022-10-holograph/src/HolographOperator.sol::655 => uint256 length
2022-10-holograph/src/HolographOperator.sol::657 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/src/HolographOperator.sol::663 => * @dev get total length of pod operators
2022-10-holograph/src/HolographOperator.sol::665 => uint256 supply = _operatorPods[pod].length;
2022-10-holograph/src/HolographOperator.sol::667 => * @dev check if length is out of bounds for this result set
2022-10-holograph/src/HolographOperator.sol::669 => if (index + length > supply) {
2022-10-holograph/src/HolographOperator.sol::671 => * @dev adjust length to return remainder of the results
2022-10-holograph/src/HolographOperator.sol::673 => length = supply - index;
2022-10-holograph/src/HolographOperator.sol::678 => operators = new address[](length);
2022-10-holograph/src/HolographOperator.sol::682 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographOperator.sol::768 => if (_operatorPods.length < pod) {
2022-10-holograph/src/HolographOperator.sol::772 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/src/HolographOperator.sol::782 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
2022-10-holograph/src/HolographOperator.sol::784 => _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;
2022-10-holograph/src/HolographOperator.sol::1038 => uint256 lastIndex = _operatorPods[pod].length - 1;
2022-10-holograph/src/HolographOperator.sol::1051 => * @dev shorten array length
2022-10-holograph/src/HolographOperator.sol::1070 => if (pod >= _operatorPods.length) {
2022-10-holograph/src/HolographOperator.sol::1074 => uint256 position = _operatorPods[pod].length;
2022-10-holograph/src/HolographRegistry.sol::76 => for (uint256 i = 0; i < reservedTypes.length; i++) {
2022-10-holograph/src/HolographRegistry.sol::142 => * @notice Get set length list, starting from index, for all holographable contracts
2022-10-holograph/src/HolographRegistry.sol::144 => * @param length The length of returned results
2022-10-holograph/src/HolographRegistry.sol::145 => * @return contracts address[] Returns a set length array of holographable contracts deployed
2022-10-holograph/src/HolographRegistry.sol::147 => function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts) {
2022-10-holograph/src/HolographRegistry.sol::148 => uint256 supply = _holographableContracts.length;
2022-10-holograph/src/HolographRegistry.sol::149 => if (index + length > supply) {
2022-10-holograph/src/HolographRegistry.sol::150 => length = supply - index;
2022-10-holograph/src/HolographRegistry.sol::152 => contracts = new address[](length);
2022-10-holograph/src/HolographRegistry.sol::153 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographRegistry.sol::162 => return _holographableContracts.length;
2022-10-holograph/src/HolographRegistry.sol::220 => for (uint256 i = 0; i < hashes.length; i++) {
2022-10-holograph/src/abstract/Admin.sol::36 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/src/abstract/Admin.sol::37 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
2022-10-holograph/src/abstract/Owner.sol::50 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/src/abstract/Owner.sol::51 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
2022-10-holograph/src/enforcer/HolographERC20.sol::465 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/src/enforcer/HolographERC20.sol::553 => if (reason.length == 0) {
2022-10-holograph/src/enforcer/HolographERC20.sol::565 => if (reason.length == 0) {
2022-10-holograph/src/enforcer/HolographERC721.sol::242 => * @notice Get set length list, starting from index, for tokens owned by wallet.
2022-10-holograph/src/enforcer/HolographERC721.sol::245 => * @param length The length of returned results.
2022-10-holograph/src/enforcer/HolographERC721.sol::246 => * @return tokenIds uint256[] Returns a set length array of token ids owned by wallet.
2022-10-holograph/src/enforcer/HolographERC721.sol::251 => uint256 length
2022-10-holograph/src/enforcer/HolographERC721.sol::254 => if (index + length > supply) {
2022-10-holograph/src/enforcer/HolographERC721.sol::255 => length = supply - index;
2022-10-holograph/src/enforcer/HolographERC721.sol::257 => tokenIds = new uint256[](length);
2022-10-holograph/src/enforcer/HolographERC721.sol::258 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::429 => //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
2022-10-holograph/src/enforcer/HolographERC721.sol::432 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::444 => //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
2022-10-holograph/src/enforcer/HolographERC721.sol::445 => //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
2022-10-holograph/src/enforcer/HolographERC721.sol::448 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::461 => //     uint256 length
2022-10-holograph/src/enforcer/HolographERC721.sol::465 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::601 => require(index < _allTokens.length, "ERC721: index out of bounds");
2022-10-holograph/src/enforcer/HolographERC721.sol::606 => * @notice Get set length list, starting from index, for all tokens.
2022-10-holograph/src/enforcer/HolographERC721.sol::608 => * @param length The length of returned results.
2022-10-holograph/src/enforcer/HolographERC721.sol::609 => * @return tokenIds uint256[] Returns a set length array of token ids minted.
2022-10-holograph/src/enforcer/HolographERC721.sol::611 => function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {
2022-10-holograph/src/enforcer/HolographERC721.sol::612 => uint256 supply = _allTokens.length;
2022-10-holograph/src/enforcer/HolographERC721.sol::613 => if (index + length > supply) {
2022-10-holograph/src/enforcer/HolographERC721.sol::614 => length = supply - index;
2022-10-holograph/src/enforcer/HolographERC721.sol::616 => tokenIds = new uint256[](length);
2022-10-holograph/src/enforcer/HolographERC721.sol::617 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::640 => return _allTokens.length;
2022-10-holograph/src/enforcer/HolographERC721.sol::682 => _allTokensIndex[tokenId] = _allTokens.length;
2022-10-holograph/src/enforcer/HolographERC721.sol::726 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-10-holograph/src/enforcer/PA1D.sol::202 => uint256 length;
2022-10-holograph/src/enforcer/PA1D.sol::204 => length := sload(slot)
2022-10-holograph/src/enforcer/PA1D.sol::206 => addresses = new address payable[](length);
2022-10-holograph/src/enforcer/PA1D.sol::208 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::219 => uint256 length = addresses.length;
2022-10-holograph/src/enforcer/PA1D.sol::221 => sstore(slot, length)
2022-10-holograph/src/enforcer/PA1D.sol::224 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::235 => uint256 length;
2022-10-holograph/src/enforcer/PA1D.sol::237 => length := sload(slot)
2022-10-holograph/src/enforcer/PA1D.sol::239 => bps = new uint256[](length);
2022-10-holograph/src/enforcer/PA1D.sol::241 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::252 => uint256 length = bps.length;
2022-10-holograph/src/enforcer/PA1D.sol::254 => sstore(slot, length)
2022-10-holograph/src/enforcer/PA1D.sol::257 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::286 => uint256 length = addresses.length;
2022-10-holograph/src/enforcer/PA1D.sol::289 => uint256 gasCost = (23300 * length) + length;
2022-10-holograph/src/enforcer/PA1D.sol::295 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::309 => uint256 length = addresses.length;
2022-10-holograph/src/enforcer/PA1D.sol::315 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::333 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/src/enforcer/PA1D.sol::338 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::355 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::368 => * @dev Addresses and bps arrays must be equal length. Bps values added together must equal 10000 exactly.
2022-10-holograph/src/enforcer/PA1D.sol::373 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
2022-10-holograph/src/enforcer/PA1D.sol::375 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/interface/HolographOperatorInterface.sol::117 => * @dev Use in conjunction with getPodOperatorsLength to know the total length of results
2022-10-holograph/src/interface/HolographOperatorInterface.sol::120 => * @param length the length of result set to be (will be shorter if reached end of array)
2022-10-holograph/src/interface/HolographOperatorInterface.sol::126 => uint256 length
2022-10-holograph/src/interface/HolographRegistryInterface.sol::20 => function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts);
2022-10-holograph/src/interface/LayerZeroEndpointInterface.sol::10 => // @param _destination - the address on destination chain (in bytes). address length/format may vary by chains
2022-10-holograph/src/library/Base64.sol::15 => uint256 rem = _bs.length % 3;
2022-10-holograph/src/library/Base64.sol::17 => uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
2022-10-holograph/src/library/Base64.sol::18 => bytes memory res = new bytes(res_length);
2022-10-holograph/src/library/Base64.sol::23 => for (; i + 3 <= _bs.length; i += 3) {
2022-10-holograph/src/library/Base64.sol::30 => uint8 la0 = uint8(_bs[_bs.length - rem]);
2022-10-holograph/src/library/Base64.sol::34 => la1 = uint8(_bs[_bs.length - 1]);
2022-10-holograph/src/library/Bytes.sol::26 => uint256 _length
2022-10-holograph/src/library/Bytes.sol::28 => require(_length + 31 >= _length, "slice_overflow");
2022-10-holograph/src/library/Bytes.sol::29 => require(_bytes.length >= _start + _length, "slice_outOfBounds");
2022-10-holograph/src/library/Bytes.sol::32 => switch iszero(_length)
2022-10-holograph/src/library/Bytes.sol::35 => let lengthmod := and(_length, 31)
2022-10-holograph/src/library/Bytes.sol::36 => let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
2022-10-holograph/src/library/Bytes.sol::37 => let end := add(mc, _length)
2022-10-holograph/src/library/Bytes.sol::39 => let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
2022-10-holograph/src/library/Bytes.sol::46 => mstore(tempBytes, _length)
2022-10-holograph/src/library/Bytes.sol::60 => uint256 length = 0;
2022-10-holograph/src/library/Bytes.sol::62 => length++;
2022-10-holograph/src/library/Bytes.sol::65 => return slice(abi.encodePacked(source), 32 - length, length);
2022-10-holograph/src/library/ECDSA.sol::29 => revert("ECDSA: invalid signature length");
2022-10-holograph/src/library/ECDSA.sol::58 => // Check the signature length
2022-10-holograph/src/library/ECDSA.sol::61 => if (signature.length == 65) {
2022-10-holograph/src/library/ECDSA.sol::73 => } else if (signature.length == 64) {
2022-10-holograph/src/library/ECDSA.sol::201 => // 32 is the length in bytes of hash,
2022-10-holograph/src/library/ECDSA.sol::215 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
2022-10-holograph/src/library/Strings.sol::15 => uint256 length = 0;
2022-10-holograph/src/library/Strings.sol::17 => length++;
2022-10-holograph/src/library/Strings.sol::20 => return toHexString(value, length);
2022-10-holograph/src/library/Strings.sol::23 => function toHexString(uint256 value, uint256 length) internal pure returns (string memory) {
2022-10-holograph/src/library/Strings.sol::24 => bytes memory buffer = new bytes(2 * length + 2);
2022-10-holograph/src/library/Strings.sol::27 => for (uint256 i = 2 * length + 1; i > 1; --i) {
2022-10-holograph/src/library/Strings.sol::31 => require(value == 0, "Strings: hex length insufficient");
2022-10-holograph/src/mock/ERC20Mock.sol::405 => if (reason.length == 0) {
2022-10-holograph/src/mock/ERC20Mock.sol::417 => if (reason.length == 0) {
2022-10-holograph/src/mock/LZEndpointMock.sol::147 => if (msgs.length > 0) {
2022-10-holograph/src/mock/LZEndpointMock.sol::152 => for (uint256 i = 0; i < msgs.length - 1; i++) {
2022-10-holograph/src/mock/LZEndpointMock.sol::163 => uint64(_payload.length),
2022-10-holograph/src/mock/LZEndpointMock.sol::183 => return msgsToDeliver[_srcChainId][_srcAddress].length;
2022-10-holograph/src/mock/LZEndpointMock.sol::208 => calldatacopy(ptr, sub(_b.offset, 2), add(_b.length, 2))
2022-10-holograph/src/mock/LZEndpointMock.sol::269 => while (msgs.length > 0) {
2022-10-holograph/src/mock/LZEndpointMock.sol::270 => QueuedPayload memory payload = msgs[msgs.length - 1];
2022-10-holograph/src/mock/LZEndpointMock.sol::305 => require(_payload.length == sp.payloadLength && keccak256(_payload) == sp.payloadHash, "LayerZero: invalid payload");
2022-10-holograph/src/mock/Mock.sol::46 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/src/mock/Mock.sol::47 => let result := call(gas(), target, callvalue(), 0, data.length, 0, 0)
2022-10-holograph/src/mock/Mock.sol::61 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/src/mock/Mock.sol::62 => let result := staticcall(gas(), target, 0, data.length, 0, 0)
2022-10-holograph/src/mock/Mock.sol::76 => calldatacopy(0, data.offset, data.length)
2022-10-holograph/src/mock/Mock.sol::77 => let result := delegatecall(gas(), target, 0, data.length, 0, 0)
2022-10-holograph/src/module/LayerZeroModule.sol::103 => calldatacopy(add(ptr, 0x0c), _srcAddress.offset, _srcAddress.length)
2022-10-holograph/src/module/LayerZeroModule.sol::148 => abi.encodePacked(uint16(1), uint256(_baseGas() + (crossChainPayload.length * _gasPerByte())))
2022-10-holograph/src/module/LayerZeroModule.sol::172 => uint256(_baseGas() + (crossChainPayload.length * _gasPerByte()))
```
#### Tools used
Manual


### Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
2022-10-holograph/contracts/HolographBridge.sol::218 => if (hTokenValue > 0) {
2022-10-holograph/contracts/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
2022-10-holograph/contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
2022-10-holograph/contracts/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
2022-10-holograph/contracts/HolographOperator.sol::398 => if (leftovers > 0) {
2022-10-holograph/contracts/HolographOperator.sol::1126 => if (operatorIndex > 0) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
2022-10-holograph/contracts/interface/ERC721.sol::46 => ///  checks if `_to` is a smart contract (code size > 0). If so, it calls
2022-10-holograph/contracts/library/ECDSA.sol::161 => if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::246 => if (msgs.length > 0) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::368 => while (msgs.length > 0) {
2022-10-holograph/contracts/token/hToken.sol::176 => require(msg.value > 0, "hToken: no value received");
2022-10-holograph/src/HolographBridge.sol::119 => if (hTokenValue > 0) {
2022-10-holograph/src/HolographOperator.sol::210 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
2022-10-holograph/src/HolographOperator.sol::251 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
2022-10-holograph/src/HolographOperator.sol::264 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
2022-10-holograph/src/HolographOperator.sol::299 => if (leftovers > 0) {
2022-10-holograph/src/HolographOperator.sol::1027 => if (operatorIndex > 0) {
2022-10-holograph/src/enforcer/HolographERC721.sol::716 => require(tokenId > 0, "ERC721: token id cannot be zero");
2022-10-holograph/src/interface/ERC721.sol::46 => ///  checks if `_to` is a smart contract (code size > 0). If so, it calls
2022-10-holograph/src/library/ECDSA.sol::161 => if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
2022-10-holograph/src/mock/LZEndpointMock.sol::147 => if (msgs.length > 0) {
2022-10-holograph/src/mock/LZEndpointMock.sol::269 => while (msgs.length > 0) {
2022-10-holograph/src/token/hToken.sol::77 => require(msg.value > 0, "hToken: no value received");
```
#### Tools used
Manual



### Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: ⚡️ Only valid for solidity versions <0.6.12 ⚡️

Access roles marked as constant results in computing the keccak256 operation each time the variable is used because assigned operations for constant variables are re-evaluated every time.

Changing the variables to immutable results in computing the hash only once on deployment, leading to gas savings.

Example
🤦 Bad:

bytes32 public constant GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");
🚀 Good:

bytes32 public immutable GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");

#### Findings:
```
2022-10-holograph/contracts/Holograph.sol::118 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/contracts/Holograph.sol::122 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.chainId')) - 1)
2022-10-holograph/contracts/Holograph.sol::126 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
2022-10-holograph/contracts/Holograph.sol::130 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographChainId')) - 1)
2022-10-holograph/contracts/Holograph.sol::134 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
2022-10-holograph/contracts/Holograph.sol::138 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/Holograph.sol::142 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/Holograph.sol::146 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.treasury')) - 1)
2022-10-holograph/contracts/Holograph.sol::150 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
2022-10-holograph/contracts/HolographBridge.sol::124 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
2022-10-holograph/contracts/HolographBridge.sol::128 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/HolographBridge.sol::132 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.jobNonce')) - 1)
2022-10-holograph/contracts/HolographBridge.sol::136 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/HolographBridge.sol::140 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/HolographFactory.sol::125 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/HolographFactory.sol::129 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/HolographFactory.sol::206 => bytes32 hash = keccak256(
2022-10-holograph/contracts/HolographFactory.sol::211 => keccak256(config.byteCode),
2022-10-holograph/contracts/HolographFactory.sol::212 => keccak256(config.initCode),
2022-10-holograph/contracts/HolographFactory.sol::226 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), hash, keccak256(holographerBytecode)))))
2022-10-holograph/contracts/HolographFactory.sol::333 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
2022-10-holograph/contracts/HolographGenesis.sol::133 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, keccak256(sourceCode)))))
2022-10-holograph/contracts/HolographOperator.sol::127 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::131 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::135 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::139 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.jobNonce')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::143 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.messagingModule')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::147 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::151 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
2022-10-holograph/contracts/HolographOperator.sol::305 => bytes32 hash = keccak256(bridgeInRequestPayload);
2022-10-holograph/contracts/HolographOperator.sol::491 => bytes32 jobHash = keccak256(bridgeInRequestPayload);
2022-10-holograph/contracts/HolographOperator.sol::499 => uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
2022-10-holograph/contracts/HolographOperator.sol::651 => emit CrossChainMessageSent(keccak256(encodedData));
2022-10-holograph/contracts/HolographOperator.sol::686 => * @dev The job hash is a keccak256 hash of the entire job payload
2022-10-holograph/contracts/HolographOperator.sol::687 => * @param jobHash keccak256 hash of the job
2022-10-holograph/contracts/HolographRegistry.sol::119 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/HolographRegistry.sol::123 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
2022-10-holograph/contracts/HolographTreasury.sol::127 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/contracts/HolographTreasury.sol::131 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/HolographTreasury.sol::135 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/HolographTreasury.sol::139 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/abstract/Admin.sol::106 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.admin')) - 1)
2022-10-holograph/contracts/abstract/EIP712.sol::13 => * they need in their contracts using a combination of `abi.encode` and `keccak256`.
2022-10-holograph/contracts/abstract/EIP712.sol::61 => bytes32 hashedName = keccak256(bytes(name));
2022-10-holograph/contracts/abstract/EIP712.sol::62 => bytes32 hashedVersion = keccak256(bytes(version));
2022-10-holograph/contracts/abstract/EIP712.sol::63 => bytes32 typeHash = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");
2022-10-holograph/contracts/abstract/EIP712.sol::88 => return keccak256(abi.encode(typeHash, nameHash, versionHash, block.chainid, address(this)));
2022-10-holograph/contracts/abstract/EIP712.sol::98 => * bytes32 digest = _hashTypedDataV4(keccak256(abi.encode(
2022-10-holograph/contracts/abstract/EIP712.sol::99 => *     keccak256("Mail(address to,string contents)"),
2022-10-holograph/contracts/abstract/EIP712.sol::101 => *     keccak256(bytes(mailContents))
2022-10-holograph/contracts/abstract/ERC1155H.sol::108 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
2022-10-holograph/contracts/abstract/ERC1155H.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/contracts/abstract/ERC20H.sol::108 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
2022-10-holograph/contracts/abstract/ERC20H.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/contracts/abstract/ERC721H.sol::108 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
2022-10-holograph/contracts/abstract/ERC721H.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/contracts/abstract/Initializable.sol::114 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.initialized')) - 1)
2022-10-holograph/contracts/abstract/NonReentrant.sol::106 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.reentrant')) - 1)
2022-10-holograph/contracts/abstract/Owner.sol::106 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/contracts/enforcer/HolographERC20.sol::140 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/enforcer/HolographERC20.sol::144 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
2022-10-holograph/contracts/enforcer/HolographERC20.sol::470 => bytes32 structHash = keccak256(
2022-10-holograph/contracts/enforcer/HolographERC721.sol::134 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/enforcer/HolographERC721.sol::138 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
2022-10-holograph/contracts/enforcer/Holographer.sol::117 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.originChain')) - 1)
2022-10-holograph/contracts/enforcer/Holographer.sol::121 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/contracts/enforcer/Holographer.sol::125 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.contractType')) - 1)
2022-10-holograph/contracts/enforcer/Holographer.sol::129 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
2022-10-holograph/contracts/enforcer/Holographer.sol::133 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.blockHeight')) - 1)
2022-10-holograph/contracts/enforcer/PA1D.sol::122 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.defaultBp')) - 1)
2022-10-holograph/contracts/enforcer/PA1D.sol::126 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.defaultReceiver')) - 1)
2022-10-holograph/contracts/enforcer/PA1D.sol::130 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.initialized')) - 1)
2022-10-holograph/contracts/enforcer/PA1D.sol::134 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.payout.addresses')) - 1)
2022-10-holograph/contracts/enforcer/PA1D.sol::138 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.payout.bps')) - 1)
2022-10-holograph/contracts/enforcer/PA1D.sol::257 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
2022-10-holograph/contracts/enforcer/PA1D.sol::269 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
2022-10-holograph/contracts/enforcer/PA1D.sol::280 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
2022-10-holograph/contracts/enforcer/PA1D.sol::292 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
2022-10-holograph/contracts/enforcer/PA1D.sol::308 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/contracts/enforcer/PA1D.sol::324 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/contracts/enforcer/PA1D.sol::341 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/contracts/enforcer/PA1D.sol::357 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/contracts/enforcer/PA1D.sol::366 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
2022-10-holograph/contracts/enforcer/PA1D.sol::373 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
2022-10-holograph/contracts/interface/ERC721.sol::48 => ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
2022-10-holograph/contracts/interface/ERC721TokenReceiver.sol::116 => /// @return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
2022-10-holograph/contracts/interface/HolographOperatorInterface.sol::186 => * @dev The job hash is a keccak256 hash of the entire job payload
2022-10-holograph/contracts/interface/HolographOperatorInterface.sol::187 => * @param jobHash keccak256 hash of the job
2022-10-holograph/contracts/library/ECDSA.sol::203 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
2022-10-holograph/contracts/library/ECDSA.sol::215 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
2022-10-holograph/contracts/library/ECDSA.sol::228 => return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
2022-10-holograph/contracts/mock/ERC20Mock.sol::226 => // bytes4(keccak256(abi.encodePacked('safeTransfer(address,uint256)'))) == 0x423f6cef
2022-10-holograph/contracts/mock/ERC20Mock.sol::228 => // bytes4(keccak256(abi.encodePacked('safeTransfer(address,uint256,bytes)'))) == 0xeb795549
2022-10-holograph/contracts/mock/ERC20Mock.sol::230 => // bytes4(keccak256(abi.encodePacked('safeTransferFrom(address,address,uint256)'))) == 0x42842e0e
2022-10-holograph/contracts/mock/ERC20Mock.sol::232 => // bytes4(keccak256(abi.encodePacked('safeTransferFrom(address,address,uint256,bytes)'))) == 0xb88d4fde
2022-10-holograph/contracts/mock/ERC20Mock.sol::388 => // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
2022-10-holograph/contracts/mock/ERC20Mock.sol::390 => bytes32 structHash = keccak256(
2022-10-holograph/contracts/mock/LZEndpointMock.sol::264 => keccak256(_payload)
2022-10-holograph/contracts/mock/LZEndpointMock.sol::404 => require(_payload.length == sp.payloadLength && keccak256(_payload) == sp.payloadHash, "LayerZero: invalid payload");
2022-10-holograph/contracts/module/LayerZeroModule.sol::124 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/contracts/module/LayerZeroModule.sol::128 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
2022-10-holograph/contracts/module/LayerZeroModule.sol::132 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.lZEndpoint')) - 1)
2022-10-holograph/contracts/module/LayerZeroModule.sol::136 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/module/LayerZeroModule.sol::140 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/module/LayerZeroModule.sol::144 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/proxy/CxipERC721Proxy.sol::112 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.contractType')) - 1)
2022-10-holograph/contracts/proxy/CxipERC721Proxy.sol::116 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/proxy/HolographBridgeProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/contracts/proxy/HolographFactoryProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
2022-10-holograph/contracts/proxy/HolographOperatorProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/contracts/proxy/HolographRegistryProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/contracts/proxy/HolographTreasuryProxy.sol::111 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.treasury')) - 1)
2022-10-holograph/contracts/token/SampleERC20.sol::149 => _walletSalts[to] = keccak256(
2022-10-holograph/src/Holograph.sol::19 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/src/Holograph.sol::23 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.chainId')) - 1)
2022-10-holograph/src/Holograph.sol::27 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
2022-10-holograph/src/Holograph.sol::31 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographChainId')) - 1)
2022-10-holograph/src/Holograph.sol::35 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
2022-10-holograph/src/Holograph.sol::39 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/Holograph.sol::43 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/Holograph.sol::47 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.treasury')) - 1)
2022-10-holograph/src/Holograph.sol::51 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
2022-10-holograph/src/HolographBridge.sol::25 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
2022-10-holograph/src/HolographBridge.sol::29 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/HolographBridge.sol::33 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.jobNonce')) - 1)
2022-10-holograph/src/HolographBridge.sol::37 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/HolographBridge.sol::41 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/HolographFactory.sol::26 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/HolographFactory.sol::30 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/HolographFactory.sol::107 => bytes32 hash = keccak256(
2022-10-holograph/src/HolographFactory.sol::112 => keccak256(config.byteCode),
2022-10-holograph/src/HolographFactory.sol::113 => keccak256(config.initCode),
2022-10-holograph/src/HolographFactory.sol::127 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), hash, keccak256(holographerBytecode)))))
2022-10-holograph/src/HolographFactory.sol::215 => return (codehash != 0x0 && codehash != precomputekeccak256(""));
2022-10-holograph/src/HolographFactory.sol::234 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
2022-10-holograph/src/HolographGenesis.sol::34 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, keccak256(sourceCode)))))
2022-10-holograph/src/HolographOperator.sol::28 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/src/HolographOperator.sol::32 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/HolographOperator.sol::36 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
2022-10-holograph/src/HolographOperator.sol::40 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.jobNonce')) - 1)
2022-10-holograph/src/HolographOperator.sol::44 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.messagingModule')) - 1)
2022-10-holograph/src/HolographOperator.sol::48 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/HolographOperator.sol::52 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
2022-10-holograph/src/HolographOperator.sol::206 => bytes32 hash = keccak256(bridgeInRequestPayload);
2022-10-holograph/src/HolographOperator.sol::392 => bytes32 jobHash = keccak256(bridgeInRequestPayload);
2022-10-holograph/src/HolographOperator.sol::400 => uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
2022-10-holograph/src/HolographOperator.sol::552 => emit CrossChainMessageSent(keccak256(encodedData));
2022-10-holograph/src/HolographOperator.sol::587 => * @dev The job hash is a keccak256 hash of the entire job payload
2022-10-holograph/src/HolographOperator.sol::588 => * @param jobHash keccak256 hash of the job
2022-10-holograph/src/HolographOperator.sol::1104 => return (codehash != 0x0 && codehash != precomputekeccak256(""));
2022-10-holograph/src/HolographRegistry.sol::20 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/HolographRegistry.sol::24 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.utilityToken')) - 1)
2022-10-holograph/src/HolographRegistry.sol::99 => require((contractType != 0x0 && contractType != precomputekeccak256("")), "HOLOGRAPH: empty contract");
2022-10-holograph/src/HolographTreasury.sol::28 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/src/HolographTreasury.sol::32 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/HolographTreasury.sol::36 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/HolographTreasury.sol::40 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/abstract/Admin.sol::7 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.admin')) - 1)
2022-10-holograph/src/abstract/EIP712.sol::13 => * they need in their contracts using a combination of `abi.encode` and `keccak256`.
2022-10-holograph/src/abstract/EIP712.sol::61 => bytes32 hashedName = keccak256(bytes(name));
2022-10-holograph/src/abstract/EIP712.sol::62 => bytes32 hashedVersion = keccak256(bytes(version));
2022-10-holograph/src/abstract/EIP712.sol::63 => bytes32 typeHash = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");
2022-10-holograph/src/abstract/EIP712.sol::88 => return keccak256(abi.encode(typeHash, nameHash, versionHash, block.chainid, address(this)));
2022-10-holograph/src/abstract/EIP712.sol::98 => * bytes32 digest = _hashTypedDataV4(keccak256(abi.encode(
2022-10-holograph/src/abstract/EIP712.sol::99 => *     keccak256("Mail(address to,string contents)"),
2022-10-holograph/src/abstract/EIP712.sol::101 => *     keccak256(bytes(mailContents))
2022-10-holograph/src/abstract/ERC1155H.sol::9 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
2022-10-holograph/src/abstract/ERC1155H.sol::13 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/src/abstract/ERC20H.sol::9 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
2022-10-holograph/src/abstract/ERC20H.sol::13 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/src/abstract/ERC721H.sol::9 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
2022-10-holograph/src/abstract/ERC721H.sol::13 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/src/abstract/Initializable.sol::15 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.initialized')) - 1)
2022-10-holograph/src/abstract/NonReentrant.sol::7 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.reentrant')) - 1)
2022-10-holograph/src/abstract/Owner.sol::7 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.owner')) - 1)
2022-10-holograph/src/enforcer/HolographERC20.sol::41 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/enforcer/HolographERC20.sol::45 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
2022-10-holograph/src/enforcer/HolographERC20.sol::371 => bytes32 structHash = keccak256(
2022-10-holograph/src/enforcer/HolographERC20.sol::373 => precomputekeccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"),
2022-10-holograph/src/enforcer/HolographERC20.sol::622 => return (codehash != 0x0 && codehash != precomputekeccak256(""));
2022-10-holograph/src/enforcer/HolographERC721.sol::35 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/enforcer/HolographERC721.sol::39 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
2022-10-holograph/src/enforcer/HolographERC721.sol::817 => return (codehash != 0x0 && codehash != precomputekeccak256(""));
2022-10-holograph/src/enforcer/Holographer.sol::18 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.originChain')) - 1)
2022-10-holograph/src/enforcer/Holographer.sol::22 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.holograph')) - 1)
2022-10-holograph/src/enforcer/Holographer.sol::26 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.contractType')) - 1)
2022-10-holograph/src/enforcer/Holographer.sol::30 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.sourceContract')) - 1)
2022-10-holograph/src/enforcer/Holographer.sol::34 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.blockHeight')) - 1)
2022-10-holograph/src/enforcer/PA1D.sol::23 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.defaultBp')) - 1)
2022-10-holograph/src/enforcer/PA1D.sol::27 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.defaultReceiver')) - 1)
2022-10-holograph/src/enforcer/PA1D.sol::31 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.initialized')) - 1)
2022-10-holograph/src/enforcer/PA1D.sol::35 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.payout.addresses')) - 1)
2022-10-holograph/src/enforcer/PA1D.sol::39 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.PA1D.payout.bps')) - 1)
2022-10-holograph/src/enforcer/PA1D.sol::158 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
2022-10-holograph/src/enforcer/PA1D.sol::170 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
2022-10-holograph/src/enforcer/PA1D.sol::181 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
2022-10-holograph/src/enforcer/PA1D.sol::193 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
2022-10-holograph/src/enforcer/PA1D.sol::209 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/src/enforcer/PA1D.sol::225 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/src/enforcer/PA1D.sol::242 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/src/enforcer/PA1D.sol::258 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/src/enforcer/PA1D.sol::267 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
2022-10-holograph/src/enforcer/PA1D.sol::274 => bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
2022-10-holograph/src/interface/ERC721.sol::48 => ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
2022-10-holograph/src/interface/ERC721TokenReceiver.sol::17 => /// @return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
2022-10-holograph/src/interface/HolographOperatorInterface.sol::87 => * @dev The job hash is a keccak256 hash of the entire job payload
2022-10-holograph/src/interface/HolographOperatorInterface.sol::88 => * @param jobHash keccak256 hash of the job
2022-10-holograph/src/library/ECDSA.sol::203 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
2022-10-holograph/src/library/ECDSA.sol::215 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
2022-10-holograph/src/library/ECDSA.sol::228 => return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
2022-10-holograph/src/mock/ERC20Mock.sol::127 => // bytes4(keccak256(abi.encodePacked('safeTransfer(address,uint256)'))) == 0x423f6cef
2022-10-holograph/src/mock/ERC20Mock.sol::129 => // bytes4(keccak256(abi.encodePacked('safeTransfer(address,uint256,bytes)'))) == 0xeb795549
2022-10-holograph/src/mock/ERC20Mock.sol::131 => // bytes4(keccak256(abi.encodePacked('safeTransferFrom(address,address,uint256)'))) == 0x42842e0e
2022-10-holograph/src/mock/ERC20Mock.sol::133 => // bytes4(keccak256(abi.encodePacked('safeTransferFrom(address,address,uint256,bytes)'))) == 0xb88d4fde
2022-10-holograph/src/mock/ERC20Mock.sol::289 => // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
2022-10-holograph/src/mock/ERC20Mock.sol::291 => bytes32 structHash = keccak256(
2022-10-holograph/src/mock/LZEndpointMock.sol::165 => keccak256(_payload)
2022-10-holograph/src/mock/LZEndpointMock.sol::305 => require(_payload.length == sp.payloadLength && keccak256(_payload) == sp.payloadHash, "LayerZero: invalid payload");
2022-10-holograph/src/module/LayerZeroModule.sol::25 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/src/module/LayerZeroModule.sol::29 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.interfaces')) - 1)
2022-10-holograph/src/module/LayerZeroModule.sol::33 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.lZEndpoint')) - 1)
2022-10-holograph/src/module/LayerZeroModule.sol::37 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/module/LayerZeroModule.sol::41 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/module/LayerZeroModule.sol::45 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/proxy/CxipERC721Proxy.sol::13 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.contractType')) - 1)
2022-10-holograph/src/proxy/CxipERC721Proxy.sol::17 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/proxy/HolographBridgeProxy.sol::12 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.bridge')) - 1)
2022-10-holograph/src/proxy/HolographFactoryProxy.sol::12 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
2022-10-holograph/src/proxy/HolographOperatorProxy.sol::12 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
2022-10-holograph/src/proxy/HolographRegistryProxy.sol::12 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.registry')) - 1)
2022-10-holograph/src/proxy/HolographTreasuryProxy.sol::12 => * @dev bytes32(uint256(keccak256('eip1967.Holograph.treasury')) - 1)
2022-10-holograph/src/token/SampleERC20.sol::50 => _walletSalts[to] = keccak256(
```
#### Tools used
Manual



### Long Revert Strings

#### Impact
Issue Information: Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Example
🤦 Bad:

require(condition, "UniswapV3: The reentrancy guard. A transaction cannot re-enter the pool mid-swap");
🚀 Good (with shorter string):

// TODO: Provide link to a reference of error codes
require(condition, "LOK");
🚀 Good (with custom errors):

/// @notice A transaction cannot re-enter the pool mid-swap.
error NoReentrancy();

// ...

if (!condition) {
    revert NoReentrancy();
}

#### Findings:
```
2022-10-holograph/contracts/Holograph.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/Holograph.sol::107 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/Holograph.sol::108 => import "./interface/HolographInterface.sol";
2022-10-holograph/contracts/HolographBridge.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographBridge.sol::107 => import "./interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/HolographBridge.sol::109 => import "./interface/HolographInterface.sol";
2022-10-holograph/contracts/HolographBridge.sol::110 => import "./interface/HolographBridgeInterface.sol";
2022-10-holograph/contracts/HolographBridge.sol::111 => import "./interface/HolographFactoryInterface.sol";
2022-10-holograph/contracts/HolographBridge.sol::112 => import "./interface/HolographOperatorInterface.sol";
2022-10-holograph/contracts/HolographBridge.sol::113 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/HolographBridge.sol::114 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/HolographFactory.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographFactory.sol::110 => import "./interface/HolographFactoryInterface.sol";
2022-10-holograph/contracts/HolographFactory.sol::111 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/HolographFactory.sol::112 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/HolographGenesis.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographGenesis.sol::104 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/HolographInterfaces.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographInterfaces.sol::121 => import "./interface/ERC721TokenReceiver.sol";
2022-10-holograph/contracts/HolographInterfaces.sol::122 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographOperator.sol::107 => import "./interface/CrossChainMessageInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::108 => import "./interface/HolographBridgeInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::109 => import "./interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/HolographOperator.sol::110 => import "./interface/HolographInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::111 => import "./interface/HolographOperatorInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::112 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::113 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/HolographOperator.sol::114 => import "./interface/HolographInterfacesInterface.sol";
2022-10-holograph/contracts/HolographRegistry.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographRegistry.sol::107 => import "./interface/HolographInterface.sol";
2022-10-holograph/contracts/HolographRegistry.sol::108 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/HolographRegistry.sol::109 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/HolographTreasury.sol::107 => import "./interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::108 => import "./interface/HolographERC721Interface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::109 => import "./interface/HolographInterface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::110 => import "./interface/HolographTreasuryInterface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::111 => import "./interface/HolographFactoryInterface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::112 => import "./interface/HolographOperatorInterface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::113 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/HolographTreasury.sol::114 => import "./interface/InitializableInterface.sol";
2022-10-holograph/contracts/abstract/Admin.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/EIP712.sol::63 => bytes32 typeHash = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");
2022-10-holograph/contracts/abstract/ERC1155H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/ERC20H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/ERC721H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/Initializable.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/Initializable.sol::104 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/abstract/NonReentrant.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/Owner.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/StrictERC1155H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/StrictERC1155H.sol::104 => import "../interface/HolographedERC1155.sol";
2022-10-holograph/contracts/abstract/StrictERC20H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/StrictERC20H.sol::104 => import "../interface/HolographedERC20.sol";
2022-10-holograph/contracts/abstract/StrictERC721H.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/abstract/StrictERC721H.sol::104 => import "../interface/HolographedERC721.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enforcer/HolographERC20.sol::115 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::122 => import "../interface/HolographedERC20.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::123 => import "../interface/HolographInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::124 => import "../interface/HolographerInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::125 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::126 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC20.sol::127 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enforcer/HolographERC721.sol::113 => import "../interface/HolographERC721Interface.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::115 => import "../interface/ERC721TokenReceiver.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::117 => import "../interface/HolographedERC721.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::118 => import "../interface/HolographInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::119 => import "../interface/HolographerInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::120 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::121 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/enforcer/HolographERC721.sol::122 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/contracts/enforcer/Holographer.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enforcer/Holographer.sol::107 => import "../interface/HolographInterface.sol";
2022-10-holograph/contracts/enforcer/Holographer.sol::108 => import "../interface/HolographerInterface.sol";
2022-10-holograph/contracts/enforcer/Holographer.sol::109 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/enforcer/Holographer.sol::110 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/enforcer/PA1D.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enforcer/PA1D.sol::109 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/enforcer/PA1D.sol::144 => string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
2022-10-holograph/contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enum/ChainIdType.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enum/HolographERC20Event.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enum/HolographERC721Event.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enum/InterfaceType.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/enum/TokenUriType.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/faucet/Faucet.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/faucet/Faucet.sol::105 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/faucet/Faucet.sol::106 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/faucet/Faucet.sol::128 => require(!_isInitialized(), "Faucet contract is already initialized");
2022-10-holograph/contracts/interface/CollectionURI.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/CrossChainMessageInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/ERC721.sol::48 => ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
2022-10-holograph/contracts/interface/ERC721TokenReceiver.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/ERC721TokenReceiver.sol::116 => /// @return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
2022-10-holograph/contracts/interface/HolographBridgeInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographERC20Interface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographERC721Interface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographFactoryInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographInterfacesInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographOperatorInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographRegistryInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographTreasuryInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/Holographable.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographedERC1155.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographedERC20.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographedERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/HolographerInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/InitializableInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/LayerZeroEndpointInterface.sol::5 => import "./LayerZeroUserApplicationConfigInterface.sol";
2022-10-holograph/contracts/interface/LayerZeroModuleInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/LayerZeroOverrides.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/Ownable.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/interface/PA1DInterface.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/library/Base64.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/library/Base64.sol::105 => bytes private constant base64stdchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
2022-10-holograph/contracts/library/Base64.sol::106 => bytes private constant base64urlchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_";
2022-10-holograph/contracts/library/Bytes.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/library/ECDSA.sol::31 => revert("ECDSA: invalid signature 's' value");
2022-10-holograph/contracts/library/ECDSA.sol::33 => revert("ECDSA: invalid signature 'v' value");
2022-10-holograph/contracts/library/Strings.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/ERC20Mock.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/ERC20Mock.sol::109 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/mock/ERC20Mock.sol::388 => // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
2022-10-holograph/contracts/mock/LZEndpointMock.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/LZEndpointMock.sol::104 => import "../interface/LayerZeroReceiverInterface.sol";
2022-10-holograph/contracts/mock/LZEndpointMock.sol::105 => import "../interface/LayerZeroEndpointInterface.sol";
2022-10-holograph/contracts/mock/LZEndpointMock.sol::191 => require(lzEndpoint != address(0), "LayerZeroMock: destination LayerZero Endpoint not found");
2022-10-holograph/contracts/mock/Mock.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/Mock.sol::106 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/mock/MockERC721Receiver.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/MockERC721Receiver.sol::106 => import "../interface/ERC721TokenReceiver.sol";
2022-10-holograph/contracts/mock/MockExternalCall.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/MockHolographChild.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/MockHolographGenesisChild.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/MockLZEndpoint.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/mock/MockLZEndpoint.sol::106 => import "../interface/LayerZeroOverrides.sol";
2022-10-holograph/contracts/module/LayerZeroModule.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/module/LayerZeroModule.sol::109 => import "../interface/CrossChainMessageInterface.sol";
2022-10-holograph/contracts/module/LayerZeroModule.sol::110 => import "../interface/HolographOperatorInterface.sol";
2022-10-holograph/contracts/module/LayerZeroModule.sol::111 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/module/LayerZeroModule.sol::112 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/contracts/module/LayerZeroModule.sol::113 => import "../interface/LayerZeroModuleInterface.sol";
2022-10-holograph/contracts/module/LayerZeroModule.sol::114 => import "../interface/LayerZeroOverrides.sol";
2022-10-holograph/contracts/proxy/CxipERC721Proxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/proxy/CxipERC721Proxy.sol::107 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/proxy/CxipERC721Proxy.sol::108 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/contracts/proxy/HolographBridgeProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/proxy/HolographBridgeProxy.sol::107 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/proxy/HolographFactoryProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/proxy/HolographFactoryProxy.sol::107 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/proxy/HolographOperatorProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/proxy/HolographOperatorProxy.sol::107 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/proxy/HolographRegistryProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/proxy/HolographRegistryProxy.sol::107 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/proxy/HolographTreasuryProxy.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/proxy/HolographTreasuryProxy.sol::107 => import "../interface/InitializableInterface.sol";
2022-10-holograph/contracts/struct/DeploymentConfig.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/struct/OperatorJob.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/struct/Verification.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/struct/ZoraBidShares.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/struct/ZoraDecimal.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/token/CxipERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/token/CxipERC721.sol::108 => import "../interface/HolographERC721Interface.sol";
2022-10-holograph/contracts/token/CxipERC721.sol::109 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/contracts/token/CxipERC721.sol::110 => import "../interface/HolographInterface.sol";
2022-10-holograph/contracts/token/CxipERC721.sol::111 => import "../interface/HolographerInterface.sol";
2022-10-holograph/contracts/token/HolographUtilityToken.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/token/HolographUtilityToken.sol::107 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/token/HolographUtilityToken.sol::108 => import "../interface/HolographInterface.sol";
2022-10-holograph/contracts/token/HolographUtilityToken.sol::109 => import "../interface/HolographerInterface.sol";
2022-10-holograph/contracts/token/SampleERC20.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/token/SampleERC20.sol::106 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/token/SampleERC721.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/token/SampleERC721.sol::106 => import "../interface/HolographERC721Interface.sol";
2022-10-holograph/contracts/token/hToken.sol::44 => The terms "reproduce," "reproduction," "derivative works," and
2022-10-holograph/contracts/token/hToken.sol::107 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/contracts/token/hToken.sol::108 => import "../interface/HolographInterface.sol";
2022-10-holograph/contracts/token/hToken.sol::109 => import "../interface/HolographerInterface.sol";
2022-10-holograph/src/Holograph.sol::8 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/Holograph.sol::9 => import "./interface/HolographInterface.sol";
2022-10-holograph/src/Holograph.sol::33 => bytes32 constant _holographChainIdSlot = precomputeslot("eip1967.Holograph.holographChainId");
2022-10-holograph/src/HolographBridge.sol::8 => import "./interface/HolographERC20Interface.sol";
2022-10-holograph/src/HolographBridge.sol::10 => import "./interface/HolographInterface.sol";
2022-10-holograph/src/HolographBridge.sol::11 => import "./interface/HolographBridgeInterface.sol";
2022-10-holograph/src/HolographBridge.sol::12 => import "./interface/HolographFactoryInterface.sol";
2022-10-holograph/src/HolographBridge.sol::13 => import "./interface/HolographOperatorInterface.sol";
2022-10-holograph/src/HolographBridge.sol::14 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/src/HolographBridge.sol::15 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/HolographFactory.sol::11 => import "./interface/HolographFactoryInterface.sol";
2022-10-holograph/src/HolographFactory.sol::12 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/src/HolographFactory.sol::13 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/HolographGenesis.sol::5 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/HolographInterfaces.sol::22 => import "./interface/ERC721TokenReceiver.sol";
2022-10-holograph/src/HolographInterfaces.sol::23 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/HolographOperator.sol::8 => import "./interface/CrossChainMessageInterface.sol";
2022-10-holograph/src/HolographOperator.sol::9 => import "./interface/HolographBridgeInterface.sol";
2022-10-holograph/src/HolographOperator.sol::10 => import "./interface/HolographERC20Interface.sol";
2022-10-holograph/src/HolographOperator.sol::11 => import "./interface/HolographInterface.sol";
2022-10-holograph/src/HolographOperator.sol::12 => import "./interface/HolographOperatorInterface.sol";
2022-10-holograph/src/HolographOperator.sol::13 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/src/HolographOperator.sol::14 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/HolographOperator.sol::15 => import "./interface/HolographInterfacesInterface.sol";
2022-10-holograph/src/HolographOperator.sol::46 => bytes32 constant _messagingModuleSlot = precomputeslot("eip1967.Holograph.messagingModule");
2022-10-holograph/src/HolographRegistry.sol::8 => import "./interface/HolographInterface.sol";
2022-10-holograph/src/HolographRegistry.sol::9 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/src/HolographRegistry.sol::10 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/HolographTreasury.sol::8 => import "./interface/HolographERC20Interface.sol";
2022-10-holograph/src/HolographTreasury.sol::9 => import "./interface/HolographERC721Interface.sol";
2022-10-holograph/src/HolographTreasury.sol::10 => import "./interface/HolographInterface.sol";
2022-10-holograph/src/HolographTreasury.sol::11 => import "./interface/HolographTreasuryInterface.sol";
2022-10-holograph/src/HolographTreasury.sol::12 => import "./interface/HolographFactoryInterface.sol";
2022-10-holograph/src/HolographTreasury.sol::13 => import "./interface/HolographOperatorInterface.sol";
2022-10-holograph/src/HolographTreasury.sol::14 => import "./interface/HolographRegistryInterface.sol";
2022-10-holograph/src/HolographTreasury.sol::15 => import "./interface/InitializableInterface.sol";
2022-10-holograph/src/abstract/EIP712.sol::63 => bytes32 typeHash = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");
2022-10-holograph/src/abstract/Initializable.sol::5 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/abstract/StrictERC1155H.sol::5 => import "../interface/HolographedERC1155.sol";
2022-10-holograph/src/abstract/StrictERC20H.sol::5 => import "../interface/HolographedERC20.sol";
2022-10-holograph/src/abstract/StrictERC721H.sol::5 => import "../interface/HolographedERC721.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::16 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::23 => import "../interface/HolographedERC20.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::24 => import "../interface/HolographInterface.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::25 => import "../interface/HolographerInterface.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::26 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::27 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::28 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/src/enforcer/HolographERC20.sol::373 => precomputekeccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"),
2022-10-holograph/src/enforcer/HolographERC721.sol::14 => import "../interface/HolographERC721Interface.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::16 => import "../interface/ERC721TokenReceiver.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::18 => import "../interface/HolographedERC721.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::19 => import "../interface/HolographInterface.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::20 => import "../interface/HolographerInterface.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::21 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::22 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/enforcer/HolographERC721.sol::23 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/src/enforcer/Holographer.sol::8 => import "../interface/HolographInterface.sol";
2022-10-holograph/src/enforcer/Holographer.sol::9 => import "../interface/HolographerInterface.sol";
2022-10-holograph/src/enforcer/Holographer.sol::10 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/src/enforcer/Holographer.sol::11 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/enforcer/PA1D.sol::10 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/enforcer/PA1D.sol::29 => bytes32 constant _defaultReceiverSlot = precomputeslot("eip1967.Holograph.PA1D.defaultReceiver");
2022-10-holograph/src/enforcer/PA1D.sol::33 => bytes32 constant _initializedPaidSlot = precomputeslot("eip1967.Holograph.PA1D.initialized");
2022-10-holograph/src/enforcer/PA1D.sol::37 => bytes32 constant _payoutAddressesSlot = precomputeslot("eip1967.Holograph.PA1D.payout.addresses");
2022-10-holograph/src/enforcer/PA1D.sol::41 => bytes32 constant _payoutBpsSlot = precomputeslot("eip1967.Holograph.PA1D.payout.bps");
2022-10-holograph/src/enforcer/PA1D.sol::45 => string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
2022-10-holograph/src/enforcer/PA1D.sol::312 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/src/enforcer/PA1D.sol::336 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/src/faucet/Faucet.sol::6 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/src/faucet/Faucet.sol::7 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/faucet/Faucet.sol::29 => require(!_isInitialized(), "Faucet contract is already initialized");
2022-10-holograph/src/interface/ERC721.sol::48 => ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
2022-10-holograph/src/interface/ERC721TokenReceiver.sol::17 => /// @return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
2022-10-holograph/src/interface/LayerZeroEndpointInterface.sol::5 => import "./LayerZeroUserApplicationConfigInterface.sol";
2022-10-holograph/src/library/Base64.sol::6 => bytes private constant base64stdchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
2022-10-holograph/src/library/Base64.sol::7 => bytes private constant base64urlchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_";
2022-10-holograph/src/library/ECDSA.sol::31 => revert("ECDSA: invalid signature 's' value");
2022-10-holograph/src/library/ECDSA.sol::33 => revert("ECDSA: invalid signature 'v' value");
2022-10-holograph/src/mock/ERC20Mock.sol::10 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/src/mock/ERC20Mock.sol::264 => sstore(precomputeslot("eip1967.Holograph.ERC20Mock.fakeData"), amount)
2022-10-holograph/src/mock/ERC20Mock.sol::289 => // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
2022-10-holograph/src/mock/LZEndpointMock.sol::5 => import "../interface/LayerZeroReceiverInterface.sol";
2022-10-holograph/src/mock/LZEndpointMock.sol::6 => import "../interface/LayerZeroEndpointInterface.sol";
2022-10-holograph/src/mock/LZEndpointMock.sol::92 => require(lzEndpoint != address(0), "LayerZeroMock: destination LayerZero Endpoint not found");
2022-10-holograph/src/mock/Mock.sol::7 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/mock/MockERC721Receiver.sol::7 => import "../interface/ERC721TokenReceiver.sol";
2022-10-holograph/src/mock/MockLZEndpoint.sol::7 => import "../interface/LayerZeroOverrides.sol";
2022-10-holograph/src/module/LayerZeroModule.sol::10 => import "../interface/CrossChainMessageInterface.sol";
2022-10-holograph/src/module/LayerZeroModule.sol::11 => import "../interface/HolographOperatorInterface.sol";
2022-10-holograph/src/module/LayerZeroModule.sol::12 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/module/LayerZeroModule.sol::13 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/src/module/LayerZeroModule.sol::14 => import "../interface/LayerZeroModuleInterface.sol";
2022-10-holograph/src/module/LayerZeroModule.sol::15 => import "../interface/LayerZeroOverrides.sol";
2022-10-holograph/src/proxy/CxipERC721Proxy.sol::8 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/proxy/CxipERC721Proxy.sol::9 => import "../interface/HolographRegistryInterface.sol";
2022-10-holograph/src/proxy/HolographBridgeProxy.sol::8 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/proxy/HolographFactoryProxy.sol::8 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/proxy/HolographOperatorProxy.sol::8 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/proxy/HolographRegistryProxy.sol::8 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/proxy/HolographTreasuryProxy.sol::8 => import "../interface/InitializableInterface.sol";
2022-10-holograph/src/token/CxipERC721.sol::9 => import "../interface/HolographERC721Interface.sol";
2022-10-holograph/src/token/CxipERC721.sol::10 => import "../interface/HolographInterfacesInterface.sol";
2022-10-holograph/src/token/CxipERC721.sol::11 => import "../interface/HolographInterface.sol";
2022-10-holograph/src/token/CxipERC721.sol::12 => import "../interface/HolographerInterface.sol";
2022-10-holograph/src/token/HolographUtilityToken.sol::8 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/src/token/HolographUtilityToken.sol::9 => import "../interface/HolographInterface.sol";
2022-10-holograph/src/token/HolographUtilityToken.sol::10 => import "../interface/HolographerInterface.sol";
2022-10-holograph/src/token/SampleERC20.sol::7 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/src/token/SampleERC721.sol::7 => import "../interface/HolographERC721Interface.sol";
2022-10-holograph/src/token/hToken.sol::8 => import "../interface/HolographERC20Interface.sol";
2022-10-holograph/src/token/hToken.sol::9 => import "../interface/HolographInterface.sol";
2022-10-holograph/src/token/hToken.sol::10 => import "../interface/HolographerInterface.sol";
```
#### Tools used
Manual



### Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

Example
🤦 Bad:

uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
🚀 Good:

uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;

#### Findings:
```
2022-10-holograph/contracts/HolographOperator.sol::533 => ); // 80 next available bit position && so far 176 bits used with only 128 left
2022-10-holograph/contracts/library/Base64.sol::116 => uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
2022-10-holograph/contracts/library/ECDSA.sol::159 => // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
2022-10-holograph/src/HolographOperator.sol::434 => ); // 80 next available bit position && so far 176 bits used with only 128 left
2022-10-holograph/src/library/Base64.sol::17 => uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
2022-10-holograph/src/library/ECDSA.sol::159 => // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
```
#### Tools used
Manual