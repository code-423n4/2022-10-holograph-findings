# Unsafe use of transfer()/transferFrom(). 

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens.

From the below code, the function seems used to transfer token and using transform() function. Some implementations of transfer and transferFrom could return ‘false’ on failure instead of reverting.   

### HolographOperator.sol

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#:L398-L400
```
if (leftovers > 0) {
     _bondedAmounts[job.operator] = 0;
     _utilityToken().transfer(job.operator, leftovers);
}
```
  
### PA1D.sol

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L394-L396

```
for (uint256 i = 0; i < length; i++) {
      sending = ((bps[i] * balance) / 10000);
      addresses[i].transfer(sending);
      // sent = sent + sending;
}
```


It is suggested to check the return value and revert on false or use safeERC20 (SafeTransferFrom) function instead of transfer
