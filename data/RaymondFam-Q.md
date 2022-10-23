## `constant` Variables Should be Capitalized
Constants should be named with all capital letters with underscores separating words if need be. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L27-L43

Please visit the following style guide for more details:

https://docs.soliditylang.org/en/v0.8.14/style-guide.html

## OpenZeppelin's Initializable
`init()` of `HolographBridge.sol` uses `!_isInitialized()` to prevent re-initialization of the contract.

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L64

Consider using OpenZeppelin's Initializable which will have this scenario better taken care of. When a contract is initialized, its `isInitialized` state variable is set to true.

## Inadequately Commented Assembly Block
Assembly block is used in numerous contracts of Holograph. While this does not pose a security risk per se, it is at the same time a complicated and critical part of the system. Moreover, as this is a low-level language that is harder to parse by readers, consider including extensive documentation regarding the rationale behind its use, clearly explaining what every single assembly instruction does. This will make it easier for users to trust the code, for reviewers to verify it, and for developers to build on top of it or update it. Note that the use of assembly discards several important safety features of Solidity, which may render the code less safe and more error-prone.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L325-L336
