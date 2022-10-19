## none-critical issue foundings
### N01: EVENT IS MISSING INDEXED FIELDS
#### prof
enforcer/PA1D.sol, 153, b'  event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);'


## low risk foundings
### L01: UNUSED/EMPTY RECEIVE()/FALLBACK() FUNCTION
#### problem
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert
#### prof
HolographOperator.sol, 1209, b'  receive() external payable {}'
enforcer/HolographERC721.sol, 962, b'  receive() external payable {}'
abstract/ERC20H.sol, 212, b'  receive() external payable {}'
abstract/ERC721H.sol, 212, b'  receive() external payable {}'
enforcer/Holographer.sol, 223, b'  receive() external payable {}'
enforcer/HolographERC20.sol, 251, b'  receive() external payable {}'

### L02:_SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
#### problem
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### prof
enforcer/HolographERC721.sol, 406, b'    _mint(to, tokenId);'
enforcer/HolographERC721.sol, 514, b'    _mint(to, token);'
enforcer/HolographERC20.sol, 385, b'    _mint(to, amount);'
enforcer/HolographERC20.sol, 416, b'    _mint(to, amount);'
enforcer/HolographERC20.sol, 557, b'    _mint(to, amount);'
enforcer/HolographERC20.sol, 565, b'      _mint(wallets[i], amounts[i]);'

### L03: INPUT ARRAY LENGTHS MAY DIFFER
#### problem
If the caller makes a copy-paste error, the lengths may be mismatchd and an operation believed to have been completed may not in fact have been completed
function withdrawMultipleERC721(address[] memory _tokens, uint256[] memory _tokenId, address _to) external override {
#### prof
enforcer/HolographERC20.sol, 567, b'  function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource '

### L04: USE A MORE RECENT VERSION OF SOLIDITY
#### prof
0.8.13 version of solidity has severe compilation bugs.
#### problem
HolographOperator.sol, 102, b'pragma solidity 0.8.13;'
HolographBridge.sol, 102, b'pragma solidity 0.8.13;'
enforcer/HolographERC721.sol, 102, b'pragma solidity 0.8.13;'
abstract/ERC20H.sol, 102, b'pragma solidity 0.8.13;'
abstract/ERC721H.sol, 102, b'pragma solidity 0.8.13;'
enforcer/Holographer.sol, 102, b'pragma solidity 0.8.13;'
HolographFactory.sol, 102, b'pragma solidity 0.8.13;'
enforcer/HolographERC20.sol, 102, b'pragma solidity 0.8.13;'
module/LayerZeroModule.sol, 102, b'pragma solidity 0.8.13;'
enforcer/PA1D.sol, 102, b'pragma solidity 0.8.13;'