## `Multiple` mappings of same `key` can be combined into a `single` mapping  to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L193-L198
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L218_L228
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L170-L180 with https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L201-L206
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L185-L196
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L156-L186

## UNUSED NAMED RETURNS

Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity:

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L717-L718
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L804-L805
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L814-L815
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L256-L274
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L281-L293
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L299-L301
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L665-L674
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L417-L426
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L396_L409

## Use `calldata` instead of `memory` for `external` functions where the function argument is `read-only`

The `memory` keyword in a function argument causes the underlying code to copy the argument into memory. `Calldata` causes the code to read the transaction without copying it directly, ultimately saves gases.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L143
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L158
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L147
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L173
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L185
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L140
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L140

## Splitting `require()` statements that use `&&` saves gas

Instead of using the `&&` operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement (saving 3 gas per `&`):

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L469
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857

##  `internal` functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L203
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L203

## `internal` functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L144-L146
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L144-L146

## `Inline` a modifier that's only used once
    
As `onlyOperator()` modifier is only used once in this contract (in `function bridgeInRequest()`), it should get inlined to save gas without losing readability:

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L199

## Don’t use `require()` functions in `loop` . 

It is better to break than to revert 

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L416
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L439
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L416
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L439

