## QA Report - low risk

### Unused/empty `receive()` or `fallback()` function
Although leaving the `receive()` below without a `revert` might save gas (as `@dev` states), it could result in a loss of funds for a user who sends Ether to the contract (having no way to get anything back)
 
[HolographOperator.sol: L1207-1209](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1207-L1209)
```solidity
   * @dev Purposefully left empty to ensure ether transfers use least amount of gas possible
   */
  receive() external payable {}
```
___
Again, leaving the `receive()` below without a `revert` could result in a loss of funds for a user who sends Ether to the contract. 

[Holographer.sol: L221-223](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L221-L223)
```solidity
   * @dev Purposefully left empty, to prevent running out of gas errors when receiving native token payments.
   */
  receive() external payable {}
```
Similarly for the following:

[HolographERC721.sol: L959-962](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L959-L962)

[HolographERC20.sol: L249-251](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L249-L251)

[ERC721H.sol: L209-212](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L209-L212)

[ERC20H.sol: L210-212](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L210-L212)
___
___

## QA Report: non-critical

### NatSpec does not match function
NatSpec statements (including `@dev @param`) do not relate directly to the function below them (a function which may be incomplete). 

[HolographOperator.sol: L656-669](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L656-L669)
```solidity
   * @dev Will provide exact costs on protocol and message side, combine the two to get total
   * @dev @param toChain holograph chain id of destination chain for payload
   * @dev @param gasLimit amount of gas to provide for executing payload on destination chain
   * @dev @param gasPrice maximum amount to pay for gas price, can be set to 0 and will be chose automatically
   * @dev @param crossChainPayload the entire packet being sent cross-chain
   * @return hlgFee the amount (in wei) of native gas token that will cost for finalizing job on destiantion chain
   * @return msgFee the amount (in wei) of native gas token that will cost for sending message to destiantion chain
   */
  function getMessageFee(
    uint32,
    uint256,
    uint256,
    bytes calldata
  ) external view returns (uint256, uint256) {
```
___
___


### Typos
___
The same typo (`initilaization`) occurs in all 10 lines referenced below:

[HolographBridge.sol: L160](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L160)

[HolographOperator.sol: L238](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L238)

[HolographFactory.sol: L141](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L141)

[LayerZeroModule.sol: L156](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L156)

[Holographer.sol: L145](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L145)

[PA1D.sol: L171](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L171)

[HolographERC721.sol: L236](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L236)

[HolographERC20.sol: L216](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L216)

[ERC721H.sol: L138](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L138)

[ERC20H.sol: L138](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L138)
```solidity
   * @param initPayload abi encoded payload to use for contract initilaization
```
Change `initilaization` to `initialization` in each case
___
The same typo (`destiantion`) occurs in all four lines referenced below:

[HolographBridge.sol: L415](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L415)

[HolographBridge.sol: L416](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L416)

[HolographOperator.sol: L661](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L661)

[HolographOperator.sol: L662](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L662)
```solidity
   * @return hlgFee the amount (in wei) of native gas token that will cost for finalizing job on destiantion chain
```
Change `destiantion` to `destination` in each case
___
The same typo (`accomodate`) occurs in both referenced below:

[HolographOperator.sol: L515](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L515)

[PA1D.sol: L387](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L387)
```solidity
       *      decrease pod size to accomodate popped operator
```
Change `accomodate` to `accommodate` in both casees
___
[HolographOperator.sol: L553](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L553)
```solidity
       * @dev bridgeInRequest doNotRevert is purposefully set to false so a rever would happen
```
Change `rever` to `revert` 
___
[HolographOperator.sol: L997](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L997)
```solidity
   * @dev All cross-chain message requests will get forwarded to this adress
```
Change `adress` to `address` 
___
[HolographFactory.sol: L188](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L188)
```solidity
   * @param config contract deployement configurations
```
Change `deployement` to `deployment` 
___
[PA1D.sol: L150](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L150)
```solidity
   * @param recipients Address array of wallets that will receive tha royalties.
```
Change `tha` to `the` 
___
[PA1D.sol: L156](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L156)
```solidity
   * @dev Use this modifier to lock public functions that should not be accesible to non-owners.
```
Change `accesible` to `accessible` 
___
[PA1D.sol: L299](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L299)
```solidity
    // The slot hash has been precomputed for gas optimizaion
```
Change `optimizaion` to `optimization` 
___
[PA1D.sol: L446](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L446)
```solidity
   * @dev This function validates that the call is being made by an authorised wallet.
```
Change `authorised` to `authorized`. The American English spelling of this word is used elsewhere in `Holograph/PA1D`, so this version should be used throughout for consistency
___
[PA1D.sol: L447](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L447)
```solidity
   * @dev Will revert entire tranaction if it fails.
```
Change `tranaction` to `tranaction`
___
[PA1D.sol: L472](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L472)
```solidity
    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
```
Change `missmatched` to `mismatched` and `lenghts` to `lengths`
___
[PA1D.sol: L477](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L477)
```solidity
    require(totalBp == 10000, "PA1D: bps down't equal 10000");
```
Change `down't` to `don't` 
___
[HolographERC721.sol: L194](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L194)
```solidity
   * @dev Usually utilised for supporting marketplace proxy wallets.
```
Change `utilised` to `utilized`. Again, this assumes the use of American Engligh throughout for consistency
___
[HolographERC721.sol: L263](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L263)
```solidity
      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
```
Change `coud` to `could`
___
[HolographERC721.sol: L319](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L319)
```solidity
   * @dev Defaults the the Arweave URI
```
Remove repeated word `the`
___
[HolographERC721.sol: L543](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L543)
```solidity
  //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
```
Change `missmatch` to `mismatch`
___
[HolographERC721.sol: L662](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L662)
```solidity
   * @dev Single operator set for a specific token. Usually used for one-time very specific authorisations.
```
Change `authorisations` to `authorizations`, again assuming American English
___
[HolographERC20.sol: L154](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L154)
```solidity
   * @dev Mapping of all the addresse's balances.
```
Change ` addresse's` to `addressee's`, assuming this was what was meant
___
___

### Open TODOs
Comments that contain TODOs or other language referring to open items should be addressed and better explained, or else modified or removed. Below are several instances:
 
___
[HolographBridge.sol: L286](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L286)
```solidity
   * @notice Do not call this function, it will always revert
```
___
[HolographOperator.sol: L275-276](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L275-L276)
```solidity
   * @dev temp function, used for quicker updates/resets during development
   *      NOT PART OF FINAL CODE !!!
```
___
[HolographOperator.sol: L701](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L701)
```solidity
        // TODO: move the bit-shifting around to have it be sequential
```
___
[PA1D.sol: L619-620](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L619-L620)
```solidity
  // To be quite honest, SuperRare is a closed marketplace. They're working on opening it up but looks like they want to use private smart contracts.
  // We'll just leave this here for just in case they open the flood gates.
```
___
[PA1D.sol: L666](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L666)
```solidity
    // this information is outside of the scope of our    [line appears to be truncated]
```
___
[HolographERC721.sol: L446-447](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L446-L447)
```solidity
   * @dev Since it's not being used, the _data variable is commented out to avoid compiler warnings.
   * are aware of the ERC721 protocol to prevent tokens from being forever locked.
```
___
___

### Named return variable is not used 
Named return variable (here, `totalPods`) is never used

[HolographOperator.sol: L714-719](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L714-L719)
```solidity
   * @notice Get number of pods available
   * @dev This returns number of pods that have been opened via bonding
   */
  function getTotalPods() external view returns (uint256 totalPods) {
    return _operatorPods.length;
  }
```
___
___


### Long single line comments 
In theory, comments over 80 characters should wrap using multi-line comment syntax. Even if somewhat longer comments (up to 120 characters) are acceptable and a scroll bar is provided, there are cases where very long comments interfere with readability. Below are five instances of extra-long comments whose readability could be improved by wrapping, as shown:

[HolographBridge.sol: L120](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L120)
```solidity
 * @dev The contract abstracts all the complexities of making bridge requests and uses a universal interface to bridge any type of holographable assets
```
Suggestion:
```solidity
 * @dev The contract abstracts all the complexities of making bridge requests and 
 *   uses a universal interface to bridge any type of holographable assets.
```
___
[HolographFactory.sol: L121](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L121)
```solidity
 * @dev The contract provides methods that allow for the creation of Holograph Protocol compliant smart contracts, that are capable of minting holographable assets
```
Suggestion:
```solidity
 * @dev The contract provides methods that allow for the creation of Holograph Protocol 
 *   compliant smart contracts, that are capable of minting holographable assets.
```
___
[Holographer.sol: L172](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L172)
```solidity
   * @dev Returns the block height of when the smart contract was deployed. Useful for retrieving deployment config for re-deployment on other EVM-compatible chains.
```
Suggestion:
```solidity
   * @dev Returns the block height of when the smart contract was deployed. Useful for 
   *   retrieving deployment config for re-deployment on other EVM-compatible chains.
```
___
[PA1D.sol: L151](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L151)
```solidity
   * @param bps Uint256 array of base points(percentages) that each wallet(specified in recipients) will receive from the royalty payouts. Make sure that all the base points add up to a total of 10000.
```
Suggestion:
```solidity
   * @param bps Uint256 array of base points(percentages) that each wallet(specified in recipients) will receive
   *   from the royalty payouts. Make sure that all the base points add up to a total of 10000.
```
___
[PA1D.sol: L619](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L619)
```solidity
  // To be quite honest, SuperRare is a closed marketplace. They're working on opening it up but looks like they want to use private smart contracts.
```
Suggestion:
```solidity
  // To be quite honest, SuperRare is a closed marketplace. They're working on
  //   opening it up but looks like they want to use private smart contracts.
```
___
___

### Missing `require` revert string
A `require` message should be included to enable users to understand the reason for failure
Recommendation: Add a brief (<= 32 char) explanatory message to each of the 31 `requires` referenced below. Also, consider using custom error codes (which would be cheaper than revert strings).

___
[HolographERC721.sol: L378](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L378)
```solidity
      require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
```
___
[HolographERC721.sol: L391](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L391)
```solidity
      require(SourceERC721().beforeBurn(wallet, tokenId));
```
___
[HolographERC721.sol: L395](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L395)
```solidity
      require(SourceERC721().afterBurn(wallet, tokenId));
```
___
[HolographERC721.sol: L460](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L460)
```solidity
      require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));
```
___
[HolographERC721.sol: L473](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L473)
```solidity
      require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));
```
___
[HolographERC721.sol: L486](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L486)
```solidity
      require(SourceERC721().beforeApprovalAll(to, approved));
```
___
[HolographERC721.sol: L491](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L491)
```solidity
      require(SourceERC721().afterApprovalAll(to, approved));
```
___
[HolographERC721.sol: L624](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L624)
```solidity
      require(SourceERC721().beforeTransfer(from, to, tokenId, data));
```
___
[HolographERC721.sol: L628](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L628)
```solidity
      require(SourceERC721().afterTransfer(from, to, tokenId, data));
```
___
[HolographERC721.sol: L759](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L759)
```solidity
      require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));
```
___
[HolographERC767.sol: L759](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L767)
```solidity
      require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));
```
___
[HolographERC20.sol: L328](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L328)
```solidity
      require(SourceERC20().beforeApprove(msg.sender, spender, amount));
```
___
[HolographERC20.sol: L332](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L332)
```solidity
      require(SourceERC20().afterApprove(msg.sender, spender, amount));
```
___
[HolographERC20.sol: L339](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L339)
```solidity
      require(SourceERC20().beforeBurn(msg.sender, amount));
```
___
[HolographERC20.sol: L343](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L343)
```solidity
      require(SourceERC20().afterBurn(msg.sender, amount));
```
___
[HolographERC20.sol: L354](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L354)
```solidity
      require(SourceERC20().beforeBurn(account, amount));
```
___
[HolographERC20.sol: L358](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L358)
```solidity
      require(SourceERC20().afterBurn(account, amount));
```
___
[HolographERC20.sol: L371](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L371)
```solidity
      require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
```
___
[HolographERC20.sol: L375](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L375)
```solidity
      require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
```
___
[HolographERC20.sol: L430](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L430)
```solidity
      require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
```
___
[HolographERC20.sol: L434](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L434)
```solidity
      require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
```
___
[HolographERC20.sol: L447](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L447)
```solidity
      require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));
```
___
[HolographERC20.sol: L455](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L455)
```solidity
      require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));
```
___
[HolographERC20.sol: L484](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L484)
```solidity
      require(SourceERC20().beforeApprove(account, spender, amount));
```
___
[HolographERC20.sol: L488](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L488)
```solidity
      require(SourceERC20().afterApprove(account, spender, amount));
```
___
[HolographERC20.sol: L502](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L502)
```solidity
      require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));
```
___
[HolographERC20.sol: L507](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L507)
```solidity
      require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));
```
___
[HolographERC20.sol: L536](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L536)
```solidity
      require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));
```
___
[HolographERC20.sol: L541](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L541)
```solidity
      require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));
```
___
[HolographERC20.sol: L582](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L582)
```solidity
      require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));
```
___
[HolographERC20.sol: L586](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L586)
```solidity
      require(SourceERC20().afterTransfer(msg.sender, recipient, amount));
```
___
[HolographERC20.sol: L606](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L606)
```solidity
      require(SourceERC20().beforeTransfer(account, recipient, amount));
```
___
[HolographERC20.sol: L610](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L610)
```solidity
      require(SourceERC20().afterTransfer(account, recipient, amount));
```
___
___

### Event is missing `indexed` fields
An `event` should use three `indexed` fields if there are three or more fields
___
[PA1D.sol: L153](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L153)
```solidity
  event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```
___
___


### Use of exponentiation
For readability, scientific notation for large multiples of ten is preferable to using exponentiation
___

[HolographOperator.sol: L256](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L256)
```solidity
      _baseBondAmount = 100 * (10**18); // one single token unit * 100
```
Suggestion: Change `10**18` to `1e18` 
___
[LayerZeroModule.sol: L274](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L274)
```solidity
    return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
```
[LayerZeroModule.sol: L293](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L293)
```solidity
    return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```
Suggestion: Change `10**10` to `1e10` in both cases
___
___