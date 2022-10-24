## <x>+= <y> COSTS MORE GAS THAN  <x>= <x>\+ <y> FOR STATE VARIABLES</y></x></x></y></x>
### Using the addition operator instead of plus-equals saves 113 gas

#### File: HolographFactory.sol

1.v += 27;
1.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L328

#### Showing how the change should be done for every other instances from this finding
#### The change would be:

1.v = v + 27;

#### File: HolographOperator.sol

2._bondedAmounts[job.operator] -= amount;
2.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L378

3.position -= threshold;
3.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1175

4._bondedAmounts[msg.sender] += amount;
4.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L382

5._bondedAmounts[operator] += amount;
5.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L834

6.current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
6.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1177




## INCREMENTS/DECREMENTS CAN BE UNCHECKED IN FOR-LOOPS
### In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

### Consider wrapping with an unchecked block here (around 25 gas saved per instance):

#### File: HolographInterfaces.sol

1.for (uint256 i = 0; i < uriTypes.length; i++) {
1.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographInterfaces.sol#L235

#### Showing how the change should be done for every other instances from this finding
#### The change would be:
+ for (uint256 i; i < uriTypes.length) {
 // ...  
+   unchecked { ++i; }

2.for (uint256 i = 0; i < length; i++) {
2.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographInterfaces.sol#L264

3.for (uint256 i = 0; i < interfaceIds.length; i++) {
3.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographInterfaces.sol#L286

#### File: HolographOperator.sol

4.for (uint256 i = 0; i < length; i++) {
4.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781

5.for (uint256 i = _operatorPods.length; i <= pod; i++) {
5.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L871

#### File: HolographRegistry.sol

6.for (uint256 i = 0; i < reservedTypes.length; i++) {
6.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L175

7.for (uint256 i = 0; i < length; i++) {
7.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L255

8.for (uint256 i = 0; i < hashes.length; i++) {
8.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L322
