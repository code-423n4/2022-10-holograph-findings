


### [G01] State variables only set in the constructor should be declared `immutable`


#### Findings:
```
2022-10-holograph/HolographOperator.sol::158 => uint256 private _blockTime;
2022-10-holograph/HolographOperator.sol::163 => uint256 private _baseBondAmount;
2022-10-holograph/HolographOperator.sol::168 => uint256 private _podMultiplier;
2022-10-holograph/HolographOperator.sol::173 => uint256 private _operatorThreshold;
2022-10-holograph/HolographOperator.sol::178 => uint256 private _operatorThresholdStep;
2022-10-holograph/HolographOperator.sol::183 => uint256 private _operatorThresholdDivisor;
2022-10-holograph/HolographOperator.sol::188 => uint256 private _inboundMessageCounter;
2022-10-holograph/HolographOperator.sol::208 => uint32 private _operatorTempStorageCounter;
2022-10-holograph/HolographOperator.sol::213 => address[][] private _operatorPods;
2022-10-holograph/enforcer/HolographERC20.sol::151 => uint256 private _eventConfig;
2022-10-holograph/enforcer/HolographERC20.sol::166 => uint256 private _totalSupply;
2022-10-holograph/enforcer/HolographERC20.sol::171 => string private _name;
2022-10-holograph/enforcer/HolographERC20.sol::176 => string private _symbol;
2022-10-holograph/enforcer/HolographERC20.sol::181 => uint8 private _decimals;
2022-10-holograph/enforcer/HolographERC721.sol::145 => uint256 private _eventConfig;
2022-10-holograph/enforcer/HolographERC721.sol::150 => string private _name;
2022-10-holograph/enforcer/HolographERC721.sol::155 => string private _symbol;
2022-10-holograph/enforcer/HolographERC721.sol::160 => uint16 private _bps;
2022-10-holograph/enforcer/HolographERC721.sol::165 => uint256[] private _allTokens;
```


### [G02] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```
2022-10-holograph/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```



### [G03] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```
2022-10-holograph/HolographOperator.sol::433 => ++_inboundMessageCounter;
2022-10-holograph/HolographOperator.sol::495 => ++_operatorTempStorageCounter;
2022-10-holograph/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/enforcer/HolographERC20.sol::713 => _nonces[account]++;
2022-10-holograph/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::568 => //       startingTokenId++;
2022-10-holograph/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::779 => _ownedTokensCount[to]++;
2022-10-holograph/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::397 => // sent = sent + sending;
2022-10-holograph/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```



### [G04] `require()`/`revert()` strings longer than 32 bytes cost extra gas


#### Findings:
```
2022-10-holograph/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
```



### [G05] Not using the named return variables when a function returns, wastes deployment gas


#### Findings:
```
2022-10-holograph/HolographOperator.sol::718 => return _operatorPods.length;
2022-10-holograph/HolographOperator.sol::729 => return _operatorPods[pod - 1].length;
2022-10-holograph/HolographOperator.sol::805 => return _bondedAmounts[operator];
2022-10-holograph/HolographOperator.sol::815 => return _bondedOperators[operator];
2022-10-holograph/abstract/ERC20H.sol::141 => return _init(initPayload);
2022-10-holograph/abstract/ERC20H.sol::182 => return _getOwner();
2022-10-holograph/abstract/ERC721H.sol::141 => return _init(initPayload);
2022-10-holograph/abstract/ERC721H.sol::182 => return _getOwner();
2022-10-holograph/enforcer/HolographERC20.sol::274 => return _decimals;
2022-10-holograph/enforcer/HolographERC20.sol::298 => return _allowances[account][spender];
2022-10-holograph/enforcer/HolographERC20.sol::302 => return _balances[account];
2022-10-holograph/enforcer/HolographERC20.sol::307 => return _domainSeparatorV4();
2022-10-holograph/enforcer/HolographERC20.sol::311 => return _name;
2022-10-holograph/enforcer/HolographERC20.sol::315 => return _nonces[account];
2022-10-holograph/enforcer/HolographERC20.sol::319 => return _symbol;
2022-10-holograph/enforcer/HolographERC20.sol::323 => return _totalSupply;
2022-10-holograph/enforcer/HolographERC20.sol::737 => return _holograph().getInterfaces();
2022-10-holograph/enforcer/HolographERC721.sol::283 => return _name;
2022-10-holograph/enforcer/HolographERC721.sol::314 => return _symbol;
2022-10-holograph/enforcer/HolographERC721.sol::337 => return _ownedTokens[wallet];
2022-10-holograph/enforcer/HolographERC721.sol::640 => return _ownedTokensCount[wallet];
2022-10-holograph/enforcer/HolographERC721.sol::644 => return _burnedTokens[tokenId];
2022-10-holograph/enforcer/HolographERC721.sol::657 => return _tokenOwner[tokenId] != address(0);
2022-10-holograph/enforcer/HolographERC721.sol::667 => return _tokenApprovals[tokenId];
2022-10-holograph/enforcer/HolographERC721.sol::678 => return _operatorApprovals[wallet][operator];
2022-10-holograph/enforcer/HolographERC721.sol::701 => return _allTokens[index];
2022-10-holograph/enforcer/HolographERC721.sol::730 => return _ownedTokens[wallet][index];
2022-10-holograph/enforcer/HolographERC721.sol::739 => return _allTokens.length;
2022-10-holograph/enforcer/HolographERC721.sol::932 => return _holograph().getInterfaces();
2022-10-holograph/enforcer/PA1D.sol::628 => return _getDefaultReceiver();
2022-10-holograph/enforcer/PA1D.sol::658 => return _getDefaultReceiver();
2022-10-holograph/enforcer/PA1D.sol::684 => return _getTokenAddress(tokenName);
```



### [G06] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
2022-10-holograph/HolographBridge.sol::218 => if (hTokenValue > 0) {
2022-10-holograph/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
2022-10-holograph/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
2022-10-holograph/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
2022-10-holograph/HolographOperator.sol::398 => if (leftovers > 0) {
2022-10-holograph/HolographOperator.sol::1126 => if (operatorIndex > 0) {
2022-10-holograph/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
```



### [G07] It costs more gas to initialize variables to zero than to let the default of zero be applied



#### Findings:
```
2022-10-holograph/HolographBridge.sol::380 => uint256 fee = 0;
2022-10-holograph/HolographOperator.sol::310 => uint256 gasLimit = 0;
2022-10-holograph/HolographOperator.sol::311 => uint256 gasPrice = 0;
2022-10-holograph/HolographOperator.sol::399 => _bondedAmounts[job.operator] = 0;
2022-10-holograph/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/HolographOperator.sol::924 => _bondedAmounts[operator] = 0;
2022-10-holograph/HolographOperator.sol::1132 => _bondedOperators[operator] = 0;
2022-10-holograph/HolographOperator.sol::1136 => _operatorPodIndex[operator] = 0;
2022-10-holograph/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::667 => bidShares.prevOwner.value = 0;
2022-10-holograph/enforcer/PA1D.sol::668 => bidShares.owner.value = 0;
```



### [G08] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
2022-10-holograph/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```



### [G09] Splitting `require()` statements that use `&&` Cost gas

#### Impact
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) for an example
#### Findings:
```

2022-10-holograph/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
2022-10-holograph/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
2022-10-holograph/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");
```



### [G10] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
2022-10-holograph/HolographBridge.sol::192 => uint32 fromChain,
2022-10-holograph/HolographBridge.sol::246 => uint32 toChain,
2022-10-holograph/HolographBridge.sol::298 => uint32 toChain,
2022-10-holograph/HolographBridge.sol::343 => uint32 toChain,
2022-10-holograph/HolographFactory.sol::161 => uint32, /* fromChain*/
2022-10-holograph/HolographFactory.sol::178 => uint32, /* toChain*/
2022-10-holograph/HolographFactory.sol::323 => uint8 v,
2022-10-holograph/HolographOperator.sol::208 => uint32 private _operatorTempStorageCounter;
2022-10-holograph/HolographOperator.sol::585 => uint32 toChain,
2022-10-holograph/HolographOperator.sol::665 => uint32,
2022-10-holograph/HolographOperator.sol::697 => uint8(packed >> 248),
2022-10-holograph/HolographOperator.sol::698 => uint16(_blockTime),
2022-10-holograph/HolographOperator.sol::699 => _operatorTempStorage[uint32(packed >> 216)],
2022-10-holograph/HolographOperator.sol::700 => uint40(packed >> 176),
2022-10-holograph/HolographOperator.sol::702 => uint64(packed >> 16),
2022-10-holograph/HolographOperator.sol::704 => uint16(packed >> 160),
2022-10-holograph/HolographOperator.sol::705 => uint16(packed >> 144),
2022-10-holograph/HolographOperator.sol::706 => uint16(packed >> 128),
2022-10-holograph/HolographOperator.sol::707 => uint16(packed >> 112),
2022-10-holograph/HolographOperator.sol::708 => uint16(packed >> 96)
2022-10-holograph/enforcer/HolographERC20.sol::181 => uint8 private _decimals;
2022-10-holograph/enforcer/HolographERC20.sol::229 => uint8 contractDecimals,
2022-10-holograph/enforcer/HolographERC20.sol::380 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
2022-10-holograph/enforcer/HolographERC20.sol::393 => uint32 toChain,
2022-10-holograph/enforcer/HolographERC20.sol::465 => uint8 v,
2022-10-holograph/enforcer/HolographERC721.sol::160 => uint16 private _bps;
2022-10-holograph/enforcer/HolographERC721.sol::248 => uint16 contractBps,
2022-10-holograph/enforcer/HolographERC721.sol::414 => uint32 toChain,
2022-10-holograph/enforcer/HolographERC721.sol::878 => function _chain() private view returns (uint32) {
2022-10-holograph/enforcer/HolographERC721.sol::879 => uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())
2022-10-holograph/enforcer/HolographERC721.sol::884 => return uint32(0);
2022-10-holograph/enforcer/Holographer.sol::150 => (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(
2022-10-holograph/enforcer/Holographer.sol::152 => (uint32, address, bytes32, address)
2022-10-holograph/enforcer/Holographer.sol::205 => function getOriginChain() external view returns (uint32 originChain) {
2022-10-holograph/module/LayerZeroModule.sol::181 => uint16, /* _srcChainId*/
2022-10-holograph/module/LayerZeroModule.sol::183 => uint64, /* _nonce*/
2022-10-holograph/module/LayerZeroModule.sol::230 => uint32 toChain,
2022-10-holograph/module/LayerZeroModule.sol::252 => uint32 toChain,
2022-10-holograph/module/LayerZeroModule.sol::257 => uint16 lzDestChain = uint16(
2022-10-holograph/module/LayerZeroModule.sol::265 => (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
2022-10-holograph/module/LayerZeroModule.sol::270 => uint16(1),
2022-10-holograph/module/LayerZeroModule.sol::278 => uint32 toChain,
2022-10-holograph/module/LayerZeroModule.sol::286 => uint16 lzDestChain = uint16(
```





### [G11] `require()` or `revert()` statements that check input arguments should be at the top of the function

#### Impact
Checks that involve constants should come before checks that involve state variables
#### Findings:
```
2022-10-holograph/enforcer/HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");
2022-10-holograph/enforcer/HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");
2022-10-holograph/enforcer/HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");
2022-10-holograph/enforcer/HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");
2022-10-holograph/enforcer/HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");
2022-10-holograph/enforcer/HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");
2022-10-holograph/enforcer/HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");
```



### [G12] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
2022-10-holograph/HolographBridge.sol::148 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
2022-10-holograph/HolographBridge.sol::163 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/HolographBridge.sol::214 => require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
2022-10-holograph/HolographBridge.sol::233 => require(doNotRevert, "HOLOGRAPH: reverted");
2022-10-holograph/HolographBridge.sol::270 => require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
2022-10-holograph/HolographFactory.sol::144 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/HolographFactory.sol::220 => require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
2022-10-holograph/HolographFactory.sol::228 => require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
2022-10-holograph/HolographOperator.sol::241 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
2022-10-holograph/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
2022-10-holograph/HolographOperator.sol::354 => require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
2022-10-holograph/HolographOperator.sol::368 => require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
2022-10-holograph/HolographOperator.sol::415 => require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
2022-10-holograph/HolographOperator.sol::446 => require(msg.sender == address(this), "HOLOGRAPH: operator only call");
2022-10-holograph/HolographOperator.sol::485 => require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
2022-10-holograph/HolographOperator.sol::591 => require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
2022-10-holograph/HolographOperator.sol::595 => require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
2022-10-holograph/HolographOperator.sol::728 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/HolographOperator.sol::739 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/HolographOperator.sol::756 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/HolographOperator.sol::829 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
2022-10-holograph/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
2022-10-holograph/HolographOperator.sol::863 => require(current <= amount, "HOLOGRAPH: bond amount too small");
2022-10-holograph/HolographOperator.sol::881 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
2022-10-holograph/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/HolographOperator.sol::903 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
2022-10-holograph/HolographOperator.sol::911 => require(_isContract(operator), "HOLOGRAPH: operator not contract");
2022-10-holograph/HolographOperator.sol::915 => require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
2022-10-holograph/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/abstract/ERC20H.sol::117 => require(msg.sender == holographer(), "ERC20: holographer only");
2022-10-holograph/abstract/ERC20H.sol::123 => require(msgSender() == _getOwner(), "ERC20: owner only function");
2022-10-holograph/abstract/ERC20H.sol::125 => require(msg.sender == _getOwner(), "ERC20: owner only function");
2022-10-holograph/abstract/ERC20H.sol::147 => require(!_isInitialized(), "ERC20: already initialized");
2022-10-holograph/abstract/ERC721H.sol::117 => require(msg.sender == holographer(), "ERC721: holographer only");
2022-10-holograph/abstract/ERC721H.sol::123 => require(msgSender() == _getOwner(), "ERC721: owner only function");
2022-10-holograph/abstract/ERC721H.sol::125 => require(msg.sender == _getOwner(), "ERC721: owner only function");
2022-10-holograph/abstract/ERC721H.sol::147 => require(!_isInitialized(), "ERC721: already initialized");
2022-10-holograph/enforcer/HolographERC20.sol::192 => require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");
2022-10-holograph/enforcer/HolographERC20.sol::204 => require(msg.sender == sourceContract, "ERC20: source only call");
2022-10-holograph/enforcer/HolographERC20.sol::219 => require(!_isInitialized(), "ERC20: already initialized");
2022-10-holograph/enforcer/HolographERC20.sol::241 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");
2022-10-holograph/enforcer/HolographERC20.sol::349 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/enforcer/HolographERC20.sol::365 => require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
2022-10-holograph/enforcer/HolographERC20.sol::387 => require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");
2022-10-holograph/enforcer/HolographERC20.sol::400 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/enforcer/HolographERC20.sol::427 => require(newAllowance >= currentAllowance, "ERC20: increased above max value");
2022-10-holograph/enforcer/HolographERC20.sol::445 => require(_isContract(account), "ERC20: operator not contract");
2022-10-holograph/enforcer/HolographERC20.sol::450 => require(balance >= amount, "ERC20: balance check failed");
2022-10-holograph/enforcer/HolographERC20.sol::452 => revert("ERC20: failed getting balance");
2022-10-holograph/enforcer/HolographERC20.sol::469 => require(block.timestamp <= deadline, "ERC20: expired deadline");
2022-10-holograph/enforcer/HolographERC20.sol::482 => require(signer == account, "ERC20: invalid signature");
2022-10-holograph/enforcer/HolographERC20.sol::505 => require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");
2022-10-holograph/enforcer/HolographERC20.sol::529 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/enforcer/HolographERC20.sol::539 => require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");
2022-10-holograph/enforcer/HolographERC20.sol::599 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/enforcer/HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::629 => require(accountBalance >= amount, "ERC20: amount exceeds balance");
2022-10-holograph/enforcer/HolographERC20.sol::645 => require(erc165support, "ERC20: no ERC165 support");
2022-10-holograph/enforcer/HolographERC20.sol::653 => revert("ERC20: non ERC20Receiver");
2022-10-holograph/enforcer/HolographERC20.sol::656 => revert(add(32, reason), mload(reason))
2022-10-holograph/enforcer/HolographERC20.sol::661 => revert("ERC20: eip-4524 not supported");
2022-10-holograph/enforcer/HolographERC20.sol::665 => revert("ERC20: no ERC165 support");
2022-10-holograph/enforcer/HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");
2022-10-holograph/enforcer/HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");
2022-10-holograph/enforcer/HolographERC20.sol::698 => require(accountBalance >= amount, "ERC20: amount exceeds balance");
2022-10-holograph/enforcer/HolographERC721.sol::212 => require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");
2022-10-holograph/enforcer/HolographERC721.sol::224 => require(msg.sender == sourceContract, "ERC721: source only call");
2022-10-holograph/enforcer/HolographERC721.sol::239 => require(!_isInitialized(), "ERC721: already initialized");
2022-10-holograph/enforcer/HolographERC721.sol::258 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
2022-10-holograph/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
2022-10-holograph/enforcer/HolographERC721.sol::323 => require(_exists(tokenId), "ERC721: token does not exist");
2022-10-holograph/enforcer/HolographERC721.sol::370 => require(to != tokenOwner, "ERC721: cannot approve self");
2022-10-holograph/enforcer/HolographERC721.sol::371 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/enforcer/HolographERC721.sol::373 => require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));
2022-10-holograph/enforcer/HolographERC721.sol::378 => require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
2022-10-holograph/enforcer/HolographERC721.sol::388 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/enforcer/HolographERC721.sol::404 => require(!_exists(tokenId), "ERC721: token already exists");
2022-10-holograph/enforcer/HolographERC721.sol::408 => require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");
2022-10-holograph/enforcer/HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");
2022-10-holograph/enforcer/HolographERC721.sol::420 => require(_isApproved(sender, tokenId), "ERC721: sender not approved");
2022-10-holograph/enforcer/HolographERC721.sol::421 => require(from == _tokenOwner[tokenId], "ERC721: from is not owner");
2022-10-holograph/enforcer/HolographERC721.sol::458 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/enforcer/HolographERC721.sol::484 => require(to != msg.sender, "ERC721: cannot approve self");
2022-10-holograph/enforcer/HolographERC721.sol::513 => require(!_burnedTokens[token], "ERC721: can't mint burned token");
2022-10-holograph/enforcer/HolographERC721.sol::622 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/enforcer/HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");
2022-10-holograph/enforcer/HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");
2022-10-holograph/enforcer/HolographERC721.sol::700 => require(index < _allTokens.length, "ERC721: index out of bounds");
2022-10-holograph/enforcer/HolographERC721.sol::729 => require(index < balanceOf(wallet), "ERC721: index out of bounds");
2022-10-holograph/enforcer/HolographERC721.sol::757 => require(_isContract(_operator), "ERC721: operator not contract");
2022-10-holograph/enforcer/HolographERC721.sol::762 => require(tokenOwner == address(this), "ERC721: contract not token owner");
2022-10-holograph/enforcer/HolographERC721.sol::764 => revert("ERC721: token does not exist");
2022-10-holograph/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
2022-10-holograph/enforcer/HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");
2022-10-holograph/enforcer/HolographERC721.sol::817 => require(!_exists(tokenId), "ERC721: token already exists");
2022-10-holograph/enforcer/HolographERC721.sol::818 => require(!_burnedTokens[tokenId], "ERC721: token has been burned");
2022-10-holograph/enforcer/HolographERC721.sol::869 => require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
2022-10-holograph/enforcer/HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");
2022-10-holograph/enforcer/HolographERC721.sol::906 => require(_exists(tokenId), "ERC721: token does not exist");
2022-10-holograph/enforcer/Holographer.sol::148 => require(!_isInitialized(), "HOLOGRAPHER: already initialized");
2022-10-holograph/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");
2022-10-holograph/enforcer/Holographer.sol::236 => revert(0, returndatasizer not an owner");
2022-10-holograph/enforcer/PA1D.sol::174 => require(!_isInitialized(), "PA1D: already initialized");
2022-10-holograph/enforcer/PA1D.sol::190 => require(initialized == 0, "PA1D: already initialized");
2022-10-holograph/enforcer/PA1D.sol::390 => require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
2022-10-holograph/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/enforcer/PA1D.sol::460 => require(matched, "PA1D: sender not authorized");
2022-10-holograph/enforcer/PA1D.sol::472 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
2022-10-holograph/enforcer/PA1D.sol::477 => require(totalBp == 10000, "PA1D: bps down't equal 10000");
2022-10-holograph/module/LayerZeroModule.sol::159 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/module/LayerZeroModule.sol::235 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
s
```



### [G13] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
2022-10-holograph/HolographBridge.sol::452 => function setFactory(address factory) external onlyAdmin {
2022-10-holograph/HolographBridge.sol::472 => function setHolograph(address holograph) external onlyAdmin {
2022-10-holograph/HolographBridge.sol::502 => function setOperator(address operator) external onlyAdmin {
2022-10-holograph/HolographBridge.sol::522 => function setRegistry(address registry) external onlyAdmin {
2022-10-holograph/HolographFactory.sol::280 => function setHolograph(address holograph) external onlyAdmin {
2022-10-holograph/HolographFactory.sol::300 => function setRegistry(address registry) external onlyAdmin {
2022-10-holograph/HolographOperator.sol::949 => function setBridge(address bridge) external onlyAdmin {
2022-10-holograph/HolographOperator.sol::969 => function setHolograph(address holograph) external onlyAdmin {
2022-10-holograph/HolographOperator.sol::989 => function setInterfaces(address interfaces) external onlyAdmin {
2022-10-holograph/HolographOperator.sol::1009 => function setMessagingModule(address messagingModule) external onlyAdmin {
2022-10-holograph/HolographOperator.sol::1029 => function setRegistry(address registry) external onlyAdmin {
2022-10-holograph/HolographOperator.sol::1049 => function setUtilityToken(address utilityToken) external onlyAdmin {
2022-10-holograph/enforcer/HolographERC20.sol::380 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
2022-10-holograph/enforcer/HolographERC20.sol::415 => function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
2022-10-holograph/enforcer/HolographERC20.sol::549 => function sourceBurn(address from, uint256 amount) external onlySource {
2022-10-holograph/enforcer/HolographERC20.sol::556 => function sourceMint(address to, uint256 amount) external onlySource {
2022-10-holograph/enforcer/HolographERC20.sol::563 => function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
2022-10-holograph/enforcer/HolographERC721.sol::399 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
2022-10-holograph/enforcer/HolographERC721.sol::500 => function sourceBurn(uint256 tokenId) external onlySource {
2022-10-holograph/enforcer/HolographERC721.sol::508 => function sourceMint(address to, uint224 tokenId) external onlySource {
2022-10-holograph/enforcer/HolographERC721.sol::520 => function sourceGetChainPrepend() external view onlySource returns (uint256) {
2022-10-holograph/enforcer/HolographERC721.sol::577 => function sourceTransfer(address to, uint256 tokenId) external onlySource {
2022-10-holograph/enforcer/PA1D.sol::471 => function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
2022-10-holograph/module/LayerZeroModule.sol::320 => function setBridge(address bridge) external onlyAdmin {
2022-10-holograph/module/LayerZeroModule.sol::340 => function setInterfaces(address interfaces) external onlyAdmin {
2022-10-holograph/module/LayerZeroModule.sol::360 => function setLZEndpoint(address lZEndpoint) external onlyAdmin {
2022-10-holograph/module/LayerZeroModule.sol::380 => function setOperator(address operator) external onlyAdmin {
2022-10-holograph/module/LayerZeroModule.sol::441 => function setBaseGas(uint256 baseGas) external onlyAdmin {
2022-10-holograph/module/LayerZeroModule.sol::470 => function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```



### [G14] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.16 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
2022-10-holograph/HolographBridge.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/HolographFactory.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/HolographOperator.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/abstract/ERC20H.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/abstract/ERC721H.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/enforcer/HolographERC20.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/enforcer/HolographERC721.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/enforcer/Holographer.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/enforcer/PA1D.sol::102 => pragma solidity 0.8.13;
2022-10-holograph/module/LayerZeroModule.sol::102 => pragma solidity 0.8.13;
```



### [G15] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.
#### Findings:
```
2022-10-holograph/HolographBridge.sol::162 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/HolographFactory.sol::143 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/HolographOperator.sol::240 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/abstract/ERC20H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
2022-10-holograph/abstract/ERC721H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
2022-10-holograph/enforcer/HolographERC20.sol::218 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/enforcer/Holographer.sol::147 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/enforcer/PA1D.sol::173 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/enforcer/PA1D.sol::185 => function initPA1D(bytes memory initPayload) external returns (bytes4) {
2022-10-holograph/enforcer/PA1D.sol::316 => function _setPayoutAddresses(address payable[] memory addresses) private {
2022-10-holograph/enforcer/PA1D.sol::349 => function _setPayoutBps(uint256[] memory bps) private {
2022-10-holograph/enforcer/PA1D.sol::365 => function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {
2022-10-holograph/enforcer/PA1D.sol::372 => function _setTokenAddress(string memory tokenName, address tokenAddress) private {
2022-10-holograph/enforcer/PA1D.sol::426 => function _payoutTokens(address[] memory tokenAddresses) private {
2022-10-holograph/enforcer/PA1D.sol::471 => function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
2022-10-holograph/enforcer/PA1D.sol::517 => function getTokensPayout(address[] memory tokenAddresses) public {
2022-10-holograph/enforcer/PA1D.sol::683 => function getTokenAddress(string memory tokenName) public view returns (address) {
2022-10-holograph/module/LayerZeroModule.sol::158 => function init(bytes memory initPayload) external override returns (bytes4) {
```



### [G16] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done in some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value check here
#### Findings:
```
2022-10-holograph/HolographOperator.sol::400 => _utilityToken().transfer(job.operator, leftovers);
2022-10-holograph/HolographOperator.sol::596 => payable(hToken).transfer(hlgFee);
2022-10-holograph/enforcer/PA1D.sol::396 => addresses[i].transfer(sending);
```




### [G17] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
2022-10-holograph/HolographOperator.sol::193 => mapping(bytes32 => uint256) private _operatorJobs;
2022-10-holograph/HolographOperator.sol::198 => mapping(bytes32 => bool) private _failedJobs;
2022-10-holograph/HolographOperator.sol::203 => mapping(uint256 => address) private _operatorTempStorage;
2022-10-holograph/HolographOperator.sol::218 => mapping(address => uint256) private _bondedOperators;
2022-10-holograph/HolographOperator.sol::223 => mapping(address => uint256) private _operatorPodIndex;
2022-10-holograph/HolographOperator.sol::228 => mapping(address => uint256) private _bondedAmounts;

2022-10-holograph/enforcer/HolographERC20.sol::156 => mapping(address => uint256) private _balances;
2022-10-holograph/enforcer/HolographERC20.sol::161 => mapping(address => mapping(address => uint256)) private _allowances;
2022-10-holograph/enforcer/HolographERC20.sol::186 => mapping(address => uint256) private _nonces;
2022-10-holograph/enforcer/HolographERC721.sol::170 => mapping(uint256 => uint256) private _ownedTokensIndex;
2022-10-holograph/enforcer/HolographERC721.sol::175 => mapping(uint256 => address) private _tokenOwner;
2022-10-holograph/enforcer/HolographERC721.sol::180 => mapping(uint256 => address) private _tokenApprovals;
2022-10-holograph/enforcer/HolographERC721.sol::185 => mapping(address => uint256) private _ownedTokensCount;
2022-10-holograph/enforcer/HolographERC721.sol::190 => mapping(address => uint256[]) private _ownedTokens;
2022-10-holograph/enforcer/HolographERC721.sol::196 => mapping(address => mapping(address => bool)) private _operatorApprovals;
2022-10-holograph/enforcer/HolographERC721.sol::201 => mapping(uint256 => uint256) private _allTokensIndex;
2022-10-holograph/enforcer/HolographERC721.sol::206 => mapping(uint256 => bool) private _burnedTokens;
```


### [G18] Using `bools` for storage incurs overhead


#### Findings:
```
2022-10-holograph/HolographOperator.sol::198 => mapping(bytes32 => bool) private _failedJobs;
2022-10-holograph/enforcer/HolographERC721.sol::196 => mapping(address => mapping(address => bool)) private _operatorApprovals;
2022-10-holograph/enforcer/HolographERC721.sol::206 => mapping(uint256 => bool) private _burnedTokens;
```




### [G19] Empty blocks should be removed or emit something

#### Impact
The code should be refactored such that they no longer exist, or the 
block should do something useful, such as emitting an event or 
reverting. If the contract is meant to be extended, the contract should 
be abstract and the function signatures be added without 
any default implementation. If the block is an empty if-statement block 
to avoid doing subsequent checks in the else-if/else conditions, the 
else-if/else conditions should be nested under the negation of the 
if-statement, because they involve different classes of checks, which 
may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})
#### Findings:
```
2022-10-holograph/HolographBridge.sol::155 => constructor() {}
2022-10-holograph/HolographFactory.sol::136 => constructor() {}
2022-10-holograph/HolographOperator.sol::233 => constructor() {}
2022-10-holograph/HolographOperator.sol::1209 => receive() external payable {}
2022-10-holograph/abstract/ERC20H.sol::133 => constructor() {}
2022-10-holograph/abstract/ERC20H.sol::212 => receive() external payable {}
2022-10-holograph/abstract/ERC721H.sol::133 => constructor() {}
2022-10-holograph/abstract/ERC721H.sol::212 => receive() external payable {}
2022-10-holograph/enforcer/HolographERC20.sol::211 => constructor() {}
2022-10-holograph/enforcer/HolographERC20.sol::251 => receive() external payable {}
2022-10-holograph/enforcer/HolographERC721.sol::231 => constructor() {}
2022-10-holograph/enforcer/HolographERC721.sol::962 => receive() external payable {}
2022-10-holograph/enforcer/Holographer.sol::140 => constructor() {}
2022-10-holograph/enforcer/Holographer.sol::223 => receive() external payable {}
2022-10-holograph/enforcer/PA1D.sol::166 => constructor() {}
2022-10-holograph/module/LayerZeroModule.sol::151 => constructor() {}
```



### [G20] Remove or replace unused state variables

#### Impact
Saves a storage slot. If the variable is assigned a non-zero value, 
saves Gsset (20000 gas). If it’s assigned a zero value, saves Gsreset 
(2900 gas). If the variable remains unassigned, there is no gas savings 
unless the variable is public, in which case the 
compiler-generated non-payable getter deployment cost is saved. If the 
state variable is overriding an interface’s public function, mark the 
variable as constant or immutable so that it does not use a storage slot
#### Findings:
```
2022-10-holograph/enforcer/HolographERC721.sol::165 => uint256[] private _allTokens;
2022-10-holograph/enforcer/HolographERC721.sol::190 => mapping(address => uint256[]) private _ownedTokens;
```



### [G21] `keccak256()` should only need to be called on a specific string literal once

#### Impact
It should be saved to an immutable variable, and the variable used 
instead. If the hash is being used as a part of a function selector, the
 cast to bytes4 should also only be done once
#### Findings:
```
2022-10-holograph/HolographBridge.sol::388 => bytes memory encodedData = abi.encodeWithSelector(
2022-10-holograph/HolographOperator.sol::597 => bytes memory encodedData = abi.encodeWithSelector(
2022-10-holograph/module/LayerZeroModule.sol::269 => bytes memory adapterParams = abi.encodePacked(
```



### [G22] `abi.encode()` is less efficient than abi.encodePacked()



#### Findings:
```
2022-10-holograph/HolographFactory.sol::252 => abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
2022-10-holograph/enforcer/HolographERC20.sol::471 => abi.encode(
2022-10-holograph/enforcer/HolographERC721.sol::260 => abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))
2022-10-holograph/enforcer/HolographERC721.sol::426 => return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```



### [G23] Optimize names to save gas

#### Impact
public/external function names and public member variable names can be optimized to save gas. See [this
 link](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) for an example of how it works. Below are the interfaces/abstract 
contracts that can be optimized so that the most frequently-called 
functions use the least amount of gas possible during method lookup. 
Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-n

#### Findings:
```
2022-10-holograph/abstract/ERC20H.sol::106 => abstract contract ERC20H is Initializable {
2022-10-holograph/abstract/ERC721H.sol::106 => abstract contract ERC721H is Initializable {
```


