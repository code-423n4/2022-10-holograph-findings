## N. Should provide message
Some functions will revert but no provide message error


### Proof of Concept
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L341
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L348
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L373
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L395

### Recommended Mitigation Steps
Should add message revert to make it clearer.

## N. Missing emit event
After some actions, functions should emit events to make tracking process easilier

### Proof of Concept
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L278-L294
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L301-L439
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L190-L234
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L245-L283
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L160-L170
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L177-L183
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L180-L222
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L227-L249


### Recommended Mitigation Steps
Emit an event for critical parameter changes.

## N. FUNCTION ORDER
Functions should be ordered following the Solidity conventions.
Link: https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-functions

### Proof of Concept
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol

## N. Lacks a zero-check
Detect missing zero address validation.
Contract `Holograper.init` not validate `holograph`

### Proof of Concept
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L147-L169

### Recommended Mitigation Steps
Should add zero check


## L. Reentrancy Possibility
All external/public functions emit an event after the reentrant code. 
Hence, any event emitted during the external call will appear before the event emitted by the functions above. 
While having no effects on transaction execution, such order complicates the monitoring and reconstruction of contract state based on the events info.

### Proof of Concept
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L427-L433

External calls:
- _utilityToken().transfer
- HolographOperatorInterface(address(this)).nonRevertingBridgeCall

State variables written after the call(s):
- _failedJobs[hash] = true (contracts/HolographOperator.sol#427)
- _inboundMessageCounter (contracts/HolographOperator.sol#433)

Event emitted after the call(s):
- FailedOperatorJob(hash) (contracts/HolographOperator.sol#428)

### Recommended Mitigation Steps
This pattern (storage update after call) is dangerous and must be avoided.

