
# Summary

There is not correct validation against address(0) for `sender` parameter at `bridgeOut()` function.

# Contract in scope

HolographERC721.sol

# Issue detail

Bridge caller to `bridgeOut()` function can call mentioned function with address(0) for sender parameter. It is because there is a validation which returns true in case of address(0) parameter:
`require(_isApproved(sender, tokenId), "ERC721: sender not approved"); `

# Suggestion

modify `_isApproved` function to revert in case spender parameter is equals to address(0):
` 
    function _isApproved(address spender, uint256 tokenId) private view returns (bool) {
    
    require(spender != address(0), "Spender cannot be null address");
    require(_exists(tokenId), "ERC721: token does not exist");
    address tokenOwner = _tokenOwner[tokenId];
    return (spender == tokenOwner || _tokenApprovals[tokenId] == spender || _operatorApprovals[tokenOwner][spender]);
  }

`

***********************************************************************
***********************************************************************
***********************************************************************

# Summary

Approve for address(0) is a valid scenario for `approve()` function.

# Contract in scope

HolographERC721.sol

# Issue detail

It doesn't look to create some mistake in the protocol but I suggest to add this validation to avoid wasting gas.

# Suggestion

modify `approve` function to revert in case `to` parameter is equals to address(0):

`   
    function approve(address to, uint256 tokenId) external payable {
    
    require(to != address(0), "spender cannot be null address");
    address tokenOwner = _tokenOwner[tokenId];
    require(to != tokenOwner, "ERC721: cannot approve self");
    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
    if (_isEventRegistered(HolographERC721Event.beforeApprove)) {
      require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));
    }
    _tokenApprovals[tokenId] = to;
    emit Approval(tokenOwner, to, tokenId);
    if (_isEventRegistered(HolographERC721Event.afterApprove)) {
      require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
    }
  }

`

***********************************************************************
***********************************************************************
***********************************************************************

# Summary

There is not length check in `sourceMintBatch()` function.

# Contract in scope

HolographERC20.sol

# Issue detail
`sourceMintBatch()` function raises error when length's `wallets` is higher than `amounts`. 
Also, the process will finish ok when sending more `amounts`  than `wallets`. I think it is more an user responsibility but, it could be a reason to raise this issue as medium because this will finish ok but not all the desired wallets were provided to the method. So, a research should be placed to know what happened with missing wallets.

# Suggestion

modify `sourceMintBatch` function to revert in case array's length are not equals:
`

  function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
    require(wallets.length == wallets.length, "Array's length are not equals");
    for (uint256 i = 0; i < wallets.length; i++) {
      _mint(wallets[i], amounts[i]);
    }
  }


`

***********************************************************************
***********************************************************************
***********************************************************************

# Summary

There is not input check in `getPodOperatorsLength()` and `getPodOperators()` functions.

# Contract in scope

HolographERC20.sol

# Issue detail
`getPodOperatorsLength()` and `getPodOperators()` functions raise underflow error when length's `_operatorPods` and `pods` parameter are equals to 0. 

# Suggestion

modify `getPodOperatorsLength()` and `getPodOperators()` functions to revert in case `_operatorPods.length` is equals to 0 and keep existing validation:
`

  function getPodOperatorsLength(uint256 pod) external view returns (uint256) {
    require(_operatorPods.length != 0 && _operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
    return _operatorPods[pod - 1].length;
  }


`