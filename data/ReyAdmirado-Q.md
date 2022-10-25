
## 1. typo in comments

initilaization --> initialization

- [HolographBridge.sol#L160](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L160)

- [HolographOperator.sol#L238](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L238)

- [HolographFactory.sol#L141](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L141)

- [LayerZeroModule.sol#L156](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L156)

- [ERC721H.sol#L138](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L138)

- [ERC20H.sol#L138](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L138)

- [Holographer.sol#L145](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L145)

- [HolographERC20.sol#L216](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L216)

- [HolographERC721.sol#L236](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L236)

- [PA1D.sol#L171](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L171)


brigeOutRequest --> bridgeOutRequest

- [HolographBridge.sol#L182](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L182)

destiantion --> destination

- [HolographBridge.sol#L415](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L415)
- [HolographBridge.sol#L416](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L416)

- [HolographOperator.sol#L661](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L661)
- [HolographOperator.sol#L662](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L662)

transfered --> transferred

- [HolographBridge.sol#L575](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L575)
- [HolographFactory.sol#L338](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L338)
- [LayerZeroModule.sol#L414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L414)

accomodate --> accommodate

- [HolographOperator.sol#L515](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L515)
- [PA1D.sol#L387](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L387)


rever --> revert

- [HolographOperator.sol#L553](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L553)

adress --> address

- [HolographOperator.sol#L997](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L997)

deployement --> deployment

- [HolographFactory.sol#L188](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L188)

addresse's --> addresses

- [HolographERC20.sol#L154](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L154)

authorisations --> authorizations

- [HolographERC721.sol#L662](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L662)

accesible --> accessible

- [PA1D.sol#L156](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L156)

optimizaion --> optimization

- [PA1D.sol#L299](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L299)

authorised --> authorized

- [PA1D.sol#L446](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L446)

tranaction --> transaction

- [PA1D.sol#L447](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L447)


## 2. open todos
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

- [HolographOperator.sol#L701](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701)


## 3. Unused/empty receive()/fallback() function
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

- [HolographOperator.sol#L1209](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209)

- [ERC721H.sol#L212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212)

- [ERC20H.sol#L212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L212)

- [Holographer.sol#L223](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223)

- [HolographERC20.sol#L251](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251)

- [HolographERC721.sol#L962](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962)


## 4. Constant redefined elsewhere
Consider defining in only one contract so that values cannot become out of sync when only one location is updated. A cheap way to store constants in a single location is to create an internal constant in a library. If the variable is a local cache of another contract’s value, consider making the cache variable internal or private, which will require external users to query the contract with the source of truth, so that callers don’t get out of sync.
https://medium.com/coinmonks/gas-cost-of-solidity-library-functions-dbe0cedd4678


## 5. Division by 0 might occur
Should check with a require or if statement first so we don’t get 0 for denominator

- [HolographOperator.sol#L346](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L346)
- [HolographOperator.sol#L1177](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1177)


## 6. lines are too long
Usually lines in source code are limited to 80 characters. Its advised to keep lines lower than 120 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

- [Holographer.sol#L172](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L172)

- [PA1D.sol#L151](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L151)

## 7. typo in require

coud --> could

- [HolographERC721.sol#L263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263)

lenghts --> lengths

- [PA1D.sol#L472](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L472)


## 8. event is missing indexed fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

- [PA1D.sol#L153](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L153)


## 9. require()/revert() statements should have descriptive reason strings or custom error

- [HolographERC20.sol#L328](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L328)
- [HolographERC20.sol#L332](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L332)
- [HolographERC20.sol#L339](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L339)
- [HolographERC20.sol#L343](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L343)
- [HolographERC20.sol#L354](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L354)
- [HolographERC20.sol#L358](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L358)
- [HolographERC20.sol#L371](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L371)
- [HolographERC20.sol#L375](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L375)
- [HolographERC20.sol#L430](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L430)
- [HolographERC20.sol#L434](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L434)
- [HolographERC20.sol#L455](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L455)
- [HolographERC20.sol#L484](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L484)
- [HolographERC20.sol#L488](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L488)
- [HolographERC20.sol#L502](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L502)
- [HolographERC20.sol#L507](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L507)
- [HolographERC20.sol#L536](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L536)
- [HolographERC20.sol#L541](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L541)
- [HolographERC20.sol#L582](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L582)
- [HolographERC20.sol#L586](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L586)
- [HolographERC20.sol#L606](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L606)
- [HolographERC20.sol#L610](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L610)
- [HolographERC20.sol#L586](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L586)
- [HolographERC20.sol#L586](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L586)
- [HolographERC20.sol#L586](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L586)
- [HolographERC20.sol#L586](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L586)

same instances in 
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol)

## 10. Duplicated require()/revert() checks should be refactored to a modifier or function


require(account != address(0)

- [HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol)


require(_isApproved(msg.sender, tokenId)

- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol)
