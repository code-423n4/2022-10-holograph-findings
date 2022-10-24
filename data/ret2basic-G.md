# Holograph Ccontest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code audit:

1. Use `calldata` instead of `memory` (37 instances)
2. Cache `<array>.length` (6 instances)
3. Use `unchecked{}` to suppress overflow/underflow check (15 instances)
4. Long `require()`/`revert()` string (2 instances)
5. Using `bool`s for storage incurs overhead (4 instances)
6. Use `!= 0` instead of `> 0` when comparing uint (7 instances)
7. Empty blocks should be removed or emit something (16 instances)
8. Don't initialize variables with default value (17 instances)
9. Use `++i`/`--i` instead of `i++`/`i--` (15 instances)
10. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (3 instances)
11. Use `abi.encodePacked()` instead of `abi.encode()` (5 instances)
12. Use custom errors instead of `revert()`/`require()` strings (116 instances)

Total 243 instances of 12 issues.

## 1. Use `calldata` instead of `memory` (37 instances)

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for loop to copy each index of the `calldata` to the `memory` index. This overhead can be optimized by using `calldata` directly.

```solidity
contracts/HolographBridge.sol::162 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/HolographOperator.sol::240 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/HolographOperator.sol::690 => function getJobDetails(bytes32 jobHash) public view returns (OperatorJob memory) {

contracts/HolographOperator.sol::738 => function getPodOperators(uint256 pod) external view returns (address[] memory operators) {

contracts/HolographFactory.sol::143 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/module/LayerZeroModule.sol::158 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/enforcer/Holographer.sol::147 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/enforcer/PA1D.sol::173 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/enforcer/PA1D.sol::185 => function initPA1D(bytes memory initPayload) external returns (bytes4) {

contracts/enforcer/PA1D.sol::298 => function _getPayoutAddresses() private view returns (address payable[] memory addresses) {

contracts/enforcer/PA1D.sol::316 => function _setPayoutAddresses(address payable[] memory addresses) private {

contracts/enforcer/PA1D.sol::332 => function _getPayoutBps() private view returns (uint256[] memory bps) {

contracts/enforcer/PA1D.sol::349 => function _setPayoutBps(uint256[] memory bps) private {

contracts/enforcer/PA1D.sol::365 => function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

contracts/enforcer/PA1D.sol::372 => function _setTokenAddress(string memory tokenName, address tokenAddress) private {

contracts/enforcer/PA1D.sol::426 => function _payoutTokens(address[] memory tokenAddresses) private {

contracts/enforcer/PA1D.sol::471 => function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

contracts/enforcer/PA1D.sol::488 => function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {

contracts/enforcer/PA1D.sol::517 => function getTokensPayout(address[] memory tokenAddresses) public {

contracts/enforcer/PA1D.sol::558 => function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {

contracts/enforcer/PA1D.sol::569 => function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {

contracts/enforcer/PA1D.sol::590 => function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

contracts/enforcer/PA1D.sol::604 => function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

contracts/enforcer/PA1D.sol::665 => function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {

contracts/enforcer/PA1D.sol::683 => function getTokenAddress(string memory tokenName) public view returns (address) {

contracts/enforcer/HolographERC721.sol::238 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/enforcer/HolographERC721.sol::274 => function contractURI() external view returns (string memory) {

contracts/enforcer/HolographERC721.sol::282 => function name() external view returns (string memory) {

contracts/enforcer/HolographERC721.sol::313 => function symbol() external view returns (string memory) {

contracts/enforcer/HolographERC721.sol::322 => function tokenURI(uint256 tokenId) external view returns (string memory) {

contracts/enforcer/HolographERC721.sol::336 => function tokensOfOwner(address wallet) external view returns (uint256[] memory) {

contracts/enforcer/HolographERC721.sol::710 => function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {

contracts/enforcer/HolographERC20.sol::218 => function init(bytes memory initPayload) external override returns (bytes4) {

contracts/enforcer/HolographERC20.sol::310 => function name() public view returns (string memory) {

contracts/enforcer/HolographERC20.sol::318 => function symbol() public view returns (string memory) {

contracts/abstract/ERC721H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {

contracts/abstract/ERC20H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
```

## 2. Cache `<array>.length` (6 instances)

If `<array>.length` is used as for loop termination condition, then the `.length` method will be called in each iteration. Caching it in a local variable can save gas.

```solidity
contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {

contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {

contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
```

## 3. Use `unchecked{}` to suppress overflow/underflow check (15 instances)

Starting from version 0.8.0, Solidity does overflow/underflow checks by default. It is a good feature to prevent vulnerabilities but it has a significant overhead, especially when used in for loop. When using uint256/int256, it is extremely hard to trigger overflow, so it makes sense to skip these checks. To suppress the overflow/underflow checks, use `unchecked {}`. For increment situations, just use `unchecked {}` directly; for decrement situations, add a `require()` statement before decrementing to prevent underflow.

```solidity
contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {

contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {

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

contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
```

## 4. Long `require()`/`revert()` string (2 instances)

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

```solidity
contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");

contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

## 5. Using `bool`s for storage incurs overhead (4 instances)

Use `uint256(1)` and `uint256(2)` for true/false. Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

```solidity
contracts/HolographBridge.sol::197 => bool doNotRevert,

contracts/enforcer/PA1D.sol::451 => bool matched;

contracts/enforcer/HolographERC721.sol::250 => bool skipInit,

contracts/enforcer/HolographERC20.sol::233 => bool skipInit,
```

## 6. Use `!= 0` instead of `> 0` when comparing uint (7 instances)

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

```solidity
contracts/HolographBridge.sol::218 => if (hTokenValue > 0) {

contracts/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");

contracts/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {

contracts/HolographOperator.sol::398 => if (leftovers > 0) {

contracts/HolographOperator.sol::1126 => if (operatorIndex > 0) {

contracts/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
```

## 7. Empty blocks should be removed or emit something (16 instances)

Empty blocks exist in the code. The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

```solidity
contracts/HolographBridge.sol::155 => constructor() {}

contracts/HolographOperator.sol::233 => constructor() {}

contracts/HolographOperator.sol::1209 => receive() external payable {}

contracts/HolographFactory.sol::136 => constructor() {}

contracts/module/LayerZeroModule.sol::151 => constructor() {}

contracts/enforcer/Holographer.sol::140 => constructor() {}

contracts/enforcer/Holographer.sol::223 => receive() external payable {}

contracts/enforcer/PA1D.sol::166 => constructor() {}

contracts/enforcer/HolographERC721.sol::231 => constructor() {}

contracts/enforcer/HolographERC721.sol::962 => receive() external payable {}

contracts/enforcer/HolographERC20.sol::211 => constructor() {}

contracts/enforcer/HolographERC20.sol::251 => receive() external payable {}

contracts/abstract/ERC721H.sol::133 => constructor() {}

contracts/abstract/ERC721H.sol::212 => receive() external payable {}

contracts/abstract/ERC20H.sol::133 => constructor() {}

contracts/abstract/ERC20H.sol::212 => receive() external payable {}
```

## 8. Don't initialize variables with default value (17 instances)

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```solidity
contracts/HolographBridge.sol::380 => uint256 fee = 0;

contracts/HolographOperator.sol::310 => uint256 gasLimit = 0;

contracts/HolographOperator.sol::311 => uint256 gasPrice = 0;

contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {

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

contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
```

## 9. Use `++i`/`--i` instead of `i++`/`i--` (15 instances)

Using `++i`/`--i` saves 6 gas per loop.

```solidity
contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {

contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {

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

contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
```

## 10. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (3 instances)

Instead of using operator && on single require check, using double `require()` checks can save more gas.

```solidity
contracts/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

contracts/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");

contracts/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
```

## 11. Use `abi.encodePacked()` instead of `abi.encode()` (5 instances)

`abi.encodePacked()` is more efficient than `abi.encode()`.

```solidity
contracts/HolographFactory.sol::252 => abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)

contracts/enforcer/HolographERC721.sol::260 => abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))

contracts/enforcer/HolographERC721.sol::426 => return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));

contracts/enforcer/HolographERC20.sol::409 => return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));

contracts/enforcer/HolographERC20.sol::471 => abi.encode(
```

## 12. Use custom errors instead of `revert()`/`require()` strings (116 instances)

Using `require()`/`revert()` strings is expensive. Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors.

Custom errors decrease both deploy and runtime gas costs. Note that runtime gas cost is only relevant when the revert condition is met.

```solidity
contracts/HolographBridge.sol::148 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

contracts/HolographBridge.sol::163 => require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/HolographBridge.sol::214 => require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

contracts/HolographBridge.sol::233 => require(doNotRevert, "HOLOGRAPH: reverted");

contracts/HolographBridge.sol::270 => require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

contracts/HolographOperator.sol::241 => require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");

contracts/HolographOperator.sol::354 => require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

contracts/HolographOperator.sol::368 => require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

contracts/HolographOperator.sol::415 => require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

contracts/HolographOperator.sol::446 => require(msg.sender == address(this), "HOLOGRAPH: operator only call");

contracts/HolographOperator.sol::485 => require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

contracts/HolographOperator.sol::591 => require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");

contracts/HolographOperator.sol::595 => require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

contracts/HolographOperator.sol::728 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

contracts/HolographOperator.sol::739 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

contracts/HolographOperator.sol::756 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

contracts/HolographOperator.sol::829 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

contracts/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

contracts/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

contracts/HolographOperator.sol::863 => require(current <= amount, "HOLOGRAPH: bond amount too small");

contracts/HolographOperator.sol::881 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

contracts/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

contracts/HolographOperator.sol::903 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

contracts/HolographOperator.sol::911 => require(_isContract(operator), "HOLOGRAPH: operator not contract");

contracts/HolographOperator.sol::915 => require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

contracts/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");

contracts/HolographFactory.sol::144 => require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/HolographFactory.sol::220 => require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

contracts/HolographFactory.sol::228 => require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

contracts/module/LayerZeroModule.sol::159 => require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/module/LayerZeroModule.sol::235 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

contracts/enforcer/Holographer.sol::148 => require(!_isInitialized(), "HOLOGRAPHER: already initialized");

contracts/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");

contracts/enforcer/PA1D.sol::159 => require(isOwner(), "PA1D: caller not an owner");

contracts/enforcer/PA1D.sol::174 => require(!_isInitialized(), "PA1D: already initialized");

contracts/enforcer/PA1D.sol::190 => require(initialized == 0, "PA1D: already initialized");

contracts/enforcer/PA1D.sol::390 => require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");

contracts/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");

contracts/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

contracts/enforcer/PA1D.sol::460 => require(matched, "PA1D: sender not authorized");

contracts/enforcer/PA1D.sol::472 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

contracts/enforcer/PA1D.sol::477 => require(totalBp == 10000, "PA1D: bps down't equal 10000");

contracts/enforcer/HolographERC721.sol::212 => require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

contracts/enforcer/HolographERC721.sol::224 => require(msg.sender == sourceContract, "ERC721: source only call");

contracts/enforcer/HolographERC721.sol::239 => require(!_isInitialized(), "ERC721: already initialized");

contracts/enforcer/HolographERC721.sol::258 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

contracts/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

contracts/enforcer/HolographERC721.sol::323 => require(_exists(tokenId), "ERC721: token does not exist");

contracts/enforcer/HolographERC721.sol::370 => require(to != tokenOwner, "ERC721: cannot approve self");

contracts/enforcer/HolographERC721.sol::371 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol::388 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol::404 => require(!_exists(tokenId), "ERC721: token already exists");

contracts/enforcer/HolographERC721.sol::408 => require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

contracts/enforcer/HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");

contracts/enforcer/HolographERC721.sol::420 => require(_isApproved(sender, tokenId), "ERC721: sender not approved");

contracts/enforcer/HolographERC721.sol::421 => require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

contracts/enforcer/HolographERC721.sol::458 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol::484 => require(to != msg.sender, "ERC721: cannot approve self");

contracts/enforcer/HolographERC721.sol::513 => require(!_burnedTokens[token], "ERC721: can't mint burned token");

contracts/enforcer/HolographERC721.sol::622 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");

contracts/enforcer/HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");

contracts/enforcer/HolographERC721.sol::700 => require(index < _allTokens.length, "ERC721: index out of bounds");

contracts/enforcer/HolographERC721.sol::729 => require(index < balanceOf(wallet), "ERC721: index out of bounds");

contracts/enforcer/HolographERC721.sol::757 => require(_isContract(_operator), "ERC721: operator not contract");

contracts/enforcer/HolographERC721.sol::762 => require(tokenOwner == address(this), "ERC721: contract not token owner");

contracts/enforcer/HolographERC721.sol::764 => revert("ERC721: token does not exist");

contracts/enforcer/HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");

contracts/enforcer/HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");

contracts/enforcer/HolographERC721.sol::817 => require(!_exists(tokenId), "ERC721: token already exists");

contracts/enforcer/HolographERC721.sol::818 => require(!_burnedTokens[tokenId], "ERC721: token has been burned");

contracts/enforcer/HolographERC721.sol::869 => require(_tokenOwner[tokenId] == from, "ERC721: token not owned");

contracts/enforcer/HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");

contracts/enforcer/HolographERC721.sol::906 => require(_exists(tokenId), "ERC721: token does not exist");

contracts/enforcer/HolographERC20.sol::192 => require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");

contracts/enforcer/HolographERC20.sol::204 => require(msg.sender == sourceContract, "ERC20: source only call");

contracts/enforcer/HolographERC20.sol::219 => require(!_isInitialized(), "ERC20: already initialized");

contracts/enforcer/HolographERC20.sol::241 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

contracts/enforcer/HolographERC20.sol::349 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol::365 => require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

contracts/enforcer/HolographERC20.sol::387 => require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

contracts/enforcer/HolographERC20.sol::400 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol::427 => require(newAllowance >= currentAllowance, "ERC20: increased above max value");

contracts/enforcer/HolographERC20.sol::445 => require(_isContract(account), "ERC20: operator not contract");

contracts/enforcer/HolographERC20.sol::450 => require(balance >= amount, "ERC20: balance check failed");

contracts/enforcer/HolographERC20.sol::452 => revert("ERC20: failed getting balance");

contracts/enforcer/HolographERC20.sol::469 => require(block.timestamp <= deadline, "ERC20: expired deadline");

contracts/enforcer/HolographERC20.sol::482 => require(signer == account, "ERC20: invalid signature");

contracts/enforcer/HolographERC20.sol::505 => require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");

contracts/enforcer/HolographERC20.sol::529 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol::539 => require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");

contracts/enforcer/HolographERC20.sol::599 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");

contracts/enforcer/HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol::629 => require(accountBalance >= amount, "ERC20: amount exceeds balance");

contracts/enforcer/HolographERC20.sol::645 => require(erc165support, "ERC20: no ERC165 support");

contracts/enforcer/HolographERC20.sol::653 => revert("ERC20: non ERC20Receiver");

contracts/enforcer/HolographERC20.sol::661 => revert("ERC20: eip-4524 not supported");

contracts/enforcer/HolographERC20.sol::665 => revert("ERC20: no ERC165 support");

contracts/enforcer/HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");

contracts/enforcer/HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");

contracts/enforcer/HolographERC20.sol::698 => require(accountBalance >= amount, "ERC20: amount exceeds balance");

contracts/abstract/ERC721H.sol::117 => require(msg.sender == holographer(), "ERC721: holographer only");

contracts/abstract/ERC721H.sol::123 => require(msgSender() == _getOwner(), "ERC721: owner only function");

contracts/abstract/ERC721H.sol::125 => require(msg.sender == _getOwner(), "ERC721: owner only function");

contracts/abstract/ERC721H.sol::147 => require(!_isInitialized(), "ERC721: already initialized");

contracts/abstract/ERC20H.sol::117 => require(msg.sender == holographer(), "ERC20: holographer only");

contracts/abstract/ERC20H.sol::123 => require(msgSender() == _getOwner(), "ERC20: owner only function");

contracts/abstract/ERC20H.sol::125 => require(msg.sender == _getOwner(), "ERC20: owner only function");

contracts/abstract/ERC20H.sol::147 => require(!_isInitialized(), "ERC20: already initialized");
```
