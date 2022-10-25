
# GAS


## use of custom errors rather than revert() / require() error message
### Summary
Custom errors reduce 38 gas if the condition is met and 22 gas otherwise.
Also reduces contract size and deployment costs.
### Details
Since version 0.8.4 the use of custom errors rather than revert() / require() saves gas as noticed in
https://blog.soliditylang.org/2021/04/21/custom-errors/
https://github.com/code-423n4/2022-04-pooltogether-findings/issues/13

### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L148
    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L163
    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L203
    require(

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L214
    require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L224
      require(

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L233
    require(doNotRevert, "HOLOGRAPH: reverted");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L255
    require(

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L270
    require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L352
    require(

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L241
    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L309
    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L350
        require(timeDifference > 0, "HOLOGRAPH: operator has time");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L354
        require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L368
            require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L415
    require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L446
    require(msg.sender == address(this), "HOLOGRAPH: operator only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L485
    require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L591
    require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L595
    require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L728
    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L739
    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L756
    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L829
    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L839
    require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L857
    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L863
      require(current <= amount, "HOLOGRAPH: bond amount too small");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L881
      require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L889
      require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L903
    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L911
      require(_isContract(operator), "HOLOGRAPH: operator not contract");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L915
      require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L932
    require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L144
    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L220
    require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L228
    require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L250
    require(

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L159
    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L235
    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L148
    require(!_isInitialized(), "HOLOGRAPHER: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L166
    require(success && selector == InitializableInterface.init.selector, "initialization failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L159
    require(isOwner(), "PA1D: caller not an owner");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L174
    require(!_isInitialized(), "PA1D: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L190
    require(initialized == 0, "PA1D: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L390
    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L411
    require(balance > 10000, "PA1D: Not enough tokens to transfer");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L416
      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L435
      require(balance > 10000, "PA1D: Not enough tokens to transfer");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L439
        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L460
      require(matched, "PA1D: sender not authorized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L472
    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L477
    require(totalBp == 10000, "PA1D: bps down't equal 10000");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L212
    require(msg.sender == _holograph().getBridge(), "ERC721
 bridge only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L224
    require(msg.sender == sourceContract, "ERC721
 source only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L239
    require(!_isInitialized(), "ERC721
 already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L258
      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721
 could not init source");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L263
      require(success && selector == InitializableInterface.init.selector, "ERC721
 coud not init PA1D");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L323
    require(_exists(tokenId), "ERC721
 token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L370
    require(to != tokenOwner, "ERC721
 cannot approve self");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L371
    require(_isApproved(msg.sender, tokenId), "ERC721
 not approved sender");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L388
    require(_isApproved(msg.sender, tokenId), "ERC721
 not approved sender");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L464
      require(

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L404
    require(!_exists(tokenId), "ERC721
 token already exists");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L408
      require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L419
    require(to != address(0), "ERC721
 zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L420
    require(_isApproved(sender, tokenId), "ERC721
 sender not approved");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L421
    require(from == _tokenOwner[tokenId], "ERC721
 from is not owner");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L458
    require(_isApproved(msg.sender, tokenId), "ERC721
 not approved sender");



https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L484
    require(to != msg.sender, "ERC721
 cannot approve self");




https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L513
    require(!_burnedTokens[token], "ERC721
 can't mint burned token");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L622
    require(_isApproved(msg.sender, tokenId), "ERC721
 not approved sender");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L639
    require(wallet != address(0), "ERC721
 zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L689
    require(tokenOwner != address(0), "ERC721
 token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L700
    require(index < _allTokens.length, "ERC721
 index out of bounds");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L729
    require(index < balanceOf(wallet), "ERC721
 index out of bounds");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L757
    require(_isContract(_operator), "ERC721
 operator not contract");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L762
      require(tokenOwner == address(this), "ERC721
 contract not token owner");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L815
    require(tokenId > 0, "ERC721
 token id cannot be zero");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L816
    require(to != address(0), "ERC721
 minting to burn address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L817
    require(!_exists(tokenId), "ERC721
 token already exists");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L818
    require(!_burnedTokens[tokenId], "ERC721
 token has been burned");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L869
    require(_tokenOwner[tokenId] == from, "ERC721
 token not owned");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L870
    require(to != address(0), "ERC721
 use burn instead");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L906
    require(_exists(tokenId), "ERC721
 token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L192
    require(msg.sender == _holograph().getBridge(), "ERC20
 bridge only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L204
    require(msg.sender == sourceContract, "ERC20
 source only call");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L219
    require(!_isInitialized(), "ERC20
 already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L241
      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20
 could not init source");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L349
    require(currentAllowance >= amount, "ERC20
 amount exceeds allowance");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L365
    require(currentAllowance >= subtractedValue, "ERC20
 decreased below zero");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L387
      require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L400
      require(currentAllowance >= amount, "ERC20
 amount exceeds allowance");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L427
      require(newAllowance >= currentAllowance, "ERC20
 increased above max value");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L445
    require(_isContract(account), "ERC20
 operator not contract");
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L450
      require(balance >= amount, "ERC20
 balance check failed");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L469
    require(block.timestamp <= deadline, "ERC20
 expired deadline");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L482
    require(signer == account, "ERC20
 invalid signature");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L505
    require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20
 non ERC20Receiver");

        require(currentAllowance >= amount, "ERC20
 amount exceeds allowance");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L539
    require(_checkOnERC20Received(account, recipient, amount, data), "ERC20
 non ERC20Receiver");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L599
        require(currentAllowance >= amount, "ERC20
 amount exceeds allowance");


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L620
    require(account != address(0), "ERC20
 account is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L621
    require(spender != address(0), "ERC20
 spender is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L627
    require(account != address(0), "ERC20
 account is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L629
    require(accountBalance >= amount, "ERC20
 amount exceeds balance");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L645
        require(erc165support, "ERC20
 no ERC165 support");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L684
    require(to != address(0), "ERC20
 minting to burn address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L695
    require(account != address(0), "ERC20
 account is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L696
    require(recipient != address(0), "ERC20
 recipient is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L698
    require(accountBalance >= amount, "ERC20
 amount exceeds balance");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L117
    require(msg.sender == holographer(), "ERC721
 holographer only");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L123
      require(msgSender() == _getOwner(), "ERC721
 owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L125
      require(msg.sender == _getOwner(), "ERC721
 owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L147
    require(!_isInitialized(), "ERC721
 already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L117
    require(msg.sender == holographer(), "ERC20
 holographer only");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L123
      require(msgSender() == _getOwner(), "ERC20
 owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L125
      require(msg.sender == _getOwner(), "ERC20
 owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L147
    require(!_isInitialized(), "ERC20
 already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L764
      revert("ERC721
 token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L452
      revert("ERC20
 failed getting balance");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L653
              revert("ERC20
 non ERC20Receiver");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L661
          revert("ERC20
 eip-4524 not supported");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L665
          revert("ERC20
 no ERC165 support");

### Mitigation
replace each error message in a require by a custom error


## splitting `require()` statements that use `&&` saves gas
### Summary
Instead of using the && operator in a single require statement to check multiple conditions, consider using multiple require statements with 1 condition per require statement (saving 3 gas per & ):
### Github Permalinks


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L857
    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L166
    require(success && selector == InitializableInterface.init.selector, "initialization failed");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L263
      require(success && selector == InitializableInterface.init.selector, "ERC721
 coud not init PA1D");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L465
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L466
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&

### Mitigation
Split require statements



## usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
### Summary
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

### Details
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html 
Use a larger size than downcast where needed

### Github Permalinks


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L181
  uint8 private _decimals;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L181
  uint8 private _decimals;



### Mitigation
Consider using some data type that uses 32 bytes, for example uint256


## Store using Struct over multiple mappings
### Summary
All these variables could be combine in a Struct in order to reduce the gas cost. 
### Details
As noticed in: 
https://gist.github.com/alexon1234/b101e3ac51bea3cbd9cf06f80eaa5bc2
When multiple mappings that access the same addresses, uints, etc, all of them can be mixed into an struct and then that data accessed like:
mapping(datatype => newStructCreated) newStructMap;
Also, you have this post where it explains the benefits of using Structs over mappings 
https://medium.com/@novablitz/storing-structs-is-costing-you-gas-774da988895e

### Github Permalinks

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L193
  mapping(bytes32 => uint256) private _operatorJobs;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L198
  mapping(bytes32 => bool) private _failedJobs;
- - - - -

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L218
  mapping(address => uint256) private _bondedOperators;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L223
  mapping(address => uint256) private _operatorPodIndex;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L228
  mapping(address => uint256) private _bondedAmounts;
- - - - -

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L170
  mapping(uint256 => uint256) private _ownedTokensIndex;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L175
  mapping(uint256 => address) private _tokenOwner;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L180
  mapping(uint256 => address) private _tokenApprovals;


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L201
  mapping(uint256 => uint256) private _allTokensIndex;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L206
  mapping(uint256 => bool) private _burnedTokens;

- - - - -

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L185
  mapping(address => uint256) private _ownedTokensCount;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L190
  mapping(address => uint256[]) private _ownedTokens;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L196
  mapping(address => mapping(address => bool)) private _operatorApprovals;
- - - - -

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L156
  mapping(address => uint256) private _balances;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L161
  mapping(address => mapping(address => uint256)) private _allowances;

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L186
  mapping(address => uint256) private _nonces;


### Mitigation
Consider mixing different mappings into an struct when able in order to save gas.

## --i costs less gas compared to i-- or i-=1
### Summary
--i costs less gas compared to i-- or i -= 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). 
This statement is true even with the optimizer enabled. 

### Details
i-- decrements i and returns the initial value of i . 
Which means:
```
uint i = 1;
i--; // == 1 but i == 2
```

But --i returns the actual decremented value:

```
uint i = 1;
--i; // == 2 and i == 2 too, so no need for a temporary variable
```

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

### Github Permalinks
`--` 
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L520

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L760

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L842

### Mitigation
Replace to `--i` as needed.

## Functions guaranteed to revert when called by normal users can be marked `payable`
### Summary
If a function modifier such as onlyAdmin is used, the function will revert if a normal user tries to pay the function.

Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Details
The extra opcodes avoided are:
CALLVALUE (2), DUP1 (3), ISZERO (3), PUSH2 (3), JUMPI (10), PUSH1 (3), DUP1 (3), REVERT(0), JUMPDEST (1), POP (2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

### Github Permalinks

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L452
  function setFactory(address factory) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L472
  function setHolograph(address holograph) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L502
  function setOperator(address operator) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L522
  function setRegistry(address registry) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L285
  ) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L949
  function setBridge(address bridge) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L969
  function setHolograph(address holograph) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L989
  function setInterfaces(address interfaces) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1009
  function setMessagingModule(address messagingModule) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1029
  function setRegistry(address registry) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1049
  function setUtilityToken(address utilityToken) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L280
  function setHolograph(address holograph) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L300
  function setRegistry(address registry) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L320
  function setBridge(address bridge) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L340
  function setInterfaces(address interfaces) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L360
  function setLZEndpoint(address lZEndpoint) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L380
  function setOperator(address operator) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L441
  function setBaseGas(uint256 baseGas) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L470
  function setGasPerByte(uint256 gasPerByte) external onlyAdmin {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L209
      msg.sender == Admin(address(this)).getAdmin());



### Mitigation
Consider adding payable to save gas


## `>=` cheaper than `>`
### Summary
Strict inequalities ( `>` ) are more expensive than non-strict ones ( `>=` ). This is due to some supplementary checks (`ISZERO`, 3 gas)

### Github Permalinks


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L218
    if (hTokenValue > 0) {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L309
    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L350
        require(timeDifference > 0, "HOLOGRAPH: operator has time");

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L363
          if (podIndex > 0 && podIndex < _operatorPods[pod].length) {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L398
          if (leftovers > 0) {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1126
    if (operatorIndex > 0) {

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L815
    require(tokenId > 0, "ERC721
 token id cannot be zero");


### Mitigation
Consider using `>= 1` instead of `> 0` to avoid some opcodes



## `<X> += <Y>` costs more gas than `<X> = <X> + <Y>` for state variables
### Summary
`x+=y` costs more gas than x=x+y for state variables

### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L382
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L834
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L685-L686
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L702
### Mitigation
Don't use `+=` for state variables as it cost more gas.




## Unused named returns
### Summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 
### Details
Also as returns variable is ignored, it wastes extra gas

### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L301

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L181

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L717-L719

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L804-L806

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L814-L816

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L296-L304

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L256

### Mitigation
Remove return or returns when both used


## `abi.encode()` is less gas efficient than `abi.encodePacked()`
### Summary
In general, `abi.encodePacked` is more gas-efficient.

### Details
Changing the abi.encode function to `abi.encodePacked` can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.
### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L260
        abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L426
    return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L409
    return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));


### Mitigation
Consider changing abi.encode to `abi.encodePacked`

