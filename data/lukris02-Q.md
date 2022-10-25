# QA Report for Holograph contest

## Overview
During the audit, 6 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
NC-1 | [Order of Functions](#nc-1-order-of-functions) | Non-Critical | 24
NC-2 | [Missing NatSpec](#nc-2-missing-natspec) | Non-Critical | 77
NC-3 | [Open TODOs](#nc-3-open-todos) | Non-Critical | 1
NC-4 | [Commented code](#nc-4-commented-code) | Non-Critical | 7
NC-5 | [Scientific notation may be used](#nc-5-scientific-notation-may-be-used) | Non-Critical | 11
NC-6 | [Constants may be used](#nc-6-constants-may-be-used) | Non-Critical | 1

## Non-Critical Risk Findings (6)
### NC-1. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private

##### Instances
receive() and fallback() functions should be placed after constructor:
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L577-L586
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209-L1215
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L340-L348
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L416-L423
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223-L242
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962-L1000
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212-L226

public functions should not be placed between external:
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L690
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L192
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L452
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L599-L658
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L273-L378
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L420-L544
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L580-L613

public functions should not be placed between private:
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L935
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L740

all public functions should not be placed after private:
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

##### Recommendation
Reorder functions where possible.

#
### NC-2. Missing NatSpec
##### Description
NatSpec is missing for 77 functions in 6 contracts. 

##### Instances
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L251
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L277
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L296
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L185
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L298
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L316
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L332
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L349
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L365
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L372
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L549
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L558
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L569
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L590
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L604
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L621
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L634
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L649
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L655
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L665
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L413
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L643
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L656
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L824
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L878
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L911
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L935
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L943
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L1002
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L174
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L185
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L193
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L197
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L203
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L174
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L185
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L193
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L197
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L203
- and 37 functions in HolographERC20.sol https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

##### Recommendation
Add NatSpec for all functions.

#
### NC-3. Open TODOs
##### Instances
HolographOperator.sol has 1 open TODO:
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701

##### Recommendation
Resolve issues.

#
### NC-4. Commented code
##### Instances
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1176
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L413
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L417
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L436
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L440
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L579-L587
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L524-L570

##### Recommendation
Delete commented code.

#
### NC-5. Scientific notation may be used
##### Description
For readability, it is better to use scientific notation.

##### Instances
For 10000:
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L395
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L415
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L438
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L477
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L551
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L553
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L641
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L643

##### Recommendation
Replace ```10000``` with ```10e4```.

#
### NC-6. Constants may be used
##### Description
Constants may be used instead of literal values.

##### Instances
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L395
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L415
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L438
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L477
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L551
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L553
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L641
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L643

##### Recommendation
Consider using a constant variable.