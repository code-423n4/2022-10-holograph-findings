## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
2022-10-holograph/contracts/HolographBridge.sol::380 => uint256 fee = 0;
2022-10-holograph/contracts/HolographOperator.sol::310 => uint256 gasLimit = 0;
2022-10-holograph/contracts/HolographOperator.sol::311 => uint256 gasPrice = 0;
2022-10-holograph/contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
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
```

## [G-02] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
2022-10-holograph/contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```

## [G-03] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
2022-10-holograph/contracts/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
2022-10-holograph/contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
```

## [G-04] Long Revert Strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

```
2022-10-holograph/contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

## [G-05] Use calldata instead of memory

Use calldata instead of memory for function parameters saves gas if the function argument is only read.

```
2022-10-holograph/contracts/HolographBridge.sol::162 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/HolographFactory.sol::143 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/HolographOperator.sol::240 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/abstract/ERC20H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
2022-10-holograph/contracts/abstract/ERC721H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::218 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::238 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/enforcer/Holographer.sol::147 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/enforcer/PA1D.sol::173 => function init(bytes memory initPayload) external override returns (bytes4) {
2022-10-holograph/contracts/enforcer/PA1D.sol::185 => function initPA1D(bytes memory initPayload) external returns (bytes4) {
2022-10-holograph/contracts/enforcer/PA1D.sol::365 => function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {
2022-10-holograph/contracts/enforcer/PA1D.sol::683 => function getTokenAddress(string memory tokenName) public view returns (address) {
2022-10-holograph/contracts/module/LayerZeroModule.sol::158 => function init(bytes memory initPayload) external override returns (bytes4) {
```

## [G-06] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
2022-10-holograph/contracts/HolographBridge.sol::452 => function setFactory(address factory) external onlyAdmin {
2022-10-holograph/contracts/HolographBridge.sol::472 => function setHolograph(address holograph) external onlyAdmin {
2022-10-holograph/contracts/HolographBridge.sol::502 => function setOperator(address operator) external onlyAdmin {
2022-10-holograph/contracts/HolographBridge.sol::522 => function setRegistry(address registry) external onlyAdmin {
2022-10-holograph/contracts/HolographFactory.sol::280 => function setHolograph(address holograph) external onlyAdmin {
2022-10-holograph/contracts/HolographFactory.sol::300 => function setRegistry(address registry) external onlyAdmin {
2022-10-holograph/contracts/HolographOperator.sol::949 => function setBridge(address bridge) external onlyAdmin {
2022-10-holograph/contracts/HolographOperator.sol::969 => function setHolograph(address holograph) external onlyAdmin {
2022-10-holograph/contracts/HolographOperator.sol::989 => function setInterfaces(address interfaces) external onlyAdmin {
2022-10-holograph/contracts/HolographOperator.sol::1009 => function setMessagingModule(address messagingModule) external onlyAdmin {
2022-10-holograph/contracts/HolographOperator.sol::1029 => function setRegistry(address registry) external onlyAdmin {
2022-10-holograph/contracts/HolographOperator.sol::1049 => function setUtilityToken(address utilityToken) external onlyAdmin {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::380 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::415 => function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::549 => function sourceBurn(address from, uint256 amount) external onlySource {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::556 => function sourceMint(address to, uint256 amount) external onlySource {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::563 => function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::399 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::500 => function sourceBurn(uint256 tokenId) external onlySource {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::508 => function sourceMint(address to, uint224 tokenId) external onlySource {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::520 => function sourceGetChainPrepend() external view onlySource returns (uint256) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::577 => function sourceTransfer(address to, uint256 tokenId) external onlySource {
2022-10-holograph/contracts/module/LayerZeroModule.sol::320 => function setBridge(address bridge) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::340 => function setInterfaces(address interfaces) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::360 => function setLZEndpoint(address lZEndpoint) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::380 => function setOperator(address operator) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::441 => function setBaseGas(uint256 baseGas) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::470 => function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

## [G-07] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. 

```
2022-10-holograph/contracts/HolographBridge.sol::155 => constructor() {}
2022-10-holograph/contracts/HolographFactory.sol::136 => constructor() {}
2022-10-holograph/contracts/HolographOperator.sol::233 => constructor() {}
2022-10-holograph/contracts/HolographOperator.sol::1209 => receive() external payable {}
2022-10-holograph/contracts/abstract/ERC20H.sol::133 => constructor() {}
2022-10-holograph/contracts/abstract/ERC20H.sol::212 => receive() external payable {}
2022-10-holograph/contracts/abstract/ERC721H.sol::133 => constructor() {}
2022-10-holograph/contracts/abstract/ERC721H.sol::212 => receive() external payable {}
2022-10-holograph/contracts/enforcer/HolographERC20.sol::211 => constructor() {}
2022-10-holograph/contracts/enforcer/HolographERC20.sol::251 => receive() external payable {}
2022-10-holograph/contracts/enforcer/HolographERC721.sol::231 => constructor() {}
2022-10-holograph/contracts/enforcer/HolographERC721.sol::962 => receive() external payable {}
2022-10-holograph/contracts/enforcer/Holographer.sol::140 => constructor() {}
2022-10-holograph/contracts/enforcer/Holographer.sol::223 => receive() external payable {}
2022-10-holograph/contracts/enforcer/PA1D.sol::166 => constructor() {}
2022-10-holograph/contracts/module/LayerZeroModule.sol::151 => constructor() {}
```

## [G-08] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-10-holograph/contracts/HolographOperator.sol::208 => uint32 private _operatorTempStorageCounter;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::181 => uint8 private _decimals;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::160 => uint16 private _bps;
2022-10-holograph/contracts/module/LayerZeroModule.sol::265 => (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
2022-10-holograph/contracts/module/LayerZeroModule.sol::289 => (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
```

## [G-09] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
2022-10-holograph/contracts/HolographOperator.sol::198 => mapping(bytes32 => bool) private _failedJobs;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::196 => mapping(address => mapping(address => bool)) private _operatorApprovals;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::206 => mapping(uint256 => bool) private _burnedTokens;
```

## [G-10] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-10-holograph/contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
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
```

## [G-11] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-10-holograph/contracts/HolographFactory.sol::328 => v += 27;
2022-10-holograph/contracts/HolographOperator.sol::378 => _bondedAmounts[job.operator] -= amount;
2022-10-holograph/contracts/HolographOperator.sol::382 => _bondedAmounts[msg.sender] += amount;
2022-10-holograph/contracts/HolographOperator.sol::834 => _bondedAmounts[operator] += amount;
2022-10-holograph/contracts/HolographOperator.sol::1175 => position -= threshold;
2022-10-holograph/contracts/HolographOperator.sol::1177 => current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
2022-10-holograph/contracts/enforcer/HolographERC20.sol::633 => _totalSupply -= amount;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::685 => _totalSupply += amount;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::686 => _balances[to] += amount;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::702 => _balances[recipient] += amount;
```

## [G-12] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
2022-10-holograph/contracts/HolographFactory.sol::252 => abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
2022-10-holograph/contracts/enforcer/HolographERC20.sol::409 => return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));
2022-10-holograph/contracts/enforcer/HolographERC20.sol::471 => abi.encode(
2022-10-holograph/contracts/enforcer/HolographERC721.sol::260 => abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))
2022-10-holograph/contracts/enforcer/HolographERC721.sol::426 => return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```

## [G-13] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

```
2022-10-holograph/contracts/HolographBridge.sol::148 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
2022-10-holograph/contracts/HolographBridge.sol::163 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/contracts/HolographBridge.sol::214 => require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
2022-10-holograph/contracts/HolographBridge.sol::233 => require(doNotRevert, "HOLOGRAPH: reverted");
2022-10-holograph/contracts/HolographBridge.sol::270 => require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
2022-10-holograph/contracts/HolographFactory.sol::144 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/contracts/HolographFactory.sol::220 => require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
2022-10-holograph/contracts/HolographFactory.sol::228 => require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
2022-10-holograph/contracts/HolographOperator.sol::241 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/contracts/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
2022-10-holograph/contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
2022-10-holograph/contracts/HolographOperator.sol::354 => require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
2022-10-holograph/contracts/HolographOperator.sol::368 => require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
2022-10-holograph/contracts/HolographOperator.sol::415 => require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
2022-10-holograph/contracts/HolographOperator.sol::446 => require(msg.sender == address(this), "HOLOGRAPH: operator only call");
2022-10-holograph/contracts/HolographOperator.sol::485 => require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
2022-10-holograph/contracts/HolographOperator.sol::591 => require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
2022-10-holograph/contracts/HolographOperator.sol::595 => require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
2022-10-holograph/contracts/HolographOperator.sol::728 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/contracts/HolographOperator.sol::739 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/contracts/HolographOperator.sol::756 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
2022-10-holograph/contracts/HolographOperator.sol::829 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
2022-10-holograph/contracts/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
2022-10-holograph/contracts/HolographOperator.sol::863 => require(current <= amount, "HOLOGRAPH: bond amount too small");
2022-10-holograph/contracts/HolographOperator.sol::881 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
2022-10-holograph/contracts/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/HolographOperator.sol::903 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
2022-10-holograph/contracts/HolographOperator.sol::911 => require(_isContract(operator), "HOLOGRAPH: operator not contract");
2022-10-holograph/contracts/HolographOperator.sol::915 => require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
2022-10-holograph/contracts/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/abstract/ERC20H.sol::117 => require(msg.sender == holographer(), "ERC20: holographer only");
2022-10-holograph/contracts/abstract/ERC20H.sol::123 => require(msgSender() == _getOwner(), "ERC20: owner only function");
2022-10-holograph/contracts/abstract/ERC20H.sol::125 => require(msg.sender == _getOwner(), "ERC20: owner only function");
2022-10-holograph/contracts/abstract/ERC20H.sol::147 => require(!_isInitialized(), "ERC20: already initialized");
2022-10-holograph/contracts/abstract/ERC721H.sol::117 => require(msg.sender == holographer(), "ERC721: holographer only");
2022-10-holograph/contracts/abstract/ERC721H.sol::123 => require(msgSender() == _getOwner(), "ERC721: owner only function");
2022-10-holograph/contracts/abstract/ERC721H.sol::125 => require(msg.sender == _getOwner(), "ERC721: owner only function");
2022-10-holograph/contracts/abstract/ERC721H.sol::147 => require(!_isInitialized(), "ERC721: already initialized");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::192 => require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::204 => require(msg.sender == sourceContract, "ERC20: source only call");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::219 => require(!_isInitialized(), "ERC20: already initialized");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::241 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::349 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::365 => require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::387 => require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::400 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::427 => require(newAllowance >= currentAllowance, "ERC20: increased above max value");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::445 => require(_isContract(account), "ERC20: operator not contract");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::450 => require(balance >= amount, "ERC20: balance check failed");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::469 => require(block.timestamp <= deadline, "ERC20: expired deadline");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::482 => require(signer == account, "ERC20: invalid signature");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::505 => require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::529 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::539 => require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::599 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::629 => require(accountBalance >= amount, "ERC20: amount exceeds balance");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::645 => require(erc165support, "ERC20: no ERC165 support");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::698 => require(accountBalance >= amount, "ERC20: amount exceeds balance");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::212 => require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::224 => require(msg.sender == sourceContract, "ERC721: source only call");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::239 => require(!_isInitialized(), "ERC721: already initialized");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::258 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::323 => require(_exists(tokenId), "ERC721: token does not exist");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::370 => require(to != tokenOwner, "ERC721: cannot approve self");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::371 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::388 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::404 => require(!_exists(tokenId), "ERC721: token already exists");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::408 => require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::420 => require(_isApproved(sender, tokenId), "ERC721: sender not approved");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::421 => require(from == _tokenOwner[tokenId], "ERC721: from is not owner");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::458 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::484 => require(to != msg.sender, "ERC721: cannot approve self");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::513 => require(!_burnedTokens[token], "ERC721: can't mint burned token");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::622 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::700 => require(index < _allTokens.length, "ERC721: index out of bounds");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::729 => require(index < balanceOf(wallet), "ERC721: index out of bounds");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::757 => require(_isContract(_operator), "ERC721: operator not contract");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::762 => require(tokenOwner == address(this), "ERC721: contract not token owner");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::817 => require(!_exists(tokenId), "ERC721: token already exists");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::818 => require(!_burnedTokens[tokenId], "ERC721: token has been burned");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::869 => require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::906 => require(_exists(tokenId), "ERC721: token does not exist");
2022-10-holograph/contracts/enforcer/Holographer.sol::148 => require(!_isInitialized(), "HOLOGRAPHER: already initialized");
2022-10-holograph/contracts/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");
2022-10-holograph/contracts/enforcer/PA1D.sol::159 => require(isOwner(), "PA1D: caller not an owner");
2022-10-holograph/contracts/enforcer/PA1D.sol::174 => require(!_isInitialized(), "PA1D: already initialized");
2022-10-holograph/contracts/enforcer/PA1D.sol::190 => require(initialized == 0, "PA1D: already initialized");
2022-10-holograph/contracts/enforcer/PA1D.sol::390 => require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/contracts/enforcer/PA1D.sol::460 => require(matched, "PA1D: sender not authorized");
2022-10-holograph/contracts/enforcer/PA1D.sol::472 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
2022-10-holograph/contracts/enforcer/PA1D.sol::477 => require(totalBp == 10000, "PA1D: bps down't equal 10000");
2022-10-holograph/contracts/module/LayerZeroModule.sol::159 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
2022-10-holograph/contracts/module/LayerZeroModule.sol::235 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
```

## [G-14] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
2022-10-holograph/contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```

## [G-15] Use bytes32 instead of string

Use bytes32 instead of string to save gas whenever possible. String is a dynamic data structure and therefore is more gas consuming then bytes32.

```
2022-10-holograph/contracts/enforcer/PA1D.sol::142 => string constant _bpString = "eip1967.Holograph.PA1D.bp";
2022-10-holograph/contracts/enforcer/PA1D.sol::143 => string constant _receiverString = "eip1967.Holograph.PA1D.receiver";
2022-10-holograph/contracts/enforcer/PA1D.sol::144 => string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
```

## [G-16] Splitting require() statements that use && saves gas

Saves 16 gas per instance.
If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:

```
2022-10-holograph/contracts/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
2022-10-holograph/contracts/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

## [G-17] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
2022-10-holograph/contracts/enforcer/HolographERC20.sol::273 => function decimals() public view returns (uint8) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::297 => function allowance(address account, address spender) public view returns (uint256) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::306 => function DOMAIN_SEPARATOR() public view returns (bytes32) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::310 => function name() public view returns (string memory) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::314 => function nonces(address account) public view returns (uint256) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::318 => function symbol() public view returns (string memory) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::322 => function totalSupply() public view returns (uint256) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::347 => function burnFrom(address account, uint256 amount) public returns (bool) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::363 => function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::420 => function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::643 => function burned(uint256 tokenId) public view returns (bool) {
2022-10-holograph/contracts/enforcer/PA1D.sol::471 => function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
2022-10-holograph/contracts/enforcer/PA1D.sol::488 => function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
2022-10-holograph/contracts/enforcer/PA1D.sol::497 => function getEthPayout() public {
2022-10-holograph/contracts/enforcer/PA1D.sol::507 => function getTokenPayout(address tokenAddress) public {
2022-10-holograph/contracts/enforcer/PA1D.sol::517 => function getTokensPayout(address[] memory tokenAddresses) public {
2022-10-holograph/contracts/enforcer/PA1D.sol::549 => function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
2022-10-holograph/contracts/enforcer/PA1D.sol::558 => function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::569 => function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::604 => function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::649 => function marketContract() public view returns (address) {
2022-10-holograph/contracts/enforcer/PA1D.sol::655 => function tokenCreators(uint256 tokenId) public view returns (address) {
2022-10-holograph/contracts/enforcer/PA1D.sol::665 => function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```

## [G-18] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
2022-10-holograph/contracts/HolographFactory.sol::181 => ) external pure returns (bytes4 selector, bytes memory data) {
2022-10-holograph/contracts/HolographOperator.sol::717 => function getTotalPods() external view returns (uint256 totalPods) {
2022-10-holograph/contracts/HolographOperator.sol::804 => function getBondedAmount(address operator) external view returns (uint256 amount) {
2022-10-holograph/contracts/HolographOperator.sol::814 => function getBondedPod(address operator) external view returns (uint256 pod) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::396 => ) external onlyBridge returns (bytes4 selector, bytes memory data) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::417 => ) external onlyBridge returns (bytes4 selector, bytes memory data) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::761 => try HolographERC721Interface(_operator).ownerOf(_tokenId) returns (address tokenOwner) {
2022-10-holograph/contracts/enforcer/PA1D.sol::549 => function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
2022-10-holograph/contracts/enforcer/PA1D.sol::569 => function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::590 => function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::604 => function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::665 => function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
2022-10-holograph/contracts/module/LayerZeroModule.sol::256 => ) external view returns (uint256 hlgFee, uint256 msgFee) {
2022-10-holograph/contracts/module/LayerZeroModule.sol::281 => ) external view returns (uint256 hlgFee) {
```

## [G-19] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
2022-10-holograph/contracts/HolographOperator.sol::218 => mapping(address => uint256) private _bondedOperators;
2022-10-holograph/contracts/HolographOperator.sol::223 => mapping(address => uint256) private _operatorPodIndex;
2022-10-holograph/contracts/HolographOperator.sol::228 => mapping(address => uint256) private _bondedAmounts;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::156 => mapping(address => uint256) private _balances;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::161 => mapping(address => mapping(address => uint256)) private _allowances;
2022-10-holograph/contracts/enforcer/HolographERC20.sol::186 => mapping(address => uint256) private _nonces;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::185 => mapping(address => uint256) private _ownedTokensCount;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::190 => mapping(address => uint256[]) private _ownedTokens;
2022-10-holograph/contracts/enforcer/HolographERC721.sol::196 => mapping(address => mapping(address => bool)) private _operatorApprovals;
```

## [G-20] Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

instances:

```
2022-10-holograph/contracts/HolographOperator.sol::333 => if (job.operator != address(0)) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");
2022-10-holograph/contracts/enforcer/HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::657 => return _tokenOwner[tokenId] != address(0);
2022-10-holograph/contracts/enforcer/HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");
2022-10-holograph/contracts/enforcer/HolographERC721.sol::895 => return tokenOwner != address(0);
```

## [G-21] Use selfbalance()

Use selfbalance() instead of address(this).balance when getting your contract's balance of ETH to save gas.

```
2022-10-holograph/contracts/enforcer/PA1D.sol::389 => uint256 balance = address(this).balance;
```

## [G-22] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read.

Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

```
2022-10-holograph/contracts/enforcer/PA1D.sol::541 => address[] memory receivers = new address[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::543 => uint256[] memory bps = new uint256[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::559 => uint256[] memory bps = new uint256[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::570 => address payable[] memory receivers = new address payable[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::591 => address payable[] memory receivers = new address payable[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::592 => uint256[] memory bps = new uint256[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::605 => address payable[] memory receivers = new address payable[](1);
2022-10-holograph/contracts/enforcer/PA1D.sol::606 => uint256[] memory bps = new uint256[](1);
```

## [G-23] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-10-holograph/contracts/abstract/ERC20H.sol::203 => function _setOwner(address ownerAddress) internal {
2022-10-holograph/contracts/abstract/ERC721H.sol::203 => function _setOwner(address ownerAddress) internal {
2022-10-holograph/contracts/module/LayerZeroModule.sol::225 => * @dev Need to add an extra function to get LZ gas amount needed for their internal cross-chain message verification
```

## [G-25] internal functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

```
2022-10-holograph/contracts/abstract/ERC20H.sol::203 => function _setOwner(address ownerAddress) internal {
2022-10-holograph/contracts/abstract/ERC721H.sol::203 => function _setOwner(address ownerAddress) internal {
2022-10-holograph/contracts/module/LayerZeroModule.sol::225 => * @dev Need to add an extra function to get LZ gas amount needed for their internal cross-chain message verification
```