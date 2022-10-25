
# **Low and Non-Critical**

Serial No. | Issue Name | Instances
--- | --- | ---
NC-1 | Open TODOs | 1
NC-2 | Unused receive() function will lock Ether in contract | 6

-----
***Total instances found in this contest: 7 | Among all files in scope***
## [NC-01] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

### Instances:
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L602
### References:

[https://code4rena.com/reports/2022-05-sturdy/#l-09-open-todos](https://code4rena.com/reports/2022-05-sturdy/#l-09-open-todos)


-----

## [NC-02] Unused receive() function will lock Ether in contract

If the intention is for the Ether to be used, the function should call another function, otherwise, it should revert.

### Instances:
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/abstract/ERC721H.sol#L113
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/abstract/ERC20H.sol#L113
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L1110
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/HolographERC721.sol#L863
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/HolographERC20.sol#L152
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/Holographer.sol#L124

### References

[https://code4rena.com/reports/2022-05-sturdy#l-06-unused-receive-function-will-lock-ether-in-contract](https://code4rena.com/reports/2022-05-sturdy#l-06-unused-receive-function-will-lock-ether-in-contract)


-----

