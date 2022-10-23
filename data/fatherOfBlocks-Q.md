**HolographOperator**
- L346 - A division is made with an input, therefore it would be possible that supply is equal to zero, this would imply that a require is necessary to be able to return a message according to the exception.

- L158-188/254-269/278-294 - As the resetOperator function is auxiliary to test the code easier and the storage variables found in that function are only set in the constructor, they could all become immutable. Another option would be not to set them in the constructor and directly make them constant.


**enforcer/PA1D.sol**
- L110 - The PA1DInterface interface is imported, but it is never used, the import should be eliminated.

- L205-210 - The function is called isOwner() but it is approved being owner and being admin, therefore the name is somewhat confusing, since if it is admin and isOwner returns true, it is somehow incorrect.


**enforcer/HolographERC721.sol**
- L123 - The PA1DInterface contract is imported, but it is never used, the import should be eliminated.

- L524-570 - There is commented code that should be used or removed as it does not provide clarity, it only generates confusion.

- L373/378/391/395/460/473/486/491/624/628 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.


**enforcer/HolographERC20.sol**
- L114/116/119/124/125 - The ERC20Burnable, ERC20Metadata, ERC20Safer, HolographerInterface, HolographRegistryInterface contracts is imported, but it is never used, the import should be eliminated.

- L328/332/339/343/354/358/371/375/430/434/455/484/488/502/507/536/541/582/586/606/610 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.

- L288-294 - Instead of doing an if/else with return true/return false, directly return the condition.


