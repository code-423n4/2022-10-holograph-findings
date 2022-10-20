# GAS ISSUES FOR 2022-10-HOLOGRAPH

##  [G-01]  Use `++i` instead of `i++`


./contracts/enforcer/HolographERC721.sol
```
L357:  for (uint256 i = 0; i < length; i++) {

L716:  for (uint256 i = 0; i < length; i++) {
```

./contracts/enforcer/PA1D.sol
```
L323:  for (uint256 i = 0; i < length; i++) {

L340:  for (uint256 i = 0; i < length; i++) {

L356:  for (uint256 i = 0; i < length; i++) {

L394:  for (uint256 i = 0; i < length; i++) {

L432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

L454:  for (uint256 i = 0; i < addresses.length; i++) {

L474:  for (uint256 i = 0; i < addresses.length; i++) {
```

./contracts/enforcer/HolographERC20.sol
```
L564:  for (uint256 i = 0; i < wallets.length; i++) {
```

./contracts/HolographOperator.sol
```
L520:  podSize--;

L760:  pod--;

L781:  for (uint256 i = 0; i < length; i++) {
```

##  [G-02]  Consider shortening some string to avoid extra gas


./contracts/enforcer/PA1D.sol
```
L411:  require(balance > 10000, "PA1D: Not enough tokens to transfer");

L435:  require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

##  [G-03]  Spread the `require()` conditions to multiple `require()` statements to save gas from `&&`


./contracts/enforcer/Holographer.sol
```
L166:  require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

./contracts/enforcer/HolographERC721.sol
```
L263:  require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

L464:  require(
L465:  (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
L466:  ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
L467:  ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
L468:  ERC721TokenReceiver.onERC721Received.selector),
L469:  "ERC721: onERC721Received fail"
L470:  );
```

./contracts/HolographOperator.sol
```
L857:  require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

##  [G-04]  A=A+B costs less gas than A+=B if A and B are located in storage


./contracts/enforcer/HolographERC20.sol
```
L686:  _balances[to] += amount;

L702:  _balances[recipient] += amount;

L633:  _totalSupply -= amount;
```

./contracts/HolographOperator.sol
```
L378:  _bondedAmounts[job.operator] -= amount;

L382:  _bondedAmounts[msg.sender] += amount;

L834:  _bondedAmounts[operator] += amount;
```

##  [G-05]  `uncheck` the `i++`/`i--` in for loops since there's no way to overflow/underflow


./contracts/HolographOperator.sol
```
L781:  for (uint256 i = 0; i < length; i++) {

L871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

./contracts/enforcer/HolographERC20.sol
```
L564:  for (uint256 i = 0; i < wallets.length; i++) {
```

./contracts/enforcer/PA1D.sol
```
L307:  for (uint256 i = 0; i < length; i++) {

L323:  for (uint256 i = 0; i < length; i++) {

L340:  for (uint256 i = 0; i < length; i++) {

L394:  for (uint256 i = 0; i < length; i++) {

L414:  for (uint256 i = 0; i < length; i++) {

L454:  for (uint256 i = 0; i < addresses.length; i++) {

L474:  for (uint256 i = 0; i < addresses.length; i++) {
```

./contracts/enforcer/HolographERC721.sol
```
L357:  for (uint256 i = 0; i < length; i++) {

L716:  for (uint256 i = 0; i < length; i++) {
```

##  [G-06]  Skip copying data from calldata to memory for function parameters
Description: Use `calldata` location for reference-type input args

./contracts/HolographBridge.sol
```
L162:  function init(bytes memory initPayload) external override returns (bytes4) {
```

./contracts/enforcer/HolographERC721.sol
```
L238:  function init(bytes memory initPayload) external override returns (bytes4) {

L456:  bytes memory data

L620:  bytes memory data
```

./contracts/abstract/ERC20H.sol
```
L140:  function init(bytes memory initPayload) external virtual override returns (bytes4) {
```

./contracts/enforcer/Holographer.sol
```
L147:  function init(bytes memory initPayload) external override returns (bytes4) {
```

./contracts/enforcer/HolographERC20.sol
```
L218:  function init(bytes memory initPayload) external override returns (bytes4) {

L499:  bytes memory data

L524:  bytes memory data
```

./contracts/abstract/ERC721H.sol
```
L140:  function init(bytes memory initPayload) external virtual override returns (bytes4) {
```

./contracts/HolographOperator.sol
```
L240:  function init(bytes memory initPayload) external override returns (bytes4) {
```

./contracts/enforcer/PA1D.sol
```
L173:  function init(bytes memory initPayload) external override returns (bytes4) {

L316:  function _setPayoutAddresses(address payable[] memory addresses) private {

L349:  function _setPayoutBps(uint256[] memory bps) private {

L365:  function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

L372:  function _setTokenAddress(string memory tokenName, address tokenAddress) private {

L426:  function _payoutTokens(address[] memory tokenAddresses) private {

L517:  function getTokensPayout(address[] memory tokenAddresses) public {

L683:  function getTokenAddress(string memory tokenName) public view returns (address) {
```

./contracts/HolographFactory.sol
```
L143:  function init(bytes memory initPayload) external override returns (bytes4) {

L193:  DeploymentConfig memory config,

L194:  Verification memory signature,
```

##  [G-07]  If a function is not open to any user, it can be marked as `payable` to save gas
Description: Under the hood the compiler checks if a function is payable. The check is skipped if it is marked as such by the developer

./contracts/enforcer/PA1D.sol
```
L471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```

./contracts/abstract/ERC20H.sol
```
L123:  require(msgSender() == _getOwner(), "ERC20: owner only function");
```

./contracts/HolographBridge.sol
```
L452:  function setFactory(address factory) external onlyAdmin {

L502:  function setOperator(address operator) external onlyAdmin {

L522:  function setRegistry(address registry) external onlyAdmin {
```

./contracts/enforcer/HolographERC721.sol
```
L399:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
```

./contracts/HolographOperator.sol
```
L445:  function nonRevertingBridgeCall(address msgSender, bytes calldata payload) external payable {

L949:  function setBridge(address bridge) external onlyAdmin {

L969:  function setHolograph(address holograph) external onlyAdmin {

L1009: function setMessagingModule(address messagingModule) external onlyAdmin {

L1029: function setRegistry(address registry) external onlyAdmin {

L1049: function setUtilityToken(address utilityToken) external onlyAdmin {
```

./contracts/HolographFactory.sol
```
L280:  function setHolograph(address holograph) external onlyAdmin {

L300:  function setRegistry(address registry) external onlyAdmin {
```

./contracts/enforcer/HolographERC20.sol
```
L380:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

L415:  function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
```

./contracts/abstract/ERC721H.sol
```
L123:  require(msgSender() == _getOwner(), "ERC721: owner only function");
```

##  [G-08]  Cache array's length in a single variable


./contracts/HolographOperator.sol
```
L871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

##  [G-09]  Mark contract constants with the `constant` and `immutable` keywords to save gas


./contracts/enforcer/HolographERC721.sol
```
L764:  revert("ERC721: token does not exist");

L767:  require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));

L769:  return ERC721TokenReceiver.onERC721Received.selector;
```

./contracts/enforcer/HolographERC20.sol
```
L151:  uint256 private _eventConfig;

L171:  string private _name;

L181:  uint8 private _decimals;
```

##  [G-10]  Use default values of variables types
Description: uint256 - 0;, string - "";, address - address(0);, etc.

./contracts/enforcer/HolographERC721.sol
```
L357:  for (uint256 i = 0; i < length; i++) {

L716:  for (uint256 i = 0; i < length; i++) {
```

./contracts/enforcer/HolographERC20.sol
```
L564:  for (uint256 i = 0; i < wallets.length; i++) {
```

./contracts/HolographOperator.sol
```
L310:  uint256 gasLimit = 0;

L311:  uint256 gasPrice = 0;

L781:  for (uint256 i = 0; i < length; i++) {
```

./contracts/enforcer/PA1D.sol
```
L307:  for (uint256 i = 0; i < length; i++) {

L340:  for (uint256 i = 0; i < length; i++) {

L394:  for (uint256 i = 0; i < length; i++) {

L414:  for (uint256 i = 0; i < length; i++) {

L437:  for (uint256 i = 0; i < addresses.length; i++) {

L454:  for (uint256 i = 0; i < addresses.length; i++) {

L474:  for (uint256 i = 0; i < addresses.length; i++) {
```

./contracts/HolographBridge.sol
```
L380:  uint256 fee = 0;
```

##  [G-11]  A < B + 1 is cheaper than A <= B
 

./contracts/enforcer/HolographERC20.sol
```
L349:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

L365:  require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

L400:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

L427:  require(newAllowance >= currentAllowance, "ERC20: increased above max value");

L450:  require(balance >= amount, "ERC20: balance check failed");

L629:  require(accountBalance >= amount, "ERC20: amount exceeds balance");

L698:  require(accountBalance >= amount, "ERC20: amount exceeds balance");
```

./contracts/HolographOperator.sol
```
L354:  require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

L386:  if (_bondedAmounts[job.operator] >= amount) {

L728:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

L756:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

L871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {

L1169: if (pod >= _operatorPods.length) {
```

##  [G-12]  Use custom errors to save gas


./contracts/abstract/ERC721H.sol
```
L123:  require(msgSender() == _getOwner(), "ERC721: owner only function");

L125:  require(msg.sender == _getOwner(), "ERC721: owner only function");

L147:  require(!_isInitialized(), "ERC721: already initialized");
```

./contracts/enforcer/Holographer.sol
```
L148:  require(!_isInitialized(), "HOLOGRAPHER: already initialized");

L166:  require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

./contracts/enforcer/HolographERC20.sol
```
L204:  require(msg.sender == sourceContract, "ERC20: source only call");

L241:  require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

L365:  require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

L387:  require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

L445:  require(_isContract(account), "ERC20: operator not contract");

L450:  require(balance >= amount, "ERC20: balance check failed");

L452:  revert("ERC20: failed getting balance");

L469:  require(block.timestamp <= deadline, "ERC20: expired deadline");

L482:  require(signer == account, "ERC20: invalid signature");

L505:  require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");

L529:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

L599:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

L621:  require(spender != address(0), "ERC20: spender is zero address");

L627:  require(account != address(0), "ERC20: account is zero address");

L629:  require(accountBalance >= amount, "ERC20: amount exceeds balance");

L645:  require(erc165support, "ERC20: no ERC165 support");

L653:  revert("ERC20: non ERC20Receiver");

L661:  revert("ERC20: eip-4524 not supported");

L665:  revert("ERC20: no ERC165 support");

L684:  require(to != address(0), "ERC20: minting to burn address");

L698:  require(accountBalance >= amount, "ERC20: amount exceeds balance");
```

./contracts/enforcer/PA1D.sol
```
L159:  require(isOwner(), "PA1D: caller not an owner");

L190:  require(initialized == 0, "PA1D: already initialized");

L390:  require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

L416:  require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

L439:  require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

L460:  require(matched, "PA1D: sender not authorized");

L472:  require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

L477:  require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

./contracts/enforcer/HolographERC721.sol
```
L212:  require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

L239:  require(!_isInitialized(), "ERC721: already initialized");

L258:  require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

L323:  require(_exists(tokenId), "ERC721: token does not exist");

L370:  require(to != tokenOwner, "ERC721: cannot approve self");

L371:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

L388:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

L408:  require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

L419:  require(to != address(0), "ERC721: zero address");

L420:  require(_isApproved(sender, tokenId), "ERC721: sender not approved");

L421:  require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

L458:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

L513:  require(!_burnedTokens[token], "ERC721: can't mint burned token");

L622:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

L639:  require(wallet != address(0), "ERC721: zero address");

L689:  require(tokenOwner != address(0), "ERC721: token does not exist");

L700:  require(index < _allTokens.length, "ERC721: index out of bounds");

L729:  require(index < balanceOf(wallet), "ERC721: index out of bounds");

L757:  require(_isContract(_operator), "ERC721: operator not contract");

L816:  require(to != address(0), "ERC721: minting to burn address");

L817:  require(!_exists(tokenId), "ERC721: token already exists");

L818:  require(!_burnedTokens[tokenId], "ERC721: token has been burned");

L870:  require(to != address(0), "ERC721: use burn instead");
```

./contracts/abstract/ERC20H.sol
```
L117:  require(msg.sender == holographer(), "ERC20: holographer only");

L123:  require(msgSender() == _getOwner(), "ERC20: owner only function");

L147:  require(!_isInitialized(), "ERC20: already initialized");
```

./contracts/HolographBridge.sol
```
L148:  require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

L163:  require(!_isInitialized(), "HOLOGRAPH: already initialized");

L214:  require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

L233:  require(doNotRevert, "HOLOGRAPH: reverted");

L255:  require(
L256:  _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
L257:  "HOLOGRAPH: not holographed"
L258:  );

L270:  require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

L352:  require(
L353:  _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
L354:  "HOLOGRAPH: not holographed"
L355:  );
```

./contracts/HolographOperator.sol
```
L350:  require(timeDifference > 0, "HOLOGRAPH: operator has time");

L354:  require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

L368:  require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

L415:  require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

L446:  require(msg.sender == address(this), "HOLOGRAPH: operator only call");

L485:  require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

L595:  require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

L728:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

L739:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

L756:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

L829:  require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

L839:  require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

L857:  require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

L889:  require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

L915:  require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

L932:  require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```

./contracts/HolographFactory.sol
```
L144:  require(!_isInitialized(), "HOLOGRAPH: already initialized");

L220:  require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

L228:  require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
```

##  [G-13]  Inline internal functions to save gas


./contracts/HolographOperator.sol
```
L1058: function _bridge() private view returns (address bridge) {

L1094: function _registry() private view returns (HolographRegistryInterface registry) {

L1198: function _isContract(address contractAddress) private view returns (bool) {
```

./contracts/HolographFactory.sol
```
L309:  function _isContract(address contractAddress) private view returns (bool) {

L320:  function _verifySigner(
```

./contracts/HolographBridge.sol
```
L549:  function _jobNonce() private returns (uint256 jobNonce) {
```

./contracts/enforcer/HolographERC20.sol
```
L711:  function _useNonce(address account) private returns (uint256 current) {

L736:  function _interfaces() private view returns (address) {
```

./contracts/enforcer/HolographERC721.sol
```
L824:  function _removeTokenFromAllTokensEnumeration(uint256 tokenId) private {
```

./contracts/enforcer/PA1D.sol
```
L226:  function _setDefaultReceiver(address receiver) private {

L246:  function _setDefaultBp(uint256 bp) private {

L316:  function _setPayoutAddresses(address payable[] memory addresses) private {

L349:  function _setPayoutBps(uint256[] memory bps) private {

L365:  function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

L405:  function _payoutToken(address tokenAddress) private {

L426:  function _payoutTokens(address[] memory tokenAddresses) private {
```

