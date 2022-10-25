### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [NC-01 ]| Include return parameters in NatSpec comments | All contracts |
| [NC-02] |`0` address check |12|
| [NC-03] |For modern and more readable code; update import usages | All contracts |
| [NC-04] | `Empty blocks` should be _removed_ or _Emit_ something | 5 |
| [NC-05] | `Function writing` that does not comply with the `Solidity Style Guide` | All contracts |
| [NC-06] | Omissions in Events | 4 |
| [NC-07] |Need Fuzzing test | 9 |
| [NC-08] | Use a more recent version of Solidity | All contracts |
| [NC-09] | NatSpec comments should be increased in contracts | All contracts |

Total 9 issues

### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]|Missing Event for Critical Parameters Change | 20 |
|[L-02]|Use `block.number` instead _block.timestamp_|1 |
|[L-03]|Use ```safeTransferOwnership``` instead of ```transferOwnership``` function|3 |
|[L-04]|Use ```safeSetAdmin``` instead of ```setAdmin``` function| 8 |
|[L-05]|Signature Malleability of EVM's ecrecover()| 1 |

Total 5 issues

### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Add to _blacklist function_ |
| [S-02] |Generate perfect code headers every time |

Total 2 suggestions


### [NC-01] Include return parameters in NatSpec comments

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.17/natspec-format.html

**Recommendation:**
Include return parameters in NatSpec comments

_Recommendation  Code Style: (from Uniswap3)_
```js
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```

### [NC-02] `0 address` check

**Context:**
[Holograph.sol#L69-L75](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L69-L75)
[HolographBridge.sol#L65](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L65)
[HolographFactory.sol#L46](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographFactory.sol#L46)
[HolographOperator.sol#L143](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L143)
[Holographer.sol#L51](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L51)
[LayerZeroModule.sol#L61](https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L61)
[Holograph.sol#L148](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L148)
[Holograph.sol#L189](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L189)
[Holograph.sol#L209](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L209)
[Holograph.sol#L229](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L229)
[Holograph.sol#L249](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L249)
[Holograph.sol#L269](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L269)


**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case. This is best practice or instead of suggesting that they verify address != 0x0, you could add some good NatSpec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

**Recommendation:**
like this;
`if (routerV2== address(0)) revert ADDRESS_ZERO();`


### [NC-03] For modern and more readable code; update import usages

**Context:**
All scope contracts

**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;
```js
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```

### [NC-04] `Empty blocks` should be _removed_ or _Emit_ something

**Context:**
[ERC20H.sol#L113](https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC20H.sol#L113)
[ERC721H.sol#L113](https://github.com/code-423n4/2022-10-holograph/blob/main/src/abstract/ERC721H.sol#L113)
[Holographer.sol#L124](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/Holographer.sol#L124) 
[HolographERC20.sol#L152](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L152) 
[HolographERC721.sol#L863](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L863)

**Description:**
Code contains empty block

**Recommendation:**
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

If the code is not upgradeable and this function is not used, remove it directly


### [NC-05] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
All scope contracts

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private
- within a grouping, place the view and pure functions last


### [NC-06] Compliance with Solidity Style rules in constant expressions

**Context:**
All scope contracts

**Recommendation:**
Variables are declared as constant utilize the UPPER_CASE_WITH_UNDERSCORES format.


### [NC-07] Need Fuzzing test

**Context:**
Project uncheckeds list:

```js
9 results – 1 file

contracts/enforcer/HolographERC20.sol:
  349      require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  350:     unchecked {
  351        _allowances[account][msg.sender] = currentAllowance - amount;

  366      uint256 newAllowance;
  367:     unchecked {
  368        newAllowance = currentAllowance - subtractedValue;

  400        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  401:       unchecked {
  402          _allowances[from][sender] = currentAllowance - amount;

  422      uint256 newAllowance;
  423:     unchecked {
  424        newAllowance = currentAllowance + addedValue;
  425      }
  426:     unchecked {
  427        require(newAllowance >= currentAllowance, "ERC20: increased above max value");

  529          require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  530:         unchecked {
  531            _allowances[account][msg.sender] = currentAllowance - amount;

  599          require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  600:         unchecked {
  601            _allowances[account][msg.sender] = currentAllowance - amount;

  629      require(accountBalance >= amount, "ERC20: amount exceeds balance");
  630:     unchecked {
  631        _balances[account] = accountBalance - amount;

  698      require(accountBalance >= amount, "ERC20: amount exceeds balance");
  699:     unchecked {
  700        _balances[account] = accountBalance - amount;
```

**Description:**
In total 1 contract, 9 unchecked are used, the functions used are critical. For this reason, there must be fuzzing tests in the tests of the project. Not seen in tests.

**Recommendation:**
Use should fuzzing test like Echidna.

As Alberto Cuesta Canada said:
_Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now._

https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05


### [NC-08] Use a more recent version of Solidity

**Context:**
All contracts

**Description:**
The solidity version 0.8.13 has below two issues:

Vulnerability related to `ABI-encoding`.
ref : https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/

Vulnerability related to `Optimizer Bug Regarding Memory Side Effects of Inline Assembly`
ref : https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/

**Recommendation:**
Old version of Solidity is used `(0.8.13)`, newer version can be used `(0.8.17)`



### [NC-09] NatSpec comments should be increased in contracts

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
NatSpec comments should be increased in contracts


### [L-01] Missing Event for Critical Parameters Change

**Context:**
[PA1D.sol#L471](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471), [Holograph.sol#L107](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L107), [Holograph.sol#L128](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L128), [Holograph.sol#L148](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L148), [Holograph.sol#L169](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L169), [Holograph.sol#L189](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L189), [Holograph.sol#L209](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L209), [Holograph.sol#L229](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L229), [Holograph.sol#L249](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L249), [Holograph.sol#L269](https://github.com/code-423n4/2022-10-holograph/blob/main/src/Holograph.sol#L269), [HolographBridge.sol#L353](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L353), [HolographBridge.sol#L373](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L373), [HolographBridge.sol#L403](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L403), [HolographBridge.sol#L423](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L423), [HolographFactory.sol#L280](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280), [HolographFactory.sol#L300](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300), [LayerZeroModule.sol#L320](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320) [LayerZeroModule.sol#L340](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340), [LayerZeroModule.sol#L360](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360), [LayerZeroModule.sol#L380](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380)

**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

**Recommendation:**
Add ```event-emit```


### [L-02] Use `block.number` instead _block.timestamp_

**Context:**
[HolographERC20.sol#L469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469)

**Description:**
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

**Recommendation:**
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking) , It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.


### [L-03] Use ```safeTransferOwnership``` instead of ```transferOwnership``` function

**Context:**
[HolographERC20.sol#L9](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L9)
[HolographERC721.sol#L7](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L7)
[PA1D.sol#L7](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L7)

```js
contracts/abstract/Owner.sol:
  139  
  140:   function transferOwnership(address newOwner) public onlyOwner {
  141      require(newOwner != address(0), "HOLOGRAPH: zero address");
  142      assembly {
  143       sstore(_ownerSlot, ownerAddress)
  144     }
  145     emit OwnershipTransferred(previousOwner, ownerAddress);
  146   }
```

**Description:**
```transferOwnership``` function is used to change Ownership

Use a 2 structure transferOwnership which is safer. 
```safeTransferOwnership```,  use it is more secure due to 2-stage ownership transfer.

**Recommendation:**
Use ``Ownable2Step.sol``
[Ownable2Step.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)

```js
 /**
     * @dev The new owner accepts the ownership transfer.
     */
    function acceptOwnership() external {
        address sender = _msgSender();
        require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
        _transferOwnership(sender);
    }
}
```


### [L-04] Use ```safeSetAdmin``` instead of ```setAdmin``` function

**Context:**
```js
8 results - 8 files
contracts/Holograph.sol:
103 
104: import "./abstract/Admin.sol";
contracts/HolographBridge.sol:
103 
104: import "./abstract/Admin.sol";

contracts/HolographFactory.sol:
103 
104: import "./abstract/Admin.sol";

contracts/enforcer/Holographer.sol:
103 
104: import "../abstract/Admin.sol";

contracts/enforcer/HolographERC20.sol:
103 
104: import "../abstract/Admin.sol";
contracts/enforcer/HolographERC721.sol:
103 
104: import "../abstract/Admin.sol";

contracts/enforcer/PA1D.sol:
103 
104: import "../abstract/Admin.sol";
contracts/module/LayerZeroModule.sol:
103 
104: import "../abstract/Admin.sol";
```

**Description:**
``` setAdmin ``` function is used to change `admin`

Use a 2 structure transferOwnership which is safer. 
``` setAdmin ```,  use it is more secure due to 2-stage ownership transfer.

**Recommendation:**
Use 2-stage ownership transfer abstract contract:


```js
abstract contract Ownable2Step is Ownable {
    address private _pendingAdmin;

    event AdminTransferStarted(address indexed previousAdmin, address indexed newAdmin);

    /**
     * @dev Returns the address of the pending Admin.
     */
    function pendingAdmin() public view virtual returns (address) {
        return _pendingAdmin;
    }

    /**
     * @dev Starts the Admin transfer of the contract to a new account. Replaces the pending transfer if there is one.
     * Can only be called by the current Admin.
     */
    function transferAdmin(address newAdmin) public virtual override onlyAdmin {
        _pendingAdmin = newAdmin;
        emit AdminTransferStarted(Admin(), newAdmin);
    }

    /**
     * @dev Transfers Admin of the contract to a new account (`newAdmin`) and deletes any pending Admin.
     * Internal function without access restriction.
     */
    function _transferAdmin(address newAdmin) internal virtual override {
        delete _pendingAdmin;
        super._transferAdmin(newAdmin);
    }

    /**
     * @dev The new Admin accepts the Admin transfer.
     */
    function acceptAdmin() external {
        address sender = _msgSender();
        require(pendingAdmin() == sender, "Ownable2Step: caller is not the new Admin");
        _transferAdmin(sender);
    }
}
```


### [L-05] Signature Malleability of EVM's ecrecover()

**Context:**
[HolographFactory.sol#L333](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333)

**Description:**
Description: The function calls the Solidity ecrecover() function directly to verify the given signatures. However, the ecrecover() EVM opcode allows malleable (non-unique) signatures and thus is susceptible to replay attacks.

Although a replay attack seems not possible for this contract, I recommend using the battle-tested OpenZeppelin's ECDSA library.

*Recommendation:**
Use the ecrecover function from OpenZeppelin's ECDSA library for signature verification. (Ensure using a version > 4.7.3 for there was a critical bug >= 4.1.0 < 4.7.3).



### [S-01] Add to _blacklist function_

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.gov/policy-issues/financial-sanctions/recent-actions/20220808

Some of these Projects;
 USDC 
 Aave 
 Uniswap
 Balancer
 Infura
 Alchemy 
 Opensea
 dYdX 

For this reason, every project in the Avalanche network must have a blacklist function, this is a good method to avoid legal problems in the future, apart from the current need.

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier.


### [S-02] Generate perfect code headers every time

**Description:**
I recommend using header for Solidity code layout and readability

https://github.com/transmissions11/headers

```js

/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/

```