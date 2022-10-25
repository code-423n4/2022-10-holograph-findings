## [G001] >= costs less gas than >

he compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas

#### Instances:
```
contracts/HolographBridge.sol::218 => if (hTokenValue > 0) {
contracts/HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
contracts/HolographOperator.sol::363 => if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
contracts/HolographOperator.sol::398 => if (leftovers > 0) {
contracts/HolographOperator.sol::519 => if (podSize > 1) {
contracts/HolographOperator.sol::1126 => if (operatorIndex > 0) {
```

## [G002] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

#### Instances:
```
contracts/HolographFactory.sol::328 => v += 27;
contracts/HolographOperator.sol::378 => _bondedAmounts[job.operator] -= amount;
contracts/HolographOperator.sol::382 => _bondedAmounts[msg.sender] += amount;
contracts/HolographOperator.sol::438 => ////  _bondedOperators[msg.sender] += reward;
contracts/HolographOperator.sol::834 => _bondedAmounts[operator] += amount;
contracts/HolographOperator.sol::1175 => position -= threshold;
contracts/HolographOperator.sol::1176 => //       current += (current / _operatorThresholdDivisor) * position;
contracts/HolographOperator.sol::1177 => current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
contracts/enforcer/HolographERC20.sol::633 => _totalSupply -= amount;
contracts/enforcer/HolographERC20.sol::685 => _totalSupply += amount;
contracts/enforcer/HolographERC20.sol::686 => _balances[to] += amount;
contracts/enforcer/HolographERC20.sol::702 => _balances[recipient] += amount;

```

## [G003] `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops

The `unchecked` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

#### Instances:
```
contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
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
```

## [G004] Using `bools` for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.Refer https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 . Use uint256(1) and uint256(2) for true/false

#### Instances:
```
contracts/mock/Mock.sol::114 => bool shouldFail = false;
```

## [G005] ++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP

#### Instances:
```
contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
contracts/HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
contracts/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
contracts/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
contracts/enforcer/HolographERC721.sol::568 => //       startingTokenId++;
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
```

## [G006] Splitting `require()` statements that use `&&` saves gas

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

#### Instances:
```
contracts/HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
contracts/enforcer/HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
contracts/enforcer/Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

## [G007] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.\[layout_in_storage.html]{https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html}\Use a larger size then downcast where needed

#### Instances:
```
contracts/enforcer/HolographERC721.sol::879 => uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())
contracts/enforcer/Holographer.sol::150 => (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(
contracts/module/LayerZeroModule.sol::257 => uint16 lzDestChain = uint16(
contracts/module/LayerZeroModule.sol::265 => (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
contracts/module/LayerZeroModule.sol::286 => uint16 lzDestChain = uint16(
contracts/module/LayerZeroModule.sol::289 => (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
```

## [G008] abi.encode() is less efficient than abi.encodePacked()

#### Instances:
```
contracts/HolographFactory.sol::252 => abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
contracts/abstract/StrictERC721H.sol::132 => _data = abi.encode(holographer());
contracts/enforcer/HolographERC20.sol::409 => return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));
contracts/enforcer/HolographERC20.sol::471 => abi.encode(
contracts/enforcer/HolographERC721.sol::260 => abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))
contracts/enforcer/HolographERC721.sol::426 => return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```

## [G009] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (`if(x){}else if(y){...}else{...}` => `if(!x){if(y){...}else{...}}`)

#### Instances:
```
contracts/HolographBridge.sol::155 => constructor() {}
contracts/HolographFactory.sol::136 => constructor() {}
contracts/HolographOperator.sol::233 => constructor() {}
contracts/HolographOperator.sol::1209 => receive() external payable {}
contracts/abstract/ERC20H.sol::133 => constructor() {}
contracts/abstract/ERC20H.sol::212 => receive() external payable {}
contracts/abstract/ERC721H.sol::133 => constructor() {}
contracts/abstract/ERC721H.sol::212 => receive() external payable {}
contracts/enforcer/HolographERC20.sol::211 => constructor() {}
contracts/enforcer/HolographERC20.sol::251 => receive() external payable {}
contracts/enforcer/HolographERC721.sol::231 => constructor() {}
contracts/enforcer/HolographERC721.sol::962 => receive() external payable {}
contracts/enforcer/Holographer.sol::140 => constructor() {}
contracts/enforcer/Holographer.sol::223 => receive() external payable {}
contracts/enforcer/PA1D.sol::166 => constructor() {}
contracts/module/LayerZeroModule.sol::151 => constructor() {}
```
## [G010] Functions with access control cheaper if payable

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

#### Instances:
```
contracts/HolographBridge.sol::452 => function setFactory(address factory) external onlyAdmin {
contracts/HolographBridge.sol::472 => function setHolograph(address holograph) external onlyAdmin {
contracts/HolographBridge.sol::502 => function setOperator(address operator) external onlyAdmin {
contracts/HolographBridge.sol::522 => function setRegistry(address registry) external onlyAdmin {
contracts/HolographFactory.sol::280 => function setHolograph(address holograph) external onlyAdmin {
contracts/HolographFactory.sol::300 => function setRegistry(address registry) external onlyAdmin {
contracts/HolographOperator.sol::285 => ) external onlyAdmin {
contracts/HolographOperator.sol::949 => function setBridge(address bridge) external onlyAdmin {
contracts/HolographOperator.sol::969 => function setHolograph(address holograph) external onlyAdmin {
contracts/HolographOperator.sol::989 => function setInterfaces(address interfaces) external onlyAdmin {
contracts/HolographOperator.sol::1009 => function setMessagingModule(address messagingModule) external onlyAdmin {
contracts/HolographOperator.sol::1029 => function setRegistry(address registry) external onlyAdmin {
contracts/HolographOperator.sol::1049 => function setUtilityToken(address utilityToken) external onlyAdmin {
contracts/enforcer/HolographERC20.sol::380 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
contracts/enforcer/HolographERC20.sol::396 => ) external onlyBridge returns (bytes4 selector, bytes memory data) {
contracts/enforcer/HolographERC20.sol::415 => function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
contracts/enforcer/HolographERC20.sol::549 => function sourceBurn(address from, uint256 amount) external onlySource {
contracts/enforcer/HolographERC20.sol::556 => function sourceMint(address to, uint256 amount) external onlySource {
contracts/enforcer/HolographERC20.sol::563 => function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
contracts/enforcer/HolographERC20.sol::576 => ) external onlySource {
contracts/enforcer/HolographERC721.sol::399 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
contracts/enforcer/HolographERC721.sol::417 => ) external onlyBridge returns (bytes4 selector, bytes memory data) {
contracts/enforcer/HolographERC721.sol::500 => function sourceBurn(uint256 tokenId) external onlySource {
contracts/enforcer/HolographERC721.sol::508 => function sourceMint(address to, uint224 tokenId) external onlySource {
contracts/enforcer/HolographERC721.sol::520 => function sourceGetChainPrepend() external view onlySource returns (uint256) {
contracts/enforcer/HolographERC721.sol::527 => //   function sourceMintBatch(address to, uint224[] calldata tokenIds) external onlySource {
contracts/enforcer/HolographERC721.sol::542 => //   function sourceMintBatch(address[] calldata wallets, uint224[] calldata tokenIds) external onlySource {
contracts/enforcer/HolographERC721.sol::561 => //   ) external onlySource {
contracts/enforcer/HolographERC721.sol::577 => function sourceTransfer(address to, uint256 tokenId) external onlySource {
contracts/enforcer/PA1D.sol::471 => function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
contracts/enforcer/PA1D.sol::533 => ) public onlyOwner {
contracts/module/LayerZeroModule.sol::320 => function setBridge(address bridge) external onlyAdmin {
contracts/module/LayerZeroModule.sol::340 => function setInterfaces(address interfaces) external onlyAdmin {
contracts/module/LayerZeroModule.sol::360 => function setLZEndpoint(address lZEndpoint) external onlyAdmin {
contracts/module/LayerZeroModule.sol::380 => function setOperator(address operator) external onlyAdmin {
contracts/module/LayerZeroModule.sol::441 => function setBaseGas(uint256 baseGas) external onlyAdmin {
contracts/module/LayerZeroModule.sol::470 => function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

## [G011] Splitting `require()` statements that use `&&` saves gas

contracts/enforcer/Holographer.sol::166

```
require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

## [G012] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version

#### Instances:
```
contracts/HolographBridge.sol::148 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
contracts/HolographBridge.sol::163 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
contracts/HolographBridge.sol::214 => require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
contracts/HolographBridge.sol::233 => require(doNotRevert, "HOLOGRAPH: reverted");
contracts/HolographBridge.sol::270 => require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
contracts/HolographFactory.sol::144 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
contracts/HolographFactory.sol::220 => require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
contracts/HolographFactory.sol::228 => require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
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
contracts/abstract/ERC20H.sol::117 => require(msg.sender == holographer(), "ERC20: holographer only");
contracts/abstract/ERC20H.sol::123 => require(msgSender() == _getOwner(), "ERC20: owner only function");
contracts/abstract/ERC20H.sol::125 => require(msg.sender == _getOwner(), "ERC20: owner only function");
contracts/abstract/ERC20H.sol::147 => require(!_isInitialized(), "ERC20: already initialized");
contracts/abstract/ERC721H.sol::117 => require(msg.sender == holographer(), "ERC721: holographer only");
contracts/abstract/ERC721H.sol::123 => require(msgSender() == _getOwner(), "ERC721: owner only function");
contracts/abstract/ERC721H.sol::125 => require(msg.sender == _getOwner(), "ERC721: owner only function");
contracts/abstract/ERC721H.sol::147 => require(!_isInitialized(), "ERC721: already initialized");
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
contracts/module/LayerZeroModule.sol::159 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
contracts/module/LayerZeroModule.sol::193 => * @dev this is the assembly version of -> revert("HOLOGRAPH: LZ only endpoint");
contracts/module/LayerZeroModule.sol::209 => * @dev this is the assembly version of -> revert("HOLOGRAPH: unauthorized sender");
contracts/module/LayerZeroModule.sol::235 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
```