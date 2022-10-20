# 2022-10-HOLOGRAPH
## Low Risk And Non-Critical Issues

### Adding a `return` statement when the function defines a named return variable, is redundant


_There are **1** instances of this issue:_

```solidity
File: contracts/enforcer/PA1D.sol

665:  function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
674:  return bidShares;
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Events not emmited on important state changes
Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **3** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

240:  function init(bytes memory initPayload) external override returns (bytes4) {

278:  function resetOperator(
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

218:  function init(bytes memory initPayload) external override returns (bytes4) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

### `public` functions not called by the contract should be declared `external` instead


_There are **28** instances of this issue:_

```solidity
File: contracts/enforcer/HolographERC20.sol

273:  function decimals() public view returns (uint8) {

297:  function allowance(address account, address spender) public view returns (uint256) {

306:  function DOMAIN_SEPARATOR() public view returns (bytes32) {

310:  function name() public view returns (string memory) {

314:  function nonces(address account) public view returns (uint256) {

318:  function symbol() public view returns (string memory) {

322:  function totalSupply() public view returns (uint256) {

363:  function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {

420:  function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {

460:  function permit(
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

599:  function transferFrom(

616:  function transferFrom(
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/Holographer.sol

192:  function getHolographEnforcer() public view returns (address) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

```solidity
File: contracts/enforcer/PA1D.sol

471:  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

488:  function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {

497:  function getEthPayout() public {

507:  function getTokenPayout(address tokenAddress) public {

517:  function getTokensPayout(address[] memory tokenAddresses) public {

549:  function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {

558:  function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {

569:  function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {

590:  function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

604:  function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

621:  function tokenCreator(

634:  function calculateRoyaltyFee(

649:  function marketContract() public view returns (address) {

655:  function tokenCreators(uint256 tokenId) public view returns (address) {

665:  function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Empty `receive()`/`fallback()` functions


_There are **6** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

1209: receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```solidity
File: contracts/abstract/ERC20H.sol

212:  receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol

```solidity
File: contracts/abstract/ERC721H.sol

212:  receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol

```solidity
File: contracts/enforcer/HolographERC20.sol

251:  receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

962:  receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/Holographer.sol

223:  receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

### Use `1e18` instead of `10**18` or `1000000000000000000`


_There are **1** instances of this issue:_

```solidity
File: contracts/HolographOperator.sol

256:  _baseBondAmount = 100 * (10**18);
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

### Event is missing `indexed` fields

_There are **1** instances of this issue:_

```solidity
File: contracts/enforcer/PA1D.sol

153:  event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

### Not used import


_There are **7** instances of this issue:_

```solidity
File: contracts/enforcer/HolographERC20.sol

114:  import "../interface/ERC20Burnable.sol";

116:  import "../interface/ERC20Metadata.sol";

119:  import "../interface/ERC20Safer.sol";

124:  import "../interface/HolographerInterface.sol";

125:  import "../interface/HolographRegistryInterface.sol";
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```solidity
File: contracts/enforcer/HolographERC721.sol

123:  import "../interface/PA1DInterface.sol";
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```solidity
File: contracts/enforcer/PA1D.sol

110:  import "../interface/PA1DInterface.sol";
```
https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

