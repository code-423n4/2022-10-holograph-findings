## [NC-001] Large multiples of ten should use scientific notation

### Description:

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability.

### Findings:

Total:11

[PA1D.sol#L291](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L291)

```
291:    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
```

[PA1D.sol#L296](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L296)

```
296:    sending = ((bps[i] * balance) / 10000);
```

[PA1D.sol#L312](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L312)

```
312:    require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

[PA1D.sol#L316](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L316)

```
316:    sending = ((bps[i] * balance) / 10000);
```

[PA1D.sol#L336](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L336)

```
336:    require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

[PA1D.sol#L339](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L339)

```
339:    sending = ((bps[i] * balance) / 10000);
```

[PA1D.sol#L378](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L378)

```
378:    require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

[PA1D.sol#L452](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L452)

```
452:    return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
```

[PA1D.sol#L454](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L454)

```
454:    return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
```

[PA1D.sol#L542](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L542)

```
542:    return (_getDefaultBp() * amount) / 10000;
```

[PA1D.sol#L544](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L544)

```
544:    return (_getBp(tokenId) * amount) / 10000;
```

## [NC-002] Constants should be defined rather than using magic numbers

### Description:

It is bad practice to use numbers directly in code without explanation.

### Findings:

Total:58

[HolographBridge.sol#L223](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L223)

```
223:    revert(add(payload, 0x20), mload(payload))
```

[HolographBridge.sol#L452](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L452)

```
452:    jobNonce := add(sload(_jobNonceSlot), 0x0000000000000000000000000000000000000000000000000000000000000001)
```

[HolographFactory.sol#L127](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L127)

```
127:    uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), hash, keccak256(holographerBytecode)))))
```

[HolographFactory.sol#L140](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L140)

```
140:    sourceContractAddress := create2(0, add(sourceByteCode, 0x20), mload(sourceByteCode), saltInt)
```

[HolographFactory.sol#L146](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L146)

```
146:    holographerAddress := create2(0, add(holographerBytecode, 0x20), mload(holographerBytecode), saltInt)
```

[HolographFactory.sol#L215](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L215)

```
215:    return (codehash != 0x0 && codehash != precomputekeccak256(""));
```

[HolographFactory.sol#L228](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L228)

```
228:    if (v < 27) {
```

[HolographFactory.sol#L229](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L229)

```
229:    v += 27;
```

[HolographOperator.sol#L217](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L217)

```
217:    gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
```

[HolographOperator.sol#L221](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L221)

```
221:    gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
```

[HolographOperator.sol#L259](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L259)

```
259:    if (timeDifference < 6) {
```

[HolographOperator.sol#L352](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L352)

```
352:    calldatacopy(0, payload.offset, sub(payload.length, 0x20))
```

[HolographOperator.sol#L356](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L356)

```
356:    mstore(0x84, msgSender)
```

[HolographOperator.sol#L362](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L362)

```
362:    mload(sub(payload.length, 0x40)),
```

[HolographOperator.sol#L370](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L370)

```
370:    sub(payload.length, 0x40),
```

[HolographOperator.sol#L424](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L424)

```
424:    ((pod + 1) << 248) |
```

[HolographOperator.sol#L425](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L425)

```
425:    (uint256(_operatorTempStorageCounter) << 216) |
```

[HolographOperator.sol#L428](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L428)

```
428:    (_randomBlockHash(random, podSize, 2) << 144) |
```

[HolographOperator.sol#L429](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L429)

```
429:    (_randomBlockHash(random, podSize, 3) << 128) |
```

[HolographOperator.sol#L430](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L430)

```
430:    (_randomBlockHash(random, podSize, 4) << 112) |
```

[HolographOperator.sol#L431](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L431)

```
431:    (_randomBlockHash(random, podSize, 5) << 96) |
```

[HolographOperator.sol#L452](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L452)

```
452:    calldatacopy(0, bridgeInRequestPayload.offset, sub(bridgeInRequestPayload.length, 0x40))
```

[HolographOperator.sol#L456](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L456)

```
456:    mstore8(0xE3, 0x00)
```

[HolographOperator.sol#L457](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L457)

```
457:    let result := call(gas(), sload(_bridgeSlot), callvalue(), 0, sub(bridgeInRequestPayload.length, 0x40), 0, 0)
```

[HolographOperator.sol#L468](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L468)

```
468:    mstore(0x00, gas())
```

[HolographOperator.sol#L469](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L469)

```
469:    return(0x00, 0x20)
```

[HolographOperator.sol#L598](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L598)

```
598:    uint8(packed >> 248),
```

[HolographOperator.sol#L600](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L600)

```
600:    _operatorTempStorage[uint32(packed >> 216)],
```

[HolographOperator.sol#L609](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L609)

```
609:    uint16(packed >> 96)
```

[HolographOperator.sol#L1015](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L1015)

```
1015:    jobNonce := add(sload(_jobNonceSlot), 0x0000000000000000000000000000000000000000000000000000000000000001)
```

[HolographOperator.sol#L1104](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L1104)

```
1104:    return (codehash != 0x0 && codehash != precomputekeccak256(""));
```

[ERC20H.sol#L62](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L62)

```
62:    sender := calldataload(sub(calldatasize(), 0x20))
```

[ERC20H.sol#L122](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L122)

```
122:    mstore(0x80, 0x0000000000000000000000000000000000000000000000000000000000000001)
```

[ERC20H.sol#L123](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L123)

```
123:    return(0x80, 0x20)
```

[ERC20H.sol#L126](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L126)

```
126:    revert(0x00, 0x00)
```

[ERC721H.sol#L62](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L62)

```
62:    sender := calldataload(sub(calldatasize(), 0x20))
```

[ERC721H.sol#L122](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L122)

```
122:    mstore(0x80, 0x0000000000000000000000000000000000000000000000000000000000000001)
```

[ERC721H.sol#L123](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L123)

```
123:    return(0x80, 0x20)
```

[ERC721H.sol#L126](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L126)

```
126:    revert(0x00, 0x00)
```

[HolographERC20.sol#L123](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L123)

```
123:    sstore(_reentrantSlot, 0x0000000000000000000000000000000000000000000000000000000000000001)
```

[HolographERC20.sol#L162](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L162)

```
162:    let result := call(gas(), sload(_sourceContractSlot), callvalue(), 0, add(calldatasize(), 32), 0, 0)
```

[HolographERC20.sol#L548](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L548)

```
548:    if (ERC165(recipient).supportsInterface(0x534f5876)) {
```

[HolographERC20.sol#L622](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L622)

```
622:    return (codehash != 0x0 && codehash != precomputekeccak256(""));
```

[HolographERC721.sol#L817](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L817)

```
817:    return (codehash != 0x0 && codehash != precomputekeccak256(""));
```

[HolographERC721.sol#L856](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L856)

```
856:    0x0000000000000000000000000000000000000000000000000000000050413144
```

[HolographERC721.sol#L890](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L890)

```
890:    let result := call(gas(), sload(_sourceContractSlot), callvalue(), 0, add(calldatasize(), 32), 0, 0)
```

[LayerZeroModule.sol#L96](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L96)

```
96:    mstore(0x80, 0x08c379a000000000000000000000000000000000000000000000000000000000)
```

[LayerZeroModule.sol#L97](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L97)

```
97:    mstore(0xa0, 0x0000002000000000000000000000000000000000000000000000000000000000)
```

[LayerZeroModule.sol#L98](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L98)

```
98:    mstore(0xc0, 0x0000001b484f4c4f47524150483a204c5a206f6e6c7920656e64706f696e7400)
```

[LayerZeroModule.sol#L99](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L99)

```
99:    mstore(0xe0, 0x0000000000000000000000000000000000000000000000000000000000000000)
```

[LayerZeroModule.sol#L100](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L100)

```
100:    revert(0x80, 0xc4)
```

[LayerZeroModule.sol#L102](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L102)

```
102:    let ptr := mload(0x40)
```

[LayerZeroModule.sol#L103](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L103)

```
103:    calldatacopy(add(ptr, 0x0c), _srcAddress.offset, _srcAddress.length)
```

[LayerZeroModule.sol#L112](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L112)

```
112:    mstore(0x80, 0x08c379a000000000000000000000000000000000000000000000000000000000)
```

[LayerZeroModule.sol#L113](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L113)

```
113:    mstore(0xa0, 0x0000002000000000000000000000000000000000000000000000000000000000)
```

[LayerZeroModule.sol#L114](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L114)

```
114:    mstore(0xc0, 0x0000001e484f4c4f47524150483a20756e617574686f72697a65642073656e64)
```

[LayerZeroModule.sol#L115](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L115)

```
115:    mstore(0xe0, 0x6572000000000000000000000000000000000000000000000000000000000000)
```

[LayerZeroModule.sol#L116](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L116)

```
116:    revert(0x80, 0xc4)
```

## [NC-003] Adding a return statement to a function that returns a value is redundant

### Description:

It is not necessary to have both a named return and a return statement.

### Findings:

Total:5

[HolographFactory.sol#L82](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L82)

```
82:    ) external pure returns (bytes4 selector, bytes memory data) {
```

[HolographOperator.sol#L618](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L618)

```
618:    function getTotalPods() external view returns (uint256 totalPods) {
```

[HolographOperator.sol#L705](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L705)

```
705:    function getBondedAmount(address operator) external view returns (uint256 amount) {
```

[HolographOperator.sol#L715](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L715)

```
715:    function getBondedPod(address operator) external view returns (uint256 pod) {
```

[HolographERC20.sol#L550](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L550)

```
550:    try ERC20Receiver(recipient).onERC20Received(address(this), account, amount, data) returns (bytes4 retval) {
```

## [L-001] Unused `reveive()/fallback()` function

### Description:

The following contracts define a payable receive function but have no way of withdrawing or utilizing the sent Ether, resulting in Ether being locked in the contracts. If the intention is to use the Ether that functionality should be added to the function, otherwise it should revert.

### Findings:

Total:18

[HolographBridge.sol#L485](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L485)

```
485:    fallback() external payable {
```

[HolographFactory.sol#L248](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L248)

```
248:    fallback() external payable {
```

[HolographOperator.sol#L260](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L260)

```
260:    uint256 podIndex = uint256(job.fallbackOperators[timeDifference - 1]);
```

[HolographOperator.sol#L265](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L265)

```
265:    address fallbackOperator = _operatorPods[pod][podIndex];
```

[HolographOperator.sol#L269](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L269)

```
269:    require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
```

[HolographOperator.sol#L1110](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L1110)

```
1110:    receive() external payable {}
```

[HolographOperator.sol#L1115](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L1115)

```
1115:    fallback() external payable {
```

[ERC20H.sol#L113](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L113)

```
113:    receive() external payable {}
```

[ERC20H.sol#L118](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L118)

```
118:    fallback() external payable {
```

[ERC721H.sol#L113](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L113)

```
113:    receive() external payable {}
```

[ERC721H.sol#L118](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L118)

```
118:    fallback() external payable {
```

[HolographERC20.sol#L152](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L152)

```
152:    receive() external payable {}
```

[HolographERC20.sol#L158](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L158)

```
158:    fallback() external payable {
```

[HolographERC721.sol#L863](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L863)

```
863:    receive() external payable {}
```

[HolographERC721.sol#L869](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L869)

```
869:    fallback() external payable {
```

[Holographer.sol#L124](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/Holographer.sol#L124)

```
124:    receive() external payable {}
```

[Holographer.sol#L129](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/Holographer.sol#L129)

```
129:    fallback() external payable {
```

[LayerZeroModule.sol#L324](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L324)

```
324:    fallback() external payable {
```

## [L-002] `decimals()` not part of ERC20 standard

### Description:

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.

### Findings:

Total:2

[HolographERC20.sol#L174](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L174)

```
174:    function decimals() public view returns (uint8) {
```

[HolographERC721.sol#L553](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L553)

```
553:    function decimals() external pure returns (uint256) {
```

## [L-003] `approve` should be replaced with `safeIncreaseAllowance()` or `safeDecreaseAllowance()`

### Description:

`approve` is subject to a known front-running attack. Consider using `safeIncreaseAllownce()` or `safeDecreaseAllowance()` instead

### Findings:

Total:7

[HolographERC20.sol#L227](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L227)

```
227:    function approve(address spender, uint256 amount) public returns (bool) {
```

[HolographERC20.sol#L231](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L231)

```
231:    _approve(msg.sender, spender, amount);
```

[HolographERC20.sol#L274](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L274)

```
274:    _approve(msg.sender, spender, newAllowance);
```

[HolographERC20.sol#L333](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L333)

```
333:    _approve(msg.sender, spender, newAllowance);
```

[HolographERC20.sol#L387](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L387)

```
387:    _approve(account, spender, amount);
```

[HolographERC20.sol#L516](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L516)

```
516:    function _approve(
```

[HolographERC721.sol#L269](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L269)

```
269:    function approve(address to, uint256 tokenId) external payable {
```

## [L-004] Events not emitted for important state changes / Missing event for critical parameter changes

### Description:

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

### Findings:

Total:20

[HolographBridge.sol#L353](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L353)

```
353:    function setFactory(address factory) external onlyAdmin {
```

[HolographBridge.sol#L373](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L373)

```
373:    function setHolograph(address holograph) external onlyAdmin {
```

[HolographBridge.sol#L403](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L403)

```
403:    function setOperator(address operator) external onlyAdmin {
```

[HolographBridge.sol#L423](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L423)

```
423:    function setRegistry(address registry) external onlyAdmin {
```

[HolographFactory.sol#L181](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L181)

```
181:    function setHolograph(address holograph) external onlyAdmin {
```

[HolographFactory.sol#L201](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L201)

```
201:    function setRegistry(address registry) external onlyAdmin {
```

[HolographOperator.sol#L850](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L850)

```
850:    function setBridge(address bridge) external onlyAdmin {
```

[HolographOperator.sol#L870](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L870)

```
870:    function setHolograph(address holograph) external onlyAdmin {
```

[HolographOperator.sol#L890](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L890)

```
890:    function setInterfaces(address interfaces) external onlyAdmin {
```

[HolographOperator.sol#L910](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L910)

```
910:    function setMessagingModule(address messagingModule) external onlyAdmin {
```

[HolographOperator.sol#L930](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L930)

```
930:    function setRegistry(address registry) external onlyAdmin {
```

[HolographOperator.sol#L950](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L950)

```
950:    function setUtilityToken(address utilityToken) external onlyAdmin {
```

[HolographERC721.sol#L384](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L384)

```
384:    function setApprovalForAll(address to, bool approved) external {
```

[PA1D.sol#L430](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L430)

```
430:    function setRoyalties(
```

[LayerZeroModule.sol#L221](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L221)

```
221:    function setBridge(address bridge) external onlyAdmin {
```

[LayerZeroModule.sol#L241](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L241)

```
241:    function setInterfaces(address interfaces) external onlyAdmin {
```

[LayerZeroModule.sol#L261](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L261)

```
261:    function setLZEndpoint(address lZEndpoint) external onlyAdmin {
```

[LayerZeroModule.sol#L281](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L281)

```
281:    function setOperator(address operator) external onlyAdmin {
```

[LayerZeroModule.sol#L342](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L342)

```
342:    function setBaseGas(uint256 baseGas) external onlyAdmin {
```

[LayerZeroModule.sol#L371](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L371)

```
371:    function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
`