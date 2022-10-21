## [G-1] Use custom error rather than require() with string explanation.

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.

***File : HolographOperator.sol***

HolographOperator.sol#L241
HolographOperator.sol#L309
HolographOperator.sol#L350
HolographOperator.sol#L354
HolographOperator.sol#L368
HolographOperator.sol#L415
HolographOperator.sol#L446
HolographOperator.sol#L485
HolographOperator.sol#L591
HolographOperator.sol#L595
HolographOperator.sol#L728
HolographOperator.sol#L739
HolographOperator.sol#L756
HolographOperator.sol#L829
HolographOperator.sol#L839
HolographOperator.sol#L857
HolographOperator.sol#L863
HolographOperator.sol#L881
HolographOperator.sol#L889
HolographOperator.sol#L903
HolographOperator.sol#L911
HolographOperator.sol#L915
HolographOperator.sol#L932

***File : HolographERC721.sol***

HolographERC721.sol#L212
HolographERC721.sol#L224
HolographERC721.sol#L239
HolographERC721.sol#L258
HolographERC721.sol#L263
HolographERC721.sol#L323
HolographERC721.sol#L370
HolographERC721.sol#L371
HolographERC721.sol#L388
HolographERC721.sol#L404
HolographERC721.sol#L408
HolographERC721.sol#L419
HolographERC721.sol#L420
HolographERC721.sol#L421
HolographERC721.sol#L458
HolographERC721.sol#L484
HolographERC721.sol#L513
HolographERC721.sol#L622
HolographERC721.sol#L639
HolographERC721.sol#L689
HolographERC721.sol#L700
HolographERC721.sol#L729
HolographERC721.sol#L757
HolographERC721.sol#L762
HolographERC721.sol#L815
HolographERC721.sol#L816
HolographERC721.sol#L817
HolographERC721.sol#L818
HolographERC721.sol#L869
HolographERC721.sol#L870
HolographERC721.sol#L906

***File : ERC20H.sol***

ERC20H.sol#L117
ERC20H.sol#L123
ERC20H.sol#L125
ERC20H.sol#L147

***File : Holographer.sol***

Holographer.sol#L148
Holographer.sol#L166

***File : HolographFactory.sol***

HolographFactory.sol#L144
HolographFactory.sol#L220
HolographFactory.sol#L228

***File : HolographERC20.sol***

HolographERC20.sol#L192
HolographERC20.sol#L204
HolographERC20.sol#L219
HolographERC20.sol#L241
HolographERC20.sol#L349
HolographERC20.sol#L365
HolographERC20.sol#L387
HolographERC20.sol#L400
HolographERC20.sol#L427
HolographERC20.sol#L445
HolographERC20.sol#L450
HolographERC20.sol#L469
HolographERC20.sol#L482
HolographERC20.sol#L505
HolographERC20.sol#L529
HolographERC20.sol#L539
HolographERC20.sol#L599
HolographERC20.sol#L620
HolographERC20.sol#L621
HolographERC20.sol#L627
HolographERC20.sol#L629
HolographERC20.sol#L645
HolographERC20.sol#L684
HolographERC20.sol#L695
HolographERC20.sol#L696
HolographERC20.sol#L698

***File : LayerZeroModule.sol***

LayerZeroModule.sol#L159
LayerZeroModule.sol#L235

***File : PA1D.sol***

PA1D.sol#L159
PA1D.sol#L174
PA1D.sol#L190
PA1D.sol#L390
PA1D.sol#L411
PA1D.sol#L416
PA1D.sol#L435
PA1D.sol#L439
PA1D.sol#L460
PA1D.sol#L472
PA1D.sol#L477

***File : Holograph.sol***

Holograph.sol#L165




## [G-2] Strings longer than 32bytes inside require()/revert cost extra gas.

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas.

***File : Holographer.sol***

Holographer.sol#L148

***File : PA1D.sol***

PA1D.sol#L411
PA1D.sol#L435




## [G-3] Pre-increment (++i) costs less gas than Post-increment (i++), especially in for loops.

Pre-increment saves 5 gas per loop.

***File : HolographOperator.sol***

HolographOperator.sol#L781
HolographOperator.sol#L871

***File : HolographERC721.sol***

HolographERC721.sol#L357
HolographERC721.sol#L531
HolographERC721.sol#L547
HolographERC721.sol#L564
HolographERC721.sol#L716

***File : HolographERC20.sol***

HolographERC20.sol#L564

***File : PA1D.sol***

PA1D.sol#L307
PA1D.sol#L323
PA1D.sol#L340
PA1D.sol#L356
PA1D.sol#L394
PA1D.sol#L414
PA1D.sol#L437
PA1D.sol#L454
PA1D.sol#L474




## [G-4] It costs more gas to initialize variables to zero than to let the default of zero be applied.

if a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

***File : HolographOperator.sol***

HolographOperator.sol#L310
HolographOperator.sol#L311
HolographOperator.sol#L781

***File : HolographERC721.sol***

HolographERC721.sol#L357
HolographERC721.sol#L716

***File : HolographERC20.sol***

HolographERC20.sol#L564

***File : LayerZeroModule.sol***

LayerZeroModule.sol#L130
LayerZeroModule.sol#L134

***File : PA1D.sol***

PA1D.sol#L307
PA1D.sol#L323
PA1D.sol#L340
PA1D.sol#L356
PA1D.sol#L394
PA1D.sol#L414
PA1D.sol#L432
PA1D.sol#L437
PA1D.sol#L454
PA1D.sol#L474



## [G-5] Using >0 costs more gas than !=0 when used on a UINT in a require statement.

This change saves 6 gas per instance. The optimization works until solidity version 0.8.13 where there is a regression in gas costs.

***File : HolographOperator.sol***

HolographOperator.sol#L309
HolographOperator.sol#L350

***File : HolographERC721.sol***

HolographERC721.sol#L815




## [G-6] ++i/i++ should be UNCHECKED{++i}/UNCHECKED{i++} when it is not possible for them to overflow (eg. in for and while loops).

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves around 40 gas per loop.

***File : HolographOperator.sol***

HolographOperator.sol#L781
HolographOperator.sol#L871

***File : HolographERC721.sol***

HolographERC721.sol#L357
HolographERC721.sol#L716

***File : HolographERC20.sol***

HolographERC20.sol#L564

***File : PA1D.sol***

PA1D.sol#L307
PA1D.sol#L323
PA1D.sol#L340
PA1D.sol#L356
PA1D.sol#L394
PA1D.sol#L414
PA1D.sol#L437
PA1D.sol#L454
PA1D.sol#L474




## [G-7] Multiple require statement instead of && operators saves gas.

splitting-up && operators into multiple require induces a higher deployment cost. However, with enough runtime calls, this ends up being 3gas cheaper.

***File : HolographOperator.sol***

HolographOperator.sol#L857

***File : HolographERC721.sol***

HolographERC721.sol#L263

***File : Holographer.sol***

Holographer.sol#L166




## [G-8] Non-strict inequalities are cheaper than strict ones.

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas). This also holds true between <= and <.

Multiple instances accros all the contracts.



## [G-9] Functions guaranteed to revert when called by normal users (eg. onlyOwner) can be marked payable.

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

***File : HolographOperator.sol***

HolographOperator.sol#L969
HolographOperator.sol#L1049
HolographOperator.sol#L989
HolographOperator.sol#L949
HolographOperator.sol#L1029
HolographOperator.sol#L1009

***File : HolographERC721.sol***

HolographERC721.sol#L500
HolographERC721.sol#L508
HolographERC721.sol#L577
HolographERC721.sol#L520
HolographERC721.sol#L399

***File : HolographFactory.sol***

HolographFactory.sol#L300
HolographFactory.sol#L280

***File : HolographERC20.sol***

HolographERC20.sol#L563
HolographERC20.sol#L415
HolographERC20.sol#L556
HolographERC20.sol#L380
HolographERC20.sol#L549

***File : LayerZeroModule.sol***

LayerZeroModule.sol#L441
LayerZeroModule.sol#L340
LayerZeroModule.sol#L470
LayerZeroModule.sol#L360
LayerZeroModule.sol#L380
LayerZeroModule.sol#L320

***File : Holograph.sol***

Holograph.sol#L348
Holograph.sol#L247
Holograph.sol#L268
Holograph.sol#L206
Holograph.sol#L308
Holograph.sol#L368
Holograph.sol#L328
Holograph.sol#L227
Holograph.sol#L288




## [G-10] Duplicated require()/revert() checks should be refactored to a modifier to save gas.

When a require statement is used multiple times, it is cheaper in deployment costs to use a modifier instead.

***File : HolographOperator.sol***

require(_operatorpods.length>=pod,"holograph:poddoesnotexist");
HolographOperator.sol#L728
HolographOperator.sol#L739
HolographOperator.sol#L756
require(_bondedoperators[operator]!=0,"holograph:operatornotbonded");
HolographOperator.sol#L829
HolographOperator.sol#L903
require(_utilitytoken().transferfrom(msg.sender,address(this),amount),"holograph:tokentransferfailed");
HolographOperator.sol#L839
HolographOperator.sol#L889

***File : HolographERC721.sol***

require(_exists(tokenid),"erc721:tokendoesnotexist");
HolographERC721.sol#L323
HolographERC721.sol#L906
require(_isapproved(msg.sender,tokenid),"erc721:notapprovedsender");
HolographERC721.sol#L371
HolographERC721.sol#L388
HolographERC721.sol#L458
HolographERC721.sol#L622
require(!_exists(tokenid),"erc721:tokenalreadyexists");
HolographERC721.sol#L404
HolographERC721.sol#L817

***File : HolographERC20.sol***

require(currentallowance>=amount,"erc20:amountexceedsallowance");
HolographERC20.sol#L349
HolographERC20.sol#L400
HolographERC20.sol#L529
HolographERC20.sol#L599
require(sourceerc20().beforeapprove(msg.sender,spender,newallowance));
HolographERC20.sol#L371
HolographERC20.sol#L430
require(sourceerc20().afterapprove(msg.sender,spender,newallowance));
HolographERC20.sol#L375
HolographERC20.sol#L434
require(account!=address(0),"erc20:accountiszeroaddress");
HolographERC20.sol#L620
HolographERC20.sol#L627
HolographERC20.sol#L695
require(accountbalance>=amount,"erc20:amountexceedsbalance");
HolographERC20.sol#L629
HolographERC20.sol#L698

***File : PA1D.sol***

require(balance>10000,"pa1d:notenoughtokenstotransfer");
PA1D.sol#L411
PA1D.sol#L435
require(erc20.transfer(addresses[i],sending),"pa1d:couldn"ttransfertoken");
PA1D.sol#L416
PA1D.sol#L439




## [G-11] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}}). Empty receive()/fallback() payable functions that are not used, can be removed to save deployment gas.

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




## [G-12] multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

***File : HolographOperator.sol***

HolographOperator.sol#L218
HolographOperator.sol#L223
HolographOperator.sol#L228

***File : HolographERC721.sol***

HolographERC721.sol#L185
HolographERC721.sol#L190
HolographERC721.sol#L196

***File : HolographERC20.sol***

HolographERC20.sol#L156
HolographERC20.sol#L161
HolographERC20.sol#L186




## [G-13] Use a more recent version of solidity.

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath, Use a solidity version of at least 0.8.2 to get compiler automatic inlining,Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads,Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings,Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
