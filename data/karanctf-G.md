## [G-01] Use `!= 0` instead of `> 0`
```solidity
HolographBridge.sol:218:    if (hTokenValue > 0) {
HolographERC721.sol:815:    require(tokenId > 0, "ERC721: token id cannot be zero");
HolographOperator.sol:309:    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
HolographOperator.sol:350:        require(timeDifference > 0, "HOLOGRAPH: operator has time");
HolographOperator.sol:363:          if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
HolographOperator.sol:398:          if (leftovers > 0) {
HolographOperator.sol:1126:    if (operatorIndex > 0) {
```

## [G-2]<x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
HolographERC20.sol:633:    _totalSupply -= amount;
HolographERC20.sol:685:    _totalSupply += amount;
HolographERC20.sol:686:    _balances[to] += amount;
HolographERC20.sol:702:    _balances[recipient] += amount;
HolographFactory.sol:328:      v += 27;
HolographOperator.sol:378:        _bondedAmounts[job.operator] -= amount;
HolographOperator.sol:382:        _bondedAmounts[msg.sender] += amount;
HolographOperator.sol:834:      _bondedAmounts[operator] += amount;
HolographOperator.sol:1175:      position -= threshold;
HolographOperator.sol:1177:      current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
```

## [G-3] Use `calldata` instead of `memory` when used only for reading
```solidity
HolographBridge.sol:162:  function init(bytes memory initPayload) external override returns (bytes4) {
HolographBridge.sol:262:    (bytes4 selector, bytes memory returnedPayload) = Holographable(holographableContract).bridgeOut(
HolographBridge.sol:301:  ) external returns (string memory revertReason) {
HolographBridge.sol:307:      bytes memory payload
HolographBridge.sol:324:    } catch Error(string memory reason) {
HolographBridge.sol:348:  ) external returns (bytes memory samplePayload) {
HolographBridge.sol:356:    bytes memory payload;
HolographBridge.sol:361:      string memory revertReason
HolographBridge.sol:367:    } catch (bytes memory realResponse) {
HolographBridge.sol:388:    bytes memory encodedData = abi.encodeWithSelector(
HolographERC20.sol:218:  function init(bytes memory initPayload) external override returns (bytes4) {
HolographERC20.sol:227:      string memory contractName,
HolographERC20.sol:228:      string memory contractSymbol,
HolographERC20.sol:231:      string memory domainSeperator,
HolographERC20.sol:232:      string memory domainVersion,
HolographERC20.sol:234:      bytes memory initCode
HolographERC20.sol:381:    (address from, address to, uint256 amount, bytes memory data) = abi.decode(
HolographERC20.sol:396:  ) external onlyBridge returns (bytes4 selector, bytes memory data) {
HolographERC20.sol:499:    bytes memory data
HolographERC20.sol:524:    bytes memory data
HolographERC20.sol:641:    bytes memory data
HolographERC20.sol:651:          } catch (bytes memory reason) {
HolographERC20.sol:663:      } catch (bytes memory reason) {
ERC20H.sol:140:  function init(bytes memory initPayload) external virtual override returns (bytes4) {
ERC20H.sol:145:    bytes memory /* initPayload*/
LayerZeroModule.sol:158:  function init(bytes memory initPayload) external override returns (bytes4) {
LayerZeroModule.sol:269:    bytes memory adapterParams = abi.encodePacked(
HolographFactory.sol:143:  function init(bytes memory initPayload) external override returns (bytes4) {
HolographFactory.sol:164:    (DeploymentConfig memory config, Verification memory signature, address signer) = abi.decode(
HolographFactory.sol:181:  ) external pure returns (bytes4 selector, bytes memory data) {
HolographFactory.sol:193:    DeploymentConfig memory config,
HolographFactory.sol:194:    Verification memory signature,
HolographFactory.sol:224:    bytes memory holographerBytecode = type(Holographer).creationCode;
HolographFactory.sol:234:    bytes memory sourceByteCode = config.byteCode;
HolographERC721.sol:238:  function init(bytes memory initPayload) external override returns (bytes4) {
HolographERC721.sol:246:      string memory contractName,
HolographERC721.sol:247:      string memory contractSymbol,
HolographERC721.sol:251:      bytes memory initCode
HolographERC721.sol:259:      (bool success, bytes memory returnData) = _royalties().delegatecall(
HolographERC721.sol:351:  ) external view returns (uint256[] memory tokenIds) {
HolographERC721.sol:400:    (address from, address to, uint256 tokenId, bytes memory data) = abi.decode(
HolographERC721.sol:417:  ) external onlyBridge returns (bytes4 selector, bytes memory data) {
HolographERC721.sol:456:    bytes memory data
HolographERC721.sol:620:    bytes memory data
HolographERC721.sol:710:  function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {
ERC721H.sol:140:  function init(bytes memory initPayload) external virtual override returns (bytes4) {
ERC721H.sol:145:    bytes memory /* initPayload*/
Holographer.sol:147:  function init(bytes memory initPayload) external override returns (bytes4) {
Holographer.sol:149:    (bytes memory encoded, bytes memory initCode) = abi.decode(initPayload, (bytes, bytes));
Holographer.sol:162:    (bool success, bytes memory returnData) = HolographRegistryInterface(HolographInterface(holograph).getRegistry())
HolographOperator.sol:240:  function init(bytes memory initPayload) external override returns (bytes4) {
HolographOperator.sol:325:    OperatorJob memory job = getJobDetails(hash);
HolographOperator.sol:466:        /// @dev data is retrieved from 0 index memory position
HolographOperator.sol:597:    bytes memory encodedData = abi.encodeWithSelector(
HolographOperator.sol:738:  function getPodOperators(uint256 pod) external view returns (address[] memory operators) {
HolographOperator.sol:755:  ) external view returns (address[] memory operators) {
PA1D.sol:173:  function init(bytes memory initPayload) external override returns (bytes4) {
PA1D.sol:185:  function initPA1D(bytes memory initPayload) external returns (bytes4) {
PA1D.sol:298:  function _getPayoutAddresses() private view returns (address payable[] memory addresses) {
PA1D.sol:316:  function _setPayoutAddresses(address payable[] memory addresses) private {
PA1D.sol:332:  function _getPayoutBps() private view returns (uint256[] memory bps) {
PA1D.sol:349:  function _setPayoutBps(uint256[] memory bps) private {
PA1D.sol:365:  function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {
PA1D.sol:372:  function _setTokenAddress(string memory tokenName, address tokenAddress) private {
PA1D.sol:383:    address payable[] memory addresses = _getPayoutAddresses();
PA1D.sol:384:    uint256[] memory bps = _getPayoutBps();
PA1D.sol:406:    address payable[] memory addresses = _getPayoutAddresses();
PA1D.sol:407:    uint256[] memory bps = _getPayoutBps();
PA1D.sol:426:  function _payoutTokens(address[] memory tokenAddresses) private {
PA1D.sol:427:    address payable[] memory addresses = _getPayoutAddresses();
PA1D.sol:428:    uint256[] memory bps = _getPayoutBps();
PA1D.sol:452:      address payable[] memory addresses = _getPayoutAddresses();
PA1D.sol:471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
PA1D.sol:488:  function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
PA1D.sol:517:  function getTokensPayout(address[] memory tokenAddresses) public {
PA1D.sol:541:    address[] memory receivers = new address[](1);
PA1D.sol:543:    uint256[] memory bps = new uint256[](1);
PA1D.sol:559:    uint256[] memory bps = new uint256[](1);
PA1D.sol:570:    address payable[] memory receivers = new address payable[](1);
PA1D.sol:591:    address payable[] memory receivers = new address payable[](1);
PA1D.sol:592:    uint256[] memory bps = new uint256[](1);
PA1D.sol:605:    address payable[] memory receivers = new address payable[](1);
PA1D.sol:606:    uint256[] memory bps = new uint256[](1);
PA1D.sol:665:  function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
PA1D.sol:683:  function getTokenAddress(string memory tokenName) public view returns (address) {
```

## [G-4] Use preincrement
`++i` costs less gas than `i++`, especially when it's used in for-loops (--i/i-- too) Saves 5 gas PER LOOP
```solidity
HolographERC20.sol:564:    for (uint256 i = 0; i < wallets.length; i++) {
HolographERC721.sol:357:    for (uint256 i = 0; i < length; i++) {
HolographERC721.sol:531:  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:547:  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:564:  //     for (uint256 i = 0; i < length; i++) {
HolographERC721.sol:716:    for (uint256 i = 0; i < length; i++) {
HolographOperator.sol:781:    for (uint256 i = 0; i < length; i++) {
HolographOperator.sol:871:        for (uint256 i = _operatorPods.length; i <= pod; i++) {
PA1D.sol:307:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:323:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:340:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:356:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:394:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:414:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:432:    for (uint256 t = 0; t < tokenAddresses.length; t++) {
PA1D.sol:437:      for (uint256 i = 0; i < addresses.length; i++) {
PA1D.sol:454:      for (uint256 i = 0; i < addresses.length; i++) {
PA1D.sol:474:    for (uint256 i = 0; i < addresses.length; i++) {
```
## [G-5] cache `.length` in loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

```solidity
HolographERC20.sol:564:    for (uint256 i = 0; i < wallets.length; i++) {
HolographERC20.sol:652:            if (reason.length == 0) {
HolographERC20.sol:664:        if (reason.length == 0) {
LayerZeroModule.sol:202:      calldatacopy(add(ptr, 0x0c), _srcAddress.offset, _srcAddress.length)
LayerZeroModule.sol:247:      abi.encodePacked(uint16(1), uint256(_baseGas() + (crossChainPayload.length * _gasPerByte())))
LayerZeroModule.sol:271:      uint256(_baseGas() + (crossChainPayload.length * _gasPerByte()))
HolographERC721.sol:528:  //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
HolographERC721.sol:531:  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:543:  //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
HolographERC721.sol:544:  //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
HolographERC721.sol:547:  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:700:    require(index < _allTokens.length, "ERC721: index out of bounds");
HolographERC721.sol:711:    uint256 supply = _allTokens.length;
HolographERC721.sol:739:    return _allTokens.length;
HolographERC721.sol:781:    _allTokensIndex[tokenId] = _allTokens.length;
HolographERC721.sol:825:    uint256 lastTokenIndex = _allTokens.length - 1;
HolographOperator.sol:316:      gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
HolographOperator.sol:320:      gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
HolographOperator.sol:363:          if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
HolographOperator.sol:391:          _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
HolographOperator.sol:408:        _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
HolographOperator.sol:451:      calldatacopy(0, payload.offset, sub(payload.length, 0x20))
HolographOperator.sol:461:        mload(sub(payload.length, 0x40)),
HolographOperator.sol:469:        sub(payload.length, 0x40),
HolographOperator.sol:503:      uint256 pod = random % _operatorPods.length;
HolographOperator.sol:507:      uint256 podSize = _operatorPods[pod].length;
HolographOperator.sol:551:      calldatacopy(0, bridgeInRequestPayload.offset, sub(bridgeInRequestPayload.length, 0x40))
HolographOperator.sol:556:      let result := call(gas(), sload(_bridgeSlot), callvalue(), 0, sub(bridgeInRequestPayload.length, 0x40), 0, 0)
HolographOperator.sol:718:    return _operatorPods.length;
HolographOperator.sol:728:    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
HolographOperator.sol:729:    return _operatorPods[pod - 1].length;
HolographOperator.sol:739:    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
HolographOperator.sol:756:    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
HolographOperator.sol:764:    uint256 supply = _operatorPods[pod].length;
HolographOperator.sol:867:      if (_operatorPods.length < pod) {
HolographOperator.sol:871:        for (uint256 i = _operatorPods.length; i <= pod; i++) {
HolographOperator.sol:881:      require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
HolographOperator.sol:883:      _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;
HolographOperator.sol:1137:        uint256 lastIndex = _operatorPods[pod].length - 1;
HolographOperator.sol:1169:    if (pod >= _operatorPods.length) {
HolographOperator.sol:1173:    uint256 position = _operatorPods[pod].length;
PA1D.sol:318:    uint256 length = addresses.length;
PA1D.sol:351:    uint256 length = bps.length;
PA1D.sol:385:    uint256 length = addresses.length;
PA1D.sol:408:    uint256 length = addresses.length;
PA1D.sol:432:    for (uint256 t = 0; t < tokenAddresses.length; t++) {
PA1D.sol:437:      for (uint256 i = 0; i < addresses.length; i++) {
PA1D.sol:454:      for (uint256 i = 0; i < addresses.length; i++) {
PA1D.sol:472:    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
PA1D.sol:474:    for (uint256 i = 0; i < addresses.length; i++) {
```

## [G-6] Donot use default values Explicit initialization with zero is not required for variable declaration of numTokens because `uints are 0` by default.removeing this will reduce contract size and save a bit of gas.

```solidity
HolographBridge.sol:380:    uint256 fee = 0;
HolographERC20.sol:564:    for (uint256 i = 0; i < wallets.length; i++) {
HolographERC721.sol:357:    for (uint256 i = 0; i < length; i++) {
HolographERC721.sol:531:  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:547:  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:564:  //     for (uint256 i = 0; i < length; i++) {
HolographERC721.sol:716:    for (uint256 i = 0; i < length; i++) {
HolographOperator.sol:310:    uint256 gasLimit = 0;
HolographOperator.sol:311:    uint256 gasPrice = 0;
HolographOperator.sol:781:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:307:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:323:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:340:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:356:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:394:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:414:    for (uint256 i = 0; i < length; i++) {
PA1D.sol:432:    for (uint256 t = 0; t < tokenAddresses.length; t++) {
PA1D.sol:437:      for (uint256 i = 0; i < addresses.length; i++) {
PA1D.sol:454:      for (uint256 i = 0; i < addresses.length; i++) {
PA1D.sol:474:    for (uint256 i = 0; i < addresses.length; i++) {
```


## [G-7]Use `variable == false|0 -> !variable` or `variable ==  true -> variable`
```solidity
HolographERC20.sol:652:            if (reason.length == 0) {
HolographERC20.sol:664:        if (reason.length == 0) {
LayerZeroModule.sol:266:    if (gasPrice == 0) {
LayerZeroModule.sol:290:    if (gasPrice == 0) {
HolographERC721.sol:850:    if (lastTokenIndex == 0) {
HolographOperator.sol:857:    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
PA1D.sol:190:    require(initialized == 0, "PA1D: already initialized");
PA1D.sol:534:    if (tokenId == 0) {
```


## [G-08] 10eX use less gas then 10**X
```solidity
LayerZeroModule.sol:274:    return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
LayerZeroModule.sol:293:    return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
HolographOperator.sol:256:      _baseBondAmount = 100 * (10**18); // one single token unit * 100
```

## [G‑09] `>=` COSTS LESS GAS THAN `>`
The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde 
```solidity
HolographBridge.sol:218:    if (hTokenValue > 0) {
HolographERC721.sol:353:    if (index + length > supply) {
HolographERC721.sol:712:    if (index + length > supply) {
HolographERC721.sol:815:    require(tokenId > 0, "ERC721: token id cannot be zero");
HolographOperator.sol:309:    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
HolographOperator.sol:350:        require(timeDifference > 0, "HOLOGRAPH: operator has time");
HolographOperator.sol:363:          if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
HolographOperator.sol:398:          if (leftovers > 0) {
HolographOperator.sol:415:    require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
HolographOperator.sol:519:      if (podSize > 1) {
HolographOperator.sol:768:    if (index + length > supply) {
HolographOperator.sol:1126:    if (operatorIndex > 0) {
HolographOperator.sol:1174:    if (position > threshold) {
PA1D.sol:390:    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
PA1D.sol:411:    require(balance > 10000, "PA1D: Not enough tokens to transfer");
PA1D.sol:435:      require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

## [G-10] Use `abi.encodePacked` for gas optimization 
Changing the abi.encode function to abi.encodePacked  can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient.
```solidity
HolographERC20.sol:409:    return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));
HolographERC20.sol:471:      abi.encode(
HolographFactory.sol:252:        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
HolographERC721.sol:260:        abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))
HolographERC721.sol:426:    return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```


## [G-11] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
check mannulay and only use of addresses is used as a key

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
```solidity
HolographERC20.sol:156:  mapping(address => uint256) private _balances;
HolographERC20.sol:161:  mapping(address => mapping(address => uint256)) private _allowances;
HolographERC20.sol:186:  mapping(address => uint256) private _nonces;
HolographERC721.sol:185:  mapping(address => uint256) private _ownedTokensCount;
HolographERC721.sol:190:  mapping(address => uint256[]) private _ownedTokens;
HolographERC721.sol:196:  mapping(address => mapping(address => bool)) private _operatorApprovals;
HolographOperator.sol:218:  mapping(address => uint256) private _bondedOperators;
HolographOperator.sol:223:  mapping(address => uint256) private _operatorPodIndex;
HolographOperator.sol:228:  mapping(address => uint256) private _bondedAmounts;
```

## [G-12] Use custom errors to save deployment and runtime costs in case of revert
Instead of using strings for error messages (e.g., `require(msg.sender == owner, “unauthorized”)`), you can use custom errors to reduce both deployment and runtime gas costs. In addition, they are very convenient as you can easily pass dynamic information to them.
```solidity

HolographERC20.sol:620:    require(account != address(0), "ERC20: account is zero address");
HolographERC20.sol:621:    require(spender != address(0), "ERC20: spender is zero address");
HolographERC20.sol:627:    require(account != address(0), "ERC20: account is zero address");
HolographERC20.sol:629:    require(accountBalance >= amount, "ERC20: amount exceeds balance");
HolographERC20.sol:695:    require(account != address(0), "ERC20: account is zero address");
HolographERC20.sol:696:    require(recipient != address(0), "ERC20: recipient is zero address");
HolographERC20.sol:698:    require(accountBalance >= amount, "ERC20: amount exceeds balance"); _holograph().getBridge(), "ERC721: bridge only call");
HolographERC721.sol:224:    require(msg.sender == sourceContract, "ERC721: source only call");
HolographERC721.sol:239:    require(!_isInitialized(), "ERC721: already initialized");
HolographERC721.sol:258:      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
HolographERC721.sol:263:      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
HolographERC721.sol:323:    require(_exists(tokenId), "ERC721: token does not exist");
HolographERC721.sol:370:    require(to != tokenOwner, "ERC721: cannot approve self");
HolographERC721.sol:371:    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
HolographERC721.sol:373:      require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));
HolographERC721.sol:378:      require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
HolographERC721.sol:388:    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
HolographERC721.sol:391:      require(SourceERC721().beforeBurn(wallet, tokenId));
HolographERC721.sol:395:      require(SourceERC721().afterBurn(wallet, tokenId));
HolographERC721.sol:404:    require(!_exists(tokenId), "ERC721: token already exists");

```

