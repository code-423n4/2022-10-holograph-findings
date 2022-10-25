## L-01 Unused receive() function will lock Ether in contract

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

8 instances in 8 files:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L124

https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L152

https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L863

https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L317

https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L113

https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L113

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L478

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1110

