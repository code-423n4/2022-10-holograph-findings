1.

Deprecated, now commented code should be removed in:

1.1

Contract: HolographERC721.sol

1.1.1

	line 524 - 570
	
2.

Grammer issues in comments in:

2.1

The following lines are the same for all contracts under scope:

2.1.1

	line 66

	"in" should be "to"
	
Modified Comment:

	of the contribution to the software.

2.2

Contract: HolographBridge.sol

2.2.1

	line 160

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.2.2

	line 364

	"non reverted" should be hyphenated into "non-reverted"

Modified Comment:
	
	* @dev a non-reverted result is actually a revert

2.2.3

	line 413

	"chose" should be "chosen"

Modified Comment:

	* @dev @param gasPrice maximum amount to pay for gas price, can be set to 0 and will be chosen automatically

2.2.4

	line 575

	"transfered" should be "transferred"

Modified Comment:

	* @dev Purposefully reverts to prevent having any type of ether transferred into the contract

2.3

Contract: HolographOperator.sol

2.3.1

	line 171

	"limiting" should be "limited"

Modified Comment:

	* @dev The threshold used for limited number of operators in a pod
	
2.3.2	

	line 298

	"is" should be "are"

Modified Comment:

	* @dev When making this call, if operating criteria are not met, the call will revert

2.3.3

	line 335

	"index based" hyphenated into "index-based"

Modified Comment:

	* @dev switch pod to index-based value

2.3.4

	line 348

	"initial" should be "initially"

Modified Comment:

	* @dev validate that initially selected operator time slot is still active

2.3.5

	line 413

	the "has" before "left" is unnecessary

Modified Comment:

	* @dev ensure that there is enough left for the job

2.3.6

	line 443

	"it's" should be "its"

Modified Comment:

	*      Check the executeJob function to understand its implementation

2.3.7

	line 488

	"lock-up" should be "lock up"

Modified Comment:

	*      to set zero address as operator to not lock up an operator unnecessarily

2.3.8

	line 515

	"accomodate" is misspelled and should be "accommodate" 

Modified Comment:

	*      decrease pod size to accommodate popped operator

2.3.9

	line 553

	"rever" is a typo that should be "revert"

Modified Comment:

	* @dev bridgeInRequest doNotRevert is purposefully set to false so a revert would happen

2.3.10

	line 573

	"cross chain" should be hyphenated into "cross-chain"

Modified Comment:

	* @notice Send cross-chain bridge request message   

2.3.11

	line 659

	"chose" should be "chosen"

Modified Comment:

	* @dev @param gasPrice maximum amount to pay for gas price, can be set to 0 and will be chosen automatically

2.3.12

	lines 661, and 662

	"destiantion" should be "destination"

Modified Comments:

	* @return hlgFee the amount (in wei) of native gas token that will cost for finalizing job on destination chain

	* @return msgFee the amount (in wei) of native gas token that will cost for sending message to destination chain

2.3.13

	line 693

	"32 byte" should be hyphenated into "32-byte"

Modified Comment:

	* @dev The job is bitwise packed into a single 32-byte slot, this unpacks it before returning the struct

2.3.14

	line 802

	"token" should be "tokens"

Modified Comment:

	* @return amount total number of utility tokens bonded

2.3.15

	line 855

	"give" should be "given"

Modified Comment:

	* @dev an operator can only bond to one pod at any given time per network
    
2.3.16    
     
	line 997

	"adress" is misspelled and should be "address"

Modified Comment:

	* @dev All cross-chain message requests will get forwarded to this address
	
2.4	
	
Contract: HolographFactory.sol

2.4.1

	line 141

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.4.2

	line 187

	consider turning "to deploy" into "deploying"

	this will then also put an "of" before "smart" for better readability

Modified Comment:

	* @dev Using this function allows deploying of smart contracts that have the same address across all EVM chains

2.4.3

	line 188

	"deployement" is misspelled and should be "deployment"

Modified Comment:

	* @param config contract deployment configurations

2.4.4

	line 237
	
	"user created" should be hyphenated into "user-created"

Modified Comment:

	* @dev deploy the user-created smart contract first

2.5

Contract: LayerZeroModule.sol

2.5.1

	line 120

	"LayerZero specific" should be hyphenated into "LayerZero-specific"

Modified Comment:

	* @dev This contract abstracts all of the LayerZero-specific logic into an isolated module

2.5.2

	line 156

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.5.3

	line 414

	"transfered" is misspelled and should be "transferred"

Modified Comment:

	* @dev Purposefully reverts to prevent having any type of ether transferred into the contract
	
2.6	
	
Contract: Holographer.sol

2.6.1

	line 113

	the "be" before "bridgeable" is unnecessary

Modified Comment:

	* @dev This contract is a binder. It puts together all the variables to make the underlying contracts functional and bridgeable.

2.6.2

	line 145

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.6.3

	lines 181, 190, and 212

	"hardcoded" should be 2 words "hard-coded"
	
Modified Comments:

	* @dev Returns a hard-coded address for the Holograph smart contract.

	* @dev Returns a hard-coded address for the Holograph smart contract that controls and enforces the ERC standards.

	* @dev Returns a hard-coded address for the custom secure storage contract deployed in parallel with this contract deployment.

2.7

Contract: PA1D.sol

2.7.1

	line 150

	"tha" is misspelled and should be "the"

Modified Comment:

	* @param recipients Address array of wallets that will receive the royalties.

2.7.2

	line 156

	"accesible" is misspelled and should be "accessible"

Modified Comment:

	* @dev Use this modifier to lock public functions that should not be accessible to non-owners.

2.7.3

	line 171

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.7.4

	line 202

	the "a" before "more" is unnecessary

Modified Comment:

	* @dev We check owner, admin, and identity for more comprehensive coverage.

2.7.5

	line 203

	"is" should be "if"

Modified Comment:

	* @return Returns true if message sender is an owner.

2.7.6

	line 299

	"optimizaion" is misspelled and should be "optimization"

Modified Comment:

	// The slot hash has been precomputed for gas optimization

2.7.7

	line 387

	"accomodate" is misspelled and should be "accommodate"

Modified Comment:

	// adding 1x for each item in array to accommodate rounding errors

2.7.8

	line 447

	"tranaction" is misspelled and should be "transaction"

Modified Comment:

	* @dev Will revert entire transaction if it fails.

2.7.9

	line 466

	"we" should be "be"

Modified Comment:

	* @dev Function can only be called by owner, admin, or identity wallet.

2.7.10

	line 525

	"parameters" should be "parameter"

Modified Comment:
	
	* @param tokenId Set a specific token id, or leave at 0 to set as default parameter.

2.7.11

	line 620

	the "for" before "just" is unnecessary

	"flood gates" should be "floodgates"

Modified Comment:

	// We'll just leave this here just in case they open the floodgates.

2.7.12

	line 679

	"usages" should be "usage"

Modified Comment:

	* @dev Used only to identify really major/common tokens. Avoid using due to gas usage.

2.8

Contract: HolographERC721.sol

2.8.1

	line 130

	"is" should be "are"

Modified Comment:

	* @dev The entire logic and functionality of the smart contract are self-contained.

2.8.2

	line 236

	"initilaization" is spelled wrong and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.8.3

	line 288

	"4 byte" should be hyphenated into "4-byte"

Modified Comment:

	* @dev Must add new 4-byte interface Ids here to acknowledge support

2.8.4

	line 319

	the second "the" before "Arweave" is unnecessary

Modified Comment:

	* @dev Defaults the Arweave URI

2.8.5

	line 575

	"transfered" is misspelled and should be "transferred"

Modified Comment:

	*  Note: token cannot be transferred if it's locked by bridge.

2.8.6

	line 724

	"token" should be "tokens"

	the "for" after "token" is unnecessary

Modified Comment:

	* @param wallet Specific address for which to get tokens.
	
2.8.7	
	
	line 810
	
	the "to" after "Can" is unnecessary

Modified Comment:

	* @dev Can mint the token to the zero address and the token cannot already exist.

2.8.8

	line 900

	"usecases" should be 2 words "use cases"

Modified Comment:

	* @dev Uses inlined checks for different use cases of approval.

2.9

Contract: HolographERC20.sol
 
2.9.1 
 
	line 136

	"is" should be "are"

Modified Comment:

	* @dev The entire logic and functionality of the smart contract are self-contained.

2.9.2

	line 154

	"addresse's" should be "addresses"

Modified Comment:

	* @dev Mapping of all the addresses balances.

2.9.3

	line 164

	"token" should be "tokens"

Modified Comment:

	* @dev Total number of tokens in circulation.

2.9.4

	line 184

	"used up" should be "used-up"

Modified Comment:

	* @dev List of used-up nonces. Used in the ERC20Permit interface functionality.

2.9.5

	line 216

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.10

Contract: ERC721H.sol

2.10.1

	line 138

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization

2.11

Contract: ERC20H.sol

2.11.1

	line 138

	"initilaization" is misspelled and should be "initialization"

Modified Comment:

	* @param initPayload abi encoded payload to use for contract initialization