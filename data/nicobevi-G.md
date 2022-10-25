# Gas Optimizations

## Use custom errors instad of string errors to save gas

### Lines

#### HolographBridge.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L148
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L163
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L163
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L214
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L224
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L233
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L255
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L270
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L352


#### HolographOperator.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L241
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L241
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L354
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L368
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L415
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L446
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L485
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L591
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L728
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L756
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L829
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L839
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L863
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L881
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L889
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L903
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L911
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L915
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L932


#### HolographFactory.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L220
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L250
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L250

#### ERC20H.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L117
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L123
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L125
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L147

### Problem
Custom errors are cheaper than string errors (using a require statement). Consider to replace all the triggered string errors with Custom errors to save gas.

### Solution
Replace string errors with custom errors;

## Remove unnecesary extra variable hlgFee

### Lines
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L382-L383

### Problem
Unncesary temporal variable

### Solution
Use fee instead of hlgFee and remove that extra variable to save gas

## Optimization for gas savings

### Lines
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L777-L783

Change 
```solidity
function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private {
  _ownedTokensIndex[tokenId] = _ownedTokensCount[to];
  _ownedTokensCount[to]++;
  _ownedTokens[to].push(tokenId);
  _allTokensIndex[tokenId] = _allTokens.length;
  _allTokens.push(tokenId);
}
```

with
```solidity
function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private {
  uint256 newIndex = ++_ownedTokensCount[to]; 
  _ownedTokensIndex[tokenId] = newIndex;
  _ownedTokens[to].push(tokenId);
  _allTokensIndex[tokenId] = _allTokens.length;
  _allTokens.push(tokenId);
}
```