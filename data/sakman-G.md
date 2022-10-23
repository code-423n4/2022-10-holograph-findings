## 1. Prefix incrementing and decrementing costs around 6 gas less than the postfix ones

### e.g. ++var is cheaper than var++

_contracts/enforcer/HolographERC721.sol:_ [L357](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L357)
[L716](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L716)

_contracts/HolographOperator.sol:_ [L520](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L520)
[L760](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L760)
[L781](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L781)

_contracts/enforcer/PA1D.sol:_ [L307](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L307)
[L340](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L340)
[L414](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L414)
[L432](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L432)
[L437](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L437)
[L454](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L454)
[L474](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L474)

_contracts/enforcer/HolographERC20.sol:_ [L564](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L564)

## 2. Custom error are cheaper than string messages

_contracts/HolographBridge.sol:_ [L148](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L148)
[L163](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L163)
[L203-L206](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L203-L206)
[L214](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L214)
[L224-L228](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L224-L228)
[L270](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L270)
[L352-L355](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L352-L355)

_contracts/abstract/ERC721H.sol:_ [L117](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L117)
[L123](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L123)
[L125](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L125)

_contracts/HolographFactory.sol:_ [L144](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L144)
[L220](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L220)
[L250-L255](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L250-L255)

_contracts/enforcer/HolographERC20.sol:_ [L192](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L192)
[L204](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L204)
[L349](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L349)
[L365](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L365)
[L387](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L387)
[L400](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L400)
[L445](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L445)
[L469](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L469)
[L482](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L482)
[L529](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L529)
[L539](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L539)
[L599](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L599)
[L620](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L620)
[L621](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L621)
[L627](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L627)
[L645](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L645)
[L653](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L653)
[L661](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L661)
[L684](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L684)
[L696](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L696)
[L698](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L698)

_contracts/enforcer/Holographer.sol:_ [L148](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L148)
[L166](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L166)

_contracts/enforcer/PA1D.sol:_ [L159](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L159)
[L174](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L174)
[L190](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L190)
[L411](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L411)
[L416](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L416)
[L435](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L435)
[L472](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L472)
[L477](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L477)

_contracts/HolographOperator.sol:_ [L350](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L350)
[L354](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L354)
[L415](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L415)
[L595](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L595)
[L728](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L728)
[L739](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L739)
[L756](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L756)
[L829](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L829)
[L839](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L839)
[L857](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L857)
[L863](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L863)
[L881](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L881)
[L889](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L889)
[L903](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L903)
[L911](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L911)
[L915](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L915)

_contracts/enforcer/HolographERC721.sol:_ [L212](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L212)
[L258](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L258)
[L263](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L263)
[L323](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L323)
[L370](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L370)
[L371](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L371)
[L404](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L404)
[L408](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L408)
[L420](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L420)
[L421](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L421)
[L458](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L458)
[L464-L470](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L464-L470)
[L484](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L484)
[L513](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L513)
[L622](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L622)
[L639](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L639)
[L700](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L700)
[L764](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L764)
[L816](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L816)
[L817](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L817)
[L818](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L818)
[L869](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L869)
[L870](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L870)

_contracts/abstract/ERC20H.sol:_ [L117](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L117)
[L125](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L125)
[L147](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L147)

## 3. Use `constant` and `immutable` for constants

_contracts/enforcer/HolographERC721.sol:_ [L762](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L762)
[L767](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L767)
[L769](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L769)

_contracts/enforcer/HolographERC20.sol:_ [L151](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L151)
[L171](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L171)
[L181](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L181)

## 4. Use `x < y + 1` in stead of `x <= y`

_contracts/HolographOperator.sol:_ [L354](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L354)
[L386](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L386)
[L728](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L728)
[L739](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L739)
[L756](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L756)
[L871](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L871)

_contracts/enforcer/HolographERC20.sol:_ [L349](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L349)
[L400](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L400)
[L450](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L450)
[L469](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L469)
[L529](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L529)
[L599](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L599)
[L629](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L629)

## 5. Cache storage variables in function call stack to save gas

_contracts/HolographOperator.sol:_ [L495-L524](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L495-L524)
[L363-L408](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L363-L408)
[L503-L517](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L503-L517)
[L867-L883](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L867-L883)

## 6. Place `i++` in an `unchecked` blocks in for-loops

_contracts/enforcer/HolographERC20.sol:_ [L564](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L564)

_contracts/enforcer/PA1D.sol:_ [L307](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L307)
[L323](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L323)
[L340](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L340)
[L394](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L394)
[L414](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L414)
[L432](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L432)
[L474](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L474)

_contracts/enforcer/HolographERC721.sol:_ [L357](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L357)
[L716](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L716)

_contracts/HolographOperator.sol:_ [L781](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L781)
[L871](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L871)

## 7. When comparing variables of type `uint`, use `require(x != 0)` instead of `require(x > 0)`

_contracts/enforcer/HolographERC721.sol:_ [L815](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L815)

_contracts/HolographOperator.sol:_ [L350](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L350)
[L363](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L363)
[L1126](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1126)

_contracts/HolographBridge.sol:_ [L218](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L218)

## 8. Cache array.length outside of for loops

_contracts/HolographOperator.sol:_ [L871](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L871)

## 9. Consider marking functions as `payable` if there is no risk of sending value through them

### This change will save gas each time a function is called

_contracts/enforcer/HolographERC20.sol:_ [L380](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L380)
[L415](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L415)

_contracts/enforcer/HolographERC721.sol:_ [L399](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L399)

_contracts/HolographOperator.sol:_ [L445](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L445)
[L484](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L484)
[L949](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L949)
[L969](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L969)
[L989](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L989)
[L1009](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1009)

_contracts/abstract/ERC20H.sol:_ [L123](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L123)

_contracts/abstract/ERC721H.sol:_ [L123](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L123)

_contracts/HolographFactory.sol:_ [L280](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L280)
[L300](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L300)

_contracts/HolographBridge.sol:_ [L472](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L472)
[L502](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L502)
[L522](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L522)

_contracts/enforcer/PA1D.sol:_ [L471](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L471)

## 10. Calldata is cheaper than memory for function input

_contracts/HolographFactory.sol:_ [L143](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L143)
[L193](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L193)
[L194](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L194)

_contracts/enforcer/HolographERC20.sol:_ [L499](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L499)
[L524](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L524)
[L641](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L641)

_contracts/enforcer/HolographERC721.sol:_ [L238](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L238)
[L456](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L456)
[L620](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L620)

_contracts/HolographOperator.sol:_ [L240](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L240)

_contracts/HolographBridge.sol:_ [L162](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L162)

_contracts/abstract/ERC721H.sol:_ [L140](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L140)

_contracts/abstract/ERC20H.sol:_ [L140](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L140)

_contracts/enforcer/Holographer.sol:_ [L147](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L147)

_contracts/enforcer/PA1D.sol:_ [L185](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L185)
[L316](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L316)
[L349](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L349)
[L365](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L365)
[L372](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L372)
[L426](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L426)
[L517](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L517)
[L683](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L683)

## 11. Explicitly assingning default values to variables is a waste of gas

### Use `uint256 i;` instead of `uint256 i = 0;`

_contracts/HolographBridge.sol:_ [L380](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L380)

_contracts/enforcer/HolographERC20.sol:_ [L564](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L564)

_contracts/enforcer/PA1D.sol:_ [L323](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L323)
[L340](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L340)
[L394](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L394)
[L414](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L414)
[L432](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L432)
[L454](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L454)
[L474](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L474)

_contracts/HolographOperator.sol:_ [L310](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L310)
[L311](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L311)
[L781](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L781)

_contracts/enforcer/HolographERC721.sol:_ [L357](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L357)
[L716](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L716)

## 12. Use multiple `require`s instead of a single one with `&&`

_contracts/enforcer/Holographer.sol:_ [L166](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L166)

_contracts/HolographOperator.sol:_ [L857](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L857)

_contracts/enforcer/HolographERC721.sol:_ [L263](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L263)
[L464-L470](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L464-L470)
