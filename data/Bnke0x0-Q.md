




### [L01] Unused `receive()` function will lock Ether in contract

#### Impact
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert
#### Findings:
```
2022-10-holograph/HolographBridge.sol::577 => receive() external payable {
2022-10-holograph/HolographFactory.sol::340 => receive() external payable {
2022-10-holograph/HolographOperator.sol::1209 => receive() external payable {}
2022-10-holograph/abstract/ERC20H.sol::212 => receive() external payable {}
2022-10-holograph/abstract/ERC721H.sol::212 => receive() external payable {}
2022-10-holograph/enforcer/HolographERC20.sol::251 => receive() external payable {}
2022-10-holograph/enforcer/HolographERC721.sol::962 => receive() external payable {}
2022-10-holograph/enforcer/Holographer.sol::223 => receive() external payable {}
2022-10-holograph/module/LayerZeroModule.sol::416 => receive() external payable {
```

### [L02] `address.call{value:x}()` should be used instead of `payable.transfer()`

#### Impact
The use of `payable.transfer()` is heavily frowned upon because it can lead to the locking of funds. The `transfer()` call requires that the recipient has a `payable` callback, only provides 2300 gas for its operation. This means the following cases can cause the transfer to fail:

- The contract does not have a `payable` callback
- The contract’s `payable` callback spends more than 2300 gas (which is only enough to emit something)
- The contract is called through a proxy which itself uses up the 2300 gas
#### Findings:
```
2022-10-holograph/HolographOperator.sol::596 => payable(hToken).transfer(hlgFee);
```


### [L03] `_safeMint()` should be used rather than `_mint()` wherever possible

#### Impact
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### Findings:
```
2022-10-holograph/enforcer/HolographERC20.sol::385 => _mint(to, amount);
2022-10-holograph/enforcer/HolographERC20.sol::416 => _mint(to, amount);
2022-10-holograph/enforcer/HolographERC20.sol::557 => _mint(to, amount);
2022-10-holograph/enforcer/HolographERC20.sol::565 => _mint(wallets[i], amounts[i]);
2022-10-holograph/enforcer/HolographERC721.sol::406 => _mint(to, tokenId);
2022-10-holograph/enforcer/HolographERC721.sol::514 => _mint(to, token);
```





#### Non-Critical Issues


### [N01] Adding a return statement when the function defines a named return variable, is redundant



#### Findings:
```
2022-10-holograph/HolographBridge.sol::325 => return reason;
2022-10-holograph/HolographOperator.sol::1170 => return current;
2022-10-holograph/HolographOperator.sol::1179 => return current;
2022-10-holograph/enforcer/HolographERC20.sol::274 => return _decimals;
2022-10-holograph/enforcer/HolographERC20.sol::311 => return _name;
2022-10-holograph/enforcer/HolographERC20.sol::319 => return _symbol;
2022-10-holograph/enforcer/HolographERC20.sol::323 => return _totalSupply;
2022-10-holograph/enforcer/HolographERC721.sol::283 => return _name;
2022-10-holograph/enforcer/HolographERC721.sol::314 => return _symbol;
2022-10-holograph/enforcer/HolographERC721.sol::882 => return currentChain;
2022-10-holograph/enforcer/HolographERC721.sol::884 => return uint32(0);
2022-10-holograph/enforcer/PA1D.sol::630 => return receiver;
2022-10-holograph/enforcer/PA1D.sol::660 => return receiver;
2022-10-holograph/enforcer/PA1D.sol::674 => return bidShares;
```



### [N02] `require()`/`revert()` statements should have descriptive reason strings



#### Findings:
```
2022-10-holograph/HolographBridge.sol::322 => revert(add(payload, 0x20), mload(payload))
2022-10-holograph/HolographBridge.sol::366 => revert(revertReason);
2022-10-holograph/HolographBridge.sol::430 => revert(0, returndatasize())
2022-10-holograph/HolographBridge.sol::578 => revert();
2022-10-holograph/HolographBridge.sol::585 => revert();
2022-10-holograph/HolographFactory.sol::341 => revert();
2022-10-holograph/HolographFactory.sol::348 => revert();
2022-10-holograph/HolographOperator.sol::474 => revert(0, 0)
2022-10-holograph/HolographOperator.sol::562 => revert(0, returndatasize())
2022-10-holograph/HolographOperator.sol::676 => revert(0, returndatasize()));
2022-10-holograph/HolographOperator.sol::1215 => revert();
2022-10-holograph/abstract/ERC20H.sol::225 => revert(0x00, 0x00)
2022-10-holograph/abstract/ERC721H.sol::225 => revert(0x00, 0x00)
2022-10-holograph/enforcer/HolographERC20.sol::265 => revert(0, returndatasize())
2022-10-holograph/enforcer/HolographERC20.sol::328 => require(SourceERC20().beforeApprove(msg.sender, spender, amount));
2022-10-holograph/enforcer/HolographERC20.sol::332 => require(SourceERC20().afterApprove(msg.sender, spender, amount));
2022-10-holograph/enforcer/HolographERC20.sol::339 => require(SourceERC20().beforeBurn(msg.sender, amount));
2022-10-holograph/enforcer/HolographERC20.sol::343 => require(SourceERC20().afterBurn(msg.sender, amount));
2022-10-holograph/enforcer/HolographERC20.sol::354 => require(SourceERC20().beforeBurn(account, amount));
2022-10-holograph/enforcer/HolographERC20.sol::358 => require(SourceERC20().afterBurn(account, amount));
2022-10-holograph/enforcer/HolographERC20.sol::371 => require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
2022-10-holograph/enforcer/HolographERC20.sol::375 => require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));;
2022-10-holograph/enforcer/HolographERC20.sol::430 => require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
2022-10-holograph/enforcer/HolographERC20.sol::434 => require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
2022-10-holograph/enforcer/HolographERC20.sol::484 => require(SourceERC20().beforeApprove(account, spender, amount));
2022-10-holograph/enforcer/HolographERC20.sol::488 => require(SourceERC20().afterApprove(account, spender, amount));
2022-10-holograph/enforcer/HolographERC20.sol::502 => require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));
2022-10-holograph/enforcer/HolographERC20.sol::582 => require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));
2022-10-holograph/enforcer/HolographERC20.sol::586 => require(SourceERC20().afterTransfer(msg.sender, recipient, amount));
2022-10-holograph/enforcer/HolographERC20.sol::606 => require(SourceERC20().beforeTransfer(account, recipient, amount));
2022-10-holograph/enforcer/HolographERC20.sol::610 => require(SourceERC20().afterTransfer(account, recipient, amount));
2022-10-holograph/enforcer/HolographERC20.sol::668 => revert(add(32, reason), mload(reason))
2022-10-holograph/enforcer/HolographERC721.sol::391 => require(SourceERC721().beforeBurn(wallet, tokenId));
2022-10-holograph/enforcer/HolographERC721.sol::395 => require(SourceERC721().afterBurn(wallet, tokenId));
2022-10-holograph/enforcer/HolographERC721.sol::460 => require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));
2022-10-holograph/enforcer/HolographERC721.sol::473 => require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));
2022-10-holograph/enforcer/HolographERC721.sol::486 => require(SourceERC721().beforeApprovalAll(to, approved));
2022-10-holograph/enforcer/HolographERC721.sol::491 => require(SourceERC721().afterApprovalAll(to, approved));
2022-10-holograph/enforcer/HolographERC721.sol::624 => require(SourceERC721().beforeTransfer(from, to, tokenId, data));
2022-10-holograph/enforcer/HolographERC721.sol::628 => require(SourceERC721().afterTransfer(from, to, tokenId, data));
2022-10-holograph/enforcer/HolographERC721.sol::759 => require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));
2022-10-holograph/enforcer/HolographERC721.sol::767 => require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));
2022-10-holograph/enforcer/HolographERC721.sol::979 => revert(0, returndatasize())
2022-10-holograph/enforcer/HolographERC721.sol::993 => revert(0, returndatasize())
2022-10-holograph/enforcer/Holographer.sol::236 => revert(0, returndatasize())
2022-10-holograph/module/LayerZeroModule.sol::199 => revert(0x80, 0xc4)
2022-10-holograph/module/LayerZeroModule.sol::215 => revert(0x80, 0xc4)
2022-10-holograph/module/LayerZeroModule.sol::417 => revert();
2022-10-holograph/module/LayerZeroModule.sol::424 => revert();
```



### [N03] constants should be defined rather than using magic numbers


#### Findings:
```
2022-10-holograph/HolographBridge.sol::551 => jobNonce := add(sload(_jobNonceSlot), 0x0000000000000000000000000000000000000000000000000000000000000001)
2022-10-holograph/HolographOperator.sol::256 => _baseBondAmount = 100 * (10**18); // one single token unit * 100
2022-10-holograph/HolographOperator.sol::265 => _operatorThresholdDivisor = 100; // == * 0.01
2022-10-holograph/HolographOperator.sol::528 => (_randomBlockHash(random, podSize, 3) << 128) |
2022-10-holograph/HolographOperator.sol::706 => uint16(packed >> 128),
2022-10-holograph/HolographOperator.sol::1114 => jobNonce := add(sload(_jobNonceSlot), 0x0000000000000000000000000000000000000000000000000000000000000001)
2022-10-holograph/HolographOperator.sol::1203 => return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
2022-10-holograph/abstract/ERC20H.sol::221 => mstore(0x80, 0x0000000000000000000000000000000000000000000000000000000000000001)
2022-10-holograph/abstract/ERC721H.sol::216 => */
2022-10-holograph/abstract/ERC721H.sol::221 => mstore(0x80, 0x0000000000000000000000000000000000000000000000000000000000000001)
2022-10-holograph/enforcer/HolographERC20.sol::222 => sstore(_reentrantSlot, 0x0000000000000000000000000000000000000000000000000000000000000001)
2022-10-holograph/enforcer/HolographERC721.sol::955 => 0x0000000000000000000000000000000000000000000000000000000050413144
2022-10-holograph/enforcer/PA1D.sol::388 => uint256 gasCost = (23300 * length) + length;
2022-10-holograph/enforcer/PA1D.sol::390 => require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
2022-10-holograph/enforcer/PA1D.sol::395 => sending = ((bps[i] * balance) / 10000);
2022-10-holograph/enforcer/PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/enforcer/PA1D.sol::415 => sending = ((bps[i] * balance) / 10000);
2022-10-holograph/enforcer/PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
2022-10-holograph/enforcer/PA1D.sol::438 => sending = ((bps[i] * balance) / 10000);
2022-10-holograph/enforcer/PA1D.sol::477 => require(totalBp == 10000, "PA1D: bps down't equal 10000");
2022-10-holograph/enforcer/PA1D.sol::551 => return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
2022-10-holograph/enforcer/PA1D.sol::553 => return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
2022-10-holograph/enforcer/PA1D.sol::641 => return (_getDefaultBp() * amount) / 10000;
2022-10-holograph/enforcer/PA1D.sol::643 => return (_getBp(tokenId) * amount) / 10000;
2022-10-holograph/module/LayerZeroModule.sol::195 => mstore(0x80, 0x08c379a000000000000000000000000000000000000000000000000000000000)
2022-10-holograph/module/LayerZeroModule.sol::196 => mstore(0xa0, 0x0000002000000000000000000000000000000000000000000000000000000000)
2022-10-holograph/module/LayerZeroModule.sol::198 => mstore(0xe0, 0x0000000000000000000000000000000000000000000000000000000000000000)
2022-10-holograph/module/LayerZeroModule.sol::211 => mstore(0x80, 0x08c379a000000000000000000000000000000000000000000000000000000000)
2022-10-holograph/module/LayerZeroModule.sol::212 => mstore(0xa0, 0x0000002000000000000000000000000000000000000000000000000000000000)
2022-10-holograph/module/LayerZeroModule.sol::214 => mstore(0xe0, 0x6572000000000000000000000000000000000000000000000000000000000000)
```






### [N04] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
2022-10-holograph/enforcer/PA1D.sol::153 => event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```






### [N05] Open TODOs

#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
#### Findings:
```
2022-10-holograph/HolographOperator.sol::701 => // TODO: move the bit-shifting around to have it be sequential
```





### [N06] `2**<n> - 1` should be re-written as `type(uint<n>).max`

#### Impact
Earlier versions of solidity can use uint<n>(-1) instead. Expressions not including the - 1 can often be re-written to accomodate the change (e.g. by using a > rather than a >=, which will also save some gas)
#### Findings:
```
2022-10-holograph/HolographOperator.sol::1172 => uint256 threshold = _operatorThreshold / (2**pod);
```




### [N07] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external.
#### Findings:
```
2022-10-holograph/HolographOperator.sol::690 => function getJobDetails(bytes32 jobHash) public view returns (OperatorJob memory) {
2022-10-holograph/enforcer/HolographERC20.sol::273 => function decimals() public view returns (uint8) {
2022-10-holograph/enforcer/HolographERC20.sol::297 => function allowance(address account, address spender) public view returns (uint256) {
2022-10-holograph/enforcer/HolographERC20.sol::301 => function balanceOf(address account) public view returns (uint256) {
2022-10-holograph/enforcer/HolographERC20.sol::306 => function DOMAIN_SEPARATOR() public view returns (bytes32) {
2022-10-holograph/enforcer/HolographERC20.sol::310 => function name() public view returns (string memory) {
2022-10-holograph/enforcer/HolographERC20.sol::314 => function nonces(address account) public view returns (uint256) {
2022-10-holograph/enforcer/HolographERC20.sol::318 => function symbol() public view returns (string memory) {
2022-10-holograph/enforcer/HolographERC20.sol::322 => function totalSupply() public view returns (uint256) {
2022-10-holograph/enforcer/HolographERC20.sol::326 => function approve(address spender, uint256 amount) public returns (bool) {
2022-10-holograph/enforcer/HolographERC20.sol::337 => function burn(uint256 amount) public {
2022-10-holograph/enforcer/HolographERC20.sol::347 => function burnFrom(address account, uint256 amount) public returns (bool) {
2022-10-holograph/enforcer/HolographERC20.sol::363 => function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
2022-10-holograph/enforcer/HolographERC20.sol::420 => function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
2022-10-holograph/enforcer/HolographERC20.sol::492 => function safeTransfer(address recipient, uint256 amount) public returns (bool) {
2022-10-holograph/enforcer/HolographERC20.sol::580 => function transfer(address recipient, uint256 amount) public returns (bool) {
2022-10-holograph/enforcer/HolographERC20.sol::740 => function owner() public view override returns (address) {
2022-10-holograph/enforcer/HolographERC721.sol::638 => function balanceOf(address wallet) public view returns (uint256) {
2022-10-holograph/enforcer/HolographERC721.sol::643 => function burned(uint256 tokenId) public view returns (bool) {
2022-10-holograph/enforcer/HolographERC721.sol::656 => function exists(uint256 tokenId) public view returns (bool) {
2022-10-holograph/enforcer/HolographERC721.sol::935 => function owner() public view override returns (address) {
2022-10-holograph/enforcer/Holographer.sol::192 => function getHolographEnforcer() public view returns (address) {
2022-10-holograph/enforcer/PA1D.sol::471 => function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
2022-10-holograph/enforcer/PA1D.sol::488 => function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
2022-10-holograph/enforcer/PA1D.sol::497 => function getEthPayout() public {
2022-10-holograph/enforcer/PA1D.sol::507 => function getTokenPayout(address tokenAddress) public {
2022-10-holograph/enforcer/PA1D.sol::517 => function getTokensPayout(address[] memory tokenAddresses) public {
2022-10-holograph/enforcer/PA1D.sol::549 => function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
2022-10-holograph/enforcer/PA1D.sol::558 => function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
2022-10-holograph/enforcer/PA1D.sol::569 => function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
2022-10-holograph/enforcer/PA1D.sol::590 => function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
2022-10-holograph/enforcer/PA1D.sol::604 => function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
2022-10-holograph/enforcer/PA1D.sol::649 => function marketContract() public view returns (address) {
2022-10-holograph/enforcer/PA1D.sol::655 => function tokenCreators(uint256 tokenId) public view returns (address) {
2022-10-holograph/enforcer/PA1D.sol::665 => function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
2022-10-holograph/enforcer/PA1D.sol::683 => function getTokenAddress(string memory tokenName) public view returns (address) {
```



### [N08] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-10-holograph/HolographOperator.sol::254 => _blockTime = 60; // 60 seconds allowed for execution
```



### [N09] Constant redefined elsewhere

#### Impact
Consider defining in only one contract so that values cannot become out of sync when only one location is updated
#### Findings:
```
2022-10-holograph/HolographFactory.sol::206 => bytes32 hash = keccak256(
2022-10-holograph/HolographOperator.sol::305 => bytes32 hash = keccak256(bridgeInRequestPayload);
2022-10-holograph/HolographOperator.sol::491 => bytes32 jobHash = keccak256(bridgeInRequestPayload);
2022-10-holograph/enforcer/HolographERC20.sol::470 => bytes32 structHash = keccak256(
2022-10-holograph/enforcer/PA1D.sol::308 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/enforcer/PA1D.sol::324 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/enforcer/PA1D.sol::341 => slot = keccak256(abi.encodePacked(i, slot));
2022-10-holograph/enforcer/PA1D.sol::357 => slot = keccak256(abi.encodePacked(i, slot));
```







