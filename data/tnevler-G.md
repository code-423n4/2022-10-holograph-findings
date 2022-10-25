# Report
## Gas Optimizations ## 


### [G-01]: The increment in for loop post condition can be made unchecked
**Context:**

1. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L682
2. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L208
3. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L224
4. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L241
5. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L257
6. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L295
7. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L315
8. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L333
9. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L338
10. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L355
11. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L375
12. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L258
13. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L617
14. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L465

**Description:**

[This](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked) can save 30-40 gas per loop iteration.

**Recommendation:**

Change:
```
for (uint256 i = 0; i < orders.length; ++i) {
   // Do the thing
}
```

To:
```
for (uint256 i = 0; i < orders.length;) {
   // Do the thing
   unchecked { ++i; }
}
```

### [G-02]: Place subtractions where the operands can't underflow in unchecked {} block
**Context:**

1. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1076
2. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L292
3. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1076

**Description:**

Some gas can be saved by using an unchecked {} block if an underflow isn't possible because of a previous require() or if-statement.


### [G-03]: Use calldata instead of memory
**Context:**

1. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L217
2. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L250
3. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L266
4. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L273
5. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L327
6. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L372
7. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L418
8. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L584
9. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L357
10. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L521

**Recommendation:**

If a reference type function parameter is read-only, it is recommended to use calldata instead of memory because this provides significant gas savings. Since Solidity v0.6.9, memory and calldata are allowed in all functions regardless of their visibility type (ie external, public, etc).
