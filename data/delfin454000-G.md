### Use `!= 0` instead of `> 0` in a `require` statement if variable is an unsigned integer
`!= 0` should be used where possible since `> 0` costs more gas
___
[HolographOperator.sol: L350](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L350)
```solidity
        require(timeDifference > 0, "HOLOGRAPH: operator has time");
```
Change `timeDifference > 0` to `timeDifference != 0`  
___
[HolographERC721.sol: L815](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L815)
```solidity
    require(tokenId > 0, "ERC721: token id cannot be zero");
```
Change `tokenId > 0` to `tokenId != 0`  
___
___


### Avoid use of '&&' within a `require` function
Splitting into separate `require()` statements saves gas
___
[HolographOperator.sol: L857](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L857)
```solidity
    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
Recommendation:
```solidity
    require(_bondedOperators[operator] == 0, "HOLOGRAPH: operator is bonded");
    require(_bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
___
[Holographer.sol: L166](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L166)
```solidity
    require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
Recommendation:
```solidity
    require(success, "initialization failed");
    require(selector == InitializableInterface.init.selector, "initialization failed");
```
___
[HolographERC721.sol: L263](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L263)
```solidity
      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
```
Recommendation:
```solidity
      require(success, "ERC721: could not init PA1D");
      require(selector == InitializableInterface.init.selector, "ERC721: could not init PA1D");
```
___
[HolographERC721.sol: L464-470](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L464-L470)
```solidity
      require(
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector),
        "ERC721: onERC721Received fail"
      );
```
Recommendation:
```solidity
      require(ERC165(to).supportsInterface(ERC165.supportsInterface.selector), "ERC721: onERC721Received fail");
      require(ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector), "ERC721: onERC721Received fail");
      require(ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector), "ERC721: onERC721Received fail");
```
___
___


### `Require` revert string is too long
The revert strings below can be shortened to 32 characters or fewer (as shown) to save gas (or else consider using custom error codes throughout, which could save even more gas)

[PA1D.sol: L411](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L411)
```solidity
    require(balance > 10000, "PA1D: Not enough tokens to transfer");
```
Change message to `PA1D: Not enough tokens to xfer`

Similarly for: [PA1D.sol: L435](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L435)
___
___

### State variables should not be initialized to their default values
Initializing `uint` variables to their default value of `0` (as occurs below) is unnecessary and costs gas
___
[HolographBridge.sol: L380](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L380)
```solidity
    uint256 fee = 0;
```
Change to: `uint256 fee;`
___
[HolographOperator.sol: L310-311](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L310-L311)
```solidity
    uint256 gasLimit = 0;
    uint256 gasPrice = 0;
```
Change to: `uint256 gasLimit;` and `uint256 gasPrice;`
___
___

### Array length should not be looked up during every iteration of a `for` loop
Since calculating the array length costs gas, it's best to read the length of the array from memory before executing the loop, as demonstrated below:

___
[PA1D.sol: L432-442](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L432-L442)
```solidity
    for (uint256 t = 0; t < tokenAddresses.length; t++) {
      erc20 = ERC20(tokenAddresses[t]);
      balance = erc20.balanceOf(address(this));
      require(balance > 10000, "PA1D: Not enough tokens to transfer");
      // uint256 sent;
      for (uint256 i = 0; i < addresses.length; i++) {
        sending = ((bps[i] * balance) / 10000);
        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
        // sent = sent + sending;
      }
    }
```
The above has an embedded `for` loop. The suggestion below addresses both of the loops:
```solidity
    uint256 totalTokenAddressesLength = tokenAddresses.length; 
    uint256 totalAddressesLength = addresses.length; 
    for (uint256 t = 0; t < totalTokenAddressesLength; t++) {
      erc20 = ERC20(tokenAddresses[t]);
      balance = erc20.balanceOf(address(this));
      require(balance > 10000, "PA1D: Not enough tokens to transfer");
      // uint256 sent;
      for (uint256 i = 0; i < totalAddressesLength; i++) {
        sending = ((bps[i] * balance) / 10000);
        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
        // sent = sent + sending;
      }
    }
```

Similarly for the additional `for` loops referenced below:

[PA1D.sol: L454-459](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L454-L459)

[PA1D.sol: L474-476](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474-L476)

[HolographERC721.sol: L531-536](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L531-L536)

[HolographERC721.sol: L547-551](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L547-L551)

[HolographERC20.sol: L564-566](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L564-L566)
___
___

### Use `++i` instead of `i++` to increase count in a `for` loop
`i++` (or similar counters) are used throughout `Holograph`. It would be better to use `++i`, as demonstrated below, since use of  `i++` costs more gas : 

[HolographOperator.sol: L781-783](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781-L783)
```solidity
    for (uint256 i = 0; i < length; i++) {
      operators[i] = _operatorPods[pod][index + i];
    }
```
Suggestion:
```solidity
    for (uint256 i = 0; i < length; ++i) {
      operators[i] = _operatorPods[pod][index + i];
    }
```
___
___

### Avoid use of default "checked" behavior in a `for` loop
Underflow/overflow checks are made every time `i++` (or equivalent counter) is called. Such checks are unnecessary since `i` is already limited. 
I found only one instance of a `for` loop that used unchecked behavior: [HolographOperator.sol: L871-876](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L871-L876) Therefore, use unchecked behavior for the remaining loops, as shown below:
___
[HolographOperator.sol: L781-783](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781-L783)
```solidity
    for (uint256 i = 0; i < length; i++) {
      operators[i] = _operatorPods[pod][index + i];
    }
```
Suggestion:
```solidity
    for (uint256 i = 0; i < length;) {
      operators[i] = _operatorPods[pod][index + i];

      unchecked {
        ++i;
      }
    }
```
Note that i++ has been changed to ++i, as recommended earlier in this submission
___
___
