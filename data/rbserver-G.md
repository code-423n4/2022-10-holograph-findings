## [G-01] `_operatorTempStorageCounter` CAN BE CACHED IN MEMORY
The following `_operatorTempStorageCounter`, which is a state variable, is accessed for multiple times. It can be cached in memory, and the cached variable value can then be used. This can replace multiple `sload` operations with one `sload`, one `mstore`, and multiple `mload` operations to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L517-L533
```solidity
      _operatorTempStorage[_operatorTempStorageCounter] = _operatorPods[pod][operatorIndex];
      _popOperator(pod, operatorIndex);
      if (podSize > 1) {
        podSize--;
      }
      _operatorJobs[jobHash] = uint256(
        ((pod + 1) << 248) |
          (uint256(_operatorTempStorageCounter) << 216) |
          (block.number << 176) |
          (_randomBlockHash(random, podSize, 1) << 160) |
          (_randomBlockHash(random, podSize, 2) << 144) |
          (_randomBlockHash(random, podSize, 3) << 128) |
          (_randomBlockHash(random, podSize, 4) << 112) |
          (_randomBlockHash(random, podSize, 5) << 96) |
          (block.timestamp << 16) |
          0
      ); // 80 next available bit position && so far 176 bits used with only 128 left
```

## [G-02] ARITHMETIC OPERATION THAT DOES NOT OVERFLOW OR UNDERFLOW CAN BE UNCHECKED
Explicitly unchecking the arithmetic operation that does not overflow or underflow by wrapping it in `unchecked {}` costs less gas than implicitly checking it.

`_operatorPods[pod].length - 1` can be unchecked in the following code.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L403-L410
```solidity
      } else {
        /**
         * @dev the selected operator is executing the job
         */
        _operatorPods[pod].push(msg.sender);
        _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
        _bondedOperators[msg.sender] = job.pod;
      }
```

`++_inboundMessageCounter` can be unchecked in the following code.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L419-L439
```solidity
    try
      HolographOperatorInterface(address(this)).nonRevertingBridgeCall{value: msg.value}(
        msg.sender,
        bridgeInRequestPayload
      )
    {
      /// @dev do nothing
    } catch {
      _failedJobs[hash] = true;
      emit FailedOperatorJob(hash);
    }
    /**
     * @dev every executed job (even if failed) increments total message counter by one
     */
    ++_inboundMessageCounter;
    /**
     * @dev reward operator (with HLG) for executing the job
     * @dev this is out of scope and is purposefully omitted from code
     */
    ////  _bondedOperators[msg.sender] += reward;
  }
```

`balance - gasCost` can be unchecked in the following code.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390-L391
```solidity
    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
    balance = balance - gasCost;
```

`msg.value - hlgFee` can be unchecked in the following code.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L640-L647
```solidity
    messagingModule.send{value: msg.value - hlgFee}(
      gasLimit,
      gasPrice,
      toChain,
      msgSender,
      msg.value - hlgFee,
      encodedData
    );
```

## [G-03] UNUSED NAMED RETURNS COST UNNECESSARY DEPLOYMENT GAS
When a function has unused named returns, unnecessary deployment gas is used. Please consider removing the following named return variables that are not used.
```
contracts\HolographBridge.sol
  301: ) external returns (string memory revertReason) { 

contracts\HolographFactory.sol
  181: ) external pure returns (bytes4 selector, bytes memory data) {  

contracts\HolographOperator.sol
  804: function getBondedAmount(address operator) external view returns (uint256 amount) { 
  814: function getBondedPod(address operator) external view returns (uint256 pod) { 
```

## [G-04] X = X + Y OR X = X - Y CAN BE USED INSTEAD OF X += Y OR X -= Y
x = x + y or x = x - y costs less gas than x += y or x -= y. For example, `v += 27` can be changed to `v = v + 27` in the following code.

```
contracts\HolographFactory.sol
  328: v += 27;

contracts\HolographOperator.sol
  378: _bondedAmounts[job.operator] -= amount;
  382: _bondedAmounts[msg.sender] += amount;
  834: _bondedAmounts[operator] += amount;
  1175: position -= threshold;
  1177: current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);

contracts\enforcer\HolographERC20.sol
  633: _totalSupply -= amount;
  685: _totalSupply += amount;
  686: _balances[to] += amount;
  702: _balances[recipient] += amount;
```