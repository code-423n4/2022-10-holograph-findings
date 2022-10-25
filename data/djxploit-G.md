## 1) Multiple mappings with same key can be combined to a struct

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L218_L228
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L185-L196
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L156-L186

## 2) Avoid unused named return variables

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L804-L805
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L256-L274
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L417-L426
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L281-L293

## 3) `require` statement with multiple conditions should be splitted into multiple require statement to save gas

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L469
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857

## 4) Use `calldata` instead of `memory` in read-only function arguments to save gas

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L147
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L173