# QA ISSUES  FOR 2022-10-HOLOGRAPH

##  [L-01]  Empty receive and/or fallback function


./contracts/enforcer/HolographERC20.sol
```
L251:  receive() external payable {}
```

./contracts/enforcer/HolographERC721.sol
```
L962:  receive() external payable {}
```

./contracts/enforcer/Holographer.sol
```
L223:  receive() external payable {}
```

./contracts/abstract/ERC721H.sol
```
L212:  receive() external payable {}
```

./contracts/HolographOperator.sol
```
L1209: receive() external payable {}
```

##  [L-02]  Not emiting events in some important functions
Description: Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

./contracts/HolographOperator.sol
```
L240:  function init(bytes memory initPayload) external override returns (bytes4) {

L278:  function resetOperator(
```

./contracts/enforcer/HolographERC20.sol
```
L218:  function init(bytes memory initPayload) external override returns (bytes4) {
```

##  [N-03]  Adding a `return` statement when the function defines a named return variable, is redundant


./contracts/enforcer/PA1D.sol
```
L665:  function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
L674:  return bidShares;
```

##  [N-04]  Use 1e18 instead of 10**18 or big integers


./contracts/HolographOperator.sol
```
L256:  _baseBondAmount = 100 * (10**18);
```

##  [N-05]  Events should have 3 `indexed` fields where possible


./contracts/enforcer/PA1D.sol
```
L153:  event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```

##  [N-06]  Unused import line


./contracts/enforcer/HolographERC721.sol
```
L123:  import "../interface/PA1DInterface.sol";
```

./contracts/enforcer/HolographERC20.sol
```
L114:  import "../interface/ERC20Burnable.sol";

L116:  import "../interface/ERC20Metadata.sol";

L119:  import "../interface/ERC20Safer.sol";

L125:  import "../interface/HolographRegistryInterface.sol";
```

./contracts/enforcer/PA1D.sol
```
L110:  import "../interface/PA1DInterface.sol";
```

