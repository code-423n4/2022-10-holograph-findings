### [L-01] CONSIDER ADDINGS CHECKS FOR SIGNATURE MALLEABILITY

The ecrecover() function returns an address of zero when the signature does not match. This can cause problems if address zero is ever the owner of assets, and someone uses the permit function on address zero. If that happens, any invalid signature will pass the checks, and the assets will be stealable. In this case, the asset of concern is the vault’s ERC20 token, and fortunately OpenZeppelin’s implementation does a good job of making sure that address zero is never able to have a positive balance. If this contract ever changes to another ERC20 implementation that is laxer in its checks in favor of saving gas, this code may become a problem.

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334

#### Recommended Mitigation
Use OpenZeppelin’s ECDSA contract rather than calling ecrecover() directly




### [L-02] ABSENCE OF ZERO ADDRESS CHECK

There are 5 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452-455
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472-476
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502-505
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522-524
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L226-L229


#### Recommended Mitigation
Should be a require() condition that check for inputed addresses are zero address or not



### [L-03] FOR SENDING ETH transfer() IS USED AND RETURN VALUE NOT CHECKED

The Istanbul hardfork increases the gas cost of the SLOAD operation and therefore breaks some existing smart contracts.

In file withdrawable.sol, contract uses transfer() to send eth from contract to EOA due which eth can get stuck.

The reason behind this is that, after the Istanbul hardfork, any smart contract that uses transfer() or send() is taking a hard dependency on gas costs by forwarding a fixed amount of gas (2300). This forwards 2300 gas, which may not be enough if the recipient is a contract and the cost of gas changes.


There are 2 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L596
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L396

#### Recommended Mitigation
Recommend using call() to send eth. 



### [L-04] RETURN VALUE OF IERC20.transfer() NOT CHECKED

Transfering ERC20 with transfer()
Not all IERC20 implementations revert() when there’s a failure in   transfer()/transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L400

#### Recommended Mitigation
Use safeTransfer()/safeTransferFrom() or at least implement a return value check for ERC20 transfer() function





### [L-05] MISSING EVENTS FOR ONLY FUNCTIONS THAT CHANGE CRITICAL PARAMETERS

The functions that change critical parameters should emit events. Events allow capturing the changed parameters so that off-chain tools/interfaces can register such changes with timelocks that allow users to evaluate them and consider if they would like to engage/exit based on how they perceive the changes as affecting the trustworthiness of the protocol or profitability of the implemented financial services. The alternative of directly querying on-chain contract state for such changes is not considered practical for most users/usages.

There are multiple instances of this issue:

#### Recommended Mitigation

Recommended to emit events on critical variable change


### [L-06] FUNCTION _payoutTokens() CAN LEAD TO DOS(Denial Of Service) CONDITION

This function using nested for loops and within them some other internal function call.
More over these all operate on a dynamic array.
If the array length increases significantly then may whole gas cost of calling this function may exceed from block gas limit and whole function reverted, so this will lead to a situation named Denial Of service

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L426-L443

#### Recommended Mitigation

Properly check gas consumption for this function for different size of array.
Then capped the array size, or specify some sorts of upper bound and handle large array in multiple time(multiple part).


### [L-07] FUNCTION sourceMintBatch() DOES NOT CHECK WHETHER BOTH PARAMETER(DYNAMIC ARRAYS) ARE OF SAME LENGTH OR NOT

Function does not check both dynamic array have same length or not, if those have different lengths then further function call will give a unintensional result or result error,

As these inputs are provided by user, so human error is possible

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L563-L566

#### Recommended Mitigation
There should be a check that ensure that both inputed array have same length


### [N-01] SOME COMMENTED CODE PRESENT IN CONTRACT FILE

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L525-L576

#### Recommended Mitigation
Remove those code before deployment