# Low Risk Issues

## 1. Missing non-zero amount check before token transfers

```HolographOperator.sol```
~~~
825:   function topupUtilityToken(address operator, uint256 amount) external {
826:     /**
827:      * @dev check that an operator is currently bonded
828:      */
829:     require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
830:     unchecked {
831:       /**
832:        * @dev add the additional amount to operator
833:        */
834:       _bondedAmounts[operator] += amount;
835:     }
836:     /**
837:      * @dev transfer tokens last, to prevent reentrancy attacks
838:      */
839:     require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
840:   }
~~~

~~~
  function unbondUtilityToken(address operator, address recipient) external {
    /**
     * @dev validate that operator is currently bonded
     */
    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
    /**
     * @dev check if sender is not actual operator
     */
    if (msg.sender != operator) {
      /**
       * @dev check if operator is a smart contract
       */
      require(_isContract(operator), "HOLOGRAPH: operator not contract");
      /**
       * @dev check if smart contract is owned by sender
       */
      require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
    }
    /**
     * @dev get current bonded amount by operator
     */
    uint256 amount = _bondedAmounts[operator];
    /**
     * @dev unset operator bond amount before making a transfer
     */
    _bondedAmounts[operator] = 0;
    /**
     * @dev remove all operator references
     */
    _popOperator(_bondedOperators[operator] - 1, _operatorPodIndex[operator]);
    /**
     * @dev transfer tokens to recipient
     */		//@audit-issue low check nonzero amount
    require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
  }
~~~





## 2. Use ```call()``` instead of deprecated ```transfer()```

This is reported as QA since the receiver is not a user, ie the receive/fallback function on the receiver end is known.

```HolographOperator.sol```
~~~
582:   function send(
583:     uint256 gasLimit,
584:     uint256 gasPrice,
585:     uint32 toChain,
586:     address msgSender,
587:     uint256 nonce,
588:     address holographableContract,
589:     bytes calldata bridgeOutPayload
590:   ) external payable {
591:     require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
592:     CrossChainMessageInterface messagingModule = _messagingModule();
593:     uint256 hlgFee = messagingModule.getHlgFee(toChain, gasLimit, gasPrice);
594:     address hToken = _registry().getHToken(_holograph().getHolographChainId());
595:     require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
596:     payable(hToken).transfer(hlgFee);   //@audit-issue QA use call() instead of transfer()
597:     bytes memory encodedData = abi.encodeWithSelector(
~~~




## 3. Missing non-zero address checks when setting important addresses

It is a good practice to include 0 address check when setting important addresses. It would be good to have non-zero address check for the addresses set within the ```init()``` functions.



	
# Non-Critical Issues

## 1. Typo in comment
```HolographOperator.sol```
~~~
412:     /**
413:      * @dev ensure that there is enough has left for the job  //@audit-issue nc typo has->gas
414:      */
415:     require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
~~~


## 2. There are code parts and comments which imply that the audited code is not final
```HolographOperator.sol```
~~~
275:    * @dev temp function, used for quicker updates/resets during development
276:    *      NOT PART OF FINAL CODE !!!
277:    */
~~~

~~~
701:         // TODO: move the bit-shifting around to have it be sequential
~~~









