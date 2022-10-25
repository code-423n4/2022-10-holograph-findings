### L-1 Unsafe ABI encoding
Throughout the codebase `abi.encodeWithSignature` and `abi.encodeWithSelector` were used for generating calldata. `abi.encodeWithSignature` is not typo safe and `abi.encodeWithSelector` is not type safe. Since Solidity 0.8.11, `abi.encodeCall` has been available which performs both type and typo checking. As the codebase uses a version > Solidity 0.8.11, I recommended to use `abi.encodeCall` wherever possible.

Locations: 
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L388
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L597
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L164
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L260

### L-2 Users may lose funds if they interact with enforcers directly
The enforcer contracts have payable functions but their state is held entirely on the holographer contract. If a user were to unknowingly attempt to interact directly with an enforcer, their funds would be locked into the contract. Consider adding this information in the docstrings and/or pursuing a whitelisting approach where only the necessary parties can call the payable functions on the contract directly.

Locations:
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L368
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L436
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L452
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L588
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L599
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L616
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L962
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L968
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L251
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L257

### L-3 Unchecked initialized addresses
Many of the contracts in the codebase leverage an initialize function that takes a bytes payload and sets key state variables such as owner, admin, and holograph addresses. These functions do not verify that the addreses are non-zero before they are set. If a user does have to redeploy a holographable contract due to accidentally setting an incorrect address, they may have to change the original contract bytecode to obtain a new address. Consider adding this check to prevent onerous redeployment on initializer payload mistakes.

Locations:
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L143
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/Holograph.sol#L164
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L162
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L240
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L168
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L173
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L147
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L238
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L218

### L-4 No event emission on sensitive function
Several of the in-scope contracts inherit from the `Owner.sol` contract. When ownership is transferred using the [`transferOwnership`](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/Owner.sol#L140-L145) function, no event is emitted. Consider emitting an event so that ownership transfers can be monitored.

### L-5 Confusing deployment block return type
The return type and name of the [`getDeploymentBlock()`](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L174) function is quite counterintuitive. For example using the current block of 15807116, the return value of `getDeploymentBlock()` is `0x0000000000000000000000000000000000F1328c`. This is confusing given the intended use case of it being `Useful for retrieving deployment config for re-deployment on other EVM-compatible chains`. I recommend making this return a uint256 named something more appropriate such as `deploymentBlock`.

### L-6 Use call instead of transfer for sending Ether
Gas prices for EVM operations are not necessarily fixed but `transfer` and `send` are limited in the gas they send. To ensure forward compatability, use `.call{value: amt}` instead of `transfer` or `send`.

Locations:
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L396
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L596

### NC-1 Lack of documentation
Several functions in the code base lacked documentation, which reduces code readability. Consider adding docstrings to functions missing them.

### NC-2 Overloaded modifier
One modifier in the codebase had multiple conditional checks despite the name of the modifier indicating a single objective. Overloading a modifier in this manner diminishes code readability. Consider separating out functionality into modifiers that uniquely describe the conditionals checked and/or changing the name of the modifier(s) to encompass all of the conditionals.

Locations:
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L205-L210

### NC-3 Outdated OZ ECDSA library
The ECDSA library that is used is v4.4.1. This version has a known signature malleability issue, however the contracts in the scope of this audit were not using the vulnerable function. Consider updating to the current stable version to benefit from the latest security patches. 

### NC-4 Typos
Several notes and error messages in the code base contained typos.

Locations:
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L447
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L472
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L477
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L188
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L319

### NC-5 Imports would benefit from explicit naming
Many of the contracts import several other contracts. Due to this, it can be arduous to track down where specifically items are being imported from. From a readability standpoint, I recommend considering using explicit imports of the form `import {X, Y, Z} from "A"`.

### NC-6 Misleading comment
The [`revertedBridgeOutRequest()`](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L286) function contains a doc string that alerts the user it will always revert. The caution itself is good practice, though it is not completely accurate. It would be more accurate to replace the second half with something like "it is intended to revert" as opposed to "it will always revert".

### NC-7 Revert and require statements lacking error messages
Error messages are helpful during testing, for users interacting with the protocol, and even from a readability perspective. Consider adding them to the many require and revert statements that are missing them.