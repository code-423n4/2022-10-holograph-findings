# QA Report

### Summary

The overall code is well-commented. Unit tests are provided. The logic is split into the corresponding files. The logic is clear when referring to the docs/information given in the code4rena contest page.
There may be instances where ether may be sent to the contract via the receive() or fallback functions. These contracts may lock the sent ether as they do not have functionality to withdraw ether from the contract.

### 1. Payable Fallback Function :

It is optional whether to add the payable functionality for a fallback function. Since the fallback() is purposefully made to revert to prevent any calls to undefined functions and there is no need to send any ether to the contract. Hence the payable keyword may be removed.

    fallback() external payable {
        revert();
    }

Can be changed to :

    fallback() external {
        revert();
    }

