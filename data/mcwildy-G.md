# GAS OPTIMIZATIONS REPORT

## HOLOGRAPH

### Mark functions as `payable` to avoid the low-level evm `is-a-payment-provided` check

The EVM checks whether a payment is provided to the function and this check costs additional gas. If the function is marked as `payable` there is no such check

[ERC721H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol):

```solidity
line#L123: require(msgSender() == _getOwner(), "ERC721: owner only function");
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L380: function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

line#L415: function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L399: function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L452: function setFactory(address factory) external onlyAdmin {

line#L472: function setHolograph(address holograph) external onlyAdmin {

line#L502: function setOperator(address operator) external onlyAdmin {

line#L522: function setRegistry(address registry) external onlyAdmin {
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L471: function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```

[HolographFactory.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol):

```solidity
line#L280: function setHolograph(address holograph) external onlyAdmin {

line#L300: function setRegistry(address registry) external onlyAdmin {
```

[ERC20H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol):

```solidity
line#L123: require(msgSender() == _getOwner(), "ERC20: owner only function");
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L445: function nonRevertingBridgeCall(address msgSender, bytes calldata payload) external payable {

line#L484: function crossChainMessage(bytes calldata bridgeInRequestPayload) external payable {

line#L969: function setHolograph(address holograph) external onlyAdmin {

line#L989: function setInterfaces(address interfaces) external onlyAdmin {

line#L1029:function setRegistry(address registry) external onlyAdmin {

line#L1049:function setUtilityToken(address utilityToken) external onlyAdmin {
```

### Avoid using `&&` in `require()` or `if()` statements

Use multiple `require()`/`if` statements instead

[Holographer.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol):

```solidity
line#L166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

line#L464: require(
line#L465: (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
line#L466: ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
line#L467: ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
line#L468: ERC721TokenReceiver.onERC721Received.selector),
line#L469: "ERC721: onERC721Received fail"
line#L470: );
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

### Use `calldata` where possible instead of `memory` to save gas

[HolographFactory.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol):

```solidity
line#L143: function init(bytes memory initPayload) external override returns (bytes4) {

line#L193: DeploymentConfig memory config,

line#L194: Verification memory signature,
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L218: function init(bytes memory initPayload) external override returns (bytes4) {

line#L499: bytes memory data

line#L524: bytes memory data

line#L641: bytes memory data
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L240: function init(bytes memory initPayload) external override returns (bytes4) {
```

[Holographer.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol):

```solidity
line#L147: function init(bytes memory initPayload) external override returns (bytes4) {
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L238: function init(bytes memory initPayload) external override returns (bytes4) {

line#L456: bytes memory data

line#L620: bytes memory data
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L173: function init(bytes memory initPayload) external override returns (bytes4) {

line#L185: function initPA1D(bytes memory initPayload) external returns (bytes4) {

line#L349: function _setPayoutBps(uint256[] memory bps) private {

line#L365: function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

line#L426: function _payoutTokens(address[] memory tokenAddresses) private {

line#L471: function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

line#L517: function getTokensPayout(address[] memory tokenAddresses) public {

line#L683: function getTokenAddress(string memory tokenName) public view returns (address) {
```

[ERC20H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol):

```solidity
line#L140: function init(bytes memory initPayload) external virtual override returns (bytes4) {
```

[ERC721H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol):

```solidity
line#L140: function init(bytes memory initPayload) external virtual override returns (bytes4) {
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L162: function init(bytes memory initPayload) external override returns (bytes4) {
```

### Use `unchecked` for the counter in `for` loops

Since it is compared to a target value (usually the length of an array) there is no way the counter overflows

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L564: for (uint256 i = 0; i < wallets.length; i++) {
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L357: for (uint256 i = 0; i < length; i++) {

line#L716: for (uint256 i = 0; i < length; i++) {
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L307: for (uint256 i = 0; i < length; i++) {

line#L323: for (uint256 i = 0; i < length; i++) {

line#L356: for (uint256 i = 0; i < length; i++) {

line#L394: for (uint256 i = 0; i < length; i++) {

line#L432: for (uint256 t = 0; t < tokenAddresses.length; t++) {

line#L437: for (uint256 i = 0; i < addresses.length; i++) {

line#L474: for (uint256 i = 0; i < addresses.length; i++) {
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L781: for (uint256 i = 0; i < length; i++) {

line#L871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

### Use `constant` and `immutable` keywords for storage varibles if they doesn't change through contract's lifecycle

That way the variable will go to the contract's bytecode and will not allocate a storage slot

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L762: require(tokenOwner == address(this), "ERC721: contract not token owner");

line#L764: revert("ERC721: token does not exist");

line#L767: require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));

line#L769: return ERC721TokenReceiver.onERC721Received.selector;
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L151: uint256 private _eventConfig;

line#L171: string private _name;

line#L176: string private _symbol;

line#L181: uint8 private _decimals;
```

### Cache each array.length before the `for` loop execution

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

### Cache storage variables in memory

Optimization comes from using MSTORE and MLOAD instead of SSTORE and SLOAD

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
           // Cache `_operatorTempStorageCounter`
line#L495: ++_operatorTempStorageCounter;
line#L517: _operatorTempStorage[_operatorTempStorageCounter] = _operatorPods[pod][operatorIndex];
line#L524: (uint256(_operatorTempStorageCounter) << 216) |

           // Cache `_operatorPods`
line#L363: if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
line#L364: address fallbackOperator = _operatorPods[pod][podIndex];
line#L390: _operatorPods[pod].push(job.operator);
line#L391: _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
line#L407: _operatorPods[pod].push(msg.sender);
line#L408: _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;

           // Cache `_operatorPods`
line#L867: if (_operatorPods.length < pod) {
line#L875: _operatorPods.push([address(0)]);
line#L881: require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
line#L882: _operatorPods[pod - 1].push(operator);
line#L883: _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;

           // Cache `_operatorPods`
line#L1128:address operator = _operatorPods[pod][operatorIndex];
line#L1137:uint256 lastIndex = _operatorPods[pod].length - 1;
line#L1148:delete _operatorPods[pod][lastIndex];
line#L1152:_operatorPods[pod].pop();
```

### Do not use strings longer than 32 bytes

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L411: require(balance > 10000, "PA1D: Not enough tokens to transfer");

line#L435: require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

### Use custom errors instead of `require` strings

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L241: require(!_isInitialized(), "HOLOGRAPH: already initialized");

line#L309: require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

line#L354: require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

line#L368: require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

line#L446: require(msg.sender == address(this), "HOLOGRAPH: operator only call");

line#L485: require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

line#L595: require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

line#L728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

line#L756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

line#L829: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

line#L857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

line#L863: require(current <= amount, "HOLOGRAPH: bond amount too small");

line#L889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

line#L903: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

line#L915: require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

line#L932: require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L148: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

line#L163: require(!_isInitialized(), "HOLOGRAPH: already initialized");

line#L214: require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

line#L224: require(
line#L225: HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==
line#L226: HolographERC20Interface.holographBridgeMint.selector,
line#L227: "HOLOGRAPH: hToken mint failed"
line#L228: );

line#L255: require(
line#L256: _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
line#L257: "HOLOGRAPH: not holographed"
line#L258: );

line#L270: require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L159: require(isOwner(), "PA1D: caller not an owner");

line#L174: require(!_isInitialized(), "PA1D: already initialized");

line#L390: require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

line#L411: require(balance > 10000, "PA1D: Not enough tokens to transfer");

line#L435: require(balance > 10000, "PA1D: Not enough tokens to transfer");

line#L439: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

line#L472: require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

line#L477: require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

[ERC721H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol):

```solidity
line#L117: require(msg.sender == holographer(), "ERC721: holographer only");

line#L123: require(msgSender() == _getOwner(), "ERC721: owner only function");

line#L125: require(msg.sender == _getOwner(), "ERC721: owner only function");

line#L147: require(!_isInitialized(), "ERC721: already initialized");
```

[HolographFactory.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol):

```solidity
line#L144: require(!_isInitialized(), "HOLOGRAPH: already initialized");

line#L220: require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

line#L228: require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

line#L250: require(
line#L251: InitializableInterface(holographerAddress).init(
line#L252: abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
line#L253: ) == InitializableInterface.init.selector,
line#L254: "initialization failed"
line#L255: );
```

[ERC20H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol):

```solidity
line#L117: require(msg.sender == holographer(), "ERC20: holographer only");

line#L123: require(msgSender() == _getOwner(), "ERC20: owner only function");

line#L125: require(msg.sender == _getOwner(), "ERC20: owner only function");

line#L147: require(!_isInitialized(), "ERC20: already initialized");
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L212: require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

line#L224: require(msg.sender == sourceContract, "ERC721: source only call");

line#L258: require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

line#L263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

line#L370: require(to != tokenOwner, "ERC721: cannot approve self");

line#L371: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

line#L404: require(!_exists(tokenId), "ERC721: token already exists");

line#L408: require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

line#L420: require(_isApproved(sender, tokenId), "ERC721: sender not approved");

line#L421: require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

line#L464: require(
line#L465: (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
line#L466: ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
line#L467: ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
line#L468: ERC721TokenReceiver.onERC721Received.selector),
line#L469: "ERC721: onERC721Received fail"
line#L470: );

line#L484: require(to != msg.sender, "ERC721: cannot approve self");

line#L622: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

line#L639: require(wallet != address(0), "ERC721: zero address");

line#L700: require(index < _allTokens.length, "ERC721: index out of bounds");

line#L729: require(index < balanceOf(wallet), "ERC721: index out of bounds");

line#L762: require(tokenOwner == address(this), "ERC721: contract not token owner");

line#L764: revert("ERC721: token does not exist");

line#L816: require(to != address(0), "ERC721: minting to burn address");

line#L817: require(!_exists(tokenId), "ERC721: token already exists");

line#L869: require(_tokenOwner[tokenId] == from, "ERC721: token not owned");

line#L870: require(to != address(0), "ERC721: use burn instead");
```

[Holographer.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol):

```solidity
line#L148: require(!_isInitialized(), "HOLOGRAPHER: already initialized");

line#L166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L192: require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");

line#L204: require(msg.sender == sourceContract, "ERC20: source only call");

line#L241: require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

line#L349: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

line#L387: require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

line#L400: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

line#L445: require(_isContract(account), "ERC20: operator not contract");

line#L450: require(balance >= amount, "ERC20: balance check failed");

line#L469: require(block.timestamp <= deadline, "ERC20: expired deadline");

line#L482: require(signer == account, "ERC20: invalid signature");

line#L529: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

line#L539: require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");

line#L620: require(account != address(0), "ERC20: account is zero address");

line#L621: require(spender != address(0), "ERC20: spender is zero address");

line#L629: require(accountBalance >= amount, "ERC20: amount exceeds balance");

line#L645: require(erc165support, "ERC20: no ERC165 support");

line#L661: revert("ERC20: eip-4524 not supported");

line#L665: revert("ERC20: no ERC165 support");

line#L695: require(account != address(0), "ERC20: account is zero address");

line#L696: require(recipient != address(0), "ERC20: recipient is zero address");
```

### Do not write default values to variables

If you declare a variables of type `uint256` it will automatically get assigned to its default value of 0. It is a gas wastage to assign it yourself.

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L310: uint256 gasLimit = 0;

line#L311: uint256 gasPrice = 0;

line#L781: for (uint256 i = 0; i < length; i++) {
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L380: uint256 fee = 0;
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L307: for (uint256 i = 0; i < length; i++) {

line#L323: for (uint256 i = 0; i < length; i++) {

line#L356: for (uint256 i = 0; i < length; i++) {

line#L394: for (uint256 i = 0; i < length; i++) {

line#L432: for (uint256 t = 0; t < tokenAddresses.length; t++) {

line#L437: for (uint256 i = 0; i < addresses.length; i++) {

line#L474: for (uint256 i = 0; i < addresses.length; i++) {
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L564: for (uint256 i = 0; i < wallets.length; i++) {
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L357: for (uint256 i = 0; i < length; i++) {

line#L716: for (uint256 i = 0; i < length; i++) {
```

### Use `if (x != 0)` instead of `if (x > 0)` for uints comparison

Saves 3 gas per `if`-check

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L815: require(tokenId > 0, "ERC721: token id cannot be zero");
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L218: if (hTokenValue > 0) {
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L350: require(timeDifference > 0, "HOLOGRAPH: operator has time");

line#L363: if (podIndex > 0 && podIndex < _operatorPods[pod].length) {

line#L398: if (leftovers > 0) {

line#L1126:if (operatorIndex > 0) {
```

### Function inlining

If a function is called only once it can be inlined in order to save 20 gas

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L711: function _useNonce(address account) private returns (uint256 current) {

line#L736: function _interfaces() private view returns (address) {
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L824: function _removeTokenFromAllTokensEnumeration(uint256 tokenId) private {
```

[HolographFactory.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol):

```solidity
line#L309: function _isContract(address contractAddress) private view returns (bool) {

line#L320: function _verifySigner(
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L226: function _setDefaultReceiver(address receiver) private {

line#L246: function _setDefaultBp(uint256 bp) private {

line#L291: function _setBp(uint256 tokenId, uint256 bp) private {

line#L316: function _setPayoutAddresses(address payable[] memory addresses) private {

line#L365: function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

line#L382: function _payoutEth() private {

line#L426: function _payoutTokens(address[] memory tokenAddresses) private {
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L1058:function _bridge() private view returns (address bridge) {

line#L1094:function _registry() private view returns (HolographRegistryInterface registry) {

line#L1112:function _jobNonce() private returns (uint256 jobNonce) {

line#L1198:function _isContract(address contractAddress) private view returns (bool) {
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L549: function _jobNonce() private returns (uint256 jobNonce) {
```

### Use `uint256` instead of the smaller uints where possible

The word-size in ethereum is 32 bytes which means that every variables which is sized with less bytes will cost more to operate with

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L160: uint16 private _bps;

line#L248: uint16 contractBps,

line#L414: uint32 toChain,

line#L879: uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L181: uint8 private _decimals;

line#L229: uint8 contractDecimals,

line#L393: uint32 toChain,

line#L465: uint8 v,
```

[HolographFactory.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol):

```solidity
line#L323: uint8 v,
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L208: uint32 private _operatorTempStorageCounter;

line#L585: uint32 toChain,
```

[HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol):

```solidity
line#L192: uint32 fromChain,

line#L246: uint32 toChain,

line#L298: uint32 toChain,

line#L343: uint32 toChain,
```

[Holographer.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol):

```solidity
line#L150: (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(

line#L205: function getOriginChain() external view returns (uint32 originChain) {
```

### `++i` costs less gas than `i++` (same for `--i`/`i--`)

Prefix increments are cheaper than postfix increments

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L307: for (uint256 i = 0; i < length; i++) {

line#L323: for (uint256 i = 0; i < length; i++) {

line#L356: for (uint256 i = 0; i < length; i++) {

line#L394: for (uint256 i = 0; i < length; i++) {

line#L432: for (uint256 t = 0; t < tokenAddresses.length; t++) {

line#L437: for (uint256 i = 0; i < addresses.length; i++) {

line#L474: for (uint256 i = 0; i < addresses.length; i++) {
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L564: for (uint256 i = 0; i < wallets.length; i++) {
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L357: for (uint256 i = 0; i < length; i++) {

line#L716: for (uint256 i = 0; i < length; i++) {
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L520: podSize--;

line#L760: pod--;

line#L781: for (uint256 i = 0; i < length; i++) {

line#L871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

### `x < y + 1` is cheaper than `x <= y`

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L349: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

line#L365: require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

line#L427: require(newAllowance >= currentAllowance, "ERC20: increased above max value");

line#L450: require(balance >= amount, "ERC20: balance check failed");

line#L529: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

line#L599: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

line#L698: require(accountBalance >= amount, "ERC20: amount exceeds balance");
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L354: require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

line#L386: if (_bondedAmounts[job.operator] >= amount) {

line#L739: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

line#L756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

line#L871: for (uint256 i = _operatorPods.length; i <= pod; i++) {

line#L1169:if (pod >= _operatorPods.length) {
```

### Use x = x + y instead of x += y for storage variables

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L686: _balances[to] += amount;

line#L702: _balances[recipient] += amount;

line#L633: _totalSupply -= amount;

line#L685: _totalSupply += amount;
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L378: _bondedAmounts[job.operator] -= amount;

line#L382: _bondedAmounts[msg.sender] += amount;

line#L834: _bondedAmounts[operator] += amount;
```
