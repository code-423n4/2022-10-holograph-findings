# Gas report

| G-N    |Issue|Instances|
|:------:|:----|:-------:|
| [G&#x2011;01] | Return directly the condition | 4 |
| [G&#x2011;02] | Cache `_getReceiver(tokenId)` in a local var | 4 |
| [G&#x2011;03] | Unnecessary casts | 3 |
| [G&#x2011;04] | Unnecessary checks | 3 |
| [G&#x2011;05] | `public` functions to `external` functions | 4 |
| [G&#x2011;06] | Duplicate code | 1 |
| [G&#x2011;07] | Duplicate contract | 1 |

## [G-01] Return directly the condition

Instead return `true`/`false` return the `condition`:
- `return <CONDITION> ? true : false` => `return <CONDITION>`
- `if (<CONDITION>) { return true; } else { return false; }` => `return <CONDITION>`

This save GAS and improve the code quality

```solidity
File: /contracts/enforcer/HolographERC20.sol

288    if (
289      interfaces.supportsInterface(InterfaceType.ERC20, interfaceId) || erc165Contract.supportsInterface(interfaceId) // check global interfaces // check if source supports interface
290    ) {
291      return true;
292    } else {
293      return false;
294    }

755    return ((_eventConfig >> uint256(_eventName)) & uint256(1) == 1 ? true : false);
```

```solidity
File: /contracts/enforcer/HolographERC721.sol

 298    if (
 299      interfaces.supportsInterface(InterfaceType.ERC721, interfaceId) || // check global interfaces
 301      interfaces.supportsInterface(InterfaceType.PA1D, interfaceId) || // check if royalties supports interface
 302      erc165Contract.supportsInterface(interfaceId) // check if source supports interface
 303    ) {
 304      return true;
 305    } else {
 306      return false;
 307    }

1003    return ((_eventConfig >> uint256(_eventName)) & uint256(1) == 1 ? true : false);
```

## [G-02] Cache `_getReceiver(tokenId)` in a local var

```solidity
File: /contracts/enforcer/PA1D.sol

/// @audit: instance 1
550    if (_getReceiver(tokenId) == address(0)) {
553      return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);

/// @audit: instance 2
571    if (_getReceiver(tokenId) == address(0)) {
574      receivers[0] = _getReceiver(tokenId);

/// @audit: instance 3
593    if (_getReceiver(tokenId) == address(0)) {
597      receivers[0] = _getReceiver(tokenId);

/// @audit: instance 4
607    if (_getReceiver(tokenId) == address(0)) {
611      receivers[0] = _getReceiver(tokenId);
```

## [G-03] Unnecessary casts

```solidity
File: /contracts/enforcer/HolographERC721.sol

/// @audit: remove payable
879    uint32 currentChain = HolographInterface(HolographerInterface(payable(address(this))).getHolograph())

/// @audit: remove payable
881    if (currentChain != HolographerInterface(payable(address(this))).getOriginChain()) {

884    return uint32(0);
```

## [G-04] Unnecessary checks

```solidity
File: /contracts/enforcer/HolographERC721.sol

/// @audit: checked in L817(inside _mint function)
404    require(!_exists(tokenId), "ERC721: token already exists");

/// @audit: It's a view function, all in the EVM it's public
520  function sourceGetChainPrepend() external view onlySource returns (uint256) {

/// @audit: checked in L818(inside _mint function)
513    require(!_burnedTokens[token], "ERC721: can't mint burned token");
```

## [G-05] `public` functions to `external` functions

```solidity
File: /contracts/enforcer/HolographERC721.sol

621  ) public payable {

643  function burned(uint256 tokenId) public view returns (bool) {

656  function exists(uint256 tokenId) public view returns (bool) {

935  function owner() public view override returns (address) {
```

```solidity
File: /contracts/enforcer/PA1D.sol

471  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

488  function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {

497  function getEthPayout() public {

507  function getTokenPayout(address tokenAddress) public {

517  function getTokensPayout(address[] memory tokenAddresses) public {
```

## [G-06] Duplicate code

The code of `getFees` and `getRoyalties` functions are exactly the same, move this code to an internal function

```solidity
File: /contracts/enforcer/PA1D.sol

590  function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

604  function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```

## [G-07] Duplicate contract

The code of `ERC721H` and `ERC20H` contracts are exactly the same, only change the error messages
Use only one implementation
