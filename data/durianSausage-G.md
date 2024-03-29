
### G01: custom error save more gas
#### problem
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information, as explained https://blog.soliditylang.org/2021/04/21/custom-errors/. 
Custom errors are defined using the error statement.
#### prof
HolographOperator.sol, 241, b'    require(!_isInitialized(), "HOLOGRAPH: already initialized");'
HolographOperator.sol, 309, b'    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");'
HolographOperator.sol, 350, b'        require(timeDifference > 0, "HOLOGRAPH: operator has time");'
HolographOperator.sol, 354, b'        require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");'
HolographOperator.sol, 368, b'            require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");'
HolographOperator.sol, 415, b'    require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");'
HolographOperator.sol, 446, b'    require(msg.sender == address(this), "HOLOGRAPH: operator only call");'
HolographOperator.sol, 485, b'    require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");'
HolographOperator.sol, 591, b'    require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");'
HolographOperator.sol, 595, b'    require(hlgFee < msg.value, "HOLOGRAPH: not enough value");'
HolographOperator.sol, 728, b'    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");'
HolographOperator.sol, 739, b'    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");'
HolographOperator.sol, 756, b'    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");'
HolographOperator.sol, 829, b'    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");'
HolographOperator.sol, 839, b'    require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");'
HolographOperator.sol, 857, b'    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");'
HolographOperator.sol, 863, b'      require(current <= amount, "HOLOGRAPH: bond amount too small");'
HolographOperator.sol, 881, b'      require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");'
HolographOperator.sol, 889, b'      require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");'
HolographOperator.sol, 903, b'    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");'
HolographOperator.sol, 911, b'      require(_isContract(operator), "HOLOGRAPH: operator not contract");'
HolographOperator.sol, 915, b'      require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");'
HolographOperator.sol, 932, b'    require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");'
HolographBridge.sol, 163, b'    require(!_isInitialized(), "HOLOGRAPH: already initialized");'
HolographBridge.sol, 206, b'    require(\n      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,\n      "HOLOGRAPH: not holographed"\n    );'
HolographBridge.sol, 214, b'    require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");'
HolographBridge.sol, 228, b'      require(\n        HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==\n          HolographERC20Interface.holographBridgeMint.selector,\n        "HOLOGRAPH: hToken mint failed"\n      );'
HolographBridge.sol, 233, b'    require(doNotRevert, "HOLOGRAPH: reverted");'
HolographBridge.sol, 258, b'    require(\n      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,\n      "HOLOGRAPH: not holographed"\n    );'
HolographBridge.sol, 270, b'    require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");'
HolographBridge.sol, 355, b'    require(\n      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,\n      "HOLOGRAPH: not holographed"\n    );'
enforcer/HolographERC721.sol, 239, b'    require(!_isInitialized(), "ERC721: already initialized");'
enforcer/HolographERC721.sol, 258, b'      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");'
enforcer/HolographERC721.sol, 263, b'      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");'
enforcer/HolographERC721.sol, 323, b'    require(_exists(tokenId), "ERC721: token does not exist");'
enforcer/HolographERC721.sol, 370, b'    require(to != tokenOwner, "ERC721: cannot approve self");'
enforcer/HolographERC721.sol, 371, b'    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");'
enforcer/HolographERC721.sol, 388, b'    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");'
enforcer/HolographERC721.sol, 404, b'    require(!_exists(tokenId), "ERC721: token already exists");'
enforcer/HolographERC721.sol, 408, b'      require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");'
enforcer/HolographERC721.sol, 419, b'    require(to != address(0), "ERC721: zero address");'
enforcer/HolographERC721.sol, 420, b'    require(_isApproved(sender, tokenId), "ERC721: sender not approved");'
enforcer/HolographERC721.sol, 421, b'    require(from == _tokenOwner[tokenId], "ERC721: from is not owner");'
enforcer/HolographERC721.sol, 458, b'    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");'
enforcer/HolographERC721.sol, 470, b'      require(\n        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&\n          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&\n          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==\n          ERC721TokenReceiver.onERC721Received.selector),\n        "ERC721: onERC721Received fail"\n      );'
enforcer/HolographERC721.sol, 484, b'    require(to != msg.sender, "ERC721: cannot approve self");'
enforcer/HolographERC721.sol, 513, b'    require(!_burnedTokens[token], "ERC721: can\'t mint burned token");'
enforcer/HolographERC721.sol, 622, b'    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");'
enforcer/HolographERC721.sol, 639, b'    require(wallet != address(0), "ERC721: zero address");'
enforcer/HolographERC721.sol, 689, b'    require(tokenOwner != address(0), "ERC721: token does not exist");'
enforcer/HolographERC721.sol, 700, b'    require(index < _allTokens.length, "ERC721: index out of bounds");'
enforcer/HolographERC721.sol, 729, b'    require(index < balanceOf(wallet), "ERC721: index out of bounds");'
enforcer/HolographERC721.sol, 757, b'    require(_isContract(_operator), "ERC721: operator not contract");'
enforcer/HolographERC721.sol, 815, b'    require(tokenId > 0, "ERC721: token id cannot be zero");'
enforcer/HolographERC721.sol, 816, b'    require(to != address(0), "ERC721: minting to burn address");'
enforcer/HolographERC721.sol, 817, b'    require(!_exists(tokenId), "ERC721: token already exists");'
enforcer/HolographERC721.sol, 818, b'    require(!_burnedTokens[tokenId], "ERC721: token has been burned");'
enforcer/HolographERC721.sol, 869, b'    require(_tokenOwner[tokenId] == from, "ERC721: token not owned");'
enforcer/HolographERC721.sol, 870, b'    require(to != address(0), "ERC721: use burn instead");'
enforcer/HolographERC721.sol, 906, b'    require(_exists(tokenId), "ERC721: token does not exist");'
abstract/ERC20H.sol, 147, b'    require(!_isInitialized(), "ERC20: already initialized");'
abstract/ERC721H.sol, 147, b'    require(!_isInitialized(), "ERC721: already initialized");'
enforcer/Holographer.sol, 148, b'    require(!_isInitialized(), "HOLOGRAPHER: already initialized");'
enforcer/Holographer.sol, 166, b'    require(success && selector == InitializableInterface.init.selector, "initialization failed");'
HolographFactory.sol, 144, b'    require(!_isInitialized(), "HOLOGRAPH: already initialized");'
HolographFactory.sol, 220, b'    require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");'
HolographFactory.sol, 228, b'    require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");'
HolographFactory.sol, 255, b'    require(\n      InitializableInterface(holographerAddress).init(\n        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)\n      ) == InitializableInterface.init.selector,\n      "initialization failed"\n    );'
enforcer/HolographERC20.sol, 219, b'    require(!_isInitialized(), "ERC20: already initialized");'
enforcer/HolographERC20.sol, 241, b'      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");'
enforcer/HolographERC20.sol, 349, b'    require(currentAllowance >= amount, "ERC20: amount exceeds allowance");'
enforcer/HolographERC20.sol, 365, b'    require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");'
enforcer/HolographERC20.sol, 387, b'      require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");'
enforcer/HolographERC20.sol, 400, b'      require(currentAllowance >= amount, "ERC20: amount exceeds allowance");'
enforcer/HolographERC20.sol, 427, b'      require(newAllowance >= currentAllowance, "ERC20: increased above max value");'
enforcer/HolographERC20.sol, 445, b'    require(_isContract(account), "ERC20: operator not contract");'
enforcer/HolographERC20.sol, 469, b'    require(block.timestamp <= deadline, "ERC20: expired deadline");'
enforcer/HolographERC20.sol, 482, b'    require(signer == account, "ERC20: invalid signature");'
enforcer/HolographERC20.sol, 505, b'    require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");'
enforcer/HolographERC20.sol, 529, b'        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");'
enforcer/HolographERC20.sol, 539, b'    require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");'
enforcer/HolographERC20.sol, 599, b'        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");'
enforcer/HolographERC20.sol, 620, b'    require(account != address(0), "ERC20: account is zero address");'
enforcer/HolographERC20.sol, 621, b'    require(spender != address(0), "ERC20: spender is zero address");'
enforcer/HolographERC20.sol, 627, b'    require(account != address(0), "ERC20: account is zero address");'
enforcer/HolographERC20.sol, 629, b'    require(accountBalance >= amount, "ERC20: amount exceeds balance");'
enforcer/HolographERC20.sol, 684, b'    require(to != address(0), "ERC20: minting to burn address");'
enforcer/HolographERC20.sol, 695, b'    require(account != address(0), "ERC20: account is zero address");'
enforcer/HolographERC20.sol, 696, b'    require(recipient != address(0), "ERC20: recipient is zero address");'
enforcer/HolographERC20.sol, 698, b'    require(accountBalance >= amount, "ERC20: amount exceeds balance");'
module/LayerZeroModule.sol, 159, b'    require(!_isInitialized(), "HOLOGRAPH: already initialized");'
module/LayerZeroModule.sol, 235, b'    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");'
enforcer/PA1D.sol, 174, b'    require(!_isInitialized(), "PA1D: already initialized");'
enforcer/PA1D.sol, 190, b'    require(initialized == 0, "PA1D: already initialized");'
enforcer/PA1D.sol, 390, b'    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");'
enforcer/PA1D.sol, 411, b'    require(balance > 10000, "PA1D: Not enough tokens to transfer");'
enforcer/PA1D.sol, 416, b'      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn\'t transfer token");'
enforcer/PA1D.sol, 435, b'      require(balance > 10000, "PA1D: Not enough tokens to transfer");'
enforcer/PA1D.sol, 439, b'        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn\'t transfer token");'
enforcer/PA1D.sol, 460, b'      require(matched, "PA1D: sender not authorized");'
enforcer/PA1D.sol, 472, b'    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");'
enforcer/PA1D.sol, 477, b'    require(totalBp == 10000, "PA1D: bps down\'t equal 10000");'

### G02: COMPARISONS WITH ZERO FOR UNSIGNED INTEGERS
#### problem
0 is less gas efficient than !0 if you enable the optimizer at 10k AND you’re in a require statement. Detailed explanation with the opcodes https://twitter.com/gzeon/status/1485428085885640706
#### prof
HolographOperator.sol, 309, b'    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");'
HolographOperator.sol, 350, b'        require(timeDifference > 0, "HOLOGRAPH: operator has time");'
HolographOperator.sol, 363, b'          if (podIndex > 0 && podIndex < _operatorPods[pod].length) {'
HolographOperator.sol, 398, b'          if (leftovers > 0) {'
HolographOperator.sol, 1126, b'    if (operatorIndex > 0) {'
HolographBridge.sol, 218, b'    if (hTokenValue > 0) {'
enforcer/HolographERC721.sol, 815, b'    require(tokenId > 0, "ERC721: token id cannot be zero");'


### G03: PREFIX INCREMENT SAVE MORE GAS
#### problem
prefix increment ++i is more cheaper than postfix i++
#### prof
HolographOperator.sol, 520, b'        podSize--;'
HolographOperator.sol, 760, b'    pod--;'
HolographOperator.sol, 781, b'    for (uint256 i = 0; i < length; i++) {'
HolographOperator.sol, 871, b'        for (uint256 i = _operatorPods.length; i <= pod; i++) {'
enforcer/HolographERC721.sol, 357, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/HolographERC721.sol, 716, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/HolographERC721.sol, 779, b'    _ownedTokensCount[to]++;'
enforcer/HolographERC721.sol, 842, b'    _ownedTokensCount[from]--;'
enforcer/HolographERC20.sol, 564, b'    for (uint256 i = 0; i < wallets.length; i++) {'
enforcer/HolographERC20.sol, 713, b'    _nonces[account]++;'
enforcer/PA1D.sol, 307, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 323, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 340, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 356, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 394, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 414, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 437, b'      for (uint256 i = 0; i < addresses.length; i++) {'
enforcer/PA1D.sol, 432, b'    for (uint256 t = 0; t < tokenAddresses.length; t++) {'
enforcer/PA1D.sol, 454, b'      for (uint256 i = 0; i < addresses.length; i++) {'
enforcer/PA1D.sol, 474, b'    for (uint256 i = 0; i < addresses.length; i++) {'

### G04: X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES
#### prof
HolographOperator.sol, 378, b'        _bondedAmounts[job.operator] -= amount;'
HolographOperator.sol, 382, b'        _bondedAmounts[msg.sender] += amount;'
HolographOperator.sol, 834, b'      _bondedAmounts[operator] += amount;'
enforcer/HolographERC20.sol, 633, b'    _totalSupply -= amount;'
enforcer/HolographERC20.sol, 685, b'    _totalSupply += amount;'
enforcer/HolographERC20.sol, 686, b'    _balances[to] += amount;'
enforcer/HolographERC20.sol, 702, b'    _balances[recipient] += amount;'


### G05: ARRAY LENTH SHOULD USE MEMoRY VARIABLE LOAD IT
#### problem
The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP (3 gas), and gets rid of the extra DUP needed to store the stack offset
#### prof
HolographOperator.sol, 871, b'        for (uint256 i = _operatorPods.length; i <= pod; i++) {'

### G06: USING BOOLS FOR STORAGE INCURS OVERHEAD
#### problem
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
#### prof
HolographOperator.sol, 198, b'  mapping(bytes32 => bool) private _failedJobs;'
enforcer/HolographERC721.sol, 196, b'  mapping(address => mapping(address => bool)) private _operatorApprovals;'
enforcer/HolographERC721.sol, 206, b'  mapping(uint256 => bool) private _burnedTokens;'


### G07: resign the default value to the variables.
#### problem
 resign the default value to the variables will cost more gas.
#### prof
HolographOperator.sol, 310, b'    uint256 gasLimit = 0;'
HolographOperator.sol, 311, b'    uint256 gasPrice = 0;'
HolographOperator.sol, 781, b'    for (uint256 i = 0; i < length; i++) {'
HolographBridge.sol, 380, b'    uint256 fee = 0;'
enforcer/HolographERC721.sol, 357, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/HolographERC721.sol, 716, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/HolographERC20.sol, 564, b'    for (uint256 i = 0; i < wallets.length; i++) {'
enforcer/PA1D.sol, 307, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 323, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 340, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 356, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 394, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 414, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 437, b'      for (uint256 i = 0; i < addresses.length; i++) {'
enforcer/PA1D.sol, 432, b'    for (uint256 t = 0; t < tokenAddresses.length; t++) {'
enforcer/PA1D.sol, 454, b'      for (uint256 i = 0; i < addresses.length; i++) {'
enforcer/PA1D.sol, 474, b'    for (uint256 i = 0; i < addresses.length; i++) {'


### G08: ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
#### problem
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
#### prof
HolographOperator.sol, 433, b'    ++_inboundMessageCounter;'
HolographOperator.sol, 495, b'      ++_operatorTempStorageCounter;'
HolographOperator.sol, 520, b'        podSize--;'
HolographOperator.sol, 760, b'    pod--;'
HolographOperator.sol, 781, b'    for (uint256 i = 0; i < length; i++) {'
HolographOperator.sol, 871, b'        for (uint256 i = _operatorPods.length; i <= pod; i++) {'
enforcer/HolographERC721.sol, 357, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/HolographERC721.sol, 716, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/HolographERC721.sol, 779, b'    _ownedTokensCount[to]++;'
enforcer/HolographERC721.sol, 842, b'    _ownedTokensCount[from]--;'
enforcer/HolographERC20.sol, 564, b'    for (uint256 i = 0; i < wallets.length; i++) {'
enforcer/HolographERC20.sol, 713, b'    _nonces[account]++;'
enforcer/PA1D.sol, 307, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 323, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 340, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 356, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 394, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 414, b'    for (uint256 i = 0; i < length; i++) {'
enforcer/PA1D.sol, 437, b'      for (uint256 i = 0; i < addresses.length; i++) {'
enforcer/PA1D.sol, 432, b'    for (uint256 t = 0; t < tokenAddresses.length; t++) {'
enforcer/PA1D.sol, 454, b'      for (uint256 i = 0; i < addresses.length; i++) {'
enforcer/PA1D.sol, 474, b'    for (uint256 i = 0; i < addresses.length; i++) {'

### G09：ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()
#### prof
enforcer/HolographERC721.sol, 260, b'        abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))'
HolographFactory.sol, 252, b'        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)'
HolographFactory.sol, 252, b'        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)'
enforcer/HolographERC20.sol, 471, b'      abi.encode('

### G10: Splitting require() statements that use && saves gas
#### problem
See this issue(https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper
#### prof:
HolographOperator.sol, 857, b'    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");'
HolographBridge.sol, 206, b'    require(\n      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,\n      "HOLOGRAPH: not holographed"\n    );'
HolographBridge.sol, 258, b'    require(\n      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,\n      "HOLOGRAPH: not holographed"\n    );'
HolographBridge.sol, 355, b'    require(\n      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,\n      "HOLOGRAPH: not holographed"\n    );'
enforcer/HolographERC721.sol, 263, b'      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");'
enforcer/HolographERC721.sol, 470, b'      require(\n        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&\n          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&\n          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==\n          ERC721TokenReceiver.onERC721Received.selector),\n        "ERC721: onERC721Received fail"\n      );'
enforcer/Holographer.sol, 166, b'    require(success && selector == InitializableInterface.init.selector, "initialization failed");'


### G11: FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
#### problem
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
#### prof
HolographOperator.sol, 272, b'  function init(bytes memory initPayload) external override returns (bytes4) '
HolographOperator.sol, 294, b'  function resetOperator(\n    uint256 blockTime,\n    uint256 baseBondAmount,\n    uint256 podMultiplier,\n    uint256 operatorThreshold,\n    uint256 operatorThresholdStep,\n    uint256 operatorThresholdDivisor\n  ) external onlyAdmin '
HolographOperator.sol, 294, b'  function resetOperator(\n    uint256 blockTime,\n    uint256 baseBondAmount,\n    uint256 podMultiplier,\n    uint256 operatorThreshold,\n    uint256 operatorThresholdStep,\n    uint256 operatorThresholdDivisor\n  ) external onlyAdmin '
HolographOperator.sol, 933, b'  function unbondUtilityToken(address operator, address recipient) external '
HolographOperator.sol, 953, b'  function setBridge(address bridge) external onlyAdmin '
HolographOperator.sol, 953, b'  function setBridge(address bridge) external onlyAdmin '
HolographOperator.sol, 973, b'  function setHolograph(address holograph) external onlyAdmin '
HolographOperator.sol, 973, b'  function setHolograph(address holograph) external onlyAdmin '
HolographOperator.sol, 993, b'  function setInterfaces(address interfaces) external onlyAdmin '
HolographOperator.sol, 993, b'  function setInterfaces(address interfaces) external onlyAdmin '
HolographOperator.sol, 1013, b'  function setMessagingModule(address messagingModule) external onlyAdmin '
HolographOperator.sol, 1013, b'  function setMessagingModule(address messagingModule) external onlyAdmin '
HolographOperator.sol, 1033, b'  function setRegistry(address registry) external onlyAdmin '
HolographOperator.sol, 1033, b'  function setRegistry(address registry) external onlyAdmin '
HolographOperator.sol, 1053, b'  function setUtilityToken(address utilityToken) external onlyAdmin '
HolographOperator.sol, 1053, b'  function setUtilityToken(address utilityToken) external onlyAdmin '
HolographBridge.sol, 177, b'  function init(bytes memory initPayload) external override returns (bytes4) '
HolographBridge.sol, 456, b'  function setFactory(address factory) external onlyAdmin '
HolographBridge.sol, 456, b'  function setFactory(address factory) external onlyAdmin '
HolographBridge.sol, 476, b'  function setHolograph(address holograph) external onlyAdmin '
HolographBridge.sol, 476, b'  function setHolograph(address holograph) external onlyAdmin '
HolographBridge.sol, 506, b'  function setOperator(address operator) external onlyAdmin '
HolographBridge.sol, 506, b'  function setOperator(address operator) external onlyAdmin '
HolographBridge.sol, 526, b'  function setRegistry(address registry) external onlyAdmin '
HolographBridge.sol, 526, b'  function setRegistry(address registry) external onlyAdmin '
enforcer/HolographERC721.sol, 268, b'  function init(bytes memory initPayload) external override returns (bytes4) '
enforcer/HolographERC721.sol, 338, b'  function tokensOfOwner(address wallet) external view returns (uint256[] memory) '
enforcer/HolographERC721.sol, 338, b'  function tokensOfOwner(address wallet) external view returns (uint256[] memory) '
enforcer/HolographERC721.sol, 360, b'  function tokensOfOwner(\n    address wallet,\n    uint256 index,\n    uint256 length\n  ) external view returns (uint256[] memory tokenIds) '
enforcer/HolographERC721.sol, 380, b'  function approve(address to, uint256 tokenId) external payable '
enforcer/HolographERC721.sol, 397, b'  function burn(uint256 tokenId) external '
enforcer/HolographERC721.sol, 427, b'  function bridgeOut(\n    uint32 toChain,\n    address sender,\n    bytes calldata payload\n  ) external onlyBridge returns (bytes4 selector, bytes memory data) '
enforcer/HolographERC721.sol, 503, b'  function sourceBurn(uint256 tokenId) external onlySource '
enforcer/HolographERC721.sol, 580, b'  function sourceTransfer(address to, uint256 tokenId) external onlySource '
enforcer/HolographERC721.sol, 658, b'  function exists(uint256 tokenId) public view returns (bool) '
enforcer/HolographERC721.sol, 691, b'  function ownerOf(uint256 tokenId) external view returns (address) '
enforcer/HolographERC721.sol, 691, b'  function ownerOf(uint256 tokenId) external view returns (address) '
enforcer/HolographERC721.sol, 731, b'  function tokenOfOwnerByIndex(address wallet, uint256 index) external view returns (uint256) '
enforcer/HolographERC721.sol, 731, b'  function tokenOfOwnerByIndex(address wallet, uint256 index) external view returns (uint256) '
enforcer/HolographERC721.sol, 770, b'  function onERC721Received(\n    address _operator,\n    address _from,\n    uint256 _tokenId,\n    bytes calldata _data\n  ) external returns (bytes4) '
enforcer/HolographERC721.sol, 783, b'  function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private '
enforcer/HolographERC721.sol, 783, b'  function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private '
enforcer/HolographERC721.sol, 797, b'  function _burn(address wallet, uint256 tokenId) private '
enforcer/HolographERC721.sol, 822, b'  function _mint(address to, uint256 tokenId) private '
enforcer/HolographERC721.sol, 856, b'  function _removeTokenFromOwnerEnumeration(address from, uint256 tokenId) private '
enforcer/HolographERC721.sol, 856, b'  function _removeTokenFromOwnerEnumeration(address from, uint256 tokenId) private '
enforcer/HolographERC721.sol, 876, b'  function _transferFrom(\n    address from,\n    address to,\n    uint256 tokenId\n  ) private '
enforcer/HolographERC721.sol, 896, b'  function _exists(uint256 tokenId) private view returns (bool) '
enforcer/HolographERC721.sol, 909, b'  function _isApproved(address spender, uint256 tokenId) private view returns (bool) '
enforcer/HolographERC721.sol, 941, b'  function owner() public view override returns (address) '
enforcer/HolographERC721.sol, 941, b'  function owner() public view override returns (address) '
abstract/ERC20H.sol, 183, b'  function owner() external view returns (address) '
abstract/ERC20H.sol, 183, b'  function owner() external view returns (address) '
abstract/ERC20H.sol, 191, b'  function isOwner() external view returns (bool) '
abstract/ERC20H.sol, 191, b'  function isOwner() external view returns (bool) '
abstract/ERC20H.sol, 195, b'  function isOwner(address wallet) external view returns (bool) '
abstract/ERC20H.sol, 195, b'  function isOwner(address wallet) external view returns (bool) '
abstract/ERC20H.sol, 201, b'  function _getOwner() internal view returns (address ownerAddress) '
abstract/ERC20H.sol, 201, b'  function _getOwner() internal view returns (address ownerAddress) '
abstract/ERC20H.sol, 207, b'  function _setOwner(address ownerAddress) internal '
abstract/ERC20H.sol, 207, b'  function _setOwner(address ownerAddress) internal '
abstract/ERC721H.sol, 183, b'  function owner() external view returns (address) '
abstract/ERC721H.sol, 183, b'  function owner() external view returns (address) '
abstract/ERC721H.sol, 191, b'  function isOwner() external view returns (bool) '
abstract/ERC721H.sol, 191, b'  function isOwner() external view returns (bool) '
abstract/ERC721H.sol, 195, b'  function isOwner(address wallet) external view returns (bool) '
abstract/ERC721H.sol, 195, b'  function isOwner(address wallet) external view returns (bool) '
abstract/ERC721H.sol, 201, b'  function _getOwner() internal view returns (address ownerAddress) '
abstract/ERC721H.sol, 201, b'  function _getOwner() internal view returns (address ownerAddress) '
abstract/ERC721H.sol, 207, b'  function _setOwner(address ownerAddress) internal '
abstract/ERC721H.sol, 207, b'  function _setOwner(address ownerAddress) internal '
enforcer/Holographer.sol, 169, b'  function init(bytes memory initPayload) external override returns (bytes4) '
HolographFactory.sol, 153, b'  function init(bytes memory initPayload) external override returns (bytes4) '
HolographFactory.sol, 284, b'  function setHolograph(address holograph) external onlyAdmin '
HolographFactory.sol, 284, b'  function setHolograph(address holograph) external onlyAdmin '
HolographFactory.sol, 304, b'  function setRegistry(address registry) external onlyAdmin '
HolographFactory.sol, 304, b'  function setRegistry(address registry) external onlyAdmin '
enforcer/HolographERC20.sol, 246, b'  function init(bytes memory initPayload) external override returns (bytes4) '
enforcer/HolographERC20.sol, 746, b'  function owner() public view override returns (address) '
enforcer/HolographERC20.sol, 746, b'  function owner() public view override returns (address) '
module/LayerZeroModule.sol, 174, b'  function init(bytes memory initPayload) external override returns (bytes4) '
module/LayerZeroModule.sol, 324, b'  function setBridge(address bridge) external onlyAdmin '
module/LayerZeroModule.sol, 324, b'  function setBridge(address bridge) external onlyAdmin '
module/LayerZeroModule.sol, 344, b'  function setInterfaces(address interfaces) external onlyAdmin '
module/LayerZeroModule.sol, 344, b'  function setInterfaces(address interfaces) external onlyAdmin '
module/LayerZeroModule.sol, 364, b'  function setLZEndpoint(address lZEndpoint) external onlyAdmin '
module/LayerZeroModule.sol, 364, b'  function setLZEndpoint(address lZEndpoint) external onlyAdmin '
module/LayerZeroModule.sol, 384, b'  function setOperator(address operator) external onlyAdmin '
module/LayerZeroModule.sol, 384, b'  function setOperator(address operator) external onlyAdmin '
module/LayerZeroModule.sol, 445, b'  function setBaseGas(uint256 baseGas) external onlyAdmin '
module/LayerZeroModule.sol, 445, b'  function setBaseGas(uint256 baseGas) external onlyAdmin '
module/LayerZeroModule.sol, 474, b'  function setGasPerByte(uint256 gasPerByte) external onlyAdmin '
module/LayerZeroModule.sol, 474, b'  function setGasPerByte(uint256 gasPerByte) external onlyAdmin '
enforcer/PA1D.sol, 183, b'  function init(bytes memory initPayload) external override returns (bytes4) '
enforcer/PA1D.sol, 210, b'  function isOwner() private view returns (bool) '
enforcer/PA1D.sol, 210, b'  function isOwner() private view returns (bool) '
enforcer/PA1D.sol, 462, b'  function _validatePayoutRequestor() private view '
enforcer/PA1D.sol, 480, b'  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner '
enforcer/PA1D.sol, 546, b'  function setRoyalties(\n    uint256 tokenId,\n    address payable receiver,\n    uint256 bp\n  ) public onlyOwner '
enforcer/PA1D.sol, 675, b'  function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) '