## Table of Contents

- [G-01] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops
- [G-02] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

### [G-01] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop. [HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L782), [HolographRegistry.sol](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L175), [HolographRegistry.sol](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L255)

### [G-02] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

Using the addition operator instead of plus-equals saves 113 gas. [HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/HolographERC20.sol#L586-L587)