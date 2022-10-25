## Use modifiers instead of using similar require statements 
### Description:

Using the same require statements again and again may make the code less readable , instead put those conditions inside a modifier and use those 
modifier on the function.

Affected code:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L203
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L255
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L352

consider putting them inside a modifier.

##  Functions lack natspec comments/Dev comments

### Description:

There were many instances where the functions lacked natspec comments/dev comments telling about the functionality of the function.

Affected Functions without comments:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L298
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L298
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L332
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L349
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L365
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L372
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L558
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L634
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L413
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L643




## Wrong comment Describing a mapping 

### Description:

The mapping on https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L352 
and https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L223 have the same description , whereas
the comments describing operatorPodIndex should be something like the mapping for an address to the index of the operator pod instead of 
`Internal mapping of bonded operators, to prevent double bonding`

## Consider Changing the name of the function to something else

## Description:

The function on https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L205 is being declared as onlyOwner 
whereas it permits the access to admins too(not owners) , there fore consider changing the name of the function to something like onlyAuth or something else.

## Wrong grammar used

### Description: 

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L319 Use single the instead of two `the`

## Considering changing the name of the function to avoid confusion

### Description:

The functions on https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L336 and 
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L347 have the same name i.e `tokensOfOwner` , instead consider changing the latter to `tokensOfOwnerLength` to avoid confusion.

## No zero address check On token approval

### Description:

The function on https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L368 does not check if
the `to` address is 0 address or not , if it is a zero address the token would be assigned to the zero address and be lost forever as the mapping 
`_tokenOwner[tokenId]` would now point to zero address and no further approvals can be made.
 
Add a zero address check for this .

Similar thing happens on https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L483 where there are no zero address checks on the address `to`  , a user can mistakenly give the zero address in the `to` field and approve all his/her nft's to the zero address making them lost/locked forever






