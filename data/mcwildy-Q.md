# Qa Report

## HOLOGRAPH

### Use `external` visibility modifier for function that are not being invoked by the contract

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L599: function transferFrom(

line#L616: function transferFrom(
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L471: function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

line#L488: function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {

line#L507: function getTokenPayout(address tokenAddress) public {

line#L517: function getTokensPayout(address[] memory tokenAddresses) public {

line#L558: function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {

line#L569: function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {

line#L604: function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

line#L621: function tokenCreator(

line#L649: function marketContract() public view returns (address) {

line#L655: function tokenCreators(uint256 tokenId) public view returns (address) {
```

[Holographer.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol):

```solidity
line#L192: function getHolographEnforcer() public view returns (address) {
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L273: function decimals() public view returns (uint8) {

line#L297: function allowance(address account, address spender) public view returns (uint256) {

line#L310: function name() public view returns (string memory) {

line#L314: function nonces(address account) public view returns (uint256) {

line#L322: function totalSupply() public view returns (uint256) {

line#L363: function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {

line#L460: function permit(
```

### Events should be emmitted on every critical state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L240: function init(bytes memory initPayload) external override returns (bytes4) {

line#L278: function resetOperator(
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L218: function init(bytes memory initPayload) external override returns (bytes4) {
```

### Unused receive/fallback function

[ERC721H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol):

```solidity
line#L212: receive() external payable {}
```

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L251: receive() external payable {}
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L962: receive() external payable {}
```

[Holographer.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol):

```solidity
line#L223: receive() external payable {}
```

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L1209: receive() external payable {}
```

[ERC20H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol):

```solidity
line#L212: receive() external payable {}
```

### Use 1e<decimals> for readability (e.g. `1e18`)

[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol):

```solidity
line#L256: _baseBondAmount = 100 * (10**18);
```

### Do not explicitly `return` from a function if it already has named declared return variables

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L674: return bidShares;
```

### Unused import statements

[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol):

```solidity
line#L114: import "../interface/ERC20Burnable.sol";

line#L116: import "../interface/ERC20Metadata.sol";

line#L124: import "../interface/HolographerInterface.sol";

line#L125: import "../interface/HolographRegistryInterface.sol";
```

[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol):

```solidity
line#L123: import "../interface/PA1DInterface.sol";
```

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L110: import "../interface/PA1DInterface.sol";
```

### Events should use all three `indexed` keywords for their parameters

[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol):

```solidity
line#L153: event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```
