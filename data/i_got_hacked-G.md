## Use `calldata` instead of `memory` for `external` functions where the function argument is `read-only`

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.

If the array is passed to an internal function which passes the array to another internal function where the array is modified and therefore memory is used in the external call, it’s still more gass-efficient to use calldata when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L143
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L158
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L173
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L185
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162

## `MULTIPLE ADDRESS MAPPINGS` CAN BE COMBINED INTO A `SINGLE MAPPING` OF AN `ADDRESS` TO A `STRUCT`, WHERE APPROPRIATE

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L185-L196
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L218_L228
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L156-L186

## unused named returns

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L426
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L409
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L815
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L274
*https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L674

## `internal` functions only called once can be `inlined` to save gas

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L144-L146
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L144-L146

## Splitting `require()` statements that use `&&` saves gas

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L469

## `Inline` a modifier that's only called once

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L199

