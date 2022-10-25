## [L1] Unused/Empty receive() function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

#### Instances 
`4` found
```
ERC721H.sol::212 => receive() external payable {}
enforcer/Holographer.sol::223 => receive() external payable {}
enforcer/HolographERC20.sol::251 => receive() external payable {}
enforcer/HolographERC721.sol::962 => receive() external payable {}
```

## [L2] Missing checks for address(0X0) in functions that are public

contracts/enforcer/PA1D.sol::507

```
function getTokenPayout(address tokenAddress) public {
    _validatePayoutRequestor();
    _payoutToken(tokenAddress);
  }
```

function `payoutToken` never checks for address passed is zero address

```
  /**
   * @dev Internal function that transfers tokens to all payout recipients.
   * @param tokenAddress Smart contract address of ERC20 token.
   */
  function _payoutToken(address tokenAddress) private {
    address payable[] memory addresses = _getPayoutAddresses();
    uint256[] memory bps = _getPayoutBps();
    uint256 length = addresses.length;
    ERC20 erc20 = ERC20(tokenAddress);
    uint256 balance = erc20.balanceOf(address(this));
    require(balance > 10000, "PA1D: Not enough tokens to transfer");
    uint256 sending;
    //uint256 sent;
    for (uint256 i = 0; i < length; i++) {
      sending = ((bps[i] * balance) / 10000);
      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
      // sent = sent + sending;
    }
  }
```

contracts/enforcer/PA1D.sol::529
```
function setRoyalties(
    uint256 tokenId,
    address payable receiver,
    uint256 bp
  ) public onlyOwner {
```


## [N1] Incorrect Comment

enforcer/PA1D.sol::386

The comment `// accommodating the 2300 gas stipend` `// adding 1x for each item in array to accomodate rounding errors`
But code looks like
```
uint256 gasCost = (23300 * length) + length;
```