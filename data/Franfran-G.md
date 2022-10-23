# Useless code

`receive` is unnecessary if `fallback` function is defined and used for the same purpose.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L574-L586
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L340-L349
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L416-L425

# Unchecked

This can be wrapped in `unchecked` block:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L433

This can be stored in a variable and done in an unchecked block:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L640
because it was already checked before: https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595

There is no need to `delete` an array element before `pop`ping it:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1148-L1152
as explained in the docs: https://docs.soliditylang.org/en/v0.5.4/types.html#array-members
Because `pop` implicitely deletes it already.