### [L01]  ``ecrecover()`` allows malleable signatures

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographFactory.sol#L234-L235

Best practice is to use OpenZeppelin’s ECDSA contract rather than calling ecrecover() directly

### [N01] Typo in PA1D

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/PA1D.sol#L378

### [N02] Don't use magic numbers use ``constant``s instead

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L259

```
 if (timeDifference < 6) {
          uint256 podIndex = uint256(job.fallbackOperators[timeDifference - 1]);
```