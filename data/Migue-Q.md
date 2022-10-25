
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