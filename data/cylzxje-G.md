#### Gas Optimizations
### [G-01] Use `external` instead of `public`
### [G-02] Use `payable` for `onlyOwner` function


---
#### Details:
### [G-01] Use `external` instead of `public`
Since these functions aren't called by the internal contract, it should be declared as `external `

contracts/enforcer/HolographERC20.sol
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L306
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L420
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L444
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L468
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L740

contracts/enforcer/PA1D.sol
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L488
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L497
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L507
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L517
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L549
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L558
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L569
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L590
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L604
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L655
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L665
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L683

Recommend changing from public to `external`   

### [G-02] Use `payable` for `onlyOwner` function 
Since ETH can't be accidently sent by `owner`, mark as payable to save some gas

contracts/enforcer/PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L533  

Recommend marking `payable`