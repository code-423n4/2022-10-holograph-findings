Gas report ( 9 findings with 35 instances )

## 1. > 0 can be replaced with != 0 for gas optimisation
*There are 7 instances of this issue:* 
```
if (hTokenValue > 0) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L218
```
require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L309
```
require(timeDifference > 0, "HOLOGRAPH: operator has time");
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L350
```
if (leftovers > 0) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L398

```
if (operatorIndex > 0) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1126
```
require(tokenId > 0, "ERC721: token id cannot be zero");
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L815



## 2. Prefix increments ++i are cheaper than postfix increments i++

*There are 8 instances of this issue:* 
```
podSize--;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L520
```
pod--;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L760
```
_nonces[account]++;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L713

```
for (uint256 i = _operatorPods.length; i <= pod; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L871

```
_ownedTokensCount[from]--;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L842

```
for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L357

```
for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L716

```
for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781






## 3. A variable is being assigned its default value which is unnecessary.
*There are 6 instances of this issue:* 
```
uint256 fee = 0;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L380

```
uint256 gasLimit = 0;
uint256 gasPrice = 0;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L310-L311
```
for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L357

```
for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L716

```
for (uint256 i = 0; i < length; i++) {
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781


## 4. Add unchecked {} for subtractions where the operands cannot underflow because of a previous require() or if-statement
*There are 6 instances of this issue:* 
```
uint256 elapsedTime = block.timestamp — uint256(job.startTimestamp);
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L345

```
if (index + length > supply) {
      /**
       * @dev adjust length to return remainder of the results
       */
      length = supply - index;
    }
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L772
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L354
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L713

```
if (position > threshold) {
      position -= threshold;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1175

```
_ownedTokensCount[to]++;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L779




 ## 5. Use Custom Errors to save Gas
Custom errors from Solidity 0.8.4 are cheaper than revert strings.
```
require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L365



## 6. Splitting require() statements that use && saves gas
See this issue which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper
*There are 3 instances of this issue:* 
```
require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L857
```
require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L166

```
require(
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector),
        "ERC721: onERC721Received fail"
      );
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L464-L470









## 7. x += y consumes more gas than x = x + y for state variables
*There are 2 instances of this issue:* 
```
 _bondedAmounts[job.operator] -= amount;
        /**
         * @dev the slashed amount is sent to current operator
         */
        _bondedAmounts[msg.sender] += amount;
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L378-L382





## 8. Additional math operations consume gas
```
_baseBondAmount = 100 * (10**18);
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L256

## 9. Change  encode to encodePacked
In the contracts, change abi.encode to abi.encodePacked can save gas.
```
return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L426




