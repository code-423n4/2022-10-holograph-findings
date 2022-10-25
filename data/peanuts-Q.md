## Table of Contents

- [L-01] Burn function is not implemented in unbondUtilityToken() when operator fails to do his job
- [L-02] _safemint() should be used rather than _mint() whereever possible in ERC721 contracts
- [N-01] Some function parameters should be renamed for clarity
- [N-02] Transferring of ownership should be a two-step process
- [N-03] Best practice is to prevent signature malleability

### [L-01] Burn function is not implemented in unbondUtilityToken() when operator fails to do his job

In [unbondUtilityToken](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L899), there is no fees when withdrawing the token as stated in the [docs](https://docs.holograph.xyz/holograph-protocol/operator-network-specification)

### [L-02] _safemint() should be used rather than _mint() whereever possible in ERC721 contracts

_mint()is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function.

### [N-01] Some function parameters should be renamed for clarity

The parameter of [_getCurrentBondAmount](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1167) should be podIndex instead of pod. Same as [_getBaseBondAmount](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1160)

### [N-02] Transferring of ownership should be a two-step process

[Owner.sol](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/Owner.sol#L140)

### [N-03] Best practice is to prevent signature malleability

Use OpenZeppelin’s ECDSA contract rather than calling ecrecover() directly. [HolographFactory.sol](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L333)