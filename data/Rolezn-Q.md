## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Missing Checks for Address(0x0)  | 69 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Use `safetransfer Instead Of `transfer  | 8 |
| [LOW&#x2011;3](#LOW&#x2011;3) | Unused `receive()` Function Will Lock Ether In Contract  | 6 |
| [LOW&#x2011;4](#LOW&#x2011;4) | Use `_safeMint` instead of `_mint` | 6 |
| [LOW&#x2011;5](#LOW&#x2011;5) | Missing Contract-existence Checks Before Low-level Calls | 2 |
| [LOW&#x2011;6](#LOW&#x2011;6) | Critical Changes Should Use Two-step Procedure | 20 |
| [LOW&#x2011;7](#LOW&#x2011;7) | Low Level Calls With Solidity Version 0.8.14 Can Result In Optimiser Bug | 5 |
| [LOW&#x2011;8](#LOW&#x2011;8) | Usage of `payable.transfer` can lead to loss of funds  | 1 |
| [LOW&#x2011;9](#LOW&#x2011;9) | `ecrecover` may return empty address | 2 |
| [LOW&#x2011;10](#LOW&#x2011;10) | `HolographFactory.deployHolographableContract()` can overpopulate `HolographRegistry._holographableContracts` | 1 |

Total: 120 instances over 10 issues

### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Event Is Missing Indexed Fields | 1 |
| [NC&#x2011;2](#NC&#x2011;2) | Public Functions Not Called By The Contract Should Be Declared External Instead | 11 |
| [NC&#x2011;3](#NC&#x2011;3) | Constants Should Be Defined Rather Than Using Magic Numbers | 22 |
| [NC&#x2011;4](#NC&#x2011;4) | Missing event for critical parameter change | 18 |
| [NC&#x2011;5](#NC&#x2011;5) | `require()` / `revert()` Statements Should Have Descriptive Reason Strings | 4 |
| [NC&#x2011;6](#NC&#x2011;6) | Implementation contract may not be initialized | 8 |
| [NC&#x2011;7](#NC&#x2011;7) | Large multiples of ten should use scientific notation | 19 |
| [NC&#x2011;8](#NC&#x2011;8) | Use of Block.Timestamp | 4 |
| [NC&#x2011;9](#NC&#x2011;9) | Non-usage of specific imports | 98 |
| [NC&#x2011;10](#NC&#x2011;10) | Lines are too long | 4 |
| [NC&#x2011;11](#NC&#x2011;11) | Use `bytes.concat()` | 44 |
| [NC&#x2011;12](#NC&#x2011;12) | Use of `ecrecover` is susceptible to signature malleability | 2 |
| [NC&#x2011;13](#NC&#x2011;13) | Commented code | 1 |

Total: 236 instances over 13 issues

## Low Risk Issues

### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Missing Checks for Address(0x0) 

Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

#### <ins>Proof Of Concept</ins>


```
193: function bridgeInRequest: address holographableContract
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L193

```
194: function bridgeInRequest: address hToken
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L194

```
195: function bridgeInRequest: address hTokenRecipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L195

```
247: function bridgeOutRequest: address holographableContract
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L247

```
297: function revertedBridgeOutRequest: address sender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L297

```
299: function revertedBridgeOutRequest: address holographableContract
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L299

```
344: function getBridgeOutRequestPayload: address holographableContract
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L344

```
452: function setFactory: address factory
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L452

```
472: function setHolograph: address holograph
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L472

```
502: function setOperator: address operator
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L502

```
522: function setRegistry: address registry
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L522

```
195: function deployHolographableContract: address signer
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L195

```
280: function setHolograph: address holograph
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L280

```
300: function setRegistry: address registry
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L300

```
445: function nonRevertingBridgeCall: address msgSender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L445

```
586: function send: address msgSender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L586

```
588: function send: address holographableContract
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L588

```
825: function topupUtilityToken: address operator
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L825

```
850: function bondUtilityToken: address operator
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L850

```
899: function unbondUtilityToken: address operator
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L899

```
899: function unbondUtilityToken: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L899

```
949: function setBridge: address bridge
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L949

```
969: function setHolograph: address holograph
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L969

```
989: function setInterfaces: address interfaces
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L989

```
1009: function setMessagingModule: address messagingModule
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1009

```
1029: function setRegistry: address registry
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1029

```
1049: function setUtilityToken: address utilityToken
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1049

```
326: function approve: address spender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L326

```
347: function burnFrom: address account
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L347

```
363: function decreaseAllowance: address spender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L363

```
415: function holographBridgeMint: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L415

```
420: function increaseAllowance: address spender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L420

```
440: function onERC20Received: address account
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L440

```
441: function onERC20Received: address sender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L441

```
461: function permit: address account
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L461

```
462: function permit: address spender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L462

```
497: function safeTransfer: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L497

```
497: function safeTransfer: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L497

```
521: function safeTransferFrom: address account
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L521

```
522: function safeTransferFrom: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L522

```
522: function safeTransferFrom: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L522

```
549: function sourceBurn: address from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L549

```
556: function sourceMint: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L556

```
573: function sourceTransfer: address from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L573

```
574: function sourceTransfer: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L574

```
580: function transfer: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L580

```
593: function transferFrom: address recipient
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L593

```
415: function bridgeOut: address sender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L415

```
453: function safeTransferFrom: address from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L453

```
454: function safeTransferFrom: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L454

```
453: function safeTransferFrom: address from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L453

```
454: function safeTransferFrom: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L454

```
508: function sourceMint: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L508

```
577: function sourceTransfer: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L577

```
588: function transfer: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L588

```
617: function transferFrom: address from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L617

```
618: function transferFrom: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L618

```
617: function transferFrom: address from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L617

```
618: function transferFrom: address to
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L618

```
752: function onERC721Received: address _operator
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L752

```
753: function onERC721Received: address _from
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L753

```
471: function configurePayouts: address payable[] memory addresses
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L471

```
507: function getTokenPayout: address tokenAddress
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L507

```
531: function setRoyalties: address payable receiver
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L531

```
231: function send: address msgSender
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L231

```
320: function setBridge: address bridge
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L320

```
340: function setInterfaces: address interfaces
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L340

```
360: function setLZEndpoint: address lZEndpoint
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L360

```
380: function setOperator: address operator
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L380



#### <ins>Recommended Mitigation Steps</ins>

Consider adding explicit zero-address validation on input parameters of address type.



### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Use `safetransfer` Instead Of `transfer` 

It is good to add a `require()` statement that checks the return value of token transfers or to use something like OpenZeppelin’s `safeTransfer`/`safeTransferFrom` unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

For example, Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert.

#### <ins>Proof Of Concept</ins>


```
400: _utilityToken().transfer(job.operator, leftovers);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L400

```
596: payable(hToken).transfer(hlgFee);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L596

```
839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L839

```
889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L889

```
932: require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L932

```
396: addresses[i].transfer(sending);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L396

```
416: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L416

```
439: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L439



#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeTransfer`/`safeTransferFrom` or `require()` consistently.



### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> Unused `receive()` Function Will Lock Ether In Contract 

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

#### <ins>Proof Of Concept</ins>


```
1209: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1209

```
212: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L212

```
212: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L212

```
223: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L223

```
251: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L251

```
962: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L962



#### <ins>Recommended Mitigation Steps</ins>

The function should call another function, otherwise it should revert



### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Use `_safeMint` instead of `_mint`

According to openzepplin's ERC721, the use of `_mint` is discouraged, use _safeMint whenever possible.
https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

#### <ins>Proof Of Concept</ins>


```
385: _mint(to, amount);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L385

```
416: _mint(to, amount);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L416

```
557: _mint(to, amount);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L557

```
565: _mint(wallets[i], amounts[i]);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L565

```
406: _mint(to, tokenId);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L406

```
514: _mint(to, token);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L514



#### <ins>Recommended Mitigation Steps</ins>

Use `_safeMint` whenever possible instead of `_mint`



### <a href="#Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Missing Contract-existence Checks Before Low-level Calls

Low-level calls return success if there is no code present at the specified address. 

#### <ins>Proof Of Concept</ins>


```
164: .delegatecall(abi.encodeWithSignature("init(bytes)", initCode));
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L164

```
259: (bool success, bytes memory returnData) = _royalties().delegatecall(
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L259




#### <ins>Recommended Mitigation Steps</ins>

In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`



### <a href="#Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```
452: function setFactory(address factory) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L452

```
472: function setHolograph(address holograph) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L472

```
502: function setOperator(address operator) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L502

```
522: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L522

```
280: function setHolograph(address holograph) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L280

```
300: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L300

```
949: function setBridge(address bridge) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L949

```
969: function setHolograph(address holograph) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L969

```
989: function setInterfaces(address interfaces) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L989

```
1009: function setMessagingModule(address messagingModule) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1009

```
1029: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1029

```
1049: function setUtilityToken(address utilityToken) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1049

```
483: function setApprovalForAll(address to, bool approved) external {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L483

```
529: function setRoyalties(
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L529

```
320: function setBridge(address bridge) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L320

```
340: function setInterfaces(address interfaces) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L340

```
360: function setLZEndpoint(address lZEndpoint) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L360

```
380: function setOperator(address operator) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L380

```
441: function setBaseGas(uint256 baseGas) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L441

```
470: function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L470



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.



### <a href="#Summary">[LOW&#x2011;7]</a><a name="LOW&#x2011;7"> Low Level Calls With Solidity Version 0.8.14 Can Result In Optimiser Bug

The project contracts in scope are using low level calls with solidity version before 0.8.14 which can result in optimizer bug.
https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

Simliar findings in Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#5-low-level-calls-with-solidity-version-0814-can-result-in-optimiser-bug

#### <ins>Proof Of Concept</ins>

POC can be found in the above medium reference url.

Functions that execute low level calls in contracts with solidity version under 0.8.14

```
447: assembly {
      /**
       * @dev remove gas price from end
       */
      calldatacopy(0, payload.offset, sub(payload.length, 0x20))
      /**
       * @dev hToken recipient is injected right before making the call
       */
      mstore(0x84, msgSender)
      /**
       * @dev make non-reverting call
       */
      let result := call(
        /// @dev gas limit is retrieved from last 32 bytes of payload in-memory value
        mload(sub(payload.length, 0x40)),
        /// @dev destination is bridge contract
        sload(_bridgeSlot),
        /// @dev any value is passed along
        callvalue(),
        /// @dev data is retrieved from 0 index memory position
        0,
        /// @dev everything except for last 32 bytes (gas limit) is sent
        sub(payload.length, 0x40),
        0,
        0
      )
      if eq(result, 0) {
        revert(0, 0)
      }
      return(0, 0)
    }
  }
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L447-478

```
550: assembly {
      calldatacopy(0, bridgeInRequestPayload.offset, sub(bridgeInRequestPayload.length, 0x40))
      /**
       * @dev bridgeInRequest doNotRevert is purposefully set to false so a rever would happen
       */
      mstore8(0xE3, 0x00)
      let result := call(gas(), sload(_bridgeSlot), callvalue(), 0, sub(bridgeInRequestPayload.length, 0x40), 0, 0)
      /**
       * @dev if for some reason the call does not revert, it is force reverted
       */
      if eq(result, 1) {
        returndatacopy(0, 0, returndatasize())
        revert(0, returndatasize())
      }
      /**
       * @dev remaining gas is set as the return value
       */
      mstore(0x00, gas())
      return(0x00, 0x20)
    }
  }
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L550-570


```
221:  assembly {
          sstore(_reentrantSlot, 0x0000000000000000000000000000000000000000000000000000000000000001)
          sstore(_ownerSlot, caller())
          sourceContract := sload(_sourceContractSlot)
        }
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L221-225

```
973: assembly {
        calldatacopy(0, 0, calldatasize())
        let result := delegatecall(gas(), _target, 0, calldatasize(), 0, 0)
        returndatacopy(0, 0, returndatasize())
        switch result
        case 0 {
          revert(0, returndatasize())
        }
        default {
          return(0, returndatasize())
        }
      }
    } else {
      assembly {
        calldatacopy(0, 0, calldatasize())
        mstore(calldatasize(), caller())
        let result := call(gas(), sload(_sourceContractSlot), callvalue(), 0, add(calldatasize(), 32), 0, 0)
        returndatacopy(0, 0, returndatasize())
        switch result
        case 0 {
          revert(0, returndatasize())
        }
        default {
          return(0, returndatasize())
        }
      }
    }
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L973-998

```
186: assembly {
      /**
       * @dev check if msg.sender is LayerZero Endpoint
       */
      switch eq(sload(_lZEndpointSlot), caller())
      case 0 {
        /**
         * @dev this is the assembly version of -> revert("HOLOGRAPH: LZ only endpoint");
         */
        mstore(0x80, 0x08c379a000000000000000000000000000000000000000000000000000000000)
        mstore(0xa0, 0x0000002000000000000000000000000000000000000000000000000000000000)
        mstore(0xc0, 0x0000001b484f4c4f47524150483a204c5a206f6e6c7920656e64706f696e7400)
        mstore(0xe0, 0x0000000000000000000000000000000000000000000000000000000000000000)
        revert(0x80, 0xc4)
      }
      let ptr := mload(0x40)
      calldatacopy(add(ptr, 0x0c), _srcAddress.offset, _srcAddress.length)
      /**
       * @dev check if LZ from address is same as address(this)
       */
      switch eq(mload(ptr), address())
      case 0 {
        /**
         * @dev this is the assembly version of -> revert("HOLOGRAPH: unauthorized sender");
         */
        mstore(0x80, 0x08c379a000000000000000000000000000000000000000000000000000000000)
        mstore(0xa0, 0x0000002000000000000000000000000000000000000000000000000000000000)
        mstore(0xc0, 0x0000001e484f4c4f47524150483a20756e617574686f72697a65642073656e64)
        mstore(0xe0, 0x6572000000000000000000000000000000000000000000000000000000000000)
        revert(0x80, 0xc4)
      }
    }
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L186-217





#### <ins>Recommended Mitigation Steps</ins>

Consider upgrading to at least solidity v0.8.15.



### <a href="#Summary">[LOW&#x2011;8]</a><a name="LOW&#x2011;8"> Usage of `payable.transfer` can lead to loss of funds 

The funds that are to be sent can be lost. The issues with `transfer()` are outlined here:
https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/

#### <ins>Proof Of Concept</ins>


```
596: payable(hToken).transfer(hlgFee);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L596



#### <ins>Recommended Mitigation Steps</ins>

Using low-level `call.value(amount)` with the corresponding result check or using the OpenZeppelin `Address.sendValue` is advised:
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Address.sol#L60



### <a href="#Summary">[LOW&#x2011;9]</a><a name="LOW&#x2011;9"> `ecrecover` may return empty address

There is a common issue that ecrecover returns empty (0x0) address when the signature is invalid. function `_verifySigner` should check that before returning the result of ecrecover.

#### <ins>Proof Of Concept</ins>


```
351: return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L333

```
334: ecrecover(hash, v, r, s) == signer);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L334



#### <ins>Recommended Mitigation Steps</ins>

See the solution here: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v3.4.0/contracts/cryptography/ECDSA.sol#L68


### <a href="#Summary">[LOW&#x2011;10]</a><a name="LOW&#x2011;10"> `HolographFactory.deployHolographableContract()` can overpopulate `HolographRegistry._holographableContracts`

The `require` checks in `HolographFactory.deployHolographableContract()` can easily by bypassed by sending an invalid signature and `signer` = 0x0.

As a result, this will deploy a holographableContract and update the `HolographRegistry` and push an additional item to `HolographRegistry._holographableContracts`.

Due to `_holographableContracts.push(contractAddress);` in `HolographRegistryInterface(registry).setHolographedHashAddress(hash, holographerAddress);`

A malicious user can overpopulate the `_holographableContracts` array with redundant data, increasing gas costs when `_holographableContracts` is iterated through.

#### <ins>Proof Of Concept</ins>
```
HolographRegistryInterface(registry).setHolographedHashAddress(hash, holographerAddress);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L259

#### <ins>Recommended Mitigation Steps</ins>
Implement valid access control on the `HolographFactory.deployHolographableContract()` to ensure only the relevant can deploy

## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Event Is Missing Indexed Fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). 

Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

#### <ins>Proof Of Concept</ins>


```
event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L153






### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Public Functions Not Called By The Contract Should Be Declared External Instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public.

#### <ins>Proof Of Concept</ins>


```
function burnFrom(address account, uint256 amount) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L347

```
function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L363

```
function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L420

```
function onERC20Received(
    address account,
    address sender,
    uint256 amount,
    bytes calldata data
  ) public returns (bytes4) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L439

```
function permit(
    address account,
    address spender,
    uint256 amount,
    uint256 deadline,
    uint8 v,
    bytes32 r,
    bytes32 s
  ) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L460

```
function safeTransfer(address recipient, uint256 amount) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L492

```
function safeTransfer(
    address recipient,
    uint256 amount,
    bytes memory data
  ) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L496

```
function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L471

```
function getEthPayout() public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L497

```
function getTokenPayout(address tokenAddress) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L507

```
function getTokensPayout(address[] memory tokenAddresses) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L517

```
function setRoyalties(
    uint256 tokenId,
    address payable receiver,
    uint256 bp
  ) public onlyOwner {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L529





### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Constants Should Be Defined Rather Than Using Magic Numbers

#### <ins>Proof Of Concept</ins>

```
314: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L314


```
254: _blockTime = 60;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L254

```
256: _baseBondAmount = 100 * (10**18);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L256

```
261: _operatorThreshold = 1000;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L261

```
263: _operatorThresholdStep = 10;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L263

```
265: _operatorThresholdDivisor = 100;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L265

```
358: if (timeDifference < 6) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L358


```
1203: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1203


```
721: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L721

```
916: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L916

```
390: require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L390

```
395: sending = ((bps[i] * balance) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L395

```
411: require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L411

```
415: sending = ((bps[i] * balance) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L415

```
435: require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L435

```
438: sending = ((bps[i] * balance) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L438

```
477: require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L477

```
551: return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L551

```
553: return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L553

```
213: mstore(0xc0, 0x0000001e484f4c4f47524150483a20756e617574686f72697a65642073656e64)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L213

```
274: return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L274

```
293: return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L293





### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Missing event for critical parameter change

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

#### <ins>Proof Of Concept</ins>


```
452: function setFactory(address factory) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L452

```
472: function setHolograph(address holograph) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L472

```
502: function setOperator(address operator) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L502

```
522: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L522

```
280: function setHolograph(address holograph) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L280

```
300: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L300

```
949: function setBridge(address bridge) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L949

```
969: function setHolograph(address holograph) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L969

```
989: function setInterfaces(address interfaces) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L989

```
1009: function setMessagingModule(address messagingModule) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1009

```
1029: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1029

```
1049: function setUtilityToken(address utilityToken) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1049

```
320: function setBridge(address bridge) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L320

```
340: function setInterfaces(address interfaces) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L340

```
360: function setLZEndpoint(address lZEndpoint) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L360

```
380: function setOperator(address operator) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L380

```
441: function setBaseGas(uint256 baseGas) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L441

```
470: function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L470





### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> `require()` / `revert()` Statements Should Have Descriptive Reason Strings

#### <ins>Proof Of Concept</ins>


```
578: revert();
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L578

```
341: revert();
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L341

```
1215: revert();
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1215


```
417: revert();
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L417






### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```
contract HolographBridge is Admin, Initializable, HolographBridgeInterface 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L122

```
contract HolographFactory is Admin, Initializable, Holographable, HolographFactoryInterface 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L123

```
contract HolographOperator is Admin, Initializable, HolographOperatorInterface 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L125

```
contract Holographer is Admin, Initializable, HolographerInterface 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L115

```
contract HolographERC20 is Admin, Owner, Initializable, NonReentrant, EIP712, HolographERC20Interface 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L138

```
contract HolographERC721 is Admin, Owner, HolographERC721Interface, Initializable 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L132

```
contract PA1D is Admin, Owner, Initializable 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L120

```
contract LayerZeroModule is Admin, Initializable, CrossChainMessageInterface, LayerZeroModuleInterface 
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L122





### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

#### <ins>Proof Of Concept</ins>


```
390: require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L390

```
395: sending = ((bps[i] * balance) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L395

```
415: sending = ((bps[i] * balance) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L415

```
438: sending = ((bps[i] * balance) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L438

```
411: require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L411

```
435: require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L435

```
477: require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L477

```
551: return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L551

```
553: return (_getReceiver(tokenId), (_getBp(tokenId) * value) / 10000);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L553

```
641: return (_getDefaultBp() * amount) / 10000;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L641

```
643: return (_getBp(tokenId) * amount) / 10000;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L643





### <a href="#Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
References: SWC ID: 116

#### <ins>Proof Of Concept</ins>


```
345: uint256 elapsedTime = block.timestamp - uint256(job.startTimestamp);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L345

```
499: uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L499

```
531: (block.timestamp << 16) |
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L531

```
469: require(block.timestamp <= deadline, "ERC20: expired deadline");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L469



#### <ins>Recommended Mitigation Steps</ins>
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.



### <a href="#Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

#### <ins>Proof Of Concept</ins>


```
104: import "./abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L104

```
105: import "./abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L105

```
107: import "./interface/HolographERC20Interface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L107

```
108: import "./interface/Holographable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L108

```
109: import "./interface/HolographInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L109

```
110: import "./interface/HolographBridgeInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L110

```
111: import "./interface/HolographFactoryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L111

```
112: import "./interface/HolographOperatorInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L112

```
113: import "./interface/HolographRegistryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L113

```
114: import "./interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L114

```
104: import "./abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L104

```
105: import "./abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L105

```
107: import "./enforcer/Holographer.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L107

```
109: import "./interface/Holographable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L109

```
110: import "./interface/HolographFactoryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L110

```
111: import "./interface/HolographRegistryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L111

```
112: import "./interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L112

```
114: import "./struct/DeploymentConfig.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L114

```
115: import "./struct/Verification.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L115

```
104: import "./abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L104

```
105: import "./abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L105

```
107: import "./interface/CrossChainMessageInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L107

```
108: import "./interface/HolographBridgeInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L108

```
109: import "./interface/HolographERC20Interface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L109

```
110: import "./interface/HolographInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L110

```
111: import "./interface/HolographOperatorInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L111

```
112: import "./interface/HolographRegistryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L112

```
113: import "./interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L113

```
114: import "./interface/HolographInterfacesInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L114

```
115: import "./interface/Ownable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L115

```
117: import "./struct/OperatorJob.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L117

```
104: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L104

```
104: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L104

```
104: import "../abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L104

```
105: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L105

```
107: import "../interface/HolographInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L107

```
108: import "../interface/HolographerInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L108

```
109: import "../interface/HolographRegistryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L109

```
110: import "../interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L110

```
104: import "../abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L104

```
105: import "../abstract/EIP712.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L105

```
106: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L106

```
107: import "../abstract/NonReentrant.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L107

```
108: import "../abstract/Owner.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L108

```
110: import "../enum/HolographERC20Event.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L110

```
111: import "../enum/InterfaceType.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L111

```
113: import "../interface/ERC20.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L113

```
114: import "../interface/ERC20Burnable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L114

```
115: import "../interface/HolographERC20Interface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L115

```
116: import "../interface/ERC20Metadata.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L116

```
117: import "../interface/ERC20Permit.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L117

```
118: import "../interface/ERC20Receiver.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L118

```
119: import "../interface/ERC20Safer.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L119

```
120: import "../interface/ERC165.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L120

```
121: import "../interface/Holographable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L121

```
122: import "../interface/HolographedERC20.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L122

```
123: import "../interface/HolographInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L123

```
124: import "../interface/HolographerInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L124

```
125: import "../interface/HolographRegistryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L125

```
126: import "../interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L126

```
127: import "../interface/HolographInterfacesInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L127

```
128: import "../interface/Ownable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L128

```
130: import "../library/ECDSA.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L130

```
104: import "../abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L104

```
105: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L105

```
106: import "../abstract/Owner.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L106

```
108: import "../enum/HolographERC721Event.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L108

```
109: import "../enum/InterfaceType.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L109

```
111: import "../interface/ERC165.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L111

```
112: import "../interface/ERC721.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L112

```
113: import "../interface/HolographERC721Interface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L113

```
114: import "../interface/ERC721Metadata.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L114

```
115: import "../interface/ERC721TokenReceiver.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L115

```
116: import "../interface/Holographable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L116

```
117: import "../interface/HolographedERC721.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L117

```
118: import "../interface/HolographInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L118

```
119: import "../interface/HolographerInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L119

```
120: import "../interface/HolographRegistryInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L120

```
121: import "../interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L121

```
122: import "../interface/HolographInterfacesInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L122

```
123: import "../interface/PA1DInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L123

```
124: import "../interface/Ownable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L124

```
104: import "../abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L104

```
105: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L105

```
106: import "../abstract/Owner.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L106

```
108: import "../interface/ERC20.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L108

```
109: import "../interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L109

```
110: import "../interface/PA1DInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L110

```
112: import "../struct/ZoraBidShares.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L112

```
104: import "../abstract/Admin.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L104

```
105: import "../abstract/Initializable.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L105

```
107: import "../enum/ChainIdType.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L107

```
109: import "../interface/CrossChainMessageInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L109

```
110: import "../interface/HolographOperatorInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L110

```
111: import "../interface/InitializableInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L111

```
112: import "../interface/HolographInterfacesInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L112

```
113: import "../interface/LayerZeroModuleInterface.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L113

```
114: import "../interface/LayerZeroOverrides.sol";
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L114




#### <ins>Recommended Mitigation Steps</ins>

Use specific imports syntax per solidity docs recommendation.




### <a href="#Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length
Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

#### <ins>Proof Of Concept</ins>

```
511: // this will prevent possible tokenId overlap if minting simultaneously on multiple chains is possible
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L511

```
542: //   function sourceMintBatch(address[] calldata wallets, uint224[] calldata tokenIds) external onlySource {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L542

```
618: // Hint taken from Manifold's RoyaltyEngine(https://github.com/manifoldxyz/royalty-registry-solidity/blob/main/contracts/RoyaltyEngineV1.sol)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L618

```
619: // To be quite honest, SuperRare is a closed marketplace. They're working on opening it up but looks like they want to use private smart contracts.
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L619





### <a href="#Summary">[NC&#x2011;11]</a><a name="NC&#x2011;11"> Use `bytes.concat()`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

#### <ins>Proof Of Concept</ins>


```
405: samplePayload = abi.encodePacked(encodedData, gasLimit, gasPrice)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographBridge.sol#L405

```
207: abi.encodePacked(
        config.contractType,
        config.chainType,
        config.salt,
        keccak256(config.byteCode)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L207

```
226: uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L226

```
333: return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L333

```
499: uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce()
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L499

```
635: encodedData = abi.encodePacked(encodedData, gasLimit, gasPrice)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L635

```
512: uint256 token = uint256(bytes32(abi.encodePacked(_chain()
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L512

```
521: return uint256(bytes32(abi.encodePacked(_chain()
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L521

```
533: //       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L533

```
548: //       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L548

```
565: //       token = uint256(bytes32(abi.encodePacked(chain, startingTokenId)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L565

```
257: bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L257

```
269: bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L269

```
280: bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L280

```
292: bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L292

```
308: slot = keccak256(abi.encodePacked(i, slot)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L308

```
324: slot = keccak256(abi.encodePacked(i, slot)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L324

```
341: slot = keccak256(abi.encodePacked(i, slot)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L341

```
357: slot = keccak256(abi.encodePacked(i, slot)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L357

```
366: bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L366

```
373: bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L373

```
243: abi.encodePacked(address(this)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L243

```
247: abi.encodePacked(uint16(1)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L247

```
269: bytes memory adapterParams = abi.encodePacked(
      uint16(1)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L269




#### <ins>Recommended Mitigation Steps</ins>

Use `bytes.concat()` and upgrade to at least Solidity version 0.8.4 if required. 



### <a href="#Summary">[NC&#x2011;12]</a><a name="NC&#x2011;12"> Use of `ecrecover` is susceptible to signature malleability

The built-in EVM precompile `ecrecover` is susceptible to signature malleability, which could lead to replay attacks.
References:  https://swcregistry.io/docs/SWC-117,  https://swcregistry.io/docs/SWC-121, and  https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57.
While this is not immediately exploitable, this may become a vulnerability if used elsewhere.

#### <ins>Proof Of Concept</ins>


```
351: return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L351

```
334: ecrecover(hash, v, r, s) == signer);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L334



#### Recommended Mitigation Steps
Consider using OpenZeppelin’s ECDSA library (which prevents this malleability) instead of the built-in function.


### <a href="#Summary">[NC&#x2011;13]</a><a name="NC&#x2011;13"> Commented code

#### <ins>Proof Of Concept</ins>

```
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
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L527-L570