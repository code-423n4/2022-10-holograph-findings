

---
## Summary

- G-01 Multiple address mappings can be combined into a single mapping of an address to a struct (8 instances)
- G-02 <x> += <y> costs more gas than <x> = <x> + <y> for state variables (9 instances)
- G-03 Empty blocks should be removed or emit something (12 instances)
- G-04 Use custom errors rather than revert()/require() strings to save deployment gas (97 instances)
- G-05 Use calldata instead of memory for function parameters (34 instances)

Total: 160 instances in 5 issues

---

## G-01 Multiple address mappings can be combined into a single mapping of an address to a struct

Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate saves a storage slot for the mapping. 
Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

8 instances in 3 files:

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L57
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L62
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L87

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L91
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L97

HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L119
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L124
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L129



## G-02 <x> += <y> costs more gas than <x> = <x> + <y> for state variables

9 instances in 3 files:

HolographFactory.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L229

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L586
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L587
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L603

HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L283
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L339
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L735
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1077
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1078


## G-03 Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. 
If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. 
If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})

12 instances in 10 files:

HolographFactory.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L37

Holographer.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L41

PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L67

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L112

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L132

LayerZeroModule.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L52

ERC20H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L34
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L113

ERC721H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L34
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L113

HolographBridge.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L56

HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L134


## G-04 Use custom errors rather than revert()/require() strings to save deployment gas

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version.

97 instances in 10 files:

HolographFactory.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L45
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L121
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L129
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L155

Holographer.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L49
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L67

PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L75
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L91
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L291
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L312
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L317
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L336
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L340
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L361
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L373
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L378

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L120
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L142
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L266
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L288
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L328
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L353
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L383
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L406
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L430
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L500
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L521
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L522
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L528
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L530
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L546
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L596
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L597
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L599

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L159
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L164
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L224
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L271
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L272
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L289
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L305
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L309
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L320
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L321
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L322
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L359
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L370
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L414
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L523
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L540
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L590
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L601
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L630
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L658
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L663
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L668
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L716
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L717
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L718
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L719
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L770
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L771
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L807

LayerZeroModule.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L60
https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L136

ERC20H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L18
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L24
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L26
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L48

ERC721H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L18
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L24
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L26
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L48

HolographBridge.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L64
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L106
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L128
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L158

HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L142
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L210
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L251
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L255
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L269
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L316
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L347
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L386
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L492
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L496
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L629
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L657
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L730
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L740
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L758
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L782
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L790
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L804
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L812
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L833


## G-05 Use calldata instead of memory for function parameters

Having function arguments use calldata instead of memory can save gas. 

Recommended Mitigation Steps: Change function arguments from memory to calldata.

34 instances in 10 files:

HolographFactory.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L44
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L94
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L95

Holographer.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L50

PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L74
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L86
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L266
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L273
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L284
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L285
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L307
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L308
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L327
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L328
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L329
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L353
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L372
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L389
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L418
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L584

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L119
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L400
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L425
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L542

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L139
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L357
https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L521

LayerZeroModule.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L59

ERC20H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L41
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L46

ERC721H.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L41
https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L46

HolographBridge.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L63

HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L141


