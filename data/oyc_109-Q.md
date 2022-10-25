 ## [L-01] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
2022-10-holograph/contracts/HolographOperator.sol::345 => uint256 elapsedTime = block.timestamp - uint256(job.startTimestamp);
2022-10-holograph/contracts/HolographOperator.sol::497 => * @dev use job hash, job nonce, block number, and block timestamp for generating a random number
2022-10-holograph/contracts/HolographOperator.sol::499 => uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
2022-10-holograph/contracts/HolographOperator.sol::531 => (block.timestamp << 16) |
2022-10-holograph/contracts/enforcer/HolographERC20.sol::469 => require(block.timestamp <= deadline, "ERC20: expired deadline");
```

## [L-02] Unused receive()/fallback() function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

```
2022-10-holograph/contracts/HolographOperator.sol::1209 => receive() external payable {}
2022-10-holograph/contracts/abstract/ERC20H.sol::212 => receive() external payable {}
2022-10-holograph/contracts/abstract/ERC721H.sol::212 => receive() external payable {}
2022-10-holograph/contracts/enforcer/HolographERC20.sol::251 => receive() external payable {}
2022-10-holograph/contracts/enforcer/HolographERC721.sol::962 => receive() external payable {}
2022-10-holograph/contracts/enforcer/Holographer.sol::223 => receive() external payable {}
```

## [L-03] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
2022-10-holograph/contracts/HolographOperator.sol::701 => // TODO: move the bit-shifting around to have it be sequential
```

## [L-04] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-10-holograph/contracts/HolographFactory.sol::226 => uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), hash, keccak256(holographerBytecode)))))
2022-10-holograph/contracts/HolographFactory.sol::333 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
2022-10-holograph/contracts/HolographOperator.sol::499 => uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
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
```

## [L-05] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-10-holograph/contracts/HolographOperator.sol::400 => _utilityToken().transfer(job.operator, leftovers);
2022-10-holograph/contracts/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/enforcer/PA1D.sol::396 => addresses[i].transfer(sending);
2022-10-holograph/contracts/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/contracts/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
```

## [L-06] Events not emitted for important state changes

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

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
2022-10-holograph/contracts/module/LayerZeroModule.sol::320 => function setBridge(address bridge) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::340 => function setInterfaces(address interfaces) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::360 => function setLZEndpoint(address lZEndpoint) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::380 => function setOperator(address operator) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::441 => function setBaseGas(uint256 baseGas) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::470 => function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

## [L-07] ecrecover() not checked for signer address of zero

The ecrecover() function returns an address of zero when the signature does not match. This can cause problems if address zero is ever the owner of assets, and someone uses the permit function on address zero. If that happens, any invalid signature will pass the checks, and the assets will be stealable. In this case, the asset of concern is the vault's ERC20 token, and fortunately OpenZeppelin's implementation does a good job of making sure that address zero is never able to have a positive balance. If this contract ever changes to another ERC20 implementation that is laxer in its checks in favor of saving gas, this code may become a problem.

```
2022-10-holograph/contracts/HolographFactory.sol::333 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
2022-10-holograph/contracts/HolographFactory.sol::334 => ecrecover(hash, v, r, s) == signer);
```

## [L-08] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-10-holograph/contracts/enforcer/HolographERC20.sol::385 => _mint(to, amount);
2022-10-holograph/contracts/enforcer/HolographERC20.sol::416 => _mint(to, amount);
2022-10-holograph/contracts/enforcer/HolographERC20.sol::557 => _mint(to, amount);
2022-10-holograph/contracts/enforcer/HolographERC20.sol::565 => _mint(wallets[i], amounts[i]);
2022-10-holograph/contracts/enforcer/HolographERC20.sol::683 => function _mint(address to, uint256 amount) private {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::406 => _mint(to, tokenId);
2022-10-holograph/contracts/enforcer/HolographERC721.sol::514 => _mint(to, token);
```

## [N-01] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

```
2022-10-holograph/contracts/enforcer/PA1D.sol::390 => require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::395 => sending = ((bps[i] * balance) / 10000);
2022-10-holograph/contracts/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::415 => sending = ((bps[i] * balance) / 10000);
2022-10-holograph/contracts/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/contracts/enforcer/PA1D.sol::438 => sending = ((bps[i] * balance) / 10000);
2022-10-holograph/contracts/enforcer/PA1D.sol::467 => * @dev Addresses and bps arrays must be equal length. Bps values added together must equal 10000 exactly.
2022-10-holograph/contracts/enforcer/PA1D.sol::477 => require(totalBp == 10000, "PA1D: bps down't equal 10000");
2022-10-holograph/contracts/enforcer/PA1D.sol::514 => * @dev Each token balance must be equal or greater than 10000. Otherwise calculating BP is difficult.
2022-10-holograph/contracts/enforcer/PA1D.sol::551 => return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
2022-10-holograph/contracts/enforcer/PA1D.sol::553 => return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
2022-10-holograph/contracts/enforcer/PA1D.sol::641 => return (_getDefaultBp() * amount) / 10000;
2022-10-holograph/contracts/enforcer/PA1D.sol::643 => return (_getBp(tokenId) * amount) / 10000;
```

## [N-02] Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

Scientific notation should be used for better code readability

```
2022-10-holograph/contracts/HolographOperator.sol::256 => _baseBondAmount = 100 * (10**18); // one single token unit * 100
2022-10-holograph/contracts/module/LayerZeroModule.sol::274 => return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
2022-10-holograph/contracts/module/LayerZeroModule.sol::293 => return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```

## [N-03] require()/revert() statements should have descriptive reason strings

Descriptive reason strings should be used so that users can troubleshot any reverted calls

```
2022-10-holograph/contracts/HolographBridge.sol::578 => revert();
2022-10-holograph/contracts/HolographBridge.sol::585 => revert();
2022-10-holograph/contracts/HolographFactory.sol::341 => revert();
2022-10-holograph/contracts/HolographFactory.sol::348 => revert();
2022-10-holograph/contracts/HolographOperator.sol::1215 => revert();
2022-10-holograph/contracts/module/LayerZeroModule.sol::417 => revert();
2022-10-holograph/contracts/module/LayerZeroModule.sol::424 => revert();
```

## [N-04] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
2022-10-holograph/contracts/enforcer/HolographERC20.sol::755 => return ((_eventConfig >> uint256(_eventName)) & uint256(1) == 1 ? true : false);
2022-10-holograph/contracts/enforcer/HolographERC721.sol::1003 => return ((_eventConfig >> uint256(_eventName)) & uint256(1) == 1 ? true : false);
2022-10-holograph/contracts/enforcer/PA1D.sol::153 => event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```

## [N-06] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public.

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
2022-10-holograph/contracts/enforcer/PA1D.sol::524 => * @dev Take great care to not make this function accessible by other public functions in your overlying smart contract.
2022-10-holograph/contracts/enforcer/PA1D.sol::549 => function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
2022-10-holograph/contracts/enforcer/PA1D.sol::558 => function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::569 => function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::604 => function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
2022-10-holograph/contracts/enforcer/PA1D.sol::649 => function marketContract() public view returns (address) {
2022-10-holograph/contracts/enforcer/PA1D.sol::655 => function tokenCreators(uint256 tokenId) public view returns (address) {
2022-10-holograph/contracts/enforcer/PA1D.sol::665 => function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```

## [N-07] Adding a return statement when the function defines a named return variable, is redundant

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

## [N-08] Missing event for critical parameter change

Emitting events after sensitive changes take place, to facilitate tracking and notify off-chain clients following changes to the contract.

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
2022-10-holograph/contracts/module/LayerZeroModule.sol::320 => function setBridge(address bridge) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::340 => function setInterfaces(address interfaces) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::360 => function setLZEndpoint(address lZEndpoint) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::380 => function setOperator(address operator) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::441 => function setBaseGas(uint256 baseGas) external onlyAdmin {
2022-10-holograph/contracts/module/LayerZeroModule.sol::470 => function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

## [N-09] Consider addings checks for signature malleability

Use OpenZeppelin's ECDSA contract rather than calling ecrecover() directly

```
2022-10-holograph/contracts/HolographFactory.sol::333 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
2022-10-holograph/contracts/HolographFactory.sol::334 => ecrecover(hash, v, r, s) == signer);
```

## [N-010] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

```
2022-10-holograph/contracts/enforcer/HolographERC20.sol::289 => interfaces.supportsInterface(InterfaceType.ERC20, interfaceId) || erc165Contract.supportsInterface(interfaceId) // check global interfaces // check if source supports interface
2022-10-holograph/contracts/enforcer/Holographer.sol::172 => * @dev Returns the block height of when the smart contract was deployed. Useful for retrieving deployment config for re-deployment on other EVM-compatible chains.
2022-10-holograph/contracts/enforcer/PA1D.sol::151 => * @param bps Uint256 array of base points(percentages) that each wallet(specified in recipients) will receive from the royalty payouts. Make sure that all the base points add up to a total of 10000.
```
