# QA

## Replace hardcoded hashes with the actual hashing execution

### Lines

#### HolographBridge.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L126
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L130
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L134
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L138
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L142

#### HolographOperator.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L129
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L133
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L137
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L141
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L145
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L149
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L153

#### HolographFactory.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L127
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L131

#### ERC20H.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L110
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L114

#### ERC721H.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L110
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L114

#### HolographERC20.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L142
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L146

#### HolographERC721.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L136
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L140

#### Holographer.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L119
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L123
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L127
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L131
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L135

#### LayerZeroModule.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L126
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L130
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L134
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L138
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L142
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L146

#### PA1D.sol
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L25
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L29
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L33
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L37
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L41

### Problem
Contracts are using custom slots for different things. These slots directions are being calculated as de hash of a custom string. The result of these hashes are hardcoded to the contracts.

A better approach would be to calculate the hash at complication time (since the hashes are constants). These won't add any extra gas cost on deployment and it would be safer (we cannot be sure that the hashes are correct or not today).

### Solution
Change the references to the slots hashes:

Example:

```solidity
/**
  * @dev bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1)
  */
bytes32 constant _holographerSlot = 0xe9fcff60011c1a99f7b7244d1f2d9da93d79ea8ef3654ce590d775575255b2bd;
```

to:

/**
bytes32 constant _holographerSlot = bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1);
*/

Also, it would be better if all these references are extracted to a new Constants contract and use inheritance. These would remove a lot of duplicated code.

Example:

```solidity
contract SlotConstants {
  bytes32 constant _holographerSlot = bytes32(uint256(keccak256('eip1967.Holograph.holographer')) - 1);
  ...
}


abstract contract ERC20H is Initializable, SlotConstants {
  ...
}
```

## Use Initializable.sol implementation from OpenZeppelin's library instead of a custom implementation

### Lines

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L104
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L106
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L105
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L104
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L6

### Problem
Unnecesary custom implementation for Initializable contracts.

### Solution
Use OpenZeppelin's Initializable.sol contract instead.

## Use Owner.sol implementation from OpenZeppelin's library instead of custom implementation

### Lines

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L108
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L106
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L7

### Problem
Unnecesary custom implementation for Owner functionality.

### Solution
Use OpenZeppelin's Owner.sol contract instead.

### Remove all the getSomething() functions used for custom slots reading (returning an address) and reuse StorageSlot.sol from OpenZeppelin's library.

### Problem

There're a buch of duplicated functions that the only thing that they're doing is to return an address from a custom slot. These adds a lot of extra lines to the contracts that could be replaced using StorageSlot.sol getAddressSlot() function. Also, it would be better to move those functions to a separated contract that could be inherited and reused.

## Remove empty constructors from contracts

### Problem
There is no need for empty constructors on the contracts. Consider to remove them to improve readability.

### Solution
Remove empty constructors

## addresses could be address(0). Resulting on unexpected behaviors

### Lines
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L164
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L242
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L145
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L150
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L160
* https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L80

### Problem
There is no validation for some input address on initialization. This could cause unexpected behaviours.

### Solution
Validate that addresess are not address(0)


## Use getJobNonce() and remove duplicated code
### Lines
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L373-L376

### Problem
Duplicated code

### Solution
Remove the lines and call getJobNonce() instead

## Use getHolograph() and getRegistry() and remove duplicated code
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L197-L202

### Problem
Duplicated code that could be removed

### Solution
Replace the assembly blocks and use getHolograph() and getRegistry() instead

## Assumtion that the token has 18 decimals

### Lines
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L255-L257

### Problem
Not all the tokens use 18 decimals, here we are asumming that and it could cause unexpected behaviours.

### Solution
Validate that the token that we're using has 18 decimals

## Use call instead of transfer() and verify that the call finishes successfully

### Lines
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L596

### Problem
transfer() is deprecated and not recommended. It must be replaced with a call()

### Solution
Replace the line with:
```solidity
(bool success, ) = hToken.call{value: hlgFee}("");
require(sucess, "payment has failed");
```

### Extract ERC20H.sol and ERC721H.sol duplicated code to a new contract and use inheritance

### Problem
There're a lot of duplicated code between ERC20H.sol and ERC721H.sol that could be extracted to a new class and reused through inheritance

### Solution
Create a new class with the duplicated code and use inheritance.

## Use ECDSA.sol OpenZeppelin's contract instead of a custom implementation

### Lines
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L130

### Problem
Unnecesary custom implementation of ECDSA recover functionality.

### Solution
Use OpenZeppelin's library instead

## Use address(SourceERC721()) and remove duplicated assembly block
### Lines
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L220-L223
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L243
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L325-L327

### Problem
There're duplicated assembly blocks accross the code.

### Solution
Use SourceERC721() instead.

## Missing error message

### Lines
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L373
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L378
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L391
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L395
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L460
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L473
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L486
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L491
 
### Problem
It's recommended to return an error message always a require is not fullfilled

### Solution
Add an error message
