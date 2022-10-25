## 1. Increments can be unchecked

In [Solidity 0.8+](https://github.com/ethereum/solidity/issues/10695), there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline. This saves 30-40 gas PER LOOP.

```
781	for (uint256 i = 0; i < length; i++) {
871	for (uint256 i = _operatorPods.length; i <= pod; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
307	for (uint256 i = 0; i < length; i++) {
323	for (uint256 i = 0; i < length; i++) {
340	for (uint256 i = 0; i < length; i++) {
356	for (uint256 i = 0; i < length; i++) {
394	for (uint256 i = 0; i < length; i++) {
414	for (uint256 i = 0; i < length; i++) {
432	for (uint256 t = 0; t < tokenAddresses.length; t++) {
437	for (uint256 i = 0; i < addresses.length; i++) {
454	for (uint256 i = 0; i < addresses.length; i++) {
474	for (uint256 i = 0; i < addresses.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
```
357	for (uint256 i = 0; i < length; i++) {
716	for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol
```
564	for (uint256 i = 0; i < wallets.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

## 2. `require()`/`revert()` strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 [incurs an MSTORE](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs 3 gas

```
411	require(balance > 10000, "PA1D: Not enough tokens to transfer");
435	require(balance > 10000, "PA1D: Not enough tokens to transfer");
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol


## 3. Splitting `require()` statements that use && saves gas


Instead of using the && operator in a single require statement to check multiple conditions,use multiple require statements with 1 condition per require statement.

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

```
857	require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
166	require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol
```
263	require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
...
464	require(
465		(ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
466		ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
467		ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
468		ERC721TokenReceiver.onERC721Received.selector),
469		"ERC721: onERC721Received fail"
470	);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol


## 4. Usage of `uint/int` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.


## 5. Empty blocks should be removed or emit something


The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be `abstract` and the function signatures be added without any default implementation. If the block is an empty `if`-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (`if(x){}else if(y){...}else{...}` => `if(!x){if(y){...}else{...}}`). Empty `receive()`/`fallback()` payable functions that are not used, can be removed to save deployment gas.

```
155	constructor() {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol
```
233	constructor() {}
1209	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
136	constructor() {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol
```
151	constructor() {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol
```
149	constructor() {}
223	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol
```
166	 constructor() {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
```
231	constructor() {}
962	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol
```
211	constructor() {}
251	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol
```
133	constructor() {}
212	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol
```
133	constructor() {}
212	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol


## 6. Not using the named return variables when a function returns, wastes deployment gas

```
718	return _operatorPods.length;
729	return _operatorPods[pod - 1].length;
1191	return (random + uint256(blockhash(block.number - n))) % podSize;
1203	return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
152	return InitializableInterface.init.selector;
169	return Holographable.bridgeIn.selector;
314	return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
333	return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol
```
274	return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
293	return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol
```
206	return (msg.sender == getOwner() ||
551	return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
553	return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
641	return (_getDefaultBp() * amount) / 10000;
643	return (_getBp(tokenId) * amount) / 10000;
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
```
328	return sourceContract.tokenURI(tokenId);
337	return _ownedTokens[wallet];
521	return uint256(bytes32(abi.encodePacked(_chain(), uint224(0))));
653	return 0;
678	return _operatorApprovals[wallet][operator];
908	return (spender == tokenOwner || _tokenApprovals[tokenId] == spender || _operatorApprovals[tokenOwner][spender]);
916	return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
1003	return ((_eventConfig >> uint256(_eventName)) & uint256(1) == 1 ? true : false);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol
```
262	returndatacopy(0, 0, returndatasize())
298	return _allowances[account][spender];
302	return _balances[account];
315	return _nonces[account];
334	return true;
360	return true;
377	return true;
409	return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));
417	return HolographERC20Interface.holographBridgeMint.selector;
436	return true;
493	return safeTransfer(recipient, amount, "");
517	return safeTransferFrom(account, recipient, amount, "");
543	return true;
721	return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
755	return ((_eventConfig >> uint256(_eventName)) & uint256(1) == 1 ? true : false);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol
```
153	return InitializableInterface.init.selector;
175	return false;
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol
```
153	return InitializableInterface.init.selector;
175	return false;
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol

## 7. Bytes constants are more efficient than string constants


From the Solidity doc:

* If you can limit the length to a certain number of bytes, always use one of bytes1 to bytes32 because they are much cheaper.

[Why do Solidity examples use bytes32 type instead of string](https://ethereum.stackexchange.com/questions/3795/why-do-solidity-examples-use-bytes32-type-instead-of-string)?

* bytes32 uses less gas because it fits in a single word of the EVM, and string is a dynamically sized-type which has current limitations in Solidity (such as can’t be returned from a function to a contract).

If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity. Basically, any fixed size variable in solidity is cheaper than variable size. That will save gas on the contract.

Instances of string constant that can be replaced by bytes(1..32) constant


```
142	string constant _bpString = "eip1967.Holograph.PA1D.bp";
143	string constant _receiverString = "eip1967.Holograph.PA1D.receiver";
144	string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol


## 8. Functions guaranteed to revert when called by normal users can be marked `payable`


If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
452	function setFactory(address factory) external onlyAdmin {
472	function setHolograph(address holograph) external onlyAdmin {
502	function setOperator(address operator) external onlyAdmin {
522	function setRegistry(address registry) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol
```
278	function resetOperator(
949	function setBridge(address bridge) external onlyAdmin {
969	function setHolograph(address holograph) external onlyAdmin {
989	function setInterfaces(address interfaces) external onlyAdmin {
1009	function setMessagingModule(address messagingModule) external onlyAdmin {
1029	function setRegistry(address registry) external onlyAdmin {
1049	function setUtilityToken(address utilityToken) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
280	function setHolograph(address holograph) external onlyAdmin {
300	function setRegistry(address registry) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol
```
320	function setBridge(address bridge) external onlyAdmin {
340	function setInterfaces(address interfaces) external onlyAdmin {
360	function setLZEndpoint(address lZEndpoint) external onlyAdmin {
380	function setOperator(address operator) external onlyAdmin {
441	function setBaseGas(uint256 baseGas) external onlyAdmin {
470	function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol


##9. Multiple `address`/`ID` mappings can be combined into a single `mapping `of an `address`/`ID` to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key’s keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

```
218	mapping(address => uint256) private _bondedOperators;
223	mapping(address => uint256) private _operatorPodIndex;
228	mapping(address => uint256) private _bondedAmounts;
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
156	mapping(address => uint256) private _balances;
161	mapping(address => mapping(address => uint256)) private _allowances;
186	mapping(address => uint256) private _nonces;
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

