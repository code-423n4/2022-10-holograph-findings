# NON-CRITICAL

## 1. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom


It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This similar medium-severity finding from Consensys Diligence Audit of Fei Protocol: https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call


```
400	_utilityToken().transfer(job.operator, leftovers);
596	payable(hToken).transfer(hlgFee);
839	require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
889	require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
932	require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
396	addresses[i].transfer(sending);
416	require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
439	require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol


## 2. Open TODOs


Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
701	// TODO: move the bit-shifting around to have it be sequential
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol


## 3. Consider adding checks for signature malleability


Use OpenZeppelin’s `ECDSA` contract rather than calling `ecrecover()` directly

```
333	return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
334		ecrecover(hash, v, r, s) == signer);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

## 4. NatSpec is incomplete


Check here for NatSpec rules:
https://docs.soliditylang.org/en/develop/natspec-format.html

```
missing @return
162	function init(bytes memory initPayload) external override returns (bytes4) {
301	) external returns (string memory revertReason) {
402	function getFactory() external view returns (address factory) {
462	function getHolograph() external view returns (address holograph) {
482	function getJobNonce() external view returns (uint256 jobNonce) {
492	function getOperator() external view returns (address operator) {
512	function getRegistry() external view returns (address registry) {
531	function _factory() private view returns (HolographFactoryInterface factory) {
540	function _holograph() private view returns (HolographInterface holograph) {
549	function _jobNonce() private returns (uint256 jobNonce) {
559	function _operator() private view returns (HolographOperatorInterface operator) {
568	function _registry() private view returns (HolographRegistryInterface registry) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol
```
missing @return
240	function init(bytes memory initPayload) external override returns (bytes4) {
717	function getTotalPods() external view returns (uint256 totalPods) {
939	function getBridge() external view returns (address bridge) {
959	function getHolograph() external view returns (address holograph) {
979	function getInterfaces() external view returns (address interfaces) {
999	function getMessagingModule() external view returns (address messagingModule) {
1019	function getRegistry() external view returns (address registry) {
1039	function getUtilityToken() external view returns (address utilityToken) {
1058	function _bridge() private view returns (address bridge) {
1112	function _jobNonce() private returns (uint256 jobNonce) {
1160	function _getBaseBondAmount(uint256 pod) private view returns (uint256) {
1167	function _getCurrentBondAmount(uint256 pod) private view returns (uint256) {
1189	) private view returns (uint256) {
1198	function _isContract(address contractAddress) private view returns (bool) {
missing @param bridgeInRequestPayload
484	function crossChainMessage(bytes calldata bridgeInRequestPayload) external payable {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
missing @return
143	function init(bytes memory initPayload) external override returns (bytes4) {
270	function getHolograph() external view returns (address holograph) {
290	function getRegistry() external view returns (address registry) {
309	function _isContract(address contractAddress) private view returns (bool) {
326	) private pure returns (bool) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol
```
missing @return
158	function init(bytes memory initPayload) external override returns (bytes4) {
256	) external view returns (uint256 hlgFee, uint256 msgFee) {
281	) external view returns (uint256 hlgFee) {
299	returns (uint128 dstPriceRatio, uint128 dstGasPriceInWei)
310	function getBridge() external view returns (address bridge) {
330	function getInterfaces() external view returns (address interfaces) {
350	function getLZEndpoint() external view returns (address lZEndpoint) {
370	function getOperator() external view returns (address operator) {
389	function _bridge() private view returns (address bridge) {
431	function getBaseGas() external view returns (uint256 baseGas) {
450	function _baseGas() private view returns (uint256 baseGas) {
460	function getGasPerByte() external view returns (uint256 gasPerByte) {
479	function _gasPerByte() private view returns (uint256 gasPerByte) {
missing @param for all variables
227	function send(
251	function getMessageFee(
277	function getHlgFee(
296	function _getPricing(LayerZeroOverrides lz, uint16 lzDestChain)
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol
```
missing @title
missing @author
missing @return
147	function init(bytes memory initPayload) external override returns (bytes4) {
174	function getDeploymentBlock() external view returns (address holograph) {
183	function getHolograph() external view returns (address holograph) {
192	function getHolographEnforcer() public view returns (address) {
205	function getOriginChain() external view returns (uint32 originChain) {
214	function getSourceContract() external view returns (address sourceContract) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol
```
missing @return
173	function init(bytes memory initPayload) external override returns (bytes4) {
185	function initPA1D(bytes memory initPayload) external returns (bytes4) {
298	function _getPayoutAddresses() private view returns (address payable[] memory addresses) {
332	function _getPayoutBps() private view returns (uint256[] memory bps) {

missing @param initPayload
185	function initPA1D(bytes memory initPayload) external returns (bytes4) {
missing @param addresses
316	function _setPayoutAddresses(address payable[] memory addresses) private {
missing @param bps
349	function _setPayoutBps(uint256[] memory bps) private {
missing @param tokenName, @return
365	function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {
missing @param tokenName, @param tokenAddress
372	function _setTokenAddress(string memory tokenName, address tokenAddress) private {
missing @param tokenId, @param value, @return, @return
549	function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
missing @param tokenId, @return
558	function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
missing @param tokenId, @return
569	function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
missing @param tokenId, @return, @return
590	function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
missing @param tokenId, @return, @return
604	function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
```
missing @return
218	function init(bytes memory initPayload) external override returns (bytes4) {
306	function DOMAIN_SEPARATOR() public view returns (bytes32) {
310	function name() public view returns (string memory) {
318	function symbol() public view returns (string memory) {
322	function totalSupply() public view returns (uint256) {

missing @param interfaceId, @return
282	function supportsInterface(bytes4 interfaceId) external view returns (bool) {
missing @param account, @param spender, @ return
297	function allowance(address account, address spender) public view returns (uint256) {
missing @param account, @ return
301	function balanceOf(address account) public view returns (uint256) {
missing @param account, @ return
314	function nonces(address account) public view returns (uint256) {
missing @param spender, @param amount, @ return
326	function approve(address spender, uint256 amount) public returns (bool) {
missing @param account, @param amount, @ return
347	function burnFrom(address account, uint256 amount) public returns (bool) {
missing @param spender, @param subtractedValue, @ return
363	function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
missing @param fromChain, @param payload, @ return
380	function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
missing @param for all variables
392	function bridgeOut(
415	function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
420 function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
439	function onERC20Received(
460	function permit(
492	function safeTransfer(address recipient, uint256 amount) public returns (bool) {
496	function safeTransfer(
512	function safeTransferFrom(
520	function safeTransferFrom(
549	function sourceBurn(address from, uint256 amount) external onlySource {
563	function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
572	unction sourceTransfer(
580	function transfer(address recipient, uint256 amount) public returns (bool) {
591	function transferFrom(
615	function _approve(
626	function _burn(address account, uint256 amount) private {
637	function _checkOnERC20Received(
690	function _transfer(
711	function _useNonce(address account) private returns (uint256 current) {
736	function _interfaces() private view returns (address) {
740	function owner() public view override returns (address) {
748	function _holograph() private view returns (HolographInterface holograph) {
754	function _isEventRegistered(HolographERC20Event _eventName) private view returns (bool) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol
```
missing @title
missing @author
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol
```
missing @title
missing @author
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol


## 5. Missing event for critical parameter change


Events help non-contract tools to track changes, and events prevent users from being surprised by changes

```
452	function setFactory(address factory) external onlyAdmin {
472	function setHolograph(address holograph) external onlyAdmin {
502	function setOperator(address operator) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol
```
949	function setBridge(address bridge) external onlyAdmin {
969	function setHolograph(address holograph) external onlyAdmin {
989	function setInterfaces(address interfaces) external onlyAdmin {
1009	function setMessagingModule(address messagingModule) external onlyAdmin {
1029	function setRegistry(address registry) external onlyAdmin {
1049	function setUtilityToken(address utilityToken) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
280	function setHolograph(address holograph) external onlyAdmin {
300	function setRegistry(address registry) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol
```
320	function setBridge(address bridge) external onlyAdmin {
340	function setInterfaces(address interfaces) external onlyAdmin {
360	function setLZEndpoint(address lZEndpoint) external onlyAdmin {
380	function setOperator(address operator) external onlyAdmin {
470	function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol


## 6. Remove commented out code

```
438	////  _bondedOperators[msg.sender] += reward;
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
397	// sent = sent + sending;
417	// sent = sent + sending;
440	// sent = sent + sending;
...
580	// struct Part {
581		//     address payable account;
582		//     uint96 value;
583	// }
584	// function getRoyalties(uint256 tokenId) public view returns (Part[] memory) {
585		//     return royalties[id];
586	// }
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
```
527	//   function sourceMintBatch(address to, uint224[] calldata tokenIds) external onlySource {
528	//	     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
529	//	     uint32 chain = _chain();
530	//	     uint256 token;
531	//	     for (uint256 i = 0; i < tokenIds.length; i++) {
532	//	       require(!_burnedTokens[token], "ERC721: can't mint burned token");
533	//	       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])));
534	//	       require(!_burnedTokens[token], "ERC721: can't mint burned token");
535	//	       _mint(to, token);
536	//	     }
537	//   }
...
542	//   function sourceMintBatch(address[] calldata wallets, uint224[] calldata tokenIds) external onlySource {
543	//	     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
544	//	     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
545	//	     uint32 chain = _chain();
546	//	     uint256 token;
547	//	     for (uint256 i = 0; i < tokenIds.length; i++) {
548	//	       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])));
549	//	       require(!_burnedTokens[token], "ERC721: can't mint burned token");
550	//	       _mint(wallets[i], token);
551	//	     }
552	//   }
...
557	//   function sourceMintBatchIncremental(
558	//     	address to,
559	//    	 uint224 startingTokenId,
560	//    	 uint256 length
561	// 	)	 external onlySource {
562	//    	 uint32 chain = _chain();
563	//   	  uint256 token;
564	//    	 for (uint256 i = 0; i < length; i++) {
565	//     	  token = uint256(bytes32(abi.encodePacked(chain, startingTokenId)));
566	//     	  require(!_burnedTokens[token], "ERC721: can't mint burned token");
567	//    	   _mint(to, token);
568	//    	   startingTokenId++;
569	//    	 }
570	//   }
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

## 7. Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

Scientific notation should be used for better code readability

```
256	_baseBondAmount = 100 * (10**18); // one single token unit * 100
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
274	return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
293	return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol


## 8. `public` functions not called by the contract should be declared `external` instead


Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from `external` to `public` and can save gas by doing so

```
472	function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
488	function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
497	function getEthPayout() public {
507	function getTokenPayout(address tokenAddress) public {
517	function getTokensPayout(address[] memory tokenAddresses) public {
549	function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
558	function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
569	function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
590	function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
604	function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
621	function tokenCreator(
634	function calculateRoyaltyFee(
649	function marketContract() public view returns (address) {
655	function tokenCreators(uint256 tokenId) public view returns (address) {
665	function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
683	function getTokenAddress(string memory tokenName) public view returns (address) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
```
306	function DOMAIN_SEPARATOR() public view returns (bytes32) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

## 9. `require()`/`revert()` statements should have descriptive reason strings

```
328	require(SourceERC20().beforeApprove(msg.sender, spender, amount));
332	require(SourceERC20().afterApprove(msg.sender, spender, amount));
339	require(SourceERC20().beforeBurn(msg.sender, amount));
343	require(SourceERC20().afterBurn(msg.sender, amount));
354	require(SourceERC20().beforeBurn(account, amount));
358	require(SourceERC20().afterBurn(account, amount));
371	require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
375	require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
430	require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
434	require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
447	require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));
455	require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));
484	require(SourceERC20().beforeApprove(account, spender, amount));
488	require(SourceERC20().afterApprove(account, spender, amount));
502	require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));
507	require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));
536	require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));
541	require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));
582	require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));
586	require(SourceERC20().afterTransfer(msg.sender, recipient, amount));
606	require(SourceERC20().beforeTransfer(account, recipient, amount));
610	require(SourceERC20().afterTransfer(account, recipient, amount));
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

## 10. Not using the named return variables anywhere in the function is confusing


Consider changing the variable to be an unnamed one

```
301	) external returns (string memory revertReason) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol
```
717	function getTotalPods() external view returns (uint256 totalPods) {
804	function getBondedAmount(address operator) external view returns (uint256 amount) {
814	function getBondedPod(address operator) external view returns (uint256 pod) {
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
256	) external view returns (uint256 hlgFee, uint256 msgFee) {
281	) external view returns (uint256 hlgFee) {
299	returns (uint128 dstPriceRatio, uint128 dstGasPriceInWei)
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol


# LOW

## 1. Unused/empty receive()/fallback() function


If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. `require(msg.sender == address(weth))`). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

```
1209	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
```
223	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol
```
251	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol
```
962	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol
```
212	receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol
```
212	 receive() external payable {}
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol

