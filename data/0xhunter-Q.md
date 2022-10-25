https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L93-L175
any user can call `deployHolographableContract` function with invalid signatures if signer be set to address(0) .
the function uses _verifySigner to check if signer matchs the signatures . however _verifySigner  does not check if ecrecover's result equals to address(0) or not . that's because in case of using an invalid signature , ecrecover would return 0 . 
impact:
a user can act like address(0) has signed the message while it hasn't .
recommendation:
a requirement check for (!=address(0)) in _verifySigner  or deployHolographableContract can fix the issue 