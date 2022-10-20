# 1. [G-1] For loops: ++i/--i  cost less gas compare to i++/i--

Instances include:

    File contracts/HolographOperator.sol, line 781:       for (uint256 i = 0; i < length; i++) {
    File contracts/HolographOperator.sol, line 871:       for (uint256 i = _operatorPods.length; i <= pod; i++) { 
    File contracts/HolographOperator.sol, line 520:       podSize--; // I suggest using `--podSize;` instead
    File contracts/HolographOperator.sol, line 760:       pod--; // I suggest using `--pod;` instead

    File contracts/enforcer/HolographERC20.sol, line 564:       for (uint256 i = 0; i < wallets.length; i++) {
    File contracts/enforcer/HolographERC20.sol, line 713:       _nonces[account]++; // I suggest using `++_nonces[account]` instead

    File contracts/enforcer/HolographERC721.sol, line 357:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/HolographERC721.sol, line 761:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/HolographERC721.sol, line 779:       _ownedTokensCount[to]++; // I suggest using `++_ownedTokensCount[to];` instead
    File contracts/enforcer/HolographERC721.sol, line 842:       _ownedTokensCount[from]--; // I suggest using `--_ownedTokensCount[from];` instead          

    File contracts/enforcer/PA1D.sol, line 307:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 323:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 340:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 356:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 394:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 432:       for (uint256 t = 0; t < tokenAddresses.length; t++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 454:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 474:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {

I suggest using `++i` instead of `i++` to increment the value of an uint variable.

# 2. [G-2] For loops: Cache the length of arrays in the loops to save gas

    File contracts/HolographOperator.sol, line 871:       for (uint256 i = _operatorPods.length; i <= pod; i++) { 

    File contracts/enforcer/HolographERC20.sol, line 564:       for (uint256 i = 0; i < wallets.length; i++) {

    File contracts/enforcer/PA1D.sol, line 432:       for (uint256 t = 0; t < tokenAddresses.length; t++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 454:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 474:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {

I suggest storing the array’s length in a variable before the for-loop, and use it instead.

# 3. [G-3] For loops: increments in for loop can be uncheck to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

    File contracts/HolographOperator.sol, line 781:       for (uint256 i = 0; i < length; i++) {
    File contracts/HolographOperator.sol, line 871:       for (uint256 i = _operatorPods.length; i <= pod; i++) { 

    File contracts/enforcer/HolographERC20.sol, line 564:       for (uint256 i = 0; i < wallets.length; i++) {

    File contracts/enforcer/HolographERC721.sol, line 357:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/HolographERC721.sol, line 761:       for (uint256 i = 0; i < length; i++) {

    File contracts/enforcer/PA1D.sol, line 307:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 323:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 340:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 356:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 394:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 432:       for (uint256 t = 0; t < tokenAddresses.length; t++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 454:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 474:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {

The code would go from:

    for (uint256 i; i < numIterations; i++) { 
      ...
    }

to:

    for (uint256 i; i < numIterations;) { 
      ...
      unchecked { ++i; }  
    }

# 4. [G-4] Variables: No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Instances include:

    File contracts/HolographOperator.sol, line 310:       uint256 gasLimit = 0;
    File contracts/HolographOperator.sol, line 311:       uint256 gasPrice = 0;
    File contracts/HolographOperator.sol, line 781:       for (uint256 i = 0; i < length; i++) {

    File contracts/enforcer/HolographERC20.sol, line 564:       for (uint256 i = 0; i < wallets.length; i++) {

    File contracts/enforcer/HolographERC721.sol, line 357:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/HolographERC721.sol, line 761:       for (uint256 i = 0; i < length; i++) {

    File contracts/enforcer/PA1D.sol, line 307:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 323:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 340:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 356:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 394:       for (uint256 i = 0; i < length; i++) {
    File contracts/enforcer/PA1D.sol, line 432:       for (uint256 t = 0; t < tokenAddresses.length; t++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 454:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 474:       for (uint256 i = 0; i < addresses.length; i++) {
    File contracts/enforcer/PA1D.sol, line 437:       for (uint256 i = 0; i < addresses.length; i++) {

I suggest removing explicit initializations for default values.

# 5. [G-5] Parameter: If we are not modifying the passed parameter we should pass it as `calldata` because `calldata` is more gas efficient than `memory`.

I suggest using `calldata` instead of `memory` here:

    File contracts/HolographFactory.sol, line 143:       function init(bytes memory initPayload)
    File contracts/HolographFactory.sol, line 192:       function deployHolographableContract(
                                                                                                      DeploymentConfig memory config,
                                                                                                      Verification memory signature,
                                                                                                      address signer
                                                                                     )

    File contracts/HolographOperator.sol, line 240:       function init(bytes memory initPayload)

    File contracts/enforcer/HolographERC20.sol, line 218:       function init(bytes memory initPayload)
    File contracts/enforcer/HolographERC20.sol, line 496:       function safeTransfer(
                                                                                                      address recipient,
                                                                                                      uint256 amount,
                                                                                                      bytes memory data
                                                                                                 )
    File contracts/enforcer/HolographERC20.sol, line 520:       function safeTransferFrom(
                                                                                                      address account,
                                                                                                      address recipient,
                                                                                                      uint256 amount,
                                                                                                      bytes memory data
                                                                                                  )
    File contracts/enforcer/HolographERC20.sol, line 637:       function _checkOnERC20Received(
                                                                                                      address account,
                                                                                                      address recipient,
                                                                                                      uint256 amount,
                                                                                                      bytes memory data
                                                                                                 )

    File contracts/enforcer/HolographERC721.sol, line 238:       function init(bytes memory initPayload)
    File contracts/enforcer/HolographERC721.sol, line 452:       function safeTransferFrom(
                                                                                                      address from,
                                                                                                      address to,
                                                                                                      uint256 tokenId,
                                                                                                      bytes memory data
                                                                                                  )
    File contracts/enforcer/HolographERC721.sol, line 616:       function transferFrom(
                                                                                                      address from,
                                                                                                      address to,
                                                                                                      uint256 tokenId,
                                                                                                      bytes memory data
                                                                                                  )

    File contracts/enforcer/PA1D.sol, line 173:       function init(bytes memory initPayload)
    File contracts/enforcer/PA1D.sol, line 185:       function initPA1D(bytes memory initPayload)
    File contracts/enforcer/PA1D.sol, line 316:       function _setPayoutAddresses(address payable[] memory addresses)
    File contracts/enforcer/PA1D.sol, line 349:       function _setPayoutBps(uint256[] memory bps)
    File contracts/enforcer/PA1D.sol, line 365:       function _getTokenAddress(string memory tokenName)
    File contracts/enforcer/PA1D.sol, line 372:       function _setTokenAddress(string memory tokenName, address tokenAddress)
    File contracts/enforcer/PA1D.sol, line 426:       function _payoutTokens(address[] memory tokenAddresses)
    File contracts/enforcer/PA1D.sol, line 517:       function getTokensPayout(address[] memory tokenAddresses)

    File contracts/enforcer/Holographer.sol, line 147:       function init(bytes memory initPayload)

# 6. [G-6] Variables: Cache read variables in memory will save gas

    File contracts/HolographOperator.sol, line 781-782:       
        for (uint256 i = 0; i < length; i++) {
            operators[i] = _operatorPods[pod][index + i]; // I suggest create a memory variable of the storage variable `_operatorPods`, and use it instead.
        }

    File contracts/HolographOperator.sol, line 871:       
        for (uint256 i = _operatorPods.length; i <= pod; i++) {
            _operatorPods.push([address(0)]); // I suggest create a memory variable of the storage variable `_operatorPods`, and use it instead.
        }

    File contracts/enforcer/HolographERC721.sol, line 357:       
        for (uint256 i = 0; i < length; i++) {
            tokenIds[i] = _ownedTokens[wallet][index + i]; // I suggest create a memory variable of the storage variable `_ownedTokens`, and use it instead.
        }

    File contracts/enforcer/HolographERC721.sol, line 716:       
        for (uint256 i = 0; i < length; i++) {
            tokenIds[i] = _allTokens[index + i]; // I suggest create a memory variable of the storage variable `_allTokens`, and use it instead.
        }

# 7. [G-7] Arithmetics: uncheck blocks for arithmetics operations that can't underflow/overflow

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn't possible, some gas can be saved by using an `unchecked` block.

I suggest wrapping with an `unchecked` block here:

    File contracts/module/LayerZeroModule.sol, line 274:       return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
    File contracts/module/LayerZeroModule.sol, line 293:       return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);

    File contracts/HolographOperator.sol, line 337:       uint256 pod = job.pod - 1;
    File contracts/HolographOperator.sol, line 345-346:
                uint256 elapsedTime = block.timestamp - uint256(job.startTimestamp);
                uint256 timeDifference = elapsedTime / job.blockTimes;
    File contracts/HolographOperator.sol, line 378:       _bondedAmounts[job.operator] -= amount;
    File contracts/HolographOperator.sol, line 382:       _bondedAmounts[msg.sender] += amount;
    File contracts/HolographOperator.sol, line 391:       _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
    File contracts/HolographOperator.sol, line 408:       _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
    File contracts/HolographOperator.sol, line 503:       uint256 pod = random % _operatorPods.length;
    File contracts/HolographOperator.sol, line 511:       uint256 operatorIndex = random % podSize;
    File contracts/HolographOperator.sol, line 768-773:  
                if (index + length > supply) {
                      /**
                      * @dev adjust length to return remainder of the results
                      */
                      length = supply - index;
                 }
    File contracts/HolographOperator.sol, line 834:       _bondedAmounts[operator] += amount;
    File contracts/HolographOperator.sol, line 883:       _operatorPodIndex[operator] = _operatorPods[pod - 1].length - 1;
    File contracts/HolographOperator.sol, line 1137:       uint256 lastIndex = _operatorPods[pod].length - 1;
    File contracts/HolographOperator.sol, line 1161:       return (_podMultiplier**pod) * _baseBondAmount;
    File contracts/HolographOperator.sol, line 1168:       uint256 current = (_podMultiplier**pod) * _baseBondAmount;
    File contracts/HolographOperator.sol, line 1172:       uint256 threshold = _operatorThreshold / (2**pod);
    File contracts/HolographOperator.sol, line 1175-1177:       
                position -= threshold;
                current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
    File contracts/HolographOperator.sol, line 1191:       
                return (random + uint256(blockhash(block.number - n))) % podSize;

    File contracts/enforcer/HolographERC721.sol, line 354:        length = supply - index;
    File contracts/enforcer/HolographERC721.sol, line 713:        length = supply - index;
    File contracts/enforcer/HolographERC721.sol, line 779:        _ownedTokensCount[to]++;
    File contracts/enforcer/HolographERC721.sol, line 825:        uint256 lastTokenIndex = _allTokens.length - 1;
    File contracts/enforcer/HolographERC721.sol, line 842:        _ownedTokensCount[from]--;

    File contracts/enforcer/PA1D.sol, line 388:        uint256 gasCost = (23300 * length) + length;
    File contracts/enforcer/PA1D.sol, line 391:        balance = balance - gasCost;
    File contracts/enforcer/PA1D.sol, line 395:        sending = ((bps[i] * balance) / 10000);
    File contracts/enforcer/PA1D.sol, line 415:        sending = ((bps[i] * balance) / 10000);
    File contracts/enforcer/PA1D.sol, line 438:        sending = ((bps[i] * balance) / 10000);
    File contracts/enforcer/PA1D.sol, line 475:        totalBp = totalBp + bps[i];
    File contracts/enforcer/PA1D.sol, line 551:        return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
    File contracts/enforcer/PA1D.sol, line 553:        return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
    File contracts/enforcer/PA1D.sol, line 641:        return (_getDefaultBp() * amount) / 10000;
    File contracts/enforcer/PA1D.sol, line 643:        return (_getBp(tokenId) * amount) / 10000;