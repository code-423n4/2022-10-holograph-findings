### G-01 IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

*Number of Instances Identified: 16*

If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < numIterations; ++i) {` should be replaced with `for (uint256 i; i < numIterations; ++i) {`

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```
380: uint256 fee = 0;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
310: uint256 gasLimit = 0;
311: uint256 gasPrice = 0;
781: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
564: for (uint256 i = 0; i < wallets.length; i++ ) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```


### G-02 ++I or I++ SHOULD BE UNCHECKED{++I} or UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

*Number of Instances Identified: 14*

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
781: for (uint256 i = 0; i < length; i++) {
871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```


https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
564: for (uint256 i = 0; i < wallets.length; i++ ) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```



### G-03 ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 (SAME FOR --I VS I-- OR I -= 1)

*Number of Instances Identified: 19*

Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

-   `i += 1` is the most expensive form
-   `i++` costs 6 gas less than `i += 1`
-   `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

-   `i -= 1` is the most expensive form
-   `i--` costs 11 gas less than `i -= 1`
-   `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)



https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
520: podSize--;
760: pod--;
781: for (uint256 i = 0; i < length; i++) {
871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```


https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
564: for (uint256 i = 0; i < wallets.length; i++ ) {
713: _nonces[account]++;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) {
779: _ownedTokensCount[to]++;
842: _ownedTokensCount[from]--;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```


### G-04 USE CUSTOM ERRORS RATHER THAN REVERT() or REQUIRE() STRINGS TO SAVE GAS

*Number of Instances Identified: 167*

Custom errors are available from solidity version 0.8.4. Custom errors save gas each time they’re hitby [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

Several instances include the following :


https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol


```
148: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
163: require(!_isInitialized(), "HOLOGRAPH: already initialized");
205: "HOLOGRAPH: not holographed"
214: require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
227: "HOLOGRAPH: hToken mint failed"
233: require(doNotRevert, "HOLOGRAPH: reverted");
257: "HOLOGRAPH: not holographed"
270: require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
354: "HOLOGRAPH: not holographed"
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
144: require(!_isInitialized(), "HOLOGRAPH: already initialized");
220: require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
228: require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
254: "initialization failed"
863: require(current <= amount, "HOLOGRAPH: bond amount too small");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
241: require(!_isInitialized(), "HOLOGRAPH: already initialized");
309: require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
350: require(timeDifference > 0, "HOLOGRAPH: operator has time");
354: require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
368: require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
415: require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
446: require(msg.sender == address(this), "HOLOGRAPH: operator only call");
485: require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
591: require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
595: require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
739: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
829: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
881: require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
903: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
911: require(_isContract(operator), "HOLOGRAPH: operator not contract");
915: require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
932: require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
117: require(msg.sender == holographer(), "ERC20: holographer only");
123: require(msgSender() == _getOwner(), "ERC20: owner only function");
125: require(msg.sender == _getOwner(), "ERC20: owner only function");
147: require(!_isInitialized(), "ERC20: already initialized");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol

```
117: require(msg.sender == holographer(), "ERC721: holographer only");
123: require(msgSender() == _getOwner(), "ERC721: owner only function");
125: require(msg.sender == _getOwner(), "ERC721: owner only function");
```


### G-05 DUPLICATED REQUIRE() or REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

Saves deployment costs

*Number of Instances Identified: 21*

Some of the identified instances are:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```
163: require(!_isInitialized(), "HOLOGRAPH: already initialized");
203:    require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
144: require(!_isInitialized(), "HOLOGRAPH: already initialized");
```


### G-06 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Number of Instances Identified: 10*

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
328: v += 27;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
378: _bondedAmounts[job.operator] -= amount;
382: _bondedAmounts[msg.sender] += amount;
834: _bondedAmounts[operator] += amount;
1175: position -= threshold;
1177: current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```
633: _totalSupply -= amount;
685: _totalSupply += amount;
686: _balances[to] += amount;
702: _balances[recipient] += amount;
```


### G-07 USAGE OF UINTS or INTS SMALLER THAN 32 BYTES - 256 BITS INCURS OVERHEAD

*Number of Instances Identified: 78*

> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

Some of the identified instances are as follows:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```
192: uint32 fromChain,
246: uint32 toChain
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
161: uint32,
```


### G-08 USING BOOLS FOR STORAGE INCURS OVERHEAD

*Number of Instances Identified: 4*

```
// Booleans are more expensive than uint256 or any type that takes up a full    // word because each write operation emits an extra SLOAD to first read the    // slot's contents, replace the bits taken up by the boolean, and then write    // back. This is the compiler's defense against contract upgrades and    // pointer aliasing, and it cannot be disabled.
```

[https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)  
Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (**[100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
198: mapping(bytes32 => bool) private _failedJobs;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
196: mapping(address => mapping(address => bool)) private _operatorApprovals;
206: mapping(uint256 => bool) private _burnedTokens;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```
451: bool matched;
```


### G-09 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

*Number of Instances Identified: 4*

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
181:  ) external pure returns (bytes4 selector, bytes memory data) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
717: function getTotalPods() external view returns (uint256 totalPods) {
804: function getBondedAmount(address operator) external view returns (uint256 amount) {
814: function getBondedPod(address operator) external view returns (uint256 pod) {
```


### G-10 SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

*Number of Instances Identified: 4*

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol

```
166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
464: require(
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector),
        "ERC721: onERC721Received fail"
      );
```

### G-11 USING `> 0` COSTS MORE GAS THAN `!= 0` WHEN USED ON A `UINT` IN A `REQUIRE()` STATEMENT

*Number of Instances Identified: 3*

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
309: require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
350: require(timeDifference > 0, "HOLOGRAPH: operator has time");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
815: require(tokenId > 0, "ERC721: token id cannot be zero");
```


### G-12 UNCHECKING ARITHMETICS OPERATIONS THAT CANT UNDERFLOW or OVERFLOW

*Number of Instances Identified: 3*

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an `unchecked` block: [Solidity Doc](https://docs.soliditylang.org/en/v0.8.10/control-structures.html#checked-or-unchecked-arithmetic)

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol

```
520: podSize--;
```

### G-13 INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol

```
203: function _setOwner(address ownerAddress) internal {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol

```
203: function _setOwner(address ownerAddress) internal {
```