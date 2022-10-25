## 1. Require statements including conditions with the `&&` operator can be broken down in multiple require statements to save gas.

## PROOF OF CONCEPT
 If you’re using the Optimizer at 200, instead of using the `&&` operator in a single require statement to check multiple conditions, Consider using multiple require statements with 1 condition per require statement:


Instances include:
### [HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857)
```
require(_bondedOperators[operator] == 0, "HOLOGRAPH: operator is bonded");
require( _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
### [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263)
```
HolographERC721.sol#L263     require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

HolographERC721.sol#L464     require((ERC165(to).supportsInterface(ERC165.supportsInterface.selector)
&&ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) 
&&ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==ERC721TokenReceiver.onERC721Received.selector)
,"ERC721: onERC721Received fail");
```
### [Holographer.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166)
```
Holographer.sol#L166     require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
> Please, note that this might not hold true at a higher number of runs for the Optimizer (10k). However, it indeed is true at 200.

## 2. DUPLICATED CONDITIONS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION TO SAVE DEPLOYMENT COSTS
 It's better to refactor these into a modifier or function if they start to grow in number.
### [HolographBridge.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L203)
```
HolographBridge.sol#L203 & #L255 :     (_registry().isHolographedContract(holographableContract) || address(_factory()) ==holographableContract,"HOLOGRAPH: not holographed");
 
HolographBridge.sol#L214 & #L270 :    (selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
```
### [HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L728)
```
HolographOperator.sol#L728           require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
HolographOperator.sol#L739           require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
HolographOperator.sol#L756           require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

HolographOperator.sol#L839           require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
HolographOperator.sol#L889           require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
```
