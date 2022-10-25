# Report

## Non-Critical Issues ##

### [N-01]: Function defines a named return variable but then it uses return statements 
**Context:** 

1. ``` return "HOLOGRAPH: bridge out failed"; ``` [L217](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L217)
2. ``` return "HOLOGRAPH: unknown error"; ``` [L228](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L228)
3. ``` return _operatorPods.length; ``` [L619](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L619)
4. ``` return _bondedAmounts[operator]; ``` [L706](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L706)
5. ``` return _bondedOperators[operator]; ``` [L716](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L716)
6. ``` return (Holographable.bridgeOut.selector, payload); ``` [L83](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L83)
7. ``` return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee); ``` [L175](https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L175)
8. ``` return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10); ``` [L194](https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L194)
9. [L202-L204](https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L202-L204)
10. ``` return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data)); ``` [327](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L327)
11. ``` return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data)); ``` [310](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L310)

**Recommendation:**

Choose named return variable or return statement. It is unnecessary to use both.

### [N-02]: Wrong order of functions 
**Context:** 

1. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L478, https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L485 (receive function and fallback function must be before all external, public, internal, and private functions)
2. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1110, https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1115 (receive function and fallback function must be before all external, public, internal, and private functions)
3. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L591 (public functions must be after all external functions)
4. https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L241, https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L248 (receive function and fallback function must be before all external, public, internal, and private functions)
5. https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L197 (private functions must be after all external, public, and internal functions)
6. https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L317, https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L324 (receive function and fallback function must be before all external, public, internal, and private functions)
7. https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L332, https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L342, https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L361, https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L371 (external functions must be before all public, internal, and private functions)
8. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L372-L586 (public functions must be after all external functions and before all private functions)
9. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L93 (public functions must be after all external functions)
10. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L124, https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L129 (receive function and fallback function must be before all external, public, internal, and private functions)
11. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L353 (public functions must be after all external functions)
12. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L553, https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L567-L671 (external functions must be before all public, internal, and private functions)
13. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L836 (public functions must be after all external functions and before all internal, and private functions)
14. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L863, https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L869 (receive function and fallback function must be before all external, public, internal, and private functions)
15. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L641, https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L174 (public functions must be after all external functions and before all internal, and private functions)
16. https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L281-L319, https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L450-L479 (external functions must be before all public, internal, and private functions)
17. https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L113, https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L118 (receive function and fallback function must be before all external, public, internal, and private functions)
18. https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L75-L96 (external functions must be before all public, internal, and private functions)
19. https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L113, https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L118 (receive function and fallback function must be before all external, public, internal, and private functions)
20. https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L75-L96 (external functions must be before all public, internal, and private functions)

**Description:**

According [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) functions should be grouped according to their visibility and ordered:

+ constructor

+ receive function (if exists)

+ fallback function (if exists)

+ external

+ public

+ internal

+ private

**Recommendation:**

Put the functions in the correct order according to the documentation.


### [N-03]: Public functions can be external 
**Context:** 

1. [PA1D.configurePayouts](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L372)
2. [PA1D.getPayoutInfo](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L389)
3. [PA1D.getEthPayout](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L398)
4. [PA1D.getTokenPayout](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L408)
5. [PA1D.getTokensPayout](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L418)
6. [PA1D.royaltyInfo](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L450)
7. [PA1D.getFeeBps](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L459)
8. [PA1D.getFeeRecipients](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L470)
9. [PA1D.getRoyalties](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L491)
10. [PA1D.getFees](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L505)
11. [PA1D.tokenCreator](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L522)
12. [PA1D.calculateRoyaltyFee](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L535)
13. [PA1D.marketContract](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L550)
14. [PA1D.tokenCreators](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L556)
15. [PA1D.bidSharesForToken](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L566)
16. [PA1D.getTokenAddress](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L584)
17. [HolographERC721.burned](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L544)
18. [HolographERC721.exists](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L557)
19. [HolographERC20.decimals](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L174)
20. [HolographERC20.allowance](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L198)
21. [HolographERC20.name](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L211)
22. [HolographERC20.symbol](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L219)
23. [HolographERC20.totalSupply](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L223)
24. [HolographERC20.approve](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L227)
25. [HolographERC20.burn](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L238)
26. [HolographERC20.burnFrom](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L248)
27. [HolographERC20.decreaseAllowance](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L264)
28. [HolographERC20.increaseAllowance](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L321)
29. [HolographERC20.permit](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L361)

**Description:**

Public functions can be declared external if they are not called by the contract.

**Recommendation:**

Declare these functions as external instead of public.

### [N-04]: Require() statements should have descriptive reason string 

1. ``` require(SourceERC721().beforeApprove(tokenOwner, to, tokenId)); ``` [L274](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L274)
2. ``` require(SourceERC721().afterApprove(tokenOwner, to, tokenId)); ``` [L279](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L279)
3. ``` require(SourceERC721().beforeBurn(wallet, tokenId)); ``` [L292](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L292)
4. ``` require(SourceERC721().afterBurn(wallet, tokenId)); ``` [L296](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L296)
5. ``` require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data)); ``` [L361](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L361)
6. ``` require(SourceERC721().afterSafeTransfer(from, to, tokenId, data)); ``` [L374](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L374)
7. ``` require(SourceERC721().beforeApprovalAll(to, approved)); ``` [L387](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L387)
8. ``` require(SourceERC721().afterApprovalAll(to, approved)); ``` [L392](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L392)
9. ``` require(SourceERC721().beforeTransfer(from, to, tokenId, data)); ``` [L525](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L525)
10. ``` require(SourceERC721().afterTransfer(from, to, tokenId, data)); ``` [L529](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L529)
11. ``` require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data)); ``` [L660](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L660)
12. ``` require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data)); ``` [L668](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L668)
13. ``` require(SourceERC20().beforeApprove(msg.sender, spender, amount)); ``` [L229](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L229)
14. ``` require(SourceERC20().afterApprove(msg.sender, spender, amount)); ``` [L233](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L233)
15. ``` require(SourceERC20().beforeBurn(msg.sender, amount)); ``` [L240](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L240)
16. ``` require(SourceERC20().afterBurn(msg.sender, amount)); ``` [L244](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L244)
17. ``` require(SourceERC20().beforeBurn(account, amount)); ``` [L255](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L255)
18. ``` require(SourceERC20().afterBurn(account, amount)); ``` [L259](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L259)
19. ``` require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance)); ``` [L272](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L272)
20. ``` require(SourceERC20().afterApprove(msg.sender, spender, newAllowance)); ``` [L276](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L276)
21. ``` require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance)); ``` [L331](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L331)
22. ``` require(SourceERC20().afterApprove(msg.sender, spender, newAllowance)); ``` [L335](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L335)
23. ``` require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data)); ``` [L348](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L348)
24. ``` require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data)); ``` [L356](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L356)
25. ``` require(SourceERC20().beforeApprove(account, spender, amount)); ``` [L385](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L385)
26. ``` require(SourceERC20().afterApprove(account, spender, amount)); ``` [L389](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L389)
27. ``` require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data)); ``` [L403](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L403)
28. ``` require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data)); ``` [L408](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L408)
29. ``` require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data)); ``` [L437](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L437)
30. ``` require(SourceERC20().afterSafeTransfer(account, recipient, amount, data)); ``` [L442](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L442)
31. ``` require(SourceERC20().beforeTransfer(msg.sender, recipient, amount)); ``` [L483](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L483)
32. ``` require(SourceERC20().afterTransfer(msg.sender, recipient, amount)); ``` [L487](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L487)
33. ``` require(SourceERC20().beforeTransfer(account, recipient, amount)); ``` [L507](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L507)
34. ``` require(SourceERC20().afterTransfer(account, recipient, amount)); ``` [L511](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L511)

