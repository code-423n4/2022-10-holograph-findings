### [L-01] Use of ```ecrecover``` is susceptible to signature malleability


#### Impact
The ```ecrecover``` function is used to recover the address from the signature. The built-in EVM precompile ecrecover is susceptible to signature malleability which could lead to replay attacks (references: https://swcregistry.io/docs/SWC-117, https://swcregistry.io/docs/SWC-121 and https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57).

Consider using OpenZeppelin’s ECDSA library (which prevents this malleability) instead of the built-in function.


#### Findings:
```
contracts/HolographFactory.sol:L333    return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||

contracts/HolographFactory.sol:L334      ecrecover(hash, v, r, s) == signer);

```

### [L-02] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
contracts/enforcer/HolographERC20.sol:L328      require(SourceERC20().beforeApprove(msg.sender, spender, amount));

contracts/enforcer/HolographERC20.sol:L332      require(SourceERC20().afterApprove(msg.sender, spender, amount));

contracts/enforcer/HolographERC20.sol:L339      require(SourceERC20().beforeBurn(msg.sender, amount));

contracts/enforcer/HolographERC20.sol:L343      require(SourceERC20().afterBurn(msg.sender, amount));

contracts/enforcer/HolographERC20.sol:L354      require(SourceERC20().beforeBurn(account, amount));

contracts/enforcer/HolographERC20.sol:L358      require(SourceERC20().afterBurn(account, amount));

contracts/enforcer/HolographERC20.sol:L371      require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

contracts/enforcer/HolographERC20.sol:L375      require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

contracts/enforcer/HolographERC20.sol:L430      require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

contracts/enforcer/HolographERC20.sol:L434      require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

contracts/enforcer/HolographERC20.sol:L447      require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));

contracts/enforcer/HolographERC20.sol:L455      require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));

contracts/enforcer/HolographERC20.sol:L484      require(SourceERC20().beforeApprove(account, spender, amount));

contracts/enforcer/HolographERC20.sol:L488      require(SourceERC20().afterApprove(account, spender, amount));

contracts/enforcer/HolographERC20.sol:L502      require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));

contracts/enforcer/HolographERC20.sol:L507      require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));

contracts/enforcer/HolographERC20.sol:L536      require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));

contracts/enforcer/HolographERC20.sol:L541      require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));

contracts/enforcer/HolographERC20.sol:L582      require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));

contracts/enforcer/HolographERC20.sol:L586      require(SourceERC20().afterTransfer(msg.sender, recipient, amount));

contracts/enforcer/HolographERC20.sol:L606      require(SourceERC20().beforeTransfer(account, recipient, amount));

contracts/enforcer/HolographERC20.sol:L610      require(SourceERC20().afterTransfer(account, recipient, amount));

contracts/enforcer/HolographERC721.sol:L373      require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));

contracts/enforcer/HolographERC721.sol:L378      require(SourceERC721().afterApprove(tokenOwner, to, tokenId));

contracts/enforcer/HolographERC721.sol:L391      require(SourceERC721().beforeBurn(wallet, tokenId));

contracts/enforcer/HolographERC721.sol:L395      require(SourceERC721().afterBurn(wallet, tokenId));

contracts/enforcer/HolographERC721.sol:L460      require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));

contracts/enforcer/HolographERC721.sol:L473      require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));

contracts/enforcer/HolographERC721.sol:L486      require(SourceERC721().beforeApprovalAll(to, approved));

contracts/enforcer/HolographERC721.sol:L491      require(SourceERC721().afterApprovalAll(to, approved));

contracts/enforcer/HolographERC721.sol:L624      require(SourceERC721().beforeTransfer(from, to, tokenId, data));

contracts/enforcer/HolographERC721.sol:L628      require(SourceERC721().afterTransfer(from, to, tokenId, data));

contracts/enforcer/HolographERC721.sol:L759      require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));

contracts/enforcer/HolographERC721.sol:L767      require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));

```

### [L-03] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
contracts/enforcer/HolographERC20.sol:L385    _mint(to, amount);

contracts/enforcer/HolographERC20.sol:L416    _mint(to, amount);

contracts/enforcer/HolographERC20.sol:L557    _mint(to, amount);

contracts/enforcer/HolographERC20.sol:L565      _mint(wallets[i], amounts[i]);

contracts/enforcer/HolographERC20.sol:L683  function _mint(address to, uint256 amount) private {

contracts/enforcer/HolographERC721.sol:L406    _mint(to, tokenId);

contracts/enforcer/HolographERC721.sol:L514    _mint(to, token);

contracts/enforcer/HolographERC721.sol:L814  function _mint(address to, uint256 tokenId) private {

```

### [L-04] Use of Solidity version 0.8.13 which has two known issues 


#### Impact 
The solidity version 0.8.13 has below two issues:

1. Vulnerability related to ABI-encoding.

ref : https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/

2. Vulnerability related to ‘Optimizer Bug Regarding Memory Side Effects of Inline Assembly’

ref : https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/

Similar issue found in Code4rena:
https://github.com/code-423n4/2022-06-putty-findings/issues/348


#### Findings:
```
contracts/HolographOperator.sol:L102  pragma solidity 0.8.13;

contracts/HolographFactory.sol:L102  pragma solidity 0.8.13;

contracts/HolographBridge.sol:L102  pragma solidity 0.8.13;

contracts/module/LayerZeroModule.sol:L102  pragma solidity 0.8.13;

contracts/enforcer/PA1D.sol:L102  pragma solidity 0.8.13;

contracts/enforcer/HolographERC20.sol:L102  pragma solidity 0.8.13;

contracts/enforcer/HolographERC721.sol:L102  pragma solidity 0.8.13;

contracts/enforcer/Holographer.sol:L102  pragma solidity 0.8.13;

contracts/abstract/ERC20H.sol:L102  pragma solidity 0.8.13;

contracts/abstract/ERC721H.sol:L102  pragma solidity 0.8.13;

```


### [N-01] Open TODOs


#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.


#### Findings:
```
contracts/HolographOperator.sol:L701        // TODO: move the bit-shifting around to have it be sequential

```

