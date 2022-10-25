## ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()
       

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L252


```solidity
        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
```
            
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L426


```solidity
    return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L409


```solidity
    return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L471


```solidity
      abi.encode(
        0x6e71edae12b1b97f4d1f60370fef10105fa2faae0126114a169c64845d6126c9,
        account,
        spender,
        amount,
        _useNonce(account),
        deadline
      )
```
            

## Splitting require() statements that use && saves gas

https://github.com/code-423n4/2022-01-xdefi-findings/issues/128

See this issue which describes the fact 
that there is a larger deployment gas cost, 
but with enough runtime calls, the change ends up being cheaper


https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L857


```solidity
    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L166


```solidity
    require(success && selector == InitializableInterface.init.selector, "initialization failed");
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L263


```solidity
      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
```
            
   
## function can be marked as external instead of public


https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L471


```solidity
  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L488


```solidity
  function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L497


```solidity
  function getEthPayout() public {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L507


```solidity
  function getTokenPayout(address tokenAddress) public {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L517


```solidity
  function getTokensPayout(address[] memory tokenAddresses) public {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L549


```solidity
  function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L558


```solidity
  function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L569


```solidity
  function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L604


```solidity
  function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L621


```solidity
  function tokenCreator(
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L634


```solidity
  function calculateRoyaltyFee(
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L649


```solidity
  function marketContract() public view returns (address) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L655


```solidity
  function tokenCreators(uint256 tokenId) public view returns (address) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L665


```solidity
  function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L683


```solidity
  function getTokenAddress(string memory tokenName) public view returns (address) {
```