# 1) Use scientific notation 1e18, 1e10
3 Instances:
HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L256

LayerZeroModule.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L274
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L293

Recommended mitigation steps:
Consider using 1e18 instead of 10 ** 18 or 1e10 instead of 10 ** 10


# 2) Open TODOs
There are open TODOs in the code. 
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.
Instance:
HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701


# 3) Typos in require messages
Typos: missmatched, down't
2 Instances:
PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L472
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L477


# 4) NATSPEC
Important functions should have a @notice comment to describe what they perform.
5 Instances:
PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L589
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L603
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L617
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L647
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L653

Recommended mitigation steps:
Consider adding @notice and @dev comments, and consider deleting unnecessary development/production 
comments before deployment