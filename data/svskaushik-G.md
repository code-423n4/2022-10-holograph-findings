# Gas Findings

## [G-1] Cache Array Length Outside of Loop
#### Impact
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack. Caching the array length in the stack saves around 3 gas per iteration.
#### Findings:
```solidity
holo\HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
holo\HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
holo\HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
holo\HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
holo\PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
holo\PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```
#### Recommendation
Store the array’s length in a variable before the for-loop.

## [G-2] Use `!= 0` instead of `> 0` for Unsigned Integer Comparison in require statements
#### Impact
`!= 0` is cheapear than `> 0` when comparing unsigned integers in require statements.
#### Findings:
```solidity
holo\HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
holo\HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
holo\HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
```
#### Recommendation
Use `!= 0` instead of `> 0`.

## [G-3] Reduce the size of error messages (Long revert Strings).
#### Impact
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met. Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.
#### Findings:
```solidity
holo\PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
holo\PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
```
#### Recommendation
Shorten the revert strings to fit in 32 bytes, or use custom errors if >0.8.4.

## [G-4] Use Custom Errors instead of Revert Strings.
#### Impact
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
#### Findings:
```solidity
holo\ERC20H.sol::117 => require(msg.sender == holographer(), "ERC20: holographer only");
holo\ERC20H.sol::123 => require(msgSender() == _getOwner(), "ERC20: owner only function");
holo\ERC20H.sol::125 => require(msg.sender == _getOwner(), "ERC20: owner only function");
holo\ERC20H.sol::147 => require(!_isInitialized(), "ERC20: already initialized");
holo\ERC721H.sol::117 => require(msg.sender == holographer(), "ERC721: holographer only");
holo\ERC721H.sol::123 => require(msgSender() == _getOwner(), "ERC721: owner only function");
holo\ERC721H.sol::125 => require(msg.sender == _getOwner(), "ERC721: owner only function");
holo\ERC721H.sol::147 => require(!_isInitialized(), "ERC721: already initialized");
holo\HolographBridge.sol::148 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
holo\HolographBridge.sol::163 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
holo\HolographBridge.sol::214 => require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
holo\HolographBridge.sol::233 => require(doNotRevert, "HOLOGRAPH: reverted");
holo\HolographBridge.sol::270 => require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
holo\HolographERC20.sol::192 => require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");
holo\HolographERC20.sol::204 => require(msg.sender == sourceContract, "ERC20: source only call");
holo\HolographERC20.sol::219 => require(!_isInitialized(), "ERC20: already initialized");
holo\HolographERC20.sol::241 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");
holo\HolographERC20.sol::349 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
holo\HolographERC20.sol::365 => require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
holo\HolographERC20.sol::387 => require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");
holo\HolographERC20.sol::400 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
holo\HolographERC20.sol::427 => require(newAllowance >= currentAllowance, "ERC20: increased above max value");
holo\HolographERC20.sol::445 => require(_isContract(account), "ERC20: operator not contract");
holo\HolographERC20.sol::450 => require(balance >= amount, "ERC20: balance check failed");
holo\HolographERC20.sol::469 => require(block.timestamp <= deadline, "ERC20: expired deadline");
holo\HolographERC20.sol::482 => require(signer == account, "ERC20: invalid signature");
holo\HolographERC20.sol::505 => require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");
holo\HolographERC20.sol::529 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
holo\HolographERC20.sol::539 => require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");
holo\HolographERC20.sol::599 => require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
holo\HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");
holo\HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");
holo\HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");
holo\HolographERC20.sol::629 => require(accountBalance >= amount, "ERC20: amount exceeds balance");
holo\HolographERC20.sol::645 => require(erc165support, "ERC20: no ERC165 support");
holo\HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");
holo\HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");
holo\HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");
holo\HolographERC20.sol::698 => require(accountBalance >= amount, "ERC20: amount exceeds balance");
holo\HolographERC721.sol::212 => require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");
holo\HolographERC721.sol::224 => require(msg.sender == sourceContract, "ERC721: source only call");
holo\HolographERC721.sol::239 => require(!_isInitialized(), "ERC721: already initialized");
holo\HolographERC721.sol::258 => require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
holo\HolographERC721.sol::263 => require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
holo\HolographERC721.sol::323 => require(_exists(tokenId), "ERC721: token does not exist");
holo\HolographERC721.sol::370 => require(to != tokenOwner, "ERC721: cannot approve self");
holo\HolographERC721.sol::371 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
holo\HolographERC721.sol::388 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
holo\HolographERC721.sol::404 => require(!_exists(tokenId), "ERC721: token already exists");
holo\HolographERC721.sol::408 => require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");
holo\HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");
holo\HolographERC721.sol::420 => require(_isApproved(sender, tokenId), "ERC721: sender not approved");
holo\HolographERC721.sol::421 => require(from == _tokenOwner[tokenId], "ERC721: from is not owner");
holo\HolographERC721.sol::458 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
holo\HolographERC721.sol::484 => require(to != msg.sender, "ERC721: cannot approve self");
holo\HolographERC721.sol::513 => require(!_burnedTokens[token], "ERC721: can't mint burned token");
holo\HolographERC721.sol::622 => require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
holo\HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");
holo\HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");
holo\HolographERC721.sol::700 => require(index < _allTokens.length, "ERC721: index out of bounds");
holo\HolographERC721.sol::729 => require(index < balanceOf(wallet), "ERC721: index out of bounds");
holo\HolographERC721.sol::757 => require(_isContract(_operator), "ERC721: operator not contract");
holo\HolographERC721.sol::762 => require(tokenOwner == address(this), "ERC721: contract not token owner");
holo\HolographERC721.sol::815 => require(tokenId > 0, "ERC721: token id cannot be zero");
holo\HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");
holo\HolographERC721.sol::817 => require(!_exists(tokenId), "ERC721: token already exists");
holo\HolographERC721.sol::818 => require(!_burnedTokens[tokenId], "ERC721: token has been burned");
holo\HolographERC721.sol::869 => require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
holo\HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");
holo\HolographERC721.sol::906 => require(_exists(tokenId), "ERC721: token does not exist");
holo\HolographFactory.sol::144 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
holo\HolographFactory.sol::220 => require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
holo\HolographFactory.sol::228 => require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
holo\HolographOperator.sol::241 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
holo\HolographOperator.sol::309 => require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
holo\HolographOperator.sol::350 => require(timeDifference > 0, "HOLOGRAPH: operator has time");
holo\HolographOperator.sol::354 => require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
holo\HolographOperator.sol::368 => require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
holo\HolographOperator.sol::415 => require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
holo\HolographOperator.sol::446 => require(msg.sender == address(this), "HOLOGRAPH: operator only call");
holo\HolographOperator.sol::485 => require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
holo\HolographOperator.sol::591 => require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
holo\HolographOperator.sol::595 => require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
holo\HolographOperator.sol::728 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
holo\HolographOperator.sol::739 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
holo\HolographOperator.sol::756 => require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
holo\HolographOperator.sol::829 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
holo\HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
holo\HolographOperator.sol::857 => require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
holo\HolographOperator.sol::863 => require(current <= amount, "HOLOGRAPH: bond amount too small");
holo\HolographOperator.sol::881 => require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
holo\HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
holo\HolographOperator.sol::903 => require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
holo\HolographOperator.sol::911 => require(_isContract(operator), "HOLOGRAPH: operator not contract");
holo\HolographOperator.sol::915 => require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
holo\HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
holo\Holographer.sol::148 => require(!_isInitialized(), "HOLOGRAPHER: already initialized");
holo\Holographer.sol::166 => require(success && selector == InitializableInterface.init.selector, "initialization failed");
holo\LayerZeroModule.sol::159 => require(!_isInitialized(), "HOLOGRAPH: already initialized");
holo\LayerZeroModule.sol::235 => require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
holo\PA1D.sol::159 => require(isOwner(), "PA1D: caller not an owner");
holo\PA1D.sol::174 => require(!_isInitialized(), "PA1D: already initialized");
holo\PA1D.sol::190 => require(initialized == 0, "PA1D: already initialized");
holo\PA1D.sol::390 => require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
holo\PA1D.sol::411 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
holo\PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
holo\PA1D.sol::435 => require(balance > 10000, "PA1D: Not enough tokens to transfer");
holo\PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
holo\PA1D.sol::460 => require(matched, "PA1D: sender not authorized");
holo\PA1D.sol::472 => require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
holo\PA1D.sol::477 => require(totalBp == 10000, "PA1D: bps down't equal 10000");
```
#### Recommendation
Use custom errors instead of revert strings.

## [G-5] No need to initialize variables with default values
#### Impact
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.
#### Findings:
```solidity
holo\HolographBridge.sol::380 => uint256 fee = 0;
holo\HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
holo\HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
holo\HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
holo\HolographOperator.sol::310 => uint256 gasLimit = 0;
holo\HolographOperator.sol::311 => uint256 gasPrice = 0;
holo\HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
holo\PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```
#### Recommendation
Remove explicit default initializations.

## [G-6] `++i` costs less gas compared to `i++` or `i += 1`
#### Impact
`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.
#### Findings:
```solidity
holo\HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
holo\HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
holo\HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
holo\HolographERC721.sol::842 => _ownedTokensCount[from]--;
holo\HolographOperator.sol::520 => podSize--;
holo\HolographOperator.sol::760 => pod--;
holo\HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
holo\HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
holo\PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
holo\PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```
#### Recommendation
Use `++i` instead of `i++` to increment the value of an uint variable. Same thing for `--i` and `i--`.


## [G-7] Use `calldata` instead of `memory` for read-only arguments in `external` functions.
#### Impact
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.
#### Findings:
```solidity
holo\ERC20H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
holo\ERC721H.sol::140 => function init(bytes memory initPayload) external virtual override returns (bytes4) {
holo\HolographBridge.sol::162 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\HolographERC20.sol::218 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\HolographERC721.sol::238 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\HolographFactory.sol::143 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\HolographOperator.sol::240 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\Holographer.sol::147 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\LayerZeroModule.sol::158 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\PA1D.sol::173 => function init(bytes memory initPayload) external override returns (bytes4) {
holo\PA1D.sol::185 => function initPA1D(bytes memory initPayload) external returns (bytes4) {
```
#### Recommendation
Use `calldata` instead of `memory`.


## [G-8] `x += y` costs more gas than `x = x + y` for state variables.
#### Impact
Same thing applies for subtraction
#### Findings:
```solidity
holo\HolographERC20.sol::633 => _totalSupply -= amount;
holo\HolographERC20.sol::685 => _totalSupply += amount;
holo\HolographERC20.sol::686 => _balances[to] += amount;
holo\HolographERC20.sol::702 => _balances[recipient] += amount;
holo\HolographOperator.sol::378 => _bondedAmounts[job.operator] -= amount;
holo\HolographOperator.sol::382 => _bondedAmounts[msg.sender] += amount;
holo\HolographOperator.sol::834 => _bondedAmounts[operator] += amount;
```
#### Recommendation
Use `x = x + y` instead of `x += y`

## [G-9] Use assembly to check for address(0)
#### Impact
Saves 6 gas per instance if using assembly to check for zero address
#### Findings:
```solidity
holo\HolographERC20.sol::620 => require(account != address(0), "ERC20: account is zero address");
holo\HolographERC20.sol::621 => require(spender != address(0), "ERC20: spender is zero address");
holo\HolographERC20.sol::627 => require(account != address(0), "ERC20: account is zero address");
holo\HolographERC20.sol::684 => require(to != address(0), "ERC20: minting to burn address");
holo\HolographERC20.sol::695 => require(account != address(0), "ERC20: account is zero address");
holo\HolographERC20.sol::696 => require(recipient != address(0), "ERC20: recipient is zero address");
holo\HolographERC721.sol::419 => require(to != address(0), "ERC721: zero address");
holo\HolographERC721.sol::639 => require(wallet != address(0), "ERC721: zero address");
holo\HolographERC721.sol::657 => return _tokenOwner[tokenId] != address(0);
holo\HolographERC721.sol::689 => require(tokenOwner != address(0), "ERC721: token does not exist");
holo\HolographERC721.sol::816 => require(to != address(0), "ERC721: minting to burn address");
holo\HolographERC721.sol::870 => require(to != address(0), "ERC721: use burn instead");
holo\HolographERC721.sol::895 => return tokenOwner != address(0);
holo\HolographOperator.sol::333 => if (job.operator != address(0)) {
```
#### Recommendation
Consider using assembly to check for zero address checks

## [G-10] Functions guaranteed to revert when called by normal users can be marked `payable`
#### Impact
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```solidity
holo\HolographBridge.sol::452 => function setFactory(address factory) external onlyAdmin {
holo\HolographBridge.sol::472 => function setHolograph(address holograph) external onlyAdmin {
holo\HolographBridge.sol::502 => function setOperator(address operator) external onlyAdmin {
holo\HolographBridge.sol::522 => function setRegistry(address registry) external onlyAdmin {
holo\HolographERC20.sol::380 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
holo\HolographERC20.sol::396 => ) external onlyBridge returns (bytes4 selector, bytes memory data) {
holo\HolographERC20.sol::415 => function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
holo\HolographERC20.sol::549 => function sourceBurn(address from, uint256 amount) external onlySource {
holo\HolographERC20.sol::556 => function sourceMint(address to, uint256 amount) external onlySource {
holo\HolographERC20.sol::563 => function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
holo\HolographERC20.sol::576 => ) external onlySource {
holo\HolographERC721.sol::399 => function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
holo\HolographERC721.sol::417 => ) external onlyBridge returns (bytes4 selector, bytes memory data) {
holo\HolographERC721.sol::500 => function sourceBurn(uint256 tokenId) external onlySource {
holo\HolographERC721.sol::508 => function sourceMint(address to, uint224 tokenId) external onlySource {
holo\HolographERC721.sol::520 => function sourceGetChainPrepend() external view onlySource returns (uint256) {
holo\HolographERC721.sol::577 => function sourceTransfer(address to, uint256 tokenId) external onlySource {
holo\HolographFactory.sol::280 => function setHolograph(address holograph) external onlyAdmin {
holo\HolographFactory.sol::300 => function setRegistry(address registry) external onlyAdmin {
holo\HolographOperator.sol::285 => ) external onlyAdmin {
holo\HolographOperator.sol::949 => function setBridge(address bridge) external onlyAdmin {
holo\HolographOperator.sol::969 => function setHolograph(address holograph) external onlyAdmin {
holo\HolographOperator.sol::989 => function setInterfaces(address interfaces) external onlyAdmin {
holo\HolographOperator.sol::1009 => function setMessagingModule(address messagingModule) external onlyAdmin {
holo\HolographOperator.sol::1029 => function setRegistry(address registry) external onlyAdmin {
holo\HolographOperator.sol::1049 => function setUtilityToken(address utilityToken) external onlyAdmin {
holo\LayerZeroModule.sol::320 => function setBridge(address bridge) external onlyAdmin {
holo\LayerZeroModule.sol::340 => function setInterfaces(address interfaces) external onlyAdmin {
holo\LayerZeroModule.sol::360 => function setLZEndpoint(address lZEndpoint) external onlyAdmin {
holo\LayerZeroModule.sol::380 => function setOperator(address operator) external onlyAdmin {
holo\LayerZeroModule.sol::441 => function setBaseGas(uint256 baseGas) external onlyAdmin {
holo\LayerZeroModule.sol::470 => function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
holo\PA1D.sol::533 => ) public onlyOwner {
```
#### Recommendation
Consider marking above functions as payable

## [G-11] `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for/while` loops
#### Impact
This saves 30-60 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)
#### Findings:
```solidity
holo\HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
holo\HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
holo\HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
holo\HolographERC721.sol::842 => _ownedTokensCount[from]--;
holo\HolographOperator.sol::520 => podSize--;
holo\HolographOperator.sol::760 => pod--;
holo\HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
holo\HolographOperator.sol::871 => for (uint256 i = _operatorPods.length; i <= pod; i++) {
holo\PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
holo\PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
holo\PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
holo\PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
```
#### Recommendation
Consider doing incrementation/decrementation `unchecked{}`

## [G-12] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
#### Impact
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined.Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot
#### Findings:
```solidity
holo\HolographERC20.sol::156 => mapping(address => uint256) private _balances;
holo\HolographERC20.sol::161 => mapping(address => mapping(address => uint256)) private _allowances;
holo\HolographERC20.sol::186 => mapping(address => uint256) private _nonces;
holo\HolographERC721.sol::185 => mapping(address => uint256) private _ownedTokensCount;
holo\HolographERC721.sol::190 => mapping(address => uint256[]) private _ownedTokens;
holo\HolographERC721.sol::196 => mapping(address => mapping(address => bool)) private _operatorApprovals;
holo\HolographOperator.sol::218 => mapping(address => uint256) private _bondedOperators;
holo\HolographOperator.sol::223 => mapping(address => uint256) private _operatorPodIndex;
holo\HolographOperator.sol::228 => mapping(address => uint256) private _bondedAmounts;
```
#### Recommendation
Consider combining mappings where appropriate

## [G-13] `internal` functions only called once can be inlined to save gas
#### Impact
Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.
#### Findings:
```solidity
holo\ERC20H.sol::203 => function _setOwner(address ownerAddress) internal {
holo\ERC721H.sol::203 => function _setOwner(address ownerAddress) internal {
```
#### Recommendation
Consider inling above functions