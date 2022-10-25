### Time based Logic on setter functions 

consider adding to all of these setter function time based logic so that users have a chance to do what they need to before a major address changes to the protocol, currently a user could be mid transaction when damin decide to change a vital address and this could cause loss of funds for the user depending on what transaction type they are creating at the time. major changes to protocol should also allow users time to do what they need prior to the change, this also allows for any sanity checks to be carried prior to any major changes also

Instances
 
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L320
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L340
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L360
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L380
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L441
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L470


### Setter function should emit an event 

When making major changes to protocol address's or values, an emit/event should also be carried out both for sanity sake and also so dapps can track changes to memory. 

Instances
 
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L320
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L340
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L360
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L380
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L441
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L470

### lack of zero address checks on setters 

All of these setter functions should have 0 address check but they do not make use of this, consider adding in 0 address checks for these functions as well as well defined require strings to inform the user what is required if the wrong values are entered, currently if the user enters a value other than an address (although this should be obvious to the user) it wont actually revert with a reason such as "cannot use 0 address" or the likes of to let them know what they did wrong.
Instances
 
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L320
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L340
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L360
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L380
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L441
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L470

### Missing @dev comments
 
Natspec and dev comments help users to understand how the code work, missing or incomplete comments can hurt readability and comprehension of the code consider adding more comments fixing spelling mistakes and typos in these instances 

layerzeromodule.sol missing dev comments @params or @returns
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L227
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L156
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L251
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L277
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L296

PA1D.sol 
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L185
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L549
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L557
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L569
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L590
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L604
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L621
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L634
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L649
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L655
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L665

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L508
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L520
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L935
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L968

### Spelling mistakes

consider fixing the following typos, spelling mistakes and incomplete comments for the sake of readability

PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L466

@dev Function can only we called by owner, admin, or identity wallet. (should be "Function can only 'be' called by owner,)

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L203

line 203 @return Returns true is message sender is an owner. should b2 (@return Returns true 'if' message sender is an owner.)

HolographERC20.sol

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L810

### Remove @devs comments with regard to code use 

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L487

     * @dev would be a good idea to check payload gas price here and if it is significantly lower than current amount
     *      to set zero address as operator to not lock-up an operator unnecessarily

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L617

 paid.sol line617
  // SuperRare
  // Hint taken from Manifold's RoyaltyEngine(https://github.com/manifoldxyz/royalty-registry-solidity/blob/main/contracts/RoyaltyEngineV1.sol)
  // To be quite honest, SuperRare is a closed marketplace. They're working on opening it up but looks like they want to use private smart contracts.
  // We'll just leave this here for just in case they open the flood gates.

### Incomplete dev comments can lead to confusion over the use of the code 

consider completing this comment to help with readability and understaning what this part of the code does.

PA1D.sol line 666 // this information is outside of the scope of our

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L666

### Redundant code left in the .sol files

Remove all redundant and commented out code before deployment to mainnet

PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L579
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L579

holographerc721.sol
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L525
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L540
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L555

line 810 fix devs typo for the comments for this function (assume the devs means "Can not mint the token to a zero address")  @dev Can to mint the token to the zero address and the token cannot already exist.
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L810

### Constants over literal values

when possible consider asigning literal values to constants to make readability and code comprehension slightly easier

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L916

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L721

### Adding nonce to signature verification

consider employing the use of recording the nonce as well here to 100% gaurantee the sig is not replayable

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L206

consider adding 

public uint256 nonce;

require(_nonce == nonce++);
require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

Or something to this effect....

### Temporary functions left in code

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L275

Remove this temporary function from the code before deployment.

currently admin can reset all aspects of operator, it may be advisable to remove this code if it is not needed now, as admin may do this inadvertantly if they are a new user and unsure what they are doing (always account for human error some of us cant help but push buttons to see what they do) , would appear this was just used during production to make life easier for the devs, so in this case just advisory to remove the code.

line 275    * @dev temp function, used for quicker updates/resets during development
   *      NOT PART OF FINAL CODE !!!
   */

### Execute job function does not actually delete value 

using the keyword delete does not remove this but set the element to 0, use the key word pop() to remove values so that not only is the value set to zero but the array element is deleted also (so as not to end up with an array full of 0 values) 
also noted this was to gaurd against replay attacks, only again the value was never deleted but simply set to 0, the dev should check if there is an instance of replay attack that cna be exploited if value is set to 0 because of this (ran out of time on the contest hence submitted on the end of my QA for FYI sanity check.) 

    * @dev to prevent replay attacks, remove job from mapping
     */
    delete _operatorJobs[hash];

holographOperator.sol

mapping(bytes32 => uint256) private _operatorJobs;

309
    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
    uint256 gasLimit = 0;
    uint256 gasPrice = 0;
    assembly {

329 
    delete _operatorJobs[hash];

### Constants should follow solidity naming comventions
 
Constant/imutable values should be ALL_CAPS  all constant accross all files do not follow the standard naming convention for naming constants but instead utilise _constant, it shoud be noted that the standard convention is infact ALL_CAPS for anything constant and or immutable.

holographBridge.sol, holographOperator, holographFactory, layerZeroModule, holographer, pa1d, holographERC712, holographERC20










