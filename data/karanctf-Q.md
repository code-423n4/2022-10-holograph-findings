# Non critical

## [N-01] Use scientific notation (e.g. `10e18`) rather than exponentiation (e.g. `10**18`)
```solidity
LayerZeroModule.sol:274:    return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
LayerZeroModule.sol:293:    return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
HolographOperator.sol:256:      _baseBondAmount = 100 * (10**18); // one single token unit * 100
```


# Low Impact
## [L-01] Use of `Block.timestamp`
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
```solidity
HolographERC20.sol:469:    require(block.timestamp <= deadline, "ERC20: expired deadline");
HolographOperator.sol:345:        uint256 elapsedTime = block.timestamp - uint256(job.startTimestamp);
HolographOperator.sol:499:      uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
HolographOperator.sol:531:          (block.timestamp << 16) |
```


## [L-02] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
```solidity
HolographBridge.sol-401-    );
HolographBridge.sol-402-    /**
HolographBridge.sol-403-     * @dev this abi encodes the data just like in Holograph Operator
HolographBridge.sol-404-     */
HolographBridge.sol:405:    samplePayload = abi.encodePacked(encodedData, gasLimit, gasPrice);
HolographBridge.sol-406-  }
HolographBridge.sol-407-
HolographBridge.sol-408-  /**
HolographBridge.sol-409-   * @notice Get the fees associated with sending specific payload
--
LayerZeroModule.sol-239-    }
LayerZeroModule.sol-240-    // need to recalculate the gas amounts for LZ to deliver message
LayerZeroModule.sol-241-    lZEndpoint.send{value: msgValue}(
LayerZeroModule.sol-242-      uint16(_interfaces().getChainId(ChainIdType.HOLOGRAPH, uint256(toChain), ChainIdType.LAYERZERO)),
LayerZeroModule.sol:243:      abi.encodePacked(address(this), address(this)),
LayerZeroModule.sol-244-      crossChainPayload,
LayerZeroModule.sol-245-      payable(msgSender),
LayerZeroModule.sol-246-      address(this),
LayerZeroModule.sol:247:      abi.encodePacked(uint16(1), uint256(_baseGas() + (crossChainPayload.length * _gasPerByte())))
LayerZeroModule.sol-248-    );
LayerZeroModule.sol-249-  }
LayerZeroModule.sol-250-
LayerZeroModule.sol-251-  function getMessageFee(
--
LayerZeroModule.sol-265-    (uint128 dstPriceRatio, uint128 dstGasPriceInWei) = _getPricing(lz, lzDestChain);
LayerZeroModule.sol-266-    if (gasPrice == 0) {
LayerZeroModule.sol-267-      gasPrice = dstGasPriceInWei;
LayerZeroModule.sol-268-    }
LayerZeroModule.sol:269:    bytes memory adapterParams = abi.encodePacked(
LayerZeroModule.sol-270-      uint16(1),
LayerZeroModule.sol-271-      uint256(_baseGas() + (crossChainPayload.length * _gasPerByte()))
LayerZeroModule.sol-272-    );
LayerZeroModule.sol-273-    (uint256 nativeFee, ) = lz.estimateFees(lzDestChain, address(this), crossChainPayload, false, adapterParams);
--
HolographFactory.sol-203-    /**
HolographFactory.sol-204-     * @dev the configuration is encoded and hashed along with signer address
HolographFactory.sol-205-     */
HolographFactory.sol-206-    bytes32 hash = keccak256(
HolographFactory.sol:207:      abi.encodePacked(
HolographFactory.sol-208-        config.contractType,
HolographFactory.sol-209-        config.chainType,
HolographFactory.sol-210-        config.salt,
HolographFactory.sol-211-        keccak256(config.byteCode),
--
HolographFactory.sol-222-     * @dev check that this contract has not already been deployed on this chain
HolographFactory.sol-223-     */
HolographFactory.sol-224-    bytes memory holographerBytecode = type(Holographer).creationCode;
HolographFactory.sol-225-    address holographerAddress = address(
HolographFactory.sol:226:      uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), hash, keccak256(holographerBytecode)))))
HolographFactory.sol-227-    );
HolographFactory.sol-228-    require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
HolographFactory.sol-229-    /**
HolographFactory.sol-230-     * @dev convert hash into uint256 which will be used as the salt for create2
--
HolographFactory.sol-329-    }
HolographFactory.sol-330-    /**
HolographFactory.sol-331-     * @dev signature is checked against EIP-191 first, then directly, to support legacy wallets
HolographFactory.sol-332-     */
HolographFactory.sol:333:    return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
HolographFactory.sol-334-      ecrecover(hash, v, r, s) == signer);
HolographFactory.sol-335-  }
HolographFactory.sol-336-
HolographFactory.sol-337-  /**
--
HolographERC721.sol-508-  function sourceMint(address to, uint224 tokenId) external onlySource {
HolographERC721.sol-509-    // uint32 is reserved for chain id to be used
HolographERC721.sol-510-    // we need to get current chain id, and prepend it to tokenId
HolographERC721.sol-511-    // this will prevent possible tokenId overlap if minting simultaneously on multiple chains is possible
HolographERC721.sol:512:    uint256 token = uint256(bytes32(abi.encodePacked(_chain(), tokenId)));
HolographERC721.sol-513-    require(!_burnedTokens[token], "ERC721: can't mint burned token");
HolographERC721.sol-514-    _mint(to, token);
HolographERC721.sol-515-  }
HolographERC721.sol-516-
HolographERC721.sol-517-  /**
HolographERC721.sol-518-   * @dev Allows source to get the prepend for their tokenIds.
HolographERC721.sol-519-   */
HolographERC721.sol-520-  function sourceGetChainPrepend() external view onlySource returns (uint256) {
HolographERC721.sol:521:    return uint256(bytes32(abi.encodePacked(_chain(), uint224(0))));
HolographERC721.sol-522-  }
HolographERC721.sol-523-
HolographERC721.sol-524-  /**
HolographERC721.sol-525-   * @dev Allows for source smart contract to mint a batch of tokens.
--
HolographERC721.sol-529-  //     uint32 chain = _chain();
HolographERC721.sol-530-  //     uint256 token;
HolographERC721.sol-531-  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol-532-  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
HolographERC721.sol:533:  //       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])));
HolographERC721.sol-534-  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
HolographERC721.sol-535-  //       _mint(to, token);
HolographERC721.sol-536-  //     }
HolographERC721.sol-537-  //   }
--
HolographERC721.sol-544-  //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
HolographERC721.sol-545-  //     uint32 chain = _chain();
HolographERC721.sol-546-  //     uint256 token;
HolographERC721.sol-547-  //     for (uint256 i = 0; i < tokenIds.length; i++) {
HolographERC721.sol:548:  //       token = uint256(bytes32(abi.encodePacked(chain, tokenIds[i])));
HolographERC721.sol-549-  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
HolographERC721.sol-550-  //       _mint(wallets[i], token);
HolographERC721.sol-551-  //     }
HolographERC721.sol-552-  //   }
--
HolographERC721.sol-561-  //   ) external onlySource {
HolographERC721.sol-562-  //     uint32 chain = _chain();
HolographERC721.sol-563-  //     uint256 token;
HolographERC721.sol-564-  //     for (uint256 i = 0; i < length; i++) {
HolographERC721.sol:565:  //       token = uint256(bytes32(abi.encodePacked(chain, startingTokenId)));
HolographERC721.sol-566-  //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
HolographERC721.sol-567-  //       _mint(to, token);
HolographERC721.sol-568-  //       startingTokenId++;
HolographERC721.sol-569-  //     }
--
HolographOperator.sol-495-      ++_operatorTempStorageCounter;
HolographOperator.sol-496-      /**
HolographOperator.sol-497-       * @dev use job hash, job nonce, block number, and block timestamp for generating a random number
HolographOperator.sol-498-       */
HolographOperator.sol:499:      uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));
HolographOperator.sol-500-      /**
HolographOperator.sol-501-       * @dev divide by total number of pods, use modulus/remainder
HolographOperator.sol-502-       */
HolographOperator.sol-503-      uint256 pod = random % _operatorPods.length;
--
HolographOperator.sol-631-    );
HolographOperator.sol-632-    /**
HolographOperator.sol-633-     * @dev add gas variables to the back for later extraction
HolographOperator.sol-634-     */
HolographOperator.sol:635:    encodedData = abi.encodePacked(encodedData, gasLimit, gasPrice);
HolographOperator.sol-636-    /**
HolographOperator.sol-637-     * @dev Send the data to the current Holograph Messaging Module
HolographOperator.sol-638-     *      This will be changed to dynamically select which messaging module to use based on destination network
HolographOperator.sol-639-     */
--
PA1D.sol-253-   * @dev Gets the royalty payment receiver address, for a particular token id, from storage slot.
PA1D.sol-254-   * @return receiver Wallet or smart contract that will receive the royalty payouts for a particular token id.
PA1D.sol-255-   */
PA1D.sol-256-  function _getReceiver(uint256 tokenId) private view returns (address payable receiver) {
PA1D.sol:257:    bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
PA1D.sol-258-    assembly {
PA1D.sol-259-      receiver := sload(slot)
PA1D.sol-260-    }
PA1D.sol-261-  }
--
PA1D.sol-265-   * @param tokenId Uint256 of the token id to set the receiver for.
PA1D.sol-266-   * @param receiver Wallet or smart contract that will receive the royalty payouts for a particular token id.
PA1D.sol-267-   */
PA1D.sol-268-  function _setReceiver(uint256 tokenId, address receiver) private {
PA1D.sol:269:    bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_receiverString, tokenId))) - 1);
PA1D.sol-270-    assembly {
PA1D.sol-271-      sstore(slot, receiver)
PA1D.sol-272-    }
PA1D.sol-273-  }
--
PA1D.sol-276-   * @dev Gets the royalty base points(percentage), for a particular token id, from storage slot.
PA1D.sol-277-   * @return bp Royalty base points(percentage) for the royalty payouts of a specific token id.
PA1D.sol-278-   */
PA1D.sol-279-  function _getBp(uint256 tokenId) private view returns (uint256 bp) {
PA1D.sol:280:    bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
PA1D.sol-281-    assembly {
PA1D.sol-282-      bp := sload(slot)
PA1D.sol-283-    }
PA1D.sol-284-  }
--
PA1D.sol-288-   * @param tokenId Uint256 of the token id to set the base points for.
PA1D.sol-289-   * @param bp Uint256 of royalty percentage, provided in base points format, for a particular token id.
PA1D.sol-290-   */
PA1D.sol-291-  function _setBp(uint256 tokenId, uint256 bp) private {
PA1D.sol:292:    bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_bpString, tokenId))) - 1);
PA1D.sol-293-    assembly {
PA1D.sol-294-      sstore(slot, bp)
PA1D.sol-295-    }
PA1D.sol-296-  }
--
PA1D.sol-304-    }
PA1D.sol-305-    addresses = new address payable[](length);
PA1D.sol-306-    address payable value;
PA1D.sol-307-    for (uint256 i = 0; i < length; i++) {
PA1D.sol:308:      slot = keccak256(abi.encodePacked(i, slot));
PA1D.sol-309-      assembly {
PA1D.sol-310-        value := sload(slot)
PA1D.sol-311-      }
PA1D.sol-312-      addresses[i] = value;
--
PA1D.sol-320-      sstore(slot, length)
PA1D.sol-321-    }
PA1D.sol-322-    address payable value;
PA1D.sol-323-    for (uint256 i = 0; i < length; i++) {
PA1D.sol:324:      slot = keccak256(abi.encodePacked(i, slot));
PA1D.sol-325-      value = addresses[i];
PA1D.sol-326-      assembly {
PA1D.sol-327-        sstore(slot, value)
PA1D.sol-328-      }
--
PA1D.sol-337-    }
PA1D.sol-338-    bps = new uint256[](length);
PA1D.sol-339-    uint256 value;
PA1D.sol-340-    for (uint256 i = 0; i < length; i++) {
PA1D.sol:341:      slot = keccak256(abi.encodePacked(i, slot));
PA1D.sol-342-      assembly {
PA1D.sol-343-        value := sload(slot)
PA1D.sol-344-      }
PA1D.sol-345-      bps[i] = value;
--
PA1D.sol-353-      sstore(slot, length)
PA1D.sol-354-    }
PA1D.sol-355-    uint256 value;
PA1D.sol-356-    for (uint256 i = 0; i < length; i++) {
PA1D.sol:357:      slot = keccak256(abi.encodePacked(i, slot));
PA1D.sol-358-      value = bps[i];
PA1D.sol-359-      assembly {
PA1D.sol-360-        sstore(slot, value)
PA1D.sol-361-      }
PA1D.sol-362-    }
PA1D.sol-363-  }
PA1D.sol-364-
PA1D.sol-365-  function _getTokenAddress(string memory tokenName) private view returns (address tokenAddress) {
PA1D.sol:366:    bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
PA1D.sol-367-    assembly {
PA1D.sol-368-      tokenAddress := sload(slot)
PA1D.sol-369-    }
PA1D.sol-370-  }
PA1D.sol-371-
PA1D.sol-372-  function _setTokenAddress(string memory tokenName, address tokenAddress) private {
PA1D.sol:373:    bytes32 slot = bytes32(uint256(keccak256(abi.encodePacked(_tokenAddressString, tokenName))) - 1);
PA1D.sol-374-    assembly {
PA1D.sol-375-      sstore(slot, tokenAddress)
PA1D.sol-376-    }
PA1D.sol-377-  }
```

