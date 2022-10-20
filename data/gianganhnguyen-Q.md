# 1. [N-01] Commented code

Please delete code snippet below as it serves no purpose.

    File contracts/HolographOperator.sol, line 438: ////  _bondedOperators[msg.sender] += reward;

    File contracts/enforcer/HolographERC721.sol, line 524-570:
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

    File contracts/enforcer/PA1D.sol, line 393:     // uint256 sent;
    File contracts/enforcer/PA1D.sol, line 393:     // sent = sent + sending;
    File contracts/enforcer/PA1D.sol, line 413:     //uint256 sent;
    File contracts/enforcer/PA1D.sol, line 417:     // sent = sent + sending;
    File contracts/enforcer/PA1D.sol, line 436:     //uint256 sent;
    File contracts/enforcer/PA1D.sol, line 440:     // sent = sent + sending;
    File contracts/enforcer/PA1D.sol, line 579-587:     
        // Rarible V2(not being used since it creates a conflict with Manifold royalties)
        // struct Part {
        //     address payable account;
        //     uint96 value;
        // }

        // function getRoyalties(uint256 tokenId) public view returns (Part[] memory) {
        //     return royalties[id];
        // }

# 2. [N-02] Uncheck `tranfer` method

Boolean return value for `transfer` is not checked in here:

    File contracts/HolographOperator.sol, line 400:
                  _utilityToken().transfer(job.operator, leftovers); // I recommend:  require(_utilityToken().transfer(job.operator, leftovers), "Holograph: !transfer");

    File contracts/HolographOperator.sol, line 596:
                  payable(hToken).transfer(hlgFee); // I recommend:  require(payable(hToken).transfer(hlgFee), "Holograph: !transfer");

    File contracts/enforcer/PA1D.sol, line 396:
                 addresses[i].transfer(sending); // I recommend:  require(addresses[i].transfer(sending), "PA1D: !transfer");