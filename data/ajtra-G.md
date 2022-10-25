# Index
1. Require instead of &&
2. I = I + (-) X is cheaper in gas cost than I += (-=) X
3. Require / Revert strings longer than 32 bytes cost extra gas
4. ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
5. Using bools for storage incurs overhead
6. Use unchecked in for/while loops when it's not possible to overflow. 
7. Calldata vs Memory
8. Optimize HolographOperator.executeJob to save gas
9. Cache variables in local variables to save gas

# Details
## 1. Require instead of &&
### Description
Split of conditions of an require sentence in different requires sentences can save gas

### Lines in the code
[HolographOperator.sol#L857](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L857)
[Holographer.sol#L166](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L166)
[HolographERC721.sol#L263](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L263)

## 2. I = I + (-) X is cheaper in gas cost than I += (-=) X
### Description
In the following example (optimizer = 10000) it's possible to demostrate that I = I + X cost less gas than I += X in state variable.

```solidity
contract Test_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a = a + 1;
        return a;
    }
}

contract Test_Without_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a += 1;
        return a;
    }
}
```
* Test_Optimization cost is 26324 gas
* Test_Without_Optimization cost is 26440 gas

With this optimization it's possible to **save 116 gas**

### Lines in the code
[HolographOperator.sol#L378](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L378)
[HolographOperator.sol#L382](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L382)
[HolographOperator.sol#L834](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L834)
[HolographOperator.sol#L1175](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1175)
[HolographOperator.sol#L1177](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1177)
[HolographFactory.sol#L328](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L328)
[HolographERC20.sol#L633](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L633)
[HolographERC20.sol#L685](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L685)
[HolographERC20.sol#L686](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L686)
[HolographERC20.sol#L702](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L702)

## 3. Require / Revert strings longer than 32 bytes cost extra gas
### Description
Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

### Lines in the code
[PA1D.sol#L411](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L411)
[PA1D.sol#L435](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L435)

## 4. ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
### Description
**abi.encode** will apply [ABI encoding rules](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html). Therefore all elementary types are padded to 32 bytes and dynamic arrays include their length. 
Therefore it is possible to also decode this data again (with abi.decode) when the type are known.

**abi.encodePacked** will only use the only use the minimal required memory to encode the data. E.g. an address will only use 20 bytes and for dynamic arrays only the elements will be stored without length. 
For more info see the [Solidity docs for packed mode](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html?highlight=encodepacked#non-standard-packed-mode)

For the input of the keccak method it is important that you can ensure that the resulting bytes of the encoding are unique. So if you always encode the same types and arrays 
always have the same length then there is no problem. But if you switch the parameters that you encode or encode multiple dynamic arrays you might have conflicts.

For example:

abi.encodePacked(address(0x0000000000000000000000000000000000000001), uint(0)) encodes to the same as abi.encodePacked(uint(0x0000000000000000000000000000000000000001000000000000000000000000), address(0))

and

abi.encodePacked(uint[](1,2), uint[](3)) encodes to the same as abi.encodePacked(uint[](1), uint[](2,3))

Therefore these examples will also generate the same hashes even so they are different inputs.

On the other hand you require less memory and therefore in most cases abi.encodePacked uses less gas than abi.encode.

### Lines in the code
[HolographFactory.sol#L252](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L252)
[HolographERC721.sol#L260](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L260)
[HolographERC721.sol#L426](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L426)
[HolographERC20.sol#L409](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L409)
[HolographERC20.sol#L471](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L471)

## 5. Using bools for storage incurs overhead
### Description
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) 
when changing from 'false' to 'true', after having been 'true' in the past

### Lines in the code
[HolographOperator.sol#L198](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L198)
[PA1D.sol#L451](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L451)
[HolographERC721.sol#L196](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L196)
[HolographERC721.sol#L206](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L206)

## 6. Use unchecked in for/while loops when it's not possible to overflow6. Use unchecked in for/while loops when it's not possible to overflow6. Use unchecked in for/while loops when it's not possible to overflow6. Use unchecked in for/while loops when it's not possible to overflow. Use unchecked in for/while loops when it's not possible to overflow
### Description
Use unchecked { i++; } or unchecked{ ++i; } when it's not possible to overflow to save gas.

### Lines in the code
[HolographOperator.sol#L781](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781)
[HolographOperator.sol#L871](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L871)
[PA1D.sol#L307](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L307)
[PA1D.sol#L323](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L323)
[PA1D.sol#L340](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L340)
[PA1D.sol#L356](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L356)
[PA1D.sol#L394](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L394)
[PA1D.sol#L414](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L414)
[PA1D.sol#L432](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L432)
[PA1D.sol#L437](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L437)
[PA1D.sol#L454](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L454)
[PA1D.sol#L474](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474)
[HolographERC721.sol#L357](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L357)
[HolographERC721.sol#L716](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L716)
[HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L564)

## 7. Calldata vs Memory
### Description
Use calldata instead of memory in a function parameter when you are only to read the data can save gas by storing it in calldata

### Lines in the code
[PA1D.sol#L316](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L316)
[PA1D.sol#L349](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L349)
[PA1D.sol#L426](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L426)
[PA1D.sol#L471](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L471)
[PA1D.sol#L517](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L517)


## 8. Optimize HolographOperator.executeJob to save gas
### Description
#### 1. Remove the local variable elapsedTime due to it's just been used once and we can save not storing the value in the local variable.
#### 2. Move the gasPrice calculation before it's going to be used due to there are possibilities where there are possibilities to don't calcule it and save gas. For example, when condition job.operator != address(0) is false.
#### 3. Move require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected") before require(timeDifference > 0, "HOLOGRAPH: operator has time"); due to in the case that gas >= tx.gasprice is false then it's not necessary to evaluate the second require timeDifference > 0
#### 4. Remove the local variable fallbackOperator
#### 5. Store the global variables _bondedAmounts in local variable and use it to avoid to access to the storage.
```diff
    uint256 gasLimit = 0;
-   uint256 gasPrice = 0;
    assembly {
      /**
       * @dev extract gasLimit
       */
      gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
-     /**
-      * @dev extract gasPrice
-      */
-     gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
    }
    /**
     * @dev unpack bitwise packed operator job details
     */
    OperatorJob memory job = getJobDetails(hash);
    /**
     * @dev to prevent replay attacks, remove job from mapping
     */
    delete _operatorJobs[hash];
    /**
     * @dev check that a specific operator was selected for the job
     */
    if (job.operator != address(0)) {
      /**
       * @dev switch pod to index based value
       */
      uint256 pod = job.pod - 1;
      /**
       * @dev check if sender is not the selected primary operator
       */
      if (job.operator != msg.sender) {
+	  	uint256 gasPrice = 0;
+   	assembly {
+     	  /**
+     	   * @dev extract gasPrice
+     	   */
+     	  gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))		
+   	}
+       /**
+        * @dev check that the selected missed the time slot due to a gas spike
+        */
+       require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");	  
        /**
         * @dev sender is not selected operator, need to check if allowed to do job
         */
-       uint256 elapsedTime = block.timestamp - uint256(job.startTimestamp);
-       uint256 timeDifference = elapsedTime / job.blockTimes;
+       uint256 timeDifference = (block.timestamp - uint256(job.startTimestamp)) / job.blockTimes;
        /**
         * @dev validate that initial selected operator time slot is still active
         */
        require(timeDifference > 0, "HOLOGRAPH: operator has time");
-       /**
-        * @dev check that the selected missed the time slot due to a gas spike
-        */
-       require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
        /**
         * @dev check if time is within fallback operator slots
         */
        if (timeDifference < 6) {
          uint256 podIndex = uint256(job.fallbackOperators[timeDifference - 1]);
          /**
           * @dev do a quick sanity check to make sure operator did not leave from index or is a zero address
           */
          if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
-           address fallbackOperator = _operatorPods[pod][podIndex];
            /**
             * @dev ensure that sender is currently valid backup operator
             */
-           require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
+           require(_operatorPods[pod][podIndex] == msg.sender, "HOLOGRAPH: invalid fallback");
          }
        }
        /**
         * @dev time to reward the current operator
         */
        uint256 amount = _getBaseBondAmount(pod);
+		mapping(address => uint256) private auxBondedAmounts = _bondedAmounts;		
        /**
         * @dev select operator that failed to do the job, is slashed the pod base fee
         */
-       _bondedAmounts[job.operator] -= amount;
+       auxBondedAmounts[job.operator] -= amount;
        /**
         * @dev the slashed amount is sent to current operator
         */
-       _bondedAmounts[msg.sender] += amount;
+       auxBondedAmounts[msg.sender] += amount;
        /**
         * @dev check if slashed operator has enough tokens bonded to stay
         */
-       if (_bondedAmounts[job.operator] >= amount) {
+       if (auxBondedAmounts[job.operator] >= amount) {
          /**
           * @dev enough bond amount leftover, put operator back in
           */
          _operatorPods[pod].push(job.operator);
          _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
          _bondedOperators[job.operator] = job.pod;
        } else {
          /**
           * @dev slashed operator does not have enough tokens bonded, return remaining tokens only
           */
-         uint256 leftovers = _bondedAmounts[job.operator];
+         uint256 leftovers = auxBondedAmounts[job.operator];
          if (leftovers > 0) {
-           _bondedAmounts[job.operator] = 0;
+           auxBondedAmounts[job.operator] = 0;
            _utilityToken().transfer(job.operator, leftovers);
          }
        }
+		_bondedAmounts[job.operator] = auxBondedAmounts[job.operator];
+		_bondedAmounts[msg.sender] = auxBondedAmounts[msg.sender];
      } else {
        /**
         * @dev the selected operator is executing the job
         */
        _operatorPods[pod].push(msg.sender);
        _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
        _bondedOperators[msg.sender] = job.pod;
      }
    }		
		
		
		
```
[HolographOperator.sol#L310-L411](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L310-L411)

## 9. Cache variables in local variables to save gas

### getPodOperators._operatorPods
```diff
  function getPodOperators(
    uint256 pod,
    uint256 index,
    uint256 length
  ) external view returns (address[] memory operators) {
+   address[][] auxOperatorPods = _operatorPods;
-   require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
+   require(auxOperatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
    /**
     * @dev if pod 0 is selected, this will create a revert
     */
    pod--;
    /**
     * @dev get total length of pod operators
     */
-   uint256 supply = _operatorPods[pod].length;
-   uint256 supply = auxOperatorPods[pod].length;
    /**
     * @dev check if length is out of bounds for this result set
     */
    if (index + length > supply) {
      /**
       * @dev adjust length to return remainder of the results
       */
      length = supply - index;
    }
    /**
     * @dev create in-memory array
     */
    operators = new address[](length);
    /**
     * @dev add operators to result set
     */
    for (uint256 i = 0; i < length; i++) {
-     operators[i] = _operatorPods[pod][index + i];
+     operators[i] = auxOperatorPods[pod][index + i];
    }
  }
```
[HolographOperator.sol#L751-L784](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L751-L784)

### bondUtilityToken._operatorPods
[HolographOperator.sol#L849-L891](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L849-L891)

### tokensOfOwner._ownedTokens
[HolographERC721.sol#L358](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L358)
