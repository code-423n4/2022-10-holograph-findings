# 2022-10-HOLOGRAPH
## Gas Optimizations Report

### The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.
This change would save up to 6 gas per instance/loop.

_There are **17** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

520:  podSize--;

760:  pod--;

781:  for (uint256 i = 0; i < length; i++) {

871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

564:  for (uint256 i = 0; i < wallets.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

357:  for (uint256 i = 0; i < length; i++) {

716:  for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/PA1D.sol

307:  for (uint256 i = 0; i < length; i++) {

323:  for (uint256 i = 0; i < length; i++) {

340:  for (uint256 i = 0; i < length; i++) {

356:  for (uint256 i = 0; i < length; i++) {

394:  for (uint256 i = 0; i < length; i++) {

414:  for (uint256 i = 0; i < length; i++) {

432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

437:  for (uint256 i = 0; i < addresses.length; i++) {

454:  for (uint256 i = 0; i < addresses.length; i++) {

474:  for (uint256 i = 0; i < addresses.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### State variables should be cached in stack variables rather than re-reading them.
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **5** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

      /// @audit Cache `_operatorTempStorageCounter`. Used 3 times in `crossChainMessage`
495:  ++_operatorTempStorageCounter;
517:  _operatorTempStorage[_operatorTempStorageCounter] = _operatorPods[pod][operatorIndex];
524:    (uint256(_operatorTempStorageCounter) << 216) |

      /// @audit Cache `_operatorPods`. Used 6 times in `executeJob`
363:    if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
364:      address fallbackOperator = _operatorPods[pod][podIndex];
390:    _operatorPods[pod].push(job.operator);
391:    _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
407:  _operatorPods[pod].push(msg.sender);
408:  _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;

      /// @audit Cache `_operatorPods`. Used 3 times in `crossChainMessage`
503:  uint256 pod = random % _operatorPods.length;
507:  uint256 podSize = _operatorPods[pod].length;
517:  _operatorTempStorage[_operatorTempStorageCounter] = _operatorPods[pod][operatorIndex];

      /// @audit Cache `_operatorPods`. Used 5 times in `bondUtilityToken`
867:  if (_operatorPods.length < pod) {
875:    _operatorPods.push([address(0)]);
881:  require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
882:  _operatorPods[pod - 1].push(operator);
883:  _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;

      /// @audit Cache `_operatorPods`. Used 4 times in `_popOperator`
1128: address operator = _operatorPods[pod][operatorIndex];
1137: uint256 lastIndex = _operatorPods[pod].length - 1;
1148: delete _operatorPods[pod][lastIndex];
1152: _operatorPods[pod].pop();
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

### `internal` and `private` functions that are called only once should be inlined.
The execution of a non-inlined function would cost up to 40 more gas because of two extra `jump`s as well as some other instructions.

_There are **20** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

549:  function _jobNonce() private returns (uint256 jobNonce) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographFactory.sol

309:  function _isContract(address contractAddress) private view returns (bool) {

320:  function _verifySigner(
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol

```solidity
File: contracts/HolographOperator.sol

1058: function _bridge() private view returns (address bridge) {

1094: function _registry() private view returns (HolographRegistryInterface registry) {

1112: function _jobNonce() private returns (uint256 jobNonce) {

1198: function _isContract(address contractAddress) private view returns (bool) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

711:  function _useNonce(address account) private returns (uint256 current) {

736:  function _interfaces() private view returns (address) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

824:  function _removeTokenFromAllTokensEnumeration(uint256 tokenId) private {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/PA1D.sol

226:  function _setDefaultReceiver(address receiver) private {

246:  function _setDefaultBp(uint256 bp) private {

268:  function _setReceiver(uint256 tokenId, address receiver) private {

291:  function _setBp(uint256 tokenId, uint256 bp) private {

316:  function _setPayoutAddresses(address payable[] memory addresses) private {

349:  function _setPayoutBps(uint256[] memory bps) private {

365:  function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

382:  function _payoutEth() private {

405:  function _payoutToken(address tokenAddress) private {

426:  function _payoutTokens(address[] memory tokenAddresses) private {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Using `!= 0` on `uints` costs less gas than `> 0`.
This change saves 3 gas per instance/loop

_There are **6** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

218:  if (hTokenValue > 0) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographOperator.sol

350:  require(timeDifference > 0, "HOLOGRAPH: operator has time");

363:    if (podIndex > 0 && podIndex < _operatorPods[pod].length) {

398:    if (leftovers > 0) {

1126: if (operatorIndex > 0) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

815:  require(tokenId > 0, "ERC721: token id cannot be zero");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied
Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **17** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

380:  uint256 fee = 0;
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographOperator.sol

310:  uint256 gasLimit = 0;

311:  uint256 gasPrice = 0;

781:  for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

564:  for (uint256 i = 0; i < wallets.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

357:  for (uint256 i = 0; i < length; i++) {

716:  for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/PA1D.sol

307:  for (uint256 i = 0; i < length; i++) {

323:  for (uint256 i = 0; i < length; i++) {

340:  for (uint256 i = 0; i < length; i++) {

356:  for (uint256 i = 0; i < length; i++) {

394:  for (uint256 i = 0; i < length; i++) {

414:  for (uint256 i = 0; i < length; i++) {

432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

437:  for (uint256 i = 0; i < addresses.length; i++) {

454:  for (uint256 i = 0; i < addresses.length; i++) {

474:  for (uint256 i = 0; i < addresses.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Functions that are access-restricted from most users may be marked as `payable`
Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **20** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

452:  function setFactory(address factory) external onlyAdmin {

472:  function setHolograph(address holograph) external onlyAdmin {

502:  function setOperator(address operator) external onlyAdmin {

522:  function setRegistry(address registry) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographFactory.sol

280:  function setHolograph(address holograph) external onlyAdmin {

300:  function setRegistry(address registry) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol

```solidity
File: contracts/HolographOperator.sol

445:  function nonRevertingBridgeCall(address msgSender, bytes calldata payload) external payable {

484:  function crossChainMessage(bytes calldata bridgeInRequestPayload) external payable {

949:  function setBridge(address bridge) external onlyAdmin {

969:  function setHolograph(address holograph) external onlyAdmin {

989:  function setInterfaces(address interfaces) external onlyAdmin {

1009: function setMessagingModule(address messagingModule) external onlyAdmin {

1029: function setRegistry(address registry) external onlyAdmin {

1049: function setUtilityToken(address utilityToken) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/abstract/ERC20H.sol

123:  require(msgSender() == _getOwner(), "ERC20: owner only function");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol

```solidity
File: contracts/abstract/ERC721H.sol

123:  require(msgSender() == _getOwner(), "ERC721: owner only function");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

380:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

415:  function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

399:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/PA1D.sol

471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### `<array>.length` should not be looked up in every loop of a `for`-loop
The overheads outlined below are PER LOOP, excluding the first loop \ - storage arrays incur a Gwarmaccess (100 gas) \ - memory arrays use `MLOAD` (3 gas) \ - calldata arrays use `CALLDATALOAD` (3 gas) 
 
 Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

_There are **1** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops
When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **15** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

781:  for (uint256 i = 0; i < length; i++) {

871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

564:  for (uint256 i = 0; i < wallets.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

357:  for (uint256 i = 0; i < length; i++) {

716:  for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/PA1D.sol

307:  for (uint256 i = 0; i < length; i++) {

323:  for (uint256 i = 0; i < length; i++) {

340:  for (uint256 i = 0; i < length; i++) {

356:  for (uint256 i = 0; i < length; i++) {

394:  for (uint256 i = 0; i < length; i++) {

414:  for (uint256 i = 0; i < length; i++) {

432:  for (uint256 t = 0; t < tokenAddresses.length; t++) {

437:  for (uint256 i = 0; i < addresses.length; i++) {

454:  for (uint256 i = 0; i < addresses.length; i++) {

474:  for (uint256 i = 0; i < addresses.length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables


_There are **7** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

378:  _bondedAmounts[job.operator] -= amount;

382:  _bondedAmounts[msg.sender] += amount;

834:  _bondedAmounts[operator] += amount;
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

686:  _balances[to] += amount;

702:  _balances[recipient] += amount;

633:  _totalSupply -= amount;

685:  _totalSupply += amount;
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead
'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **19** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

192:  uint32 fromChain,

246:  uint32 toChain,

298:  uint32 toChain,

343:  uint32 toChain,
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographFactory.sol

323:  uint8 v,
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol

```solidity
File: contracts/HolographOperator.sol

208:  uint32 private _operatorTempStorageCounter;

585:  uint32 toChain,
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

181:  uint8 private _decimals;

229:  uint8 contractDecimals,

380:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

393:  uint32 toChain,

465:  uint8 v,
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

160:  uint16 private _bps;

248:  uint16 contractBps,

399:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

414:  uint32 toChain,

879:  uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/Holographer.sol

150:  (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(

205:  function getOriginChain() external view returns (uint32 originChain) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

### Splitting `require()` statements that use `&&` saves gas
Instead of using `&&` on single `require` check using two `require` checks can save gas

_There are **4** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

857:  require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

263:  require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

464:  require(
465:  (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
466:    ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
467:    ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
468:    ERC721TokenReceiver.onERC721Received.selector),
469:  "ERC721: onERC721Received fail"
470:  );
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/Holographer.sol

166:  require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

### Use `calldata` instead of `memory` for function parameters
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **26** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

162:  function init(bytes memory initPayload) external override returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographFactory.sol

143:  function init(bytes memory initPayload) external override returns (bytes4) {

193:  DeploymentConfig memory config,

194:  Verification memory signature,
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol

```solidity
File: contracts/HolographOperator.sol

240:  function init(bytes memory initPayload) external override returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/abstract/ERC20H.sol

140:  function init(bytes memory initPayload) external virtual override returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol

```solidity
File: contracts/abstract/ERC721H.sol

140:  function init(bytes memory initPayload) external virtual override returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

218:  function init(bytes memory initPayload) external override returns (bytes4) {

499:  bytes memory data

524:  bytes memory data

641:  bytes memory data
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

238:  function init(bytes memory initPayload) external override returns (bytes4) {

456:  bytes memory data

620:  bytes memory data
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/Holographer.sol

147:  function init(bytes memory initPayload) external override returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

```solidity
File: contracts/enforcer/PA1D.sol

173:  function init(bytes memory initPayload) external override returns (bytes4) {

185:  function initPA1D(bytes memory initPayload) external returns (bytes4) {

316:  function _setPayoutAddresses(address payable[] memory addresses) private {

349:  function _setPayoutBps(uint256[] memory bps) private {

365:  function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {

372:  function _setTokenAddress(string memory tokenName, address tokenAddress) private {

426:  function _payoutTokens(address[] memory tokenAddresses) private {

      /// @audit Store `addresses` in calldata.
471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

      /// @audit Store `bps` in calldata.
471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

517:  function getTokensPayout(address[] memory tokenAddresses) public {

683:  function getTokenAddress(string memory tokenName) public view returns (address) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`
In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **18** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

354:  require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

386:  if (_bondedAmounts[job.operator] >= amount) {

728:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

739:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

756:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

863:  require(current <= amount, "HOLOGRAPH: bond amount too small");

871:  for (uint256 i = _operatorPods.length; i <= pod; i++) {

1169: if (pod >= _operatorPods.length) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

349:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

365:  require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

400:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

427:  require(newAllowance >= currentAllowance, "ERC20: increased above max value");

450:  require(balance >= amount, "ERC20: balance check failed");

469:  require(block.timestamp <= deadline, "ERC20: expired deadline");

529:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

599:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

629:  require(accountBalance >= amount, "ERC20: amount exceeds balance");

698:  require(accountBalance >= amount, "ERC20: amount exceeds balance");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

### Use `immutable` & `constant` for state variables that do not change their value


_There are **8** instances of this issue:_

```solidity
File: contracts/enforcer/HolographERC20.sol

151:  uint256 private _eventConfig;

171:  string private _name;

176:  string private _symbol;

181:  uint8 private _decimals;
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

762:  require(tokenOwner == address(this), "ERC721: contract not token owner");

764:  revert("ERC721: token does not exist");

767:  require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));

769:  return ERC721TokenReceiver.onERC721Received.selector;
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

### `require()/revert()` strings longer than 32 bytes cost extra gas
Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

_There are **2** instances of this issue:_

```solidity
File: contracts/enforcer/PA1D.sol

411:  require(balance > 10000, "PA1D: Not enough tokens to transfer");

435:  require(balance > 10000, "PA1D: Not enough tokens to transfer");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Use custom errors rather than `revert()`/`require()` strings to save gas
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **120** instances of this issue:_

```solidity
File: contracts/HolographBridge.sol

148:  require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

163:  require(!_isInitialized(), "HOLOGRAPH: already initialized");

203:  require(
204:  _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
205:  "HOLOGRAPH: not holographed"
206:  );

214:  require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

224:  require(
225:  HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==
226:    HolographERC20Interface.holographBridgeMint.selector,
227:  "HOLOGRAPH: hToken mint failed"
228:  );

233:  require(doNotRevert, "HOLOGRAPH: reverted");

255:  require(
256:  _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
257:  "HOLOGRAPH: not holographed"
258:  );

270:  require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

352:  require(
353:  _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
354:  "HOLOGRAPH: not holographed"
355:  );
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol

```solidity
File: contracts/HolographFactory.sol

144:  require(!_isInitialized(), "HOLOGRAPH: already initialized");

220:  require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

228:  require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

250:  require(
251:  InitializableInterface(holographerAddress).init(
252:  abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
253:  ) == InitializableInterface.init.selector,
254:  "initialization failed"
255:  );
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol

```solidity
File: contracts/HolographOperator.sol

241:  require(!_isInitialized(), "HOLOGRAPH: already initialized");

309:  require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

350:  require(timeDifference > 0, "HOLOGRAPH: operator has time");

354:  require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

368:      require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

415:  require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

446:  require(msg.sender == address(this), "HOLOGRAPH: operator only call");

485:  require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

591:  require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");

595:  require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

728:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

739:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

756:  require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

829:  require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

839:  require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

857:  require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

863:  require(current <= amount, "HOLOGRAPH: bond amount too small");

881:  require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

889:  require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

903:  require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

911:  require(_isContract(operator), "HOLOGRAPH: operator not contract");

915:  require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

932:  require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/abstract/ERC20H.sol

117:  require(msg.sender == holographer(), "ERC20: holographer only");

123:  require(msgSender() == _getOwner(), "ERC20: owner only function");

125:  require(msg.sender == _getOwner(), "ERC20: owner only function");

147:  require(!_isInitialized(), "ERC20: already initialized");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol

```solidity
File: contracts/abstract/ERC721H.sol

117:  require(msg.sender == holographer(), "ERC721: holographer only");

123:  require(msgSender() == _getOwner(), "ERC721: owner only function");

125:  require(msg.sender == _getOwner(), "ERC721: owner only function");

147:  require(!_isInitialized(), "ERC721: already initialized");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

192:  require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");

204:  require(msg.sender == sourceContract, "ERC20: source only call");

219:  require(!_isInitialized(), "ERC20: already initialized");

241:  require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

349:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

365:  require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

387:  require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

400:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

427:  require(newAllowance >= currentAllowance, "ERC20: increased above max value");

445:  require(_isContract(account), "ERC20: operator not contract");

450:  require(balance >= amount, "ERC20: balance check failed");

452:  revert("ERC20: failed getting balance");

469:  require(block.timestamp <= deadline, "ERC20: expired deadline");

482:  require(signer == account, "ERC20: invalid signature");

505:  require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");

529:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

539:  require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");

599:  require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

620:  require(account != address(0), "ERC20: account is zero address");

621:  require(spender != address(0), "ERC20: spender is zero address");

627:  require(account != address(0), "ERC20: account is zero address");

629:  require(accountBalance >= amount, "ERC20: amount exceeds balance");

645:  require(erc165support, "ERC20: no ERC165 support");

653:        revert("ERC20: non ERC20Receiver");

661:    revert("ERC20: eip-4524 not supported");

665:    revert("ERC20: no ERC165 support");

684:  require(to != address(0), "ERC20: minting to burn address");

695:  require(account != address(0), "ERC20: account is zero address");

696:  require(recipient != address(0), "ERC20: recipient is zero address");

698:  require(accountBalance >= amount, "ERC20: amount exceeds balance");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

212:  require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

224:  require(msg.sender == sourceContract, "ERC721: source only call");

239:  require(!_isInitialized(), "ERC721: already initialized");

258:  require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

263:  require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

323:  require(_exists(tokenId), "ERC721: token does not exist");

370:  require(to != tokenOwner, "ERC721: cannot approve self");

371:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

388:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

404:  require(!_exists(tokenId), "ERC721: token already exists");

408:  require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

419:  require(to != address(0), "ERC721: zero address");

420:  require(_isApproved(sender, tokenId), "ERC721: sender not approved");

421:  require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

458:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

464:  require(
465:  (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
466:    ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
467:    ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
468:    ERC721TokenReceiver.onERC721Received.selector),
469:  "ERC721: onERC721Received fail"
470:  );

484:  require(to != msg.sender, "ERC721: cannot approve self");

513:  require(!_burnedTokens[token], "ERC721: can't mint burned token");

622:  require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

639:  require(wallet != address(0), "ERC721: zero address");

689:  require(tokenOwner != address(0), "ERC721: token does not exist");

700:  require(index < _allTokens.length, "ERC721: index out of bounds");

729:  require(index < balanceOf(wallet), "ERC721: index out of bounds");

757:  require(_isContract(_operator), "ERC721: operator not contract");

762:  require(tokenOwner == address(this), "ERC721: contract not token owner");

764:  revert("ERC721: token does not exist");

815:  require(tokenId > 0, "ERC721: token id cannot be zero");

816:  require(to != address(0), "ERC721: minting to burn address");

817:  require(!_exists(tokenId), "ERC721: token already exists");

818:  require(!_burnedTokens[tokenId], "ERC721: token has been burned");

869:  require(_tokenOwner[tokenId] == from, "ERC721: token not owned");

870:  require(to != address(0), "ERC721: use burn instead");

906:  require(_exists(tokenId), "ERC721: token does not exist");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/Holographer.sol

148:  require(!_isInitialized(), "HOLOGRAPHER: already initialized");

166:  require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

```solidity
File: contracts/enforcer/PA1D.sol

159:  require(isOwner(), "PA1D: caller not an owner");

174:  require(!_isInitialized(), "PA1D: already initialized");

190:  require(initialized == 0, "PA1D: already initialized");

390:  require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

411:  require(balance > 10000, "PA1D: Not enough tokens to transfer");

416:  require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

435:  require(balance > 10000, "PA1D: Not enough tokens to transfer");

439:  require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

460:  require(matched, "PA1D: sender not authorized");

472:  require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

477:  require(totalBp == 10000, "PA1D: bps down't equal 10000");
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

