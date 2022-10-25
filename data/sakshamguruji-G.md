## MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per
mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in 
the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having
to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

Affected Code(mappings):

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L170
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L175
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L180
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L185
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L190
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L201
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L206

##   X += Y costs 22 more gas than X = X + Y (Same with -)

This can mean a lot of gas wasted in a function call when the computation is repeated n times (loops)
Recommended Mitigation
use X = X + Y instead of X += Y (same with -).

Affected Code:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378

## USING PRIVATE RATHER THAN PUBLIC/INTERNAL FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Saves 3406-3606 gas in deployment gas due to the
compiler not having to create non-payable getter functions for deployment call data, not having to store the bytes of the value
outside of where it’s used, and not adding another entry to the method ID table.
As the constants declared in the holograph contracts have no visibility set , they are internal by default , change them to private.

Affected code:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L129
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L133
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L137
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L141
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L145
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L149
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L153

Similarly consider changing the constants for all the contracts

