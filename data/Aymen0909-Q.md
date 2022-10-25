# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1     | Use `call()` instead of `transfer()` | Low | 2 |
| 2     | Setters should check the input value and revert if it's the zero address or zero | Low | 12 |
| 3     | `require()`/`revert()` statements should have descriptive reason strings | NC | 34 |
| 4     | Event should be emitted in setters | NC | 12 |
| 5     | Use scientific notation | NC | 10 |

## Findings

### 1- Use `call()` instead of `transfer()` :

When transferring ETH, `call()` should be used instead of `transfer()`.

The `transfer()` function only allows the recipient to use 2300 gas. If the recipient uses more than that, transfers will fail. In the future gas costs might change increasing the likelihood of that happening.

Keep in mind that `call()` introduces the risk of reentrancy, but as the code follows the CEI pattern it should be fine.

#### Risk : Low

#### Proof of Concept
Instances include:

File: contracts/HolographOperator.sol [Line 596](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L596)
```
payable(hToken).transfer(hlgFee);
```

File: contracts/enforcer/PA1D.sol [Line 396](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L396)
```
addresses[i].transfer(sending);
```

#### Mitigation
Replace `transfer()` calls with `call()`, keep in mind to check whether the call was successful by validating the return value.

### 2- Setters should check the input value and revert if it's the zero address or zero  :

When setting a new value to a state variable the setter function must check the new value and revert if it's the zero address or zero.

#### Risk : Low

#### Proof of Concept

Instances include:

File: contracts/HolographFactory.sol

[function setHolograph(address holograph) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280)

[function setRegistry(address registry) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300)

File: contracts/HolographBridge.sol

[function setFactory(address factory) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452)

[function setHolograph(address holograph) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472)

[function setOperator(address operator) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502)

[function setRegistry(address registry) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522)

File: contracts/HolographOperator.sol

[function setBridge(address bridge) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949)

[function setHolograph(address holograph) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969)

[function setInterfaces(address interfaces) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989)

[function setMessagingModule(address messagingModule) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009)

[function setRegistry(address registry) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029)

[function setUtilityToken(address utilityToken) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049)

#### Mitigation
Add non-zero address/uint checks in the setters for the instances aforementioned.

### 3- `require()`/`revert()` statements should have descriptive reason strings :

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:
 
```
File: contracts/enforcer/HolographERC20.sol

328     require(SourceERC20().beforeApprove(msg.sender, spender, amount));
332     require(SourceERC20().afterApprove(msg.sender, spender, amount));
339     require(SourceERC20().beforeBurn(msg.sender, amount));
343     require(SourceERC20().afterBurn(msg.sender, amount));
354     require(SourceERC20().beforeBurn(account, amount));
358     require(SourceERC20().afterBurn(account, amount));
371     require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
375     require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
430     require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
434     require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
447     require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));
455     require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));
484     require(SourceERC20().beforeApprove(account, spender, amount));
488     require(SourceERC20().afterApprove(account, spender, amount));
502     require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));
507     require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));
536     require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));
541     require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));
582     require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));
586     require(SourceERC20().afterTransfer(msg.sender, recipient, amount));
606     require(SourceERC20().beforeTransfer(account, recipient, amount));
610     require(SourceERC20().afterTransfer(account, recipient, amount));

File: contracts/enforcer/HolographERC721.sol

373     require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));
378     require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
391     require(SourceERC721().beforeBurn(wallet, tokenId));
395     require(SourceERC721().afterBurn(wallet, tokenId));
460     require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));
473     require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));
486     require(SourceERC721().beforeApprovalAll(to, approved));
491     require(SourceERC721().afterApprovalAll(to, approved));
624     require(SourceERC721().beforeTransfer(from, to, tokenId, data));
628     require(SourceERC721().afterTransfer(from, to, tokenId, data));
759     require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));
767     require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));
```

#### Mitigation
Add reason strings to the aforementioned require statements for better comprehension.

### 4- Event should be emitted in setters :

Setters should emit an event so that Dapps and users can detect important changes to storage. 

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: contracts/HolographFactory.sol

[function setHolograph(address holograph) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280)

[function setRegistry(address registry) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300)

File: contracts/HolographBridge.sol

[function setFactory(address factory) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452)

[function setHolograph(address holograph) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472)

[function setOperator(address operator) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502)

[function setRegistry(address registry) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522)

File: contracts/HolographOperator.sol

[function setBridge(address bridge) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949)

[function setHolograph(address holograph) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969)

[function setInterfaces(address interfaces) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989)

[function setMessagingModule(address messagingModule) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009)

[function setRegistry(address registry) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029)

[function setUtilityToken(address utilityToken) external](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049)

#### Mitigation
Emit an event in all the aforementioned setters.

### 5- Use scientific notation :

When using multiples of 10 you shouldn't use decimal literals or exponentiation (e.g. 1000000, 10**18) but use scientific notation for better readability.

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

```
File: contracts/enforcer/PA1D.sol

395     sending = ((bps[i] * balance) / 10000);
415     sending = ((bps[i] * balance) / 10000);
438     sending = ((bps[i] * balance) / 10000);
551     return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
553     return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
641     return (_getDefaultBp() * amount) / 10000;
643     return (_getBp(tokenId) * amount) / 10000;

File: contracts/module/LayerZeroModule.sol

274     return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
293     return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);

File: contracts/HolographOperator.sol

256     _baseBondAmount = 100 * (10**18);
```

#### Mitigation
Use scientific notation for the instances aforementioned.