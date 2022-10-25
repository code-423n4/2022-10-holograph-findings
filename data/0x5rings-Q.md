 ## `Withdrawerc721`: using the `transferFrom()` function of an erc721 contract may freeze the user's nft 
 ### Files Found: 
 There are 2 instances of this issue. 
 - File: contracts/HolographOperator.sol - line 839 
- File: contracts/HolographOperator.sol - line 889 
 
 
 ### Mitigation: 
 When using the transferFrom function of an ERC721 contract to send an NFT, if the receiving address is a smart contract and does not support ERC721, the NFT can be frozen in the contract. Use the ERC721 contract's safeTransferFrom function to send NFTs. 