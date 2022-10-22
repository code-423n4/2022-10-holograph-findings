# QA REPORT

## [QA 00] Some tokens needs approve 0 before any approve above 0
Consider using increase/decrease approve notation instead approve to deal with that.

### Proof of concept:
- [HolographERC721.sol#L487](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L487)
- [HolographERC20.sol#L372](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L372)
- [HolographERC721.sol#L376](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L376)
- [HolographERC20.sol#L231](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L231)
- [HolographERC20.sol#L333](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L333)

## [QA 01] The following require statements are missing an error message


### Proof of concept:
- [HolographERC721.sol#L662](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L662)
- [HolographERC721.sol#L761](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L761)
- [HolographERC20.sol#L350](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L350)
- [HolographERC20.sol#L449](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L449)

## [QA 02] Missing MIT licence for the following contracts


### Proof of concept:
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol)
- [HolographTreasury.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographTreasury.sol)
- [HolographERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol)
- [NonReentrant.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/NonReentrant.sol)
- [HolographGenesis.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographGenesis.sol)

## [QA 03] Use the multiplication math operators before the divisions in the following locations


### Proof of concept:
- [hToken.sol#L101](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/hToken.sol#L101)
- [hToken.sol#L160](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/hToken.sol#L160)
- [hToken.sol#L259](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/token/hToken.sol#L259)

## [QA 04] Not emitted event for state changes


### Proof of concept:
- [StrictERC1155H.sol#L152](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC1155H.sol#L152)
- [StrictERC1155H.sol#L78](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC1155H.sol#L78)
- [StrictERC1155H.sol#L215](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC1155H.sol#L215)
- [StrictERC721H.sol#L159](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC721H.sol#L159)
- [EIP712.sol#L60](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/EIP712.sol#L60)
