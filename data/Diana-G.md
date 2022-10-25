
## G-01 IT COSTS MORE GAS TO INITIALIZE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < length; i++)` should be replaced with `for (uint256 i; i < length; i++)`

_There are 17 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L380

```
File: contracts/HolographBridge.sol

380: uint256 fee = 0;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

310: uint256 gasLimit = 0;
311: uint256 gasPrice = 0;
781: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564

```
File: contracts/enforcer/HolographERC20.sol

564: for (uint256 i = 0; i < wallets.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
File: contracts/enforcer/PA1D.sol

307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
432: for (uint256 t = 0; t < tokenAddresses.length; t++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```

-------------

## G-02 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

_There are 35 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```
File: contracts/HolographBridge.sol

192: uint32 fromChain,
246: uint32 toChain,
298: uint32 toChain,
343: uint32 toChain,
419: uint32;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
File: contracts/HolographFactory.sol

161: uint32,
178: uint32,
323: uint8 v,
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

208: uint32 private _operatorTempStorageCounter;
585: uint32 toChain,
665: uint32,
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol

```
File: contracts/enforcer/Holographer.sol

150: (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(
152: (uint32, address, bytes32, address)
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
File: contracts/enforcer/HolographERC20.sol

181: uint8 private _decimals;
229: uint8 contractDecimals,
273: function decimals() public view returns (uint8) {
380: function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
393: uint32 toChain,
465: uint8 v,
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

160: uint16 private _bps;
248: uint16 contractBps,
399: function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
414: uint32 toChain,
508: function sourceMint(address to, uint224 tokenId) external onlySource {
878: function _chain() private view returns (uint32) {
879: uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```
File: contracts/module/LayerZeroModule.sol

181: uint16,
183: uint64,
230: uint32 toChain,
252: uint32 toChain,
265: (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
278: uint32 toChain,
289: (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
296: function _getPricing(LayerZeroOverrides lz, uint16 lzDestChain)
299: returns (uint128 dstPriceRatio, uint128 dstGasPriceInWei)
```

------------

## G-03 USE CUSTOM ERRORS RATHER THAN REVERT() OR REQUIRE() STRINGS TO SAVE DEPLOYMENT GAS

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version

Source: [https://blog.soliditylang.org/2021/04/21/custom-errors/](https://blog.soliditylang.org/2021/04/21/custom-errors/):

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the `error` statement, which can be used inside and outside of contracts (including interfaces and libraries).

_There are multiple (120+) instances of this issue_

Some instances include:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```
File: contracts/module/LayerZeroModule.sol

159: require(!_isInitialized(), "HOLOGRAPH: already initialized");
235: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
```
-----------

## G-04 x += y COSTS MORE GAS THAN x = x + y FOR STATE VARIABLES (SAME APPLIES FOR x -= y , x = x - y)

_There are 10 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
File: contracts/HolographFactory.sol

328: v += 27;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

378: _bondedAmounts[job.operator] -= amount;
382: _bondedAmounts[msg.sender] += amount;
834: _bondedAmounts[operator] += amount;
1175: position -= threshold;
1177: current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
File: contracts/enforcer/HolographERC20.sol

633: _totalSupply -= amount;
685: _totalSupply += amount;
686: _balances[to] += amount;
702: _balances[recipient] += amount;
```
----------

## G-05 ++i COSTS LESS GAS THAN i++ ESPECIALLY WHEN IT'S USED IN FOR LOOPS (SAME APPLIES FOR --i, i-- TOO)

Saves 6 gas _PER LOOP_

Prefix increments are cheaper than postfix increments.  

Further more, using unchecked `{++i}` is even more gas efficient, and the gas saving accumulates every iteration and can make a real change  

_There are 20 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

520: podSize--;
760: pod--;
781: for (uint256 i = 0; i < length; i++) {
871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
File: contracts/enforcer/HolographERC20.sol

564: for (uint256 i = 0; i < wallets.length; i++) {
713: _nonces[account]++;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) {
779: _ownedTokensCount[to]++;
842: _ownedTokensCount[from]--;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
File: contracts/enforcer/PA1D.sol

307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
432: for (uint256 t = 0; t < tokenAddresses.length; t++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```

-------------

## G-06 INCREMENTS CAN BE UNCHECKED

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline

Prior to Solidity 0.8.0, arithmetic operations would always wrap in case of under- or overflow leading to widespread use of libraries that introduce additional checks.

Since Solidity 0.8.0, all arithmetic operations revert on over- and underflow by default, thus making the use of these libraries unnecessary.

To obtain the previous behaviour, an unchecked block can be used

_There are 16 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

520: podSize--;
781: for (uint256 i = 0; i < length; i++) {
871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
File: contracts/enforcer/HolographERC20.sol

564: for (uint256 i = 0; i < wallets.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) 
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
File: contracts/enforcer/PA1D.sol

307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
432: for (uint256 t = 0; t < tokenAddresses.length; t++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```
-------

## G-07 USING BOOLS FOR STORAGE INCURS OVERHEAD

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

_There are 4 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

198: mapping(bytes32 => bool) private _failedJobs;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

196: mapping(address => mapping(address => bool)) private _operatorApprovals;
206: mapping(uint256 => bool) private _burnedTokens;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
File: contracts/enforcer/PA1D.sol

451: bool matched;
```
-------

## G-08 USING GREATER THAN 0 COSTS MORE THAN !=0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas). This change saves 6 gas per instance

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

309: require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
350: require(timeDifference > 0, "HOLOGRAPH: operator has time");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

815: require(tokenId > 0, "ERC721: token id cannot be zero");
```
-----------

## G-09 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

_There are 10 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
File: contracts/HolographFactory.sol

177-181: function bridgeOut(
    uint32, /* toChain*/
    address, /* sender*/
    bytes calldata payload
  ) external pure returns (bytes4 selector, bytes memory data) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

717: function getTotalPods() external view returns (uint256 totalPods) {
804: function getBondedAmount(address operator) external view returns (uint256 amount) {
815: function getBondedPod(address operator) external view returns (uint256 pod) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
File: contracts/enforcer/HolographERC20.sol

392-396: function bridgeOut(
    uint32 toChain,
    address sender,
    bytes calldata payload
  ) external onlyBridge returns (bytes4 selector, bytes memory data) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

413-417: function bridgeOut(
    uint32 toChain,
    address sender,
    bytes calldata payload
  ) external onlyBridge returns (bytes4 selector, bytes memory data) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
File: contracts/enforcer/PA1D.sol

674: function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```
File: contracts/module/LayerZeroModule.sol

251-256: function getMessageFee(
    uint32 toChain,
    uint256 gasLimit,
    uint256 gasPrice,
    bytes calldata crossChainPayload
  ) external view returns (uint256 hlgFee, uint256 msgFee) {
277-281: function getHlgFee(
    uint32 toChain,
    uint256 gasLimit,
    uint256 gasPrice
  ) external view returns (uint256 hlgFee) {
296-299: function _getPricing(LayerZeroOverrides lz, uint16 lzDestChain)
    private
    view
    returns (uint128 dstPriceRatio, uint128 dstGasPriceInWei)
```
-----------

## G-10 INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

_There are 2 instances of this issue

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L203

```
File: contracts/abstract/ERC20H.sol

203: function _setOwner(address ownerAddress) internal {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L203

```
File: contracts/abstract/ERC721H.sol

203: function _setOwner(address ownerAddress) internal {
```
---------

## G-11 DUPLICATED REQUIRE() OR REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

_There are 6 instances of this issue

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L163

```
File: contracts/HolographBridge.sol

163: require(!_isInitialized(), "HOLOGRAPH: already initialized");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L144

```
File: contracts/HolographFactory.sol

144: require(!_isInitialized(), "HOLOGRAPH: already initialized");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

241: require(!_isInitialized(), "HOLOGRAPH: already initialized");
728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
739: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
```

------------------

## G-12 SPLITTING REQURE() STATEMENTS THAT USE && SAVES GAS

_There are 4 instances of this issue

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857

```
File: contracts/HolographOperator.sol

857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166

```
File: contracts/enforcer/Holographer.sol

166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
464-470:  require(
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector),
        "ERC721: onERC721Received fail"
      );
```

----------

