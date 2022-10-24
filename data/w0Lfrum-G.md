# Gas Optimization Report

This report describes opportunity for gas optimization in the order of appearance in the code. 

### 1. Using Unchecked Blocks(Along with Pre-Increment) for Incrementing the Loop Variable

**Lines of Code :**

[HolographOperator.sol#L781-783](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781-L783)

[HolographOperator.sol#L871-L876](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871-L876)

**Details :**

Checks for overflow/underflow consume extra gas. Using an `unchecked` block for incrementing loop variable saves gas. 

For Example :

    for (uint256 i = 0; i < length; i++) {
        operators[i] = _operatorPods[pod][index + i];
    } 

Can be changed to :

    for (uint256 i; i < length;) {  // default value of i will be 0
        operators[i] = _operatorPods[pod][index + i];
        unchecked {
            ++i;
        }
    }

The value of `length` would not be greater than (2^256) - 1, so the value of `i` would not overflow. Hence an `unchecked` block can be used. 