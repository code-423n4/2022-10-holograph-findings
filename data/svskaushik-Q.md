# QA Issues found:

# Low Findings

## [L-1] Unsafe ERC20 Operation(s)
#### Impact
The return value of an external `transfer`/`transferFrom` call is not checked
#### Findings:
```solidity
holo\HolographOperator.sol::400 => _utilityToken().transfer(job.operator, leftovers);
holo\HolographOperator.sol::596 => payable(hToken).transfer(hlgFee);
holo\PA1D.sol::396 => addresses[i].transfer(sending);
```
#### Recommendation
Use `SafeERC20`, or ensure that the `transfer`/`transferFrom` return value is checked.

## [L-2] Open TODOs
#### Impact
There is one open TODO in HolographOperator.sol.
#### Findings:
```solidity
holo\HolographOperator.sol::701 => // TODO: move the bit-shifting around to have it be sequential
```
#### Recommendation
Remove TODO's before deployment

## [L-3] `_safeMint()` should be used rather than `_mint()` wherever possible.
#### Impact
`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`.
#### Findings:
```solidity
holo\HolographERC20.sol::385 => _mint(to, amount);
holo\HolographERC20.sol::416 => _mint(to, amount);
holo\HolographERC20.sol::557 => _mint(to, amount);
holo\HolographERC20.sol::565 => _mint(wallets[i], amounts[i]);
holo\HolographERC20.sol::683 => function _mint(address to, uint256 amount) private {
holo\HolographERC721.sol::406 => _mint(to, tokenId);
holo\HolographERC721.sol::514 => _mint(to, token);
holo\HolographERC721.sol::814 => function _mint(address to, uint256 tokenId) private {
```
#### Recommendation
Use either [OpenZeppelin's](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) or [solmate's](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) version of this function.

# Non-Critical Findings

## [N-1] Use of `ecrecover()` is susceptible to signature malleability
#### Findings:
```solidity
holo\HolographFactory.sol::333 => return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
holo\HolographFactory.sol::334 => ecrecover(hash, v, r, s) == signer);
```
#### Recommendation
Use OpenZeppelin's `ECDSA` contract rather than calling `ecrecover()` directly.

## [N-2] Commented out code blocks
#### Findings
The following code block is commented out (HolographERC721.sol 525-570):
```
  /**
   * @dev Allows for source smart contract to mint a batch of tokens.
   */
  //   function sourceMintBatch(address to, uint224[] calldata tokenIds) external onlySource {
  //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
  //     uint32 chain = _chain();
  //     uint256 token;
  //     for (uint256 i = 0; i < tokenIds.length; i++) {
  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  //       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])));
  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  //       _mint(to, token);
  //     }
  //   }

  /**
   * @dev Allows for source smart contract to mint a batch of tokens.
   */
  //   function sourceMintBatch(address[] calldata wallets, uint224[] calldata tokenIds) external onlySource {
  //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
  //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
  //     uint32 chain = _chain();
  //     uint256 token;
  //     for (uint256 i = 0; i < tokenIds.length; i++) {
  //       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])));
  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  //       _mint(wallets[i], token);
  //     }
  //   }

  /**
   * @dev Allows for source smart contract to mint a batch of tokens.
   */
  //   function sourceMintBatchIncremental(
  //     address to,
  //     uint224 startingTokenId,
  //     uint256 length
  //   ) external onlySource {
  //     uint32 chain = _chain();
  //     uint256 token;
  //     for (uint256 i = 0; i < length; i++) {
  //       token = uint256(bytes32(abi.encodePacked(chain, startingTokenId)));
  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  //       _mint(to, token);
  //       startingTokenId++;
  //     }
  //   }
```
#### Recommendation
For the sake of code clarity, consider removing irrelevant code and un-commenting operational code before deployment.
