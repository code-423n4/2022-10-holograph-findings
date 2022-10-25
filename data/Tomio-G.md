1.
Title: empty `constructor`

Proof of Concept:
[HolographBridge.sol#L155](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L155)
[HolographOperator.sol#L233](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L233)
[ERC721H.sol#L133](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L133)
[ERC20H.sol#L133](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L133)
[HolographFactory.sol#L136](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L136)
[LayerZeroModule.sol#L151](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L151)
[PA1D.sol#L166](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L166)

Recommended Mitigation Steps:
Remove if unused for gas saving
________________________________________________________________________

2.
Title: Consider remove empty block

impact:
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

Proof of Concept:
[ERC721H.sol#L212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212)
[ERC20H.sol#L212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L212)
[HolographOperator.sol#L1209](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209)
________________________________________________________________________

3.
Title: Custom errors from Solidity 0.8.4 are cheaper than revert strings

Impact:
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information

Custom errors are defined using the error statement
reference: https://blog.soliditylang.org/2021/04/21/custom-errors/

Proof of Concept:
[HolographBridge.sol#L148](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L148)
[HolographBridge.sol#L205](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L205)
[HolographBridge.sol#L214](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L214)
[HolographBridge.sol#L233](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L233)
[HolographBridge.sol#L257](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L257)
[HolographBridge.sol#L270](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L270)
[HolographOperator.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol) (various line)
[HolographFactory.sol#L220](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L220)
[HolographFactory.sol#L228](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L228)
[HolographFactory.sol#L254](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L254)
[PA1D.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol) (various line)
[HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol) (various line)
[HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol) (various line)

Recommended Mitigation Steps:
Replace require statements with custom errors.
________________________________________________________________________

4.
Title: Default value initialization

Impact:
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Proof of Concept:
[HolographBridge.sol#L380](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L380)
[HolographOperator.sol#L310-L311](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L310-L311)
[HolographOperator.sol#L781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)
[HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

Recommended Mitigation Steps:
Remove explicit initialization for default values.
________________________________________________________________________

5.
Title: Using `!=` in `require` statement is more gas efficient

Proof of Concept:
[HolographOperator.sol#L309](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309)
[HolographOperator.sol#L350](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350)
[HolographERC721.sol#L815](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815)

Recommended Mitigation Steps:
Change `> 0` to `!= 0`
________________________________________________________________________

6.
Title: Using unchecked and prefix increment is more effective for gas saving:

Proof of Concept:
[HolographOperator.sol#L781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)
[HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)
[HolographERC721.sol#L357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357)

Recommended Mitigation Steps:
Change to:

```
for (uint256 i = 0; i < length;) {
// ...
unchecked { ++i; }
}
```
________________________________________________________________________

7.
Title: Using multiple `require` instead `&&` can save gas

Proof of Concept:
[HolographOperator.sol#L857](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857)
[HolographERC721.sol#L263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263)
[HolographERC721.sol#L464-L470](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L470)

Recommended Mitigation Steps:
Change to:

```
	require(_bondedOperators[operator] == 0, "HOLOGRAPH: operator is bonded");
	require(_bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
________________________________________________________________________

8.
Title: Using `delete` statement can save gas

Proof of Concept:
[HolographOperator.sol#L1132-L1136](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1132-L1136)

Recommended Mitigation Steps:
Change to:

```
    delete fundingPoolWeights[_pool];
```
________________________________________________________________________

9.
Title: Unchecking arithmetics operations that can't underflow/overflow

Proof of Concept:
[HolographOperator.sol#L1175](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1175) Should be unchecked due to L#1174

Recommended Mitigation Steps:
Use `unchecked`
________________________________________________________________________

10.
Title: Using bytes constant is more gas efficient

Reference: [Here](https://ethereum.stackexchange.com/questions/3795/why-do-solidity-examples-use-bytes32-type-instead-of-string)

Proof of Concept:
[PA1D.sol#L142-L144](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L142-L144)

Recommended Mitigation Steps:
Change it to `bytes(1..32) constant`
________________________________________________________________________

11.
Title: Reduce the size of error messages (Long revert Strings)

Impact:
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

Proof of Concept:
[PA1D.sol#L390](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390)
[PA1D.sol#L411](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411)

Recommended Mitigation Steps:
Consider shortening the revert strings to fit in 32 bytes
________________________________________________________________________

12.
Title: Using `-=` can save gas

Proof of Concept:
[PA1D.sol#L391](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L391)

Recommended Mitigation Steps:
Change to:

```
	balance -= gasCost;
```
________________________________________________________________________

13.
Title: Caching `length` for loop can save gas

Proof of Concept:
[PA1D.sol#L432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432)
[PA1D.sol#L437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437)
[PA1D.sol#L454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454)
[PA1D.sol#L474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)
[HolographERC20.sol#L564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

Recommended Mitigation Steps:
Change to:

```
	uint256 Length = addresses.length;
	for (uint256 i = 0; i < Length; i++) {
```
________________________________________________________________________

14.
Title: Comparison operators

Proof of Concept:
[HolographERC20.sol#L365](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L365)
[HolographERC20.sol#L400](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L400)
[HolographERC20.sol#L469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469)

Recommended Mitigation Steps:
Replace `<=` with `<`, and `>=` with `>` for gas optimization
________________________________________________________________________

15.
Title: abi.encode() is less efficient than abi.encodePacked()

Proof of Concept:
[HolographERC20.sol#L470](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L470)
________________________________________________________________________