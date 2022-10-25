1:
UNSAFE USAGE OF ERC20 TRANSFER

Some ERC20 tokens functions don’t return a boolean, for example USDT, BNB, OMG. So the Multiswap contract simply won’t work with tokens like that as the token.

The USDT’s transfer and transferFrom functions doesn’t return a bool, so the call to these functions will revert although the user has enough balance and the PA1D contract won’t work, assuming that token is USDT.

```
  function _payoutToken(address tokenAddress) private {
...
    for (uint256 i = 0; i < length; i++) {

      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token"); /**** UNSAFE USAGE OF ERC20 TRANSFER **/

    }
  }
```

Suggestions:
Use the OpenZepplin’s safeTransfer and safeTransferFrom functions.


L2:
configurePayouts() Add the judgment that payaddress cannot be address(0);
PA1D.sol
```
  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

    for (uint256 i = 0; i < addresses.length; i++) {
+     require(addresses[i]!=address(0));
      totalBp = totalBp + bps[i];
    }
...
  }
```



L-03:
Variable naming error

```
- function getDeploymentBlock() external view returns (address holograph) {
+ function getDeploymentBlock() external view returns (address blockHeight) {
    assembly {
-     holograph := sload(_blockHeightSlot)
+     blockHeight := sload(_blockHeightSlot)
    }
  }
```