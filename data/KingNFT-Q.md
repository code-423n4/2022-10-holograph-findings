1. [low] The 'sourceMintBatch' function of HolographERC20.sol not verifying if the lengths of 'wallets' and 'amounts' are equal.
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L563

2. [non-critical] constants should be defined rather than using magic numbers
(1)
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L721
(2)
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L916
(3)
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L955

3. [non-critical] Typo errors
(1) 'coud ' should be 'could'
```
      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

```
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L263

4.  [non-critical] Incorrect NatSpec comments
(1)
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L196

