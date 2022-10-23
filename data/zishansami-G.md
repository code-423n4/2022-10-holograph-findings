## Gas Optimizations
 *** 


### G-01: pre-increment `++i/--i` costs less gas than post-increment `i++/i--`
Saves 6 gas per loop in a for loop

Total instances of this issue: 14

```solidity
src/HolographOperator.sol

682:    for (uint256 i = 0; i < length; i++) {

772:        for (uint256 i = _operatorPods.length; i <= pod; i++) {


```
```solidity
src/enforcer/PA1D.sol

208:    for (uint256 i = 0; i < length; i++) {

224:    for (uint256 i = 0; i < length; i++) {

241:    for (uint256 i = 0; i < length; i++) {

257:    for (uint256 i = 0; i < length; i++) {

295:    for (uint256 i = 0; i < length; i++) {

315:    for (uint256 i = 0; i < length; i++) {

338:      for (uint256 i = 0; i < addresses.length; i++) {

355:      for (uint256 i = 0; i < addresses.length; i++) {

375:    for (uint256 i = 0; i < addresses.length; i++) {


```
```solidity
src/enforcer/HolographERC721.sol

258:    for (uint256 i = 0; i < length; i++) {

617:    for (uint256 i = 0; i < length; i++) {


```
```solidity
src/enforcer/HolographERC20.sol

465:    for (uint256 i = 0; i < wallets.length; i++) {


```
 *** 


### G-02: Length of the array (`<array>.length`) need not be looked up in every iteration of a for-loop
Reading array length at each iteration of the loop takes total 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the `array.length` saves around `3 gas` per iteration.

Total instances of this issue: 6

```solidity
src/HolographOperator.sol

772:        for (uint256 i = _operatorPods.length; i <= pod; i++) {


```
```solidity
src/enforcer/PA1D.sol

333:    for (uint256 t = 0; t < tokenAddresses.length; t++) {

338:      for (uint256 i = 0; i < addresses.length; i++) {

355:      for (uint256 i = 0; i < addresses.length; i++) {

375:    for (uint256 i = 0; i < addresses.length; i++) {


```
```solidity
src/enforcer/HolographERC20.sol

465:    for (uint256 i = 0; i < wallets.length; i++) {


```
 *** 


### G-03: `x += y` costs more gas than `x = x + y` for state variables

Total instances of this issue: 7

```solidity
src/HolographOperator.sol

283:        _bondedAmounts[msg.sender] += amount;

735:      _bondedAmounts[operator] += amount;

1078:      current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);


```
```solidity
src/HolographFactory.sol

229:      v += 27;


```
```solidity
src/enforcer/HolographERC20.sol

586:    _totalSupply += amount;

587:    _balances[to] += amount;

603:    _balances[recipient] += amount;


```
 *** 


### G-04: Adding `payable` to functions which are only meant to be called by specific actors like `onlyOwner` will save gas cost
Marking functions payable removes additional checks for whether a payment was provided, hence reducing gas cost

Total instances of this issue: 19

```solidity
src/HolographBridge.sol

353:  function setFactory(address factory) external onlyAdmin {

373:  function setHolograph(address holograph) external onlyAdmin {

403:  function setOperator(address operator) external onlyAdmin {

423:  function setRegistry(address registry) external onlyAdmin {


```
```solidity
src/HolographOperator.sol

179:  function resetOperator(
180:    uint256 blockTime,
181:    uint256 baseBondAmount,
182:    uint256 podMultiplier,
183:    uint256 operatorThreshold,
184:    uint256 operatorThresholdStep,
185:    uint256 operatorThresholdDivisor
186:  ) external onlyAdmin {

850:  function setBridge(address bridge) external onlyAdmin {

870:  function setHolograph(address holograph) external onlyAdmin {

890:  function setInterfaces(address interfaces) external onlyAdmin {

910:  function setMessagingModule(address messagingModule) external onlyAdmin {

930:  function setRegistry(address registry) external onlyAdmin {

950:  function setUtilityToken(address utilityToken) external onlyAdmin {


```
```solidity
src/HolographFactory.sol

181:  function setHolograph(address holograph) external onlyAdmin {

201:  function setRegistry(address registry) external onlyAdmin {


```
```solidity
src/module/LayerZeroModule.sol

221:  function setBridge(address bridge) external onlyAdmin {

241:  function setInterfaces(address interfaces) external onlyAdmin {

261:  function setLZEndpoint(address lZEndpoint) external onlyAdmin {

281:  function setOperator(address operator) external onlyAdmin {

342:  function setBaseGas(uint256 baseGas) external onlyAdmin {

371:  function setGasPerByte(uint256 gasPerByte) external onlyAdmin {


```
 *** 


### G-05: No need to initialize non-constant/non-immutable variables to zero
Since the default value is already zero, overwriting is not required.
Saves `8 gas` per instance.

Total instances of this issue: 17

```solidity
src/HolographBridge.sol

281:    uint256 fee = 0;


```
```solidity
src/HolographOperator.sol

211:    uint256 gasLimit = 0;

212:    uint256 gasPrice = 0;

682:    for (uint256 i = 0; i < length; i++) {


```
```solidity
src/enforcer/PA1D.sol

208:    for (uint256 i = 0; i < length; i++) {

224:    for (uint256 i = 0; i < length; i++) {

241:    for (uint256 i = 0; i < length; i++) {

257:    for (uint256 i = 0; i < length; i++) {

295:    for (uint256 i = 0; i < length; i++) {

315:    for (uint256 i = 0; i < length; i++) {

333:    for (uint256 t = 0; t < tokenAddresses.length; t++) {

338:      for (uint256 i = 0; i < addresses.length; i++) {

355:      for (uint256 i = 0; i < addresses.length; i++) {

375:    for (uint256 i = 0; i < addresses.length; i++) {


```
```solidity
src/enforcer/HolographERC721.sol

258:    for (uint256 i = 0; i < length; i++) {

617:    for (uint256 i = 0; i < length; i++) {


```
```solidity
src/enforcer/HolographERC20.sol

465:    for (uint256 i = 0; i < wallets.length; i++) {


```
 *** 


### G-06: `require()` containing multiple checks joined with && should be broken into multiple require statements

Total instances of this issue: 4

```solidity
src/HolographOperator.sol

758:    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");


```
```solidity
src/enforcer/Holographer.sol

67:    require(success && selector == InitializableInterface.init.selector, "initialization failed");


```
```solidity
src/enforcer/HolographERC721.sol

164:      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

365:      require(
366:        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
367:          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
368:          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
369:          ERC721TokenReceiver.onERC721Received.selector),
370:        "ERC721: onERC721Received fail"
371:      );


```
 *** 


### G-07: Using uints/ints smaller than 256 bits increases overhead
Gas usage becomes higher with uint/int smaller than 256 bits because EVM operates on 32 bytes and uses additional operations to reduce the size from 32 bytes to the target size.

Total instances of this issue: 28

```solidity
src/HolographBridge.sol

93:    uint32 fromChain,
94:    address holographableContract,
95:    address hToken,
96:    address hTokenRecipient,
97:    uint256 hTokenValue,
98:    bool doNotRevert,
99:    bytes calldata bridgeInPayload
100:  ) external payable onlyOperator {

146:  function bridgeOutRequest(
147:    uint32 toChain,
148:    address holographableContract,
149:    uint256 gasLimit,
150:    uint256 gasPrice,
151:    bytes calldata bridgeOutPayload
152:  ) external payable {

197:  function revertedBridgeOutRequest(
198:    address sender,
199:    uint32 toChain,
200:    address holographableContract,
201:    bytes calldata bridgeOutPayload
202:  ) external returns (string memory revertReason) {

243:  function getBridgeOutRequestPayload(
244:    uint32 toChain,
245:    address holographableContract,
246:    uint256 gasLimit,
247:    uint256 gasPrice,
248:    bytes calldata bridgeOutPayload
249:  ) external returns (bytes memory samplePayload) {


```
```solidity
src/HolographOperator.sol

109:  uint32 private _operatorTempStorageCounter;

483:  function send(
484:    uint256 gasLimit,
485:    uint256 gasPrice,
486:    uint32 toChain,
487:    address msgSender,
488:    uint256 nonce,
489:    address holographableContract,
490:    bytes calldata bridgeOutPayload
491:  ) external payable {


```
```solidity
src/HolographFactory.sol

221:  function _verifySigner(
222:    bytes32 r,
223:    bytes32 s,
224:    uint8 v,
225:    bytes32 hash,
226:    address signer
227:  ) private pure returns (bool) {


```
```solidity
src/module/LayerZeroModule.sol

131:    uint32 toChain,
132:    address msgSender,
133:    uint256 msgValue,
134:    bytes calldata crossChainPayload
135:  ) external payable {

152:  function getMessageFee(
153:    uint32 toChain,
154:    uint256 gasLimit,
155:    uint256 gasPrice,
156:    bytes calldata crossChainPayload
157:  ) external view returns (uint256 hlgFee, uint256 msgFee) {

158:    uint16 lzDestChain = uint16(
159:      _interfaces().getChainId(ChainIdType.HOLOGRAPH, uint256(toChain), ChainIdType.LAYERZERO)
160:    );

166:    (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);

178:  function getHlgFee(
179:    uint32 toChain,
180:    uint256 gasLimit,
181:    uint256 gasPrice
182:  ) external view returns (uint256 hlgFee) {

187:    uint16 lzDestChain = uint16(
188:      _interfaces().getChainId(ChainIdType.HOLOGRAPH, uint256(toChain), ChainIdType.LAYERZERO)
189:    );

190:    (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);

197:  function _getPricing(LayerZeroOverrides lz, uint16 lzDestChain)
198:    private
199:    view
200:    returns (uint128 dstPriceRatio, uint128 dstGasPriceInWei)
201:  {


```
```solidity
src/enforcer/Holographer.sol

51:    (uint32 originChain, address holograph, bytes32 contractType, address sourceContract) = abi.decode(
52:      encoded,
53:      (uint32, address, bytes32, address)
54:    );

106:  function getOriginChain() external view returns (uint32 originChain) {


```
```solidity
src/enforcer/HolographERC721.sol

61:  uint16 private _bps;

146:    (
147:      string memory contractName,
148:      string memory contractSymbol,
149:      uint16 contractBps,
150:      uint256 eventConfig,
151:      bool skipInit,
152:      bytes memory initCode
153:    ) = abi.decode(initPayload, (string, string, uint16, uint256, bool, bytes));

300:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

314:  function bridgeOut(
315:    uint32 toChain,
316:    address sender,
317:    bytes calldata payload
318:  ) external onlyBridge returns (bytes4 selector, bytes memory data) {

409:  function sourceMint(address to, uint224 tokenId) external onlySource {

780:    uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())
781:      .getHolographChainId();


```
```solidity
src/enforcer/HolographERC20.sol

82:  uint8 private _decimals;

127:    (
128:      string memory contractName,
129:      string memory contractSymbol,
130:      uint8 contractDecimals,
131:      uint256 eventConfig,
132:      string memory domainSeperator,
133:      string memory domainVersion,
134:      bool skipInit,
135:      bytes memory initCode
136:    ) = abi.decode(initPayload, (string, string, uint8, uint256, string, string, bool, bytes));

281:  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

293:  function bridgeOut(
294:    uint32 toChain,
295:    address sender,
296:    bytes calldata payload
297:  ) external onlyBridge returns (bytes4 selector, bytes memory data) {

361:  function permit(
362:    address account,
363:    address spender,
364:    uint256 amount,
365:    uint256 deadline,
366:    uint8 v,
367:    bytes32 r,
368:    bytes32 s
369:  ) public {


```
 *** 


### G-08: Using `!= 0` costs less gas than `> 0` in a require() statement
Saves 6 gas per instance for solidity version less than 0.8.12.

Total instances of this issue: 3

```solidity
src/HolographOperator.sol

210:    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

251:        require(timeDifference > 0, "HOLOGRAPH: operator has time");


```
```solidity
src/enforcer/HolographERC721.sol

716:    require(tokenId > 0, "ERC721: token id cannot be zero");


```
