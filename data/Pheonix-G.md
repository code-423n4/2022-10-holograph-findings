|  | Issue | Instances |
| --- | --- | --- |
| [G-01] | Preincrements/Predecrements are cheaper especially  in for() loops | 14 |
| [G-02] | Replace require()/revert() statements with Custom Errors . | 119 |
| [G-03] | Cache the length of array  | 5 |
| [G-04] | Splitting require() statements that use && | 3 |
| [G-05] | It costs more gas to initialize variables with their default value than letting the default value be applied | 13 |
| [G-06] | Bytes constant are cheaper than string constants | 3 |

# [G-01] Preincrements/Predecrements are cheaper especially  in `for()` loops

1. `++i` costs less gas than `i++` , especially when it’s used in for-loops (--i/i--` too)
Pre-increments and pre-decrements are cheaper.
2. For a `uint256 i` variable, the following is true with the Optimizer enabled

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (**11 gas less than `i += 1`**)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `-i` costs 5 gas less than `i--` (**16 gas less than `i -= 1`**)
- 

`i++` increments `i` and returns the initial value of `i`. Which means:

```
uint i = 1;
i++; // == 1 but i == 2

```

But `++i` returns the actual incremented value:

```
uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable

```

In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`

## Instances -

File: contracts\HolographOperator.sol

```solidity
781: for (uint256 i = 0; i < length; i++) {
871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

File: contracts\enforcer\HolographERC20.sol

```solidity
564: for (uint256 i = 0; i < wallets.length; i++) {
```

File: contracts\enforcer\HolographERC721.sol

```solidity
357: for (uint256 i = 0; i < length; i++) {
716: for (uint256 i = 0; i < length; i++) {
```

File: contracts\enforcer\PA1D.sol

```solidity
307: for (uint256 i = 0; i < length; i++) {
323: for (uint256 i = 0; i < length; i++) {
340: for (uint256 i = 0; i < length; i++) {
356: for (uint256 i = 0; i < length; i++) {
394: for (uint256 i = 0; i < length; i++) {
414: for (uint256 i = 0; i < length; i++) {
437: for (uint256 i = 0; i < addresses.length; i++) {
454: for (uint256 i = 0; i < addresses.length; i++) {
474: for (uint256 i = 0; i < addresses.length; i++) {
```

NOTE - THE ABOVE REASONING IS ALSO VALID FOR ‘ x++’ IN GENERAL 

REPLACE  `i++/i--` with `++i/--i`  OR  x++ with ++x IN THE INSTANCES MENTIONED ABOVE . 

# [G-02] Replace `require()`/`revert()` statements with Custom Errors .

1. Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4) ,  **custom errors** were introduced in order to tell the user about any error . It was done through the use of strings to give any info about errors
2. Use of custom errors can save **[~50 gas](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746)** each time they are hit because it  dosen’t need  to allocate and store the revert string .
3. Not defining the strings also save deployment gas 
4. You can implement custom errors as shown in the link below . 
5. [https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth)
6. [https://solidity-by-example.org/error/](https://solidity-by-example.org/error/)

## Instances -

```solidity
File: contracts\HolographBridge.sol

148: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
163: require(!_isInitialized(), "HOLOGRAPH: already initialized");
203: require(
204:       _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
205:       "HOLOGRAPH: not holographed"
206:     );
214: require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
224: require(
225:         HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==
226:           HolographERC20Interface.holographBridgeMint.selector,
227:         "HOLOGRAPH: hToken mint failed"
228:       );
233: require(doNotRevert, "HOLOGRAPH: reverted");
255: require(
256:       _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
257:       "HOLOGRAPH: not holographed"
270: require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
352: require(
353:       _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
354:       "HOLOGRAPH: not holographed"
355:     );

```

```solidity
File: contracts\HolographOperator.sol

241: require(!_isInitialized(), "HOLOGRAPH: already initialized");
309: require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
350: require(timeDifference > 0, "HOLOGRAPH: operator has time");
354: require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
368: require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
415: require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
446: require(msg.sender == address(this), "HOLOGRAPH: operator only call");
485: require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
591: require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
595: require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
739: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
829: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
863: require(current <= amount, "HOLOGRAPH: bond amount too small");
881: require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
903: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
911: require(_isContract(operator), "HOLOGRAPH: operator not contract");
915: require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
932: require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```

```solidity
File: contracts\HolographFactory.sol

144: require(!_isInitialized(), "HOLOGRAPH: already initialized");
220: require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
228: require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
250: require(
251:       InitializableInterface(holographerAddress).init(
252:         abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
253:       ) == InitializableInterface.init.selector,
254:       "initialization failed"
255:     );
```

```solidity
File: contracts\module\LayerZeroModule.sol

159: require(!_isInitialized(), "HOLOGRAPH: already initialized");
235: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
```

```solidity
File: contracts\enforcer\Holographer.sol

148: require(!_isInitialized(), "HOLOGRAPHER: already initialized");
166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

```solidity
File: contracts\abstract\ERC721H.sol

117: require(msg.sender == holographer(), "ERC721: holographer only");
123: require(msgSender() == _getOwner(), "ERC721: owner only function");
125: require(msg.sender == _getOwner(), "ERC721: owner only function");
147: require(!_isInitialized(), "ERC721: already initialized");

```

```solidity
File: contracts\abstract\ERC20H.sol]

117: require(msg.sender == holographer(), "ERC20: holographer only");
123: require(msgSender() == _getOwner(), "ERC20: owner only function");
125: require(msg.sender == _getOwner(), "ERC20: owner only function");
147: require(!_isInitialized(), "ERC20: already initialized");
```

```solidity
File: contracts\enforcer\HolographERC20.sol

192: require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");
204: require(msg.sender == sourceContract, "ERC20: source only call");
219: require(!_isInitialized(), "ERC20: already initialized");
241: require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");
349: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
365: require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
387: require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");
400: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
427: require(newAllowance >= currentAllowance, "ERC20: increased above max value");
445: require(_isContract(account), "ERC20: operator not contract");
450: require(balance >= amount, "ERC20: balance check failed");
469: require(block.timestamp <= deadline, "ERC20: expired deadline");
482: require(signer == account, "ERC20: invalid signature");
505: require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");
529: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
539: require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");
599: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
620: require(account != address(0), "ERC20: account is zero address");
621: require(spender != address(0), "ERC20: spender is zero address");
627: require(account != address(0), "ERC20: account is zero address");
629: require(accountBalance >= amount, "ERC20: amount exceeds balance");
645: require(erc165support, "ERC20: no ERC165 support");
684: require(to != address(0), "ERC20: minting to burn address");
695: require(account != address(0), "ERC20: account is zero address");
696: require(recipient != address(0), "ERC20: recipient is zero address");
698: require(accountBalance >= amount, "ERC20: amount exceeds balance");
```

```solidity
File: contracts\enforcer\HolographERC721.sol

212:     require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");
224:     require(msg.sender == sourceContract, "ERC721: source only call");
239:     require(!_isInitialized(), "ERC721: already initialized");
258:     require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
263:       require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
323:     require(_exists(tokenId), "ERC721: token does not exist");
370:     require(to != tokenOwner, "ERC721: cannot approve self");
371:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
388:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
404:     require(!_exists(tokenId), "ERC721: token already exists");
408:     require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");
419:     require(to != address(0), "ERC721: zero address");
420:     require(_isApproved(sender, tokenId), "ERC721: sender not approved");
421:     require(from == _tokenOwner[tokenId], "ERC721: from is not owner");
458:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
464:     require(
465:         (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
466:           ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
467:           ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
468:           ERC721TokenReceiver.onERC721Received.selector),
469:         "ERC721: onERC721Received fail"
470:       );
484:     require(to != msg.sender, "ERC721: cannot approve self");
486:     require(SourceERC721().beforeApprovalAll(to, approved));
491:     require(SourceERC721().afterApprovalAll(to, approved));
513:     require(!_burnedTokens[token], "ERC721: can't mint burned token");
622:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
639:     require(wallet != address(0), "ERC721: zero address");
689:     require(tokenOwner != address(0), "ERC721: token does not exist");
700:     require(index < _allTokens.length, "ERC721: index out of bounds");
729:     require(index < balanceOf(wallet), "ERC721: index out of bounds");
757:     require(_isContract(_operator), "ERC721: operator not contract");
762:     require(tokenOwner == address(this), "ERC721: contract not token owner");
815:     require(tokenId > 0, "ERC721: token id cannot be zero");
816:     require(to != address(0), "ERC721: minting to burn address");
817:     require(!_exists(tokenId), "ERC721: token already exists");
818:     require(!_burnedTokens[tokenId], "ERC721: token has been burned");
869:     require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
870:     require(to != address(0), "ERC721: use burn instead");
906:     require(_exists(tokenId), "ERC721: token does not exist");
```

```solidity
File: contracts\enforcer\PA1D.sol

159:     require(isOwner(), "PA1D: caller not an owner");
174:     require(!_isInitialized(), "PA1D: already initialized");
190:     require(initialized == 0, "PA1D: already initialized");
390:     require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
411:     require(balance > 10000, "PA1D: Not enough tokens to transfer");
416:     require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
435:     require(balance > 10000, "PA1D: Not enough tokens to transfer");
439:     require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
460:     require(matched, "PA1D: sender not authorized");
472:     require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
477:     require(totalBp == 10000, "PA1D: bps down't equal 10000");

```

CONSIDER CHANGING ALL `require()` AND  `revert()`  STATMENT WITH **CUSTOM ERRORS** 

# [G-03] Cache the length of array

 The solidity compiler will always read the length of the array during each iteration. That is,

1.  if it is a storage array, this is an extra `sload` operation (**100 additional extra gas (EIP-2929 2) for each iteration except for the first**),
2. if it is a memory array, this is an extra `mload` operation (3 additional gas for each iteration except for the first),
3. if it is a `calldata` array, this is an extra `calldataload` operation (3 additional gas for each iteration except for the first)

Avoid using storage variables to set the length of a for loop.

`function expensiveLoopLength {
  for (uint i = 0; i < array.length; i++) {}
}`

Instead assign it to a variable in memory to avoid unnecessary SLOADs.

`function cheapLoopLength {
  uint length = array.length;
  for (uint i = 0; i < length; i++) {}
}`

 Store the array’s length in a variable before the for-loop, and use it instead:

The overheads outlined below are *PER LOOP*, excluding the first loop

- storage arrays incur a Gwarmaccess (**100 gas**)
- memory arrays use `MLOAD` (**3 gas**)
- calldata arrays use `CALLDATALOAD` (**3 gas**)

## Instances -

```solidity
File: contracts\HolographOperator.sol

871:     for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

```solidity
File: contracts\enforcer\HolographERC20.sol
564:     for (uint256 i = 0; i < wallets.length; i++) {
```

```solidity
File: contracts\enforcer\PA1D.sol
437:     for (uint256 i = 0; i < addresses.length; i++) {
454:     for (uint256 i = 0; i < addresses.length; i++) {
474:     for (uint256 i = 0; i < addresses.length; i++) {
```

# [G-04] **Splitting require() statements that use &&**

Instead of using the && operator in a single require statement 
to check multiple conditions,using multiple require statements with 1 
condition per require statement will save **8 GAS per &&**

The gas difference would only be realized if the revert condition is realized(met).

## Instances -

```solidity
File: contracts\HolographOperator.sol

857:     require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

```solidity
File: contracts\enforcer\Holographer.sol

166:     require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

```solidity
File: contracts\enforcer\HolographERC721.sol

263:     require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
```

# [G-05] It costs more gas to initialize variables with their default value than letting the default value be applied

If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < numIterations; ++i) {` should be replaced with `for (uint256 i; i < numIterations; ++i) {`

## Instances -

```solidity
File: contracts\HolographOperator.sol

781:     for (uint256 i = 0; i < length; i++) {
```

```solidity
File: contracts\enforcer\HolographERC20.sol

564:     for (uint256 i = 0; i < wallets.length; i++) {
```

```solidity
File: contracts\enforcer\HolographERC721.sol

357:     for (uint256 i = 0; i < length; i++) {
716:     for (uint256 i = 0; i < length; i++) {
```

```solidity
File: contracts\enforcer\PA1D.sol

307:     for (uint256 i = 0; i < length; i++) {
323:     for (uint256 i = 0; i < length; i++) {
340:     for (uint256 i = 0; i < length; i++) {
356:     for (uint256 i = 0; i < length; i++) {
394:     for (uint256 i = 0; i < length; i++) {
414:     for (uint256 i = 0; i < length; i++) {
437:     for (uint256 i = 0; i < addresses.length; i++) {
454:     for (uint256 i = 0; i < addresses.length; i++) {
474:     for (uint256 i = 0; i < addresses.length; i++) {
```

# [G-06] **Bytes constant are cheaper than string constants**

If the string can fit into 32 bytes, then `bytes32` is cheaper than `string` . `string`is a dynamically sized-type, which has current limitations in Solidity compared to a statically sized variable.

## Instances -

```solidity
File: contracts\enforcer\PA1D.sol
142:   string constant _bpString = "eip1967.Holograph.PA1D.bp";
143:   string constant _receiverString = "eip1967.Holograph.PA1D.receiver";
144:   string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";
```