## [L-1] Unused/Empty RECEIVE()/FALLBACK() function.

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds.

***File : HolographOperator.sol***

HolographOperator.sol#L1209

***File : HolographERC721.sol***

HolographERC721.sol#L962

***File : ERC20H.sol***

ERC20H.sol#L212

***File : Holographer.sol***

Holographer.sol#L223

***File : HolographERC20.sol***

HolographERC20.sol#L251




## [L-2] Hard Coded GAS limit.

These numbers are not future-proof as some hardforks introduce changes to gas costs. These potential future changes to gas costs might break some of the functionalities of the smart contracts that use these constants. This is something to keep in mind. If some hardfork, would break a smart contract using these numbers you would need to deploy new contracts with adjusted gas limit constants. Or you can also have these gas limits be mutable by admins on-chain. For example, all 3 of these values can be stored on-chain in 1 storage slot.

***File : HolographOperator.sol***

HolographOperator.sol#L310




## [N-1] Constants should be defined rather than using magic numbers.

Even assembly can benefit from using readable constants instead of hex/numeric literals.

***File : HolographOperator.sol***


HolographOperator.sol#L523
HolographOperator.sol#L524
HolographOperator.sol#L525
HolographOperator.sol#L526
HolographOperator.sol#L527
HolographOperator.sol#L528
HolographOperator.sol#L529
HolographOperator.sol#L530
HolographOperator.sol#L531
HolographOperator.sol#L697
HolographOperator.sol#L699
HolographOperator.sol#L700
HolographOperator.sol#L702
HolographOperator.sol#L704
HolographOperator.sol#L705
HolographOperator.sol#L706
HolographOperator.sol#L707
HolographOperator.sol#L708

***File : HolographERC721.sol***

HolographERC721.sol#L127

***File : HolographFactory.sol***

HolographFactory.sol#L327
HolographFactory.sol#L331

***File : HolographERC20.sol***

HolographERC20.sol#L278
HolographERC20.sol#L648
HolographERC20.sol#L661




## [N-2] Use a more recent version of solidity.

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath, Use a solidity version of at least 0.8.2 to get compiler automatic inlining,Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads,Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings,Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value



## [N-3] Lines are too long

Usually lines in source code are limited to 80 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

***File : HolographERC20.sol***

HolographERC20.sol#L289

***File : PA1D.sol***

PA1D.sol#L151




## [N-4] Use Scientific notation (e.g. 1E18) rather than Exponentiation (e.g. 10**18)

Scientific notation should be used for better code readability

***File : HolographOperator.sol***

HolographOperator.sol#L256

***File : LayerZeroModule.sol***

LayerZeroModule.sol#L274
LayerZeroModule.sol#L293




## [N-5] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

***File : PA1D.sol***

PA1D.sol#L390
PA1D.sol#L395
PA1D.sol#L411
PA1D.sol#L415
PA1D.sol#L435
PA1D.sol#L438



## [N-6] Use of block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

***File : HolographOperator.sol***

HolographOperator.sol#L345
HolographOperator.sol#L499
HolographOperator.sol#L531

***File : HolographERC20.sol***

HolographERC20.sol#L469
