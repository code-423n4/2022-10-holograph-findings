## [L-01] BUGS IN SOLIDITY 0.8.13
This protocol uses Solidity 0.8.13, which has a size check bug, which involves nested calldata array and `abi.encode`, and an optimizer bug that affects inline assembly. The code in scope appears to not be affected by these bugs but the protocol team should be aware of them.

Please see the following links for reference:
- https://blog.soliditylang.org/2022/05/17/calldata-reencode-size-check-bug/
- https://blog.soliditylang.org/2022/06/15/inline-assembly-memory-side-effects-bug/

Please consider using at least Solidity 0.8.15 instead of 0.8.13 to be more future-proofed.

## [L-02] MISSING `address(0)` CHECKS FOR CRITICAL ADDRESSES DECODED FROM INPUT
To prevent unintended behaviors, critical addresses decoded from the input should be checked against `address(0)`.

Please consider checking `factory`, `holograph`, `operator`, and `registry` in the following `init` function.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162-L177
```solidity
  function init(bytes memory initPayload) external override returns (bytes4) {
    require(!_isInitialized(), "HOLOGRAPH: already initialized");
    (address factory, address holograph, address operator, address registry) = abi.decode(
      initPayload,
      (address, address, address, address)
    );
    assembly {
      sstore(_adminSlot, origin())
      sstore(_factorySlot, factory)
      sstore(_holographSlot, holograph)
      sstore(_operatorSlot, operator)
      sstore(_registrySlot, registry)
    }
    _setInitialized();
    return InitializableInterface.init.selector;
  }
```

Please consider checking `holograph` and `registry` in the following `init` function.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L143-L153
```solidity
  function init(bytes memory initPayload) external override returns (bytes4) {
    require(!_isInitialized(), "HOLOGRAPH: already initialized");
    (address holograph, address registry) = abi.decode(initPayload, (address, address));
    assembly {
      sstore(_adminSlot, origin())
      sstore(_holographSlot, holograph)
      sstore(_registrySlot, registry)
    }
    _setInitialized();
    return InitializableInterface.init.selector;
  }
```

Please consider checking `bridge`, `holograph`, `interfaces`, `registry`, and `utilityToken` in the following `init` function.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240-L272
```solidity
  function init(bytes memory initPayload) external override returns (bytes4) {
    require(!_isInitialized(), "HOLOGRAPH: already initialized");
    (address bridge, address holograph, address interfaces, address registry, address utilityToken) = abi.decode(
      initPayload,
      (address, address, address, address, address)
    );
    assembly {
      sstore(_adminSlot, origin())
      sstore(_bridgeSlot, bridge)
      sstore(_holographSlot, holograph)
      sstore(_interfacesSlot, interfaces)
      sstore(_registrySlot, registry)
      sstore(_utilityTokenSlot, utilityToken)
    }
    ...
  }
```

Please consider checking `bridge`, `interfaces`, and `operator` in the following `init` function.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L158-L174
```solidity
  function init(bytes memory initPayload) external override returns (bytes4) {
    require(!_isInitialized(), "HOLOGRAPH: already initialized");
    (address bridge, address interfaces, address operator, uint256 baseGas, uint256 gasPerByte) = abi.decode(
      initPayload,
      (address, address, address, uint256, uint256)
    );
    assembly {
      sstore(_adminSlot, origin())
      sstore(_bridgeSlot, bridge)
      sstore(_interfacesSlot, interfaces)
      sstore(_operatorSlot, operator)
      sstore(_baseGasSlot, baseGas)
      sstore(_gasPerByteSlot, gasPerByte)
    }
    _setInitialized();
    return InitializableInterface.init.selector;
  }
```

## [L-03] UNRESOLVED TODO COMMENTS
Comments regarding todos indicate that there are unresolved action items for implementation, which need to be addressed before the protocol deployment. Please review the todo comments in the following code.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L274-L294
```solidity
  /**
   * @dev temp function, used for quicker updates/resets during development
   *      NOT PART OF FINAL CODE !!!
   */
  function resetOperator(
    uint256 blockTime,
    uint256 baseBondAmount,
    uint256 podMultiplier,
    uint256 operatorThreshold,
    uint256 operatorThresholdStep,
    uint256 operatorThresholdDivisor
  ) external onlyAdmin {
    _blockTime = blockTime;
    _baseBondAmount = baseBondAmount;
    _podMultiplier = podMultiplier;
    _operatorThreshold = operatorThreshold;
    _operatorThresholdStep = operatorThresholdStep;
    _operatorThresholdDivisor = operatorThresholdDivisor;
    _operatorPods = [[address(0)]];
    _bondedOperators[address(0)] = 1;
  }
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L690-L711
```solidity
  function getJobDetails(bytes32 jobHash) public view returns (OperatorJob memory) {
    uint256 packed = _operatorJobs[jobHash];
    /**
     * @dev The job is bitwise packed into a single 32 byte slot, this unpacks it before returning the struct
     */
    return
      OperatorJob(
        uint8(packed >> 248),
        uint16(_blockTime),
        _operatorTempStorage[uint32(packed >> 216)],
        uint40(packed >> 176),
        // TODO: move the bit-shifting around to have it be sequential
        uint64(packed >> 16),
        [
          uint16(packed >> 160),
          uint16(packed >> 144),
          uint16(packed >> 128),
          uint16(packed >> 112),
          uint16(packed >> 96)
        ]
      );
  }
```

## [L-04] INCORRECT OR OUT-OF-DATE COMMENTS
Incorrect or out-of-date comments can be misleading. For better readability and maintainability, please consider updating the following incorrect or out-of-date comments.

There are operations in the following `catch` block, which does not match the comment.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L419-L429
```solidity
    try
      HolographOperatorInterface(address(this)).nonRevertingBridgeCall{value: msg.value}(
        msg.sender,
        bridgeInRequestPayload
      )
    {
      /// @dev do nothing
    } catch {
      _failedJobs[hash] = true;
      emit FailedOperatorJob(hash);
    }
```

The following `onERC721Received` function is not empty, which does not match the comment.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L742-L770
```solidity
  /**
   * @notice Empty function that is triggered by external contract on NFT transfer.
   * @dev We have this blank function in place to make sure that external contract sending in NFTs don't error out.
   * @dev Since it's not being used, the _operator variable is commented out to avoid compiler warnings.
   * @dev Since it's not being used, the _from variable is commented out to avoid compiler warnings.
   * @dev Since it's not being used, the _tokenId variable is commented out to avoid compiler warnings.
   * @dev Since it's not being used, the _data variable is commented out to avoid compiler warnings.
   * @return bytes4 Returns the interfaceId of onERC721Received.
   */
  function onERC721Received(
    address _operator,
    address _from,
    uint256 _tokenId,
    bytes calldata _data
  ) external returns (bytes4) {
    require(_isContract(_operator), "ERC721: operator not contract");
    if (_isEventRegistered(HolographERC721Event.beforeOnERC721Received)) {
      require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));
    }
    try HolographERC721Interface(_operator).ownerOf(_tokenId) returns (address tokenOwner) {
      require(tokenOwner == address(this), "ERC721: contract not token owner");
    } catch {
      revert("ERC721: token does not exist");
    }
    if (_isEventRegistered(HolographERC721Event.afterOnERC721Received)) {
      require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));
    }
    return ERC721TokenReceiver.onERC721Received.selector;
  }
```

The following `_mint` function does not allow token to be minted to `address(0)`, which does not match the comment.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L808-L822
```solidity
  /**
   * @notice Mints an NFT.
   * @dev Can to mint the token to the zero address and the token cannot already exist.
   * @param to Address to mint to.
   * @param tokenId The new token.
   */
  function _mint(address to, uint256 tokenId) private {
    require(tokenId > 0, "ERC721: token id cannot be zero");
    require(to != address(0), "ERC721: minting to burn address");
    require(!_exists(tokenId), "ERC721: token already exists");
    require(!_burnedTokens[tokenId], "ERC721: token has been burned");
    _tokenOwner[tokenId] = to;
    emit Transfer(address(0), to, tokenId);
    _addTokenToOwnerEnumeration(to, tokenId);
  }
```

## [N-01] MISSING REASON STRINGS IN `require` STATEMENTS
When the reason strings are missing in the `require` statements, it is unclear about why certain conditions revert. Please add descriptive reason strings for the following `require` statements.

```solidity
contracts\enforcer\HolographERC721.sol
  373: require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));
  378: require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
  473: require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));
  486: require(SourceERC721().beforeApprovalAll(to, approved)); 
  491: require(SourceERC721().afterApprovalAll(to, approved)); 
  624: require(SourceERC721().beforeTransfer(from, to, tokenId, data)); 
  628: require(SourceERC721().afterTransfer(from, to, tokenId, data)); 
```

## [N-02] COMMENTED OUT CODE CAN BE REMOVED
The following code are commented out. To improve readability and maintainability, please consider removing them.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L524-L537
```solidity
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
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L539-L552
```solidity
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
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L554-L570
```solidity
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

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L579-L587
```solidity
  // Rarible V2(not being used since it creates a conflict with Manifold royalties)
  // struct Part {
  //     address payable account;
  //     uint96 value;
  // }

  // function getRoyalties(uint256 tokenId) public view returns (Part[] memory) {
  //     return royalties[id];
  // }
```

## [N-03] CONSTANTS CAN BE NAMED USING CAPITAL LETTERS AND UNDERSCORES
As a convention, constants can be named using capital letters and underscores, which improves readability and maintainability. Please consider renaming the following constants by using capital letters and underscores.

```solidity
contracts\HolographBridge.sol
  126: bytes32 constant _factorySlot = 0xa49f20855ba576e09d13c8041c8039fa655356ea27f6c40f1ec46a4301cd5b23;
  130: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
  134: bytes32 constant _jobNonceSlot = 0x1cda64803f3b43503042e00863791e8d996666552d5855a78d53ee1dd4b3286d;
  138: bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;
  142: bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;

contracts\HolographFactory.sol
  127: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
  131: bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;

contracts\HolographOperator.sol
  129: bytes32 constant _bridgeSlot = 0xeb87cbb21687feb327e3d58c6c16d552231d12c7a0e8115042a4165fac8a77f9; 
  133: bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a; 
  137: bytes32 constant _interfacesSlot = 0xbd3084b8c09da87ad159c247a60e209784196be2530cecbbd8f337fdd1848827; 
  141: bytes32 constant _jobNonceSlot = 0x1cda64803f3b43503042e00863791e8d996666552d5855a78d53ee1dd4b3286d; 
  145: bytes32 constant _messagingModuleSlot = 0x54176250282e65985d205704ffce44a59efe61f7afd99e29fda50f55b48c061a; 
  149: bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7; 
  153: bytes32 constant _utilityTokenSlot = 0xbf76518d46db472b71aa7677a0908b8016f3dee568415ffa24055f9a670f9c37; 

contracts\module\LayerZeroModule.sol
  126: bytes32 constant _bridgeSlot = 0xeb87cbb21687feb327e3d58c6c16d552231d12c7a0e8115042a4165fac8a77f9; 
  130: bytes32 constant _interfacesSlot = 0xbd3084b8c09da87ad159c247a60e209784196be2530cecbbd8f337fdd1848827; 
  134: bytes32 constant _lZEndpointSlot = 0x56825e447adf54cdde5f04815fcf9b1dd26ef9d5c053625147c18b7c13091686; 
  138: bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f; 
  142: bytes32 constant _baseGasSlot = 0x1eaa99919d5563fbfdd75d9d906ff8de8cf52beab1ed73875294c8a0c9e9d83a; 
  146: bytes32 constant _gasPerByteSlot = 0x99d8b07d37c89d4c4f4fa0fd9b7396caeb5d1d4e58b41c61c71e3cf7d424a625; 
```

## [N-04] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. NatSpec comments are missing for the following functions. Please consider adding them.

```solidity
contracts\enforcer\HolographERC721.sol
  399: function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {  
  413: function bridgeOut(  
  643: function burned(uint256 tokenId) public view returns (bool) { 
  656: function exists(uint256 tokenId) public view returns (bool) { 
  824: function _removeTokenFromAllTokensEnumeration(uint256 tokenId) private {  
  878: function _chain() private view returns (uint32) { 
  911: function _isContract(address contractAddress) private view returns (bool) { 
  935: function owner() public view override returns (address) { 
  943: function _holograph() private view returns (HolographInterface holograph) { 
  1002: function _isEventRegistered(HolographERC721Event _eventName) private view returns (bool) { 

contracts\module\LayerZeroModule.sol
  251: function getMessageFee(  
  277: function getHlgFee(  
  296: function _getPricing(LayerZeroOverrides lz, uint16 lzDestChain)  
```

## [N-05] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. @param and/or @return comments are missing for the following functions. Please consider completing NatSpec comments for them.

```solidity
contracts\HolographBridge.sol
  296: function revertedBridgeOutRequest(  
  442: function getFactory() external view returns (address factory) { 
  462: function getHolograph() external view returns (address holograph) { 
  482: function getJobNonce() external view returns (uint256 jobNonce) { 
  492: function getOperator() external view returns (address operator) { 
  512: function getRegistry() external view returns (address registry) { 
  531: function _factory() private view returns (HolographFactoryInterface factory) { 
  540: function _holograph() private view returns (HolographInterface holograph) { 
  549: function _jobNonce() private returns (uint256 jobNonce) { 
  559: function _operator() private view returns (HolographOperatorInterface operator) { 
  568: function _registry() private view returns (HolographRegistryInterface registry) { 

contracts\HolographFactory.sol
  143: function init(bytes memory initPayload) external override returns (bytes4) {  
  160: function bridgeIn(  
  177: function bridgeOut(  
  270: function getHolograph() external view returns (address holograph) {  
  290: function getRegistry() external view returns (address registry) {  
  309: function _isContract(address contractAddress) private view returns (bool) {  
  320: function _verifySigner(  

contracts\HolographOperator.sol
  240: function init(bytes memory initPayload) external override returns (bytes4) {  
  278: function resetOperator( 
  445: function nonRevertingBridgeCall(address msgSender, bytes calldata payload) external payable { 
  582: function send(  
  717: function getTotalPods() external view returns (uint256 totalPods) { 
  939: function getBridge() external view returns (address bridge) { 
  959: function getHolograph() external view returns (address holograph) { 
  979: function getInterfaces() external view returns (address interfaces) { 
  999: function getMessagingModule() external view returns (address messagingModule) { 
  1019: function getRegistry() external view returns (address registry) { 
  1039: function getUtilityToken() external view returns (address utilityToken) { 
  1058: function _bridge() private view returns (address bridge) { 
  1067: function _holograph() private view returns (HolographInterface holograph) { 
  1076: function _interfaces() private view returns (HolographInterfacesInterface interfaces) { 
  1085: function _messagingModule() private view returns (CrossChainMessageInterface messagingModule) { 
  1094: function _registry() private view returns (HolographRegistryInterface registry) { 
  1103: function _utilityToken() private view returns (HolographERC20Interface utilityToken) { 
  1112: function _jobNonce() private returns (uint256 jobNonce) { 
  1122: function _popOperator(uint256 pod, uint256 operatorIndex) private { 
  1160: function _getBaseBondAmount(uint256 pod) private view returns (uint256) { 
  1167: function _getCurrentBondAmount(uint256 pod) private view returns (uint256) { 
  1185: function _randomBlockHash( 
  1198: function _isContract(address contractAddress) private view returns (bool) { 

contracts\enforcer\HolographERC721.sol
  238: function init(bytes memory initPayload) external override returns (bytes4) {  
  500: function sourceBurn(uint256 tokenId) external onlySource {  
  508: function sourceMint(address to, uint224 tokenId) external onlySource {  
  520: function sourceGetChainPrepend() external view onlySource returns (uint256) {  
  577: function sourceTransfer(address to, uint256 tokenId) external onlySource {  
  751: function onERC721Received(  
  922: function SourceERC721() private view returns (HolographedERC721 sourceContract) { 
  931: function _interfaces() private view returns (address) { 
  952: function _royalties() private view returns (address) { 

contracts\module\LayerZeroModule.sol
  158: function init(bytes memory initPayload) external override returns (bytes4) {  
  180: function lzReceive( 
  227: function send(  
  310: function getBridge() external view returns (address bridge) {  
  330: function getInterfaces() external view returns (address interfaces) {  
  350: function getLZEndpoint() external view returns (address lZEndpoint) {  
  370: function getOperator() external view returns (address operator) {  
  389: function _bridge() private view returns (address bridge) {  
  398: function _interfaces() private view returns (HolographInterfacesInterface interfaces) {  
  407: function _operator() private view returns (HolographOperatorInterface operator) {  
  431: function getBaseGas() external view returns (uint256 baseGas) {  
  450: function _baseGas() private view returns (uint256 baseGas) {  
  460: function getGasPerByte() external view returns (uint256 gasPerByte) {  
  479: function _gasPerByte() private view returns (uint256 gasPerByte) {  
```