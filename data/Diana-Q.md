
## L-01 EVENT IS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fields

### Proof of Concept

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L153

```
File: contracts/enforcer/PA1D.sol

153: event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```

----

## L-02 USE SAFETRANSFERFROM() INSTEAD OF TRANSFERFROM()

### Proof of Concept

_There are 4 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

589: transferFrom(msg.sender, to, tokenId, "");
604: transferFrom(from, to, tokenId, "");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
```

-------------
## L-03 _SAFEMINT()_ SHOULD BE USED RATHER THAN _MINT()_ WHEREVER POSSIBLE

`_mint()` is discouraged in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open OpenZeppelin and solmate have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

### Proof of Concept

_There are 2 instances of this issue

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
File: contracts/enforcer/HolographERC721.sol

406: _mint(to, tokenId);
514: _mint(to, token);
```

------------

## N-01 OPEN TODOS

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701

```
File: contracts/HolographOperator.sol

701: // TODO: move the bit-shifting around to have it be sequential
```
----------

## N-02 NATSPEC IS INCOMPLETE

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

_There are several instances of this issue throughout the in scope contracts_

For example: 

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1109-L1111

```
File: contracts/HolographOperator.sol

/**
   * @dev Internal nonce, that increments on each call, used for randomness
   */
```

Missing: `@param jobNonce`

--------------
## N-03 MISSING EVENTS
 
_There are 3 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

949: function setBridge(address bridge) external onlyAdmin {
969: function setHolograph(address holograph) external onlyAdmin {
1009: function setMessagingModule(address messagingModule) external onlyAdmin {
```
-----

## N-04 MISSING ZERO-ADDRESS CHECK IN SETTER FUNCTIONS

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
File: contracts/HolographOperator.sol

949: function setBridge(address bridge) external onlyAdmin {
969: function setHolograph(address holograph) external onlyAdmin {
1009: function setMessagingModule(address messagingModule) external onlyAdmin {
```

### Recommended Mitigation Steps

Consider adding zero-address checks in the discussed functions

----------