### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] |Using storage instead of memory for struct saves gas | 1 |
| [G-02] | _Functions guaranteed to revert_ when callled by normal users can be marked `payable`  | 23 |
| [G-03] |```x += y  (x -= y)``` costs more gas than ```x = x + y (x = x – y)``` for state variables | 5 |
| [G-04] | Optimize names to save gas | All contracts |
| [G-05] | Use assembly to check for ```address(0)``` | 9|
| [G-06] | The solady Library's Ownable contract is significantly gas-optimized, which can be used| 21 |
| [G-07] |It is more efficient to use `abi.encodePacked` instead of `abi.encode`  | 5 |
| [G-08] | Sort Solidity operations using short-circuit mode| 15 |
| [G-09] | Use a different pattern when using for loops| 13 |
| [G-10] |Use inline assembly for contract balance | 3 |

Total 10 issues


### [G-01] Using storage instead of memory for struct saves gas

**Context:**
[HolographFactory.sol#L193-L194](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L193-L194)


**Description:**
When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. 


**Recommendation:**

Current Code:
```js
HolographFactory.sol#L193-L194

DeploymentConfig memory config,
Verification memory signature,
```
Recommendation Code:
```js
HolographFactory.sol#L193-L194

DeploymentConfig storage config,
Verification storage signature,
```


### [G-02] _Functions guaranteed to revert_ when callled by normal users can be marked `payable` 

**Context:** 

```js
23 results 5 files

contracts/enforcer/PA1D.sol:
  470     */
  471:   function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
  472      require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

  532      uint256 bp
  533:   ) public onlyOwner {
  534      if (tokenId == 0) {

contracts/Holograph.sol:
205 */
206: function setBridge(address bridge) external onlyAdmin {
207 assembly {

226 */
227: function setChainId(uint256 chainId) external onlyAdmin {
228 assembly {

246 */
247: function setFactory(address factory) external onlyAdmin {
248 assembly {

267 */
268: function setHolographChainId(uint32 holographChainId) external onlyAdmin {
269 assembly {

287 */
288: function setInterfaces(address interfaces) external onlyAdmin {
289 assembly {

307 */
308: function setOperator(address operator) external onlyAdmin {
309 assembly {

327 */
328: function setRegistry(address registry) external onlyAdmin {
329 assembly {

347 */
348: function setTreasury(address treasury) external onlyAdmin {
349 assembly {

367 */
368: function setUtilityToken(address utilityToken) external onlyAdmin {
369 assembly {

contracts/HolographBridge.sol:
451 */
452: function setFactory(address factory) external onlyAdmin {
453 assembly {

471 */
472: function setHolograph(address holograph) external onlyAdmin {
473 assembly {

501 */
502: function setOperator(address operator) external onlyAdmin {
503 assembly {

521 */
522: function setRegistry(address registry) external onlyAdmin {
523 assembly {

contracts/HolographFactory.sol:
279 */
280: function setHolograph(address holograph) external onlyAdmin {
281 assembly {

299 */
300: function setRegistry(address registry) external onlyAdmin {
301 assembly {

contracts/module/LayerZeroModule.sol:
319 */
320: function setBridge(address bridge) external onlyAdmin {
321 assembly {

339 */
340: function setInterfaces(address interfaces) external onlyAdmin {
341 assembly {

359 */
360: function setLZEndpoint(address lZEndpoint) external onlyAdmin {
361 assembly {

379 */
380: function setOperator(address operator) external onlyAdmin {
381 assembly {

440 */
441: function setBaseGas(uint256 baseGas) external onlyAdmin {
442 assembly {

469 */
470: function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
471 assembly {
```

**Description:**
If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

**Recommendation:**
Functions guaranteed to revert when called by normal users can be marked payable  (for only ```onlyowner or admin``` functions)

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo();
        c1.foo();
    }
}

contract Contract0 {
    uint256 versionNFTDropCollection;
    
    function foo() external {
        versionNFTDropCollection++;
    }
}

contract Contract1 {
    uint256 versionNFTDropCollection;
    
    function foo() external payable {
        versionNFTDropCollection++;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44293                                     ┆ 252             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22308           ┆ 22308 ┆ 22308  ┆ 22308 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 41893                                     ┆ 240             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22284           ┆ 22284 ┆ 22284  ┆ 22284 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```

### [G-03] ```x += y  (x -= y)``` costs more gas than ```x = x + y (x = x – y)``` for state variables 

**Context:**
```js
5 results - 3 files
contracts/HolographFactory.sol:

327 if (v < 27) {
328: v += 27;
329 }

contracts/enforcer/HolographERC20.sol:

684 require(to != address(0), "ERC20: minting to burn address");
685: _totalSupply += amount;
686: _balances[to] += amount;
687 emit Transfer(address(0), to, amount);

701 }
702: _balances[recipient] += amount;
703 emit Transfer(account, recipient, amount);

contracts/enforcer/HolographERC20.sol:

632 }
633: _totalSupply -= amount;
634 emit Transfer(account, address(0), amount);
```

**Description:**
```x += y  (x -= y)``` costs more gas than ```x = x + y (x = x – y)``` for state variables.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
  
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.swap(2,3);
        c1.swap1(2,3);
    }
}
contract Contract0 {
    uint256 public amountIn = 10;
    
    
    function swap(uint256 amountInToBin, uint256 fee)  external returns (uint256 ) {
       
       return amountIn -= amountInToBin + fee;
    }
}

contract Contract1 {
    uint256 public amountIn = 10;
    
    function swap1(uint256 amountInToBin, uint256 fee)  external returns (uint256 result1) {
       return (amountIn = amountIn - (amountInToBin + fee));
    }
}
```
**Gas Report:**
```js
╭──────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/test.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞══════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 83017                                ┆ 341             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ swap                                 ┆ 5454            ┆ 5454 ┆ 5454   ┆ 5454 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭──────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/test.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞══════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 83017                                ┆ 341             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ swap1                                ┆ 5431            ┆ 5431 ┆ 5431   ┆ 5431 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

### [G-04] Optimize names to save gas

**Context:** 
All Contracts

**Description:** 
Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ```HolographBridge.sol ``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

HolographBridge.sol function names can be named and sorted according to METHOD ID

```js
========================
Sighash | Function Signature
========================
4ddf47d4 => init(bytes)
16f1be70 => bridgeInRequest(uint256,uint32,address,address,address,uint256,bool,bytes)
e5585666 => bridgeOutRequest(uint32,address,uint256,uint256,bytes)
565ff49e => revertedBridgeOutRequest(address,uint32,address,bytes)
636ee68b => getBridgeOutRequestPayload(uint32,address,uint256,uint256,bytes)
ff1370d9 => getMessageFee(uint32,uint256,uint256,bytes)
88cc58e4 => getFactory()
5bb47808 => setFactory(address)
4827ae0c => getHolograph()
25d5cac8 => setHolograph(address)
6200d9fc => getJobNonce()
e7f43c68 => getOperator()
b3ab15fb => setOperator(address)
5ab1bd53 => getRegistry()
a91ee0dc => setRegistry(address)
c5cc6b6a => _factory()
dbeebe4c => _holograph()
3b18ecb4 => _jobNonce()
70b0a843 => _operator()
79cbc5fa => _registry()
```

### [G-05] Use assembly to check for ```address(0)```

**Context:**
```js
9 results - 1 files

contracts/enforcer/PA1D.sol:

549 function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
550: if (_getReceiver(tokenId) == address(0)) {
551 return (_getDefaultReceiver(), (_getDefaultBp() * value) / 10000);

559 uint256[] memory bps = new uint256[](1);
560: if (_getReceiver(tokenId) == address(0)) {
561 bps[0] = _getDefaultBp();

570 address payable[] memory receivers = new address payable[](1);
571: if (_getReceiver(tokenId) == address(0)) {
572 receivers[0] = _getDefaultReceiver();

592 uint256[] memory bps = new uint256[](1);
593: if (_getReceiver(tokenId) == address(0)) {
594 receivers[0] = _getDefaultReceiver();

606 uint256[] memory bps = new uint256[](1);
607: if (_getReceiver(tokenId) == address(0)) {
608 receivers[0] = _getDefaultReceiver();

626 address receiver = _getReceiver(tokenId);
627: if (receiver == address(0)) {
628 return _getDefaultReceiver();

639 ) public view returns (uint256) {
640: if (_getReceiver(tokenId) == address(0)) {
641 return (_getDefaultBp() * amount) / 10000;

656 address receiver = _getReceiver(tokenId);
657: if (receiver == address(0)) {
658 return _getDefaultReceiver();

668 bidShares.owner.value = 0;
669: if (_getReceiver(tokenId) == address(0)) {
670 bidShares.creator.value = _getDefaultBp();
```

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    function testGas() public view {
        c0.setOperator(address(this));
        c1.setOperator(address(this));
    }
}
contract Contract0 {
    function setOperator(address operator_) public pure {
        require(operator_) != address(0), "INVALID_RECIPIENT");    }
}
contract Contract1 {
    function setOperator(address operator_) public pure {
        assembly {
            if iszero(operator_) {
                mstore(0x00, "Callback_InvalidParams")
                revert(0x00, 0x20)
            }
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50899                                     ┆ 285             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 258             ┆ 258 ┆ 258    ┆ 258 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44893                                     ┆ 255             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 252             ┆ 252 ┆ 252    ┆ 252 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

### [G-06] The solady Library's Ownable contract is significantly gas-optimized, which can be used

**Description:**
The project uses the `onlyOwner` authorization model with the `Ownable.sol` contract. I recommend using _Solady's highly gas optimized contract._

https://github.com/Vectorized/solady/blob/main/src/auth/OwnableRoles.sol

```js
21 results - 4 file

contracts/Holograph.sol:

205 */
206: function setBridge(address bridge) external onlyAdmin {
207 assembly {

226 */
227: function setChainId(uint256 chainId) external onlyAdmin {
228 assembly {

246 */
247: function setFactory(address factory) external onlyAdmin {
248 assembly {

267 */
268: function setHolographChainId(uint32 holographChainId) external onlyAdmin {
269 assembly {

287 */
288: function setInterfaces(address interfaces) external onlyAdmin {
289 assembly {

307 */
308: function setOperator(address operator) external onlyAdmin {
309 assembly {

327 */
328: function setRegistry(address registry) external onlyAdmin {
329 assembly {

347 */
348: function setTreasury(address treasury) external onlyAdmin {
349 assembly {

367 */
368: function setUtilityToken(address utilityToken) external onlyAdmin {
369 assembly {

contracts/HolographBridge.sol:

451 */
452: function setFactory(address factory) external onlyAdmin {
453 assembly {

471 */
472: function setHolograph(address holograph) external onlyAdmin {
473 assembly {

501 */
502: function setOperator(address operator) external onlyAdmin {
503 assembly {

521 */
522: function setRegistry(address registry) external onlyAdmin {
523 assembly {

contracts/HolographFactory.sol:

279 */
280: function setHolograph(address holograph) external onlyAdmin {
281 assembly {

299 */
300: function setRegistry(address registry) external onlyAdmin {
301 assembly {

contracts/module/LayerZeroModule.sol:

319 */
320: function setBridge(address bridge) external onlyAdmin {
321 assembly {

339 */
340: function setInterfaces(address interfaces) external onlyAdmin {
341 assembly {

359 */
360: function setLZEndpoint(address lZEndpoint) external onlyAdmin {
361 assembly {

379 */
380: function setOperator(address operator) external onlyAdmin {
381 assembly {

440 */
441: function setBaseGas(uint256 baseGas) external onlyAdmin {
442 assembly {

469 */
470: function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
471 assembly {
```

### [G-07] It is more efficient to use `abi.encodePacked` instead of `abi.encode`

**Context:**
[HolographFactory.sol#L252](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L252), [HolographERC20.sol#L409](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L409), [HolographERC20.sol#L471](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L471), [HolographERC721.sol#L260](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L260), [HolographERC721.sol#L426](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L426)

**Description:**
 It is more efficient to use `abi.encodePacked` instead of `abi.encode`.

_When are these used?_
```abi.encode:```
abi.encode encode its parameters using the ABI specs. The ABI was designed to make calls to contracts. Parameters are padded to 32 bytes. If you are making calls to a contract you likely have to use abi.encode
If you are dealing with more than one dynamic data type as it prevents collision.

```abi.encodePacked:```
abi.encodePacked encode its parameters using the minimal space required by the type. Encoding an uint8 it will use 1 byte. It is used when you want to save some space, and not calling a contract.Takes all types of data and any amount of input.

```js
contract GasTest is DSTest {
  
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        
        c0.collision("AAA","BBB");
        c1.hard("AAA","BBB");
    }
}

contract Contract0 {

    // abi.encode
    // (AAA,BBB) keccak = 0xd6da8b03238087e6936e7b951b58424ff2783accb08af423b2b9a71cb02bd18b
    // (AA,ABBB) keccak = 0x54bc5894818c61a0ab427d51905bc936ae11a8111a8b373e53c8a34720957abb
    function collision(string memory _text, string memory _anotherText)
        public pure returns (bytes32)
    {
        return keccak256(abi.encode(_text, _anotherText));
    }
}

contract Contract1 {

    // abi.encodePacked
    // (AAA,BBB) keccak = 0xf6568e65381c5fc6a447ddf0dcb848248282b798b2121d9944a87599b7921358
    // (AA,ABBB) keccak = 0xf6568e65381c5fc6a447ddf0dcb848248282b798b2121d9944a87599b7921358

    function hard(string memory _text, string memory _anotherText)
        public pure returns (bytes32)
    {
        return keccak256(abi.encodePacked(_text, _anotherText));
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 132377                                    ┆ 693             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ collision                                 ┆ 1737            ┆ 1737 ┆ 1737   ┆ 1737 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 119365                                    ┆ 628             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ hard                                      ┆ 1573            ┆ 1573 ┆ 1573   ┆ 1573 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```


### [G-08] Sort Solidity operations using short-circuit mode

**Description:**
Short-circuiting is a solidity contract development model that uses ```OR/AND``` logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.
```
//f(x) is a low gas cost operation 
//g(y) is a high gas cost operation 

//Sort operations with different gas costs as follows 
f(x) || g(y) 
f(x) && g(y)
```

**Context:**

15 results - 5 files;
```js
contracts/HolographBridge.sol:

203 require(
204: _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
205 "HOLOGRAPH: not holographed"

255 require(
256: _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
257 "HOLOGRAPH: not holographed"

352 require(
353: _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
354 "HOLOGRAPH: not holographed"

contracts/HolographFactory.sol:

313 }
314: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
315 }

contracts/enforcer/Holographer.sol:

165 bytes4 selector = abi.decode(returnData, (bytes4));
166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
167 _setInitialized();

contracts/enforcer/HolographERC20.sol:

288 if (
289: interfaces.supportsInterface(InterfaceType.ERC20, interfaceId) || erc165Contract.supportsInterface(interfaceId) // check global interfaces // check if source supports interface
290 ) {

380 uint256 fee = 0;
381: if (gasPrice < type(uint256).max && gasLimit < type(uint256).max) {
382 (uint256 hlgFee, ) = _operator().getMessageFee(toChain, gasLimit, gasPrice, bridgeOutPayload);

526 if (account != msg.sender) {
527: if (msg.sender != _holograph().getBridge() && msg.sender != _holograph().getOperator()) {
528 uint256 currentAllowance = _allowances[account][msg.sender];

596 if (account != msg.sender) {
597: if (msg.sender != _holograph().getBridge() && msg.sender != _holograph().getOperator()) {
598 uint256 currentAllowance = _allowances[account][msg.sender];

720 }
721: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
722 }

contracts/enforcer/HolographERC721.sol:

262 bytes4 selector = abi.decode(returnData, (bytes4));
263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
264 }

298 if (
299: interfaces.supportsInterface(InterfaceType.ERC721, interfaceId) || // check global interfaces
300: interfaces.supportsInterface(InterfaceType.PA1D, interfaceId) || // check if royalties supports interface
301 erc165Contract.supportsInterface(interfaceId) // check if source supports interface

907 address tokenOwner = _tokenOwner[tokenId];
908: return (spender == tokenOwner || _tokenApprovals[tokenId] == spender || _operatorApprovals[tokenOwner][spender]);
909 }

915 }
916: return (codehash != 0x0 && codehash != 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470);
917 }
```
**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo();
        c1.foo();
    }
}

contract Contract0 {
    uint8 number = 50;
    uint256 numberbig = 50000000000000;
    
    function foo() public {
        require(numberbig == 50000000000000 ||number == 50 );
    }
}

contract Contract1 {
    uint8 number = 50;
    uint256 numberbig = 50000000000000;
    
    function foo() public {
        require(number == 50 || numberbig == 50000000000000 );
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 72911                                     ┆ 196             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 2262            ┆ 2262 ┆ 2262   ┆ 2262 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 72911                                     ┆ 196             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 2268            ┆ 2268 ┆ 2268   ┆ 2268 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

### [G-09] Use a different pattern when using for loops

**Context:**
```js
13 results - 3 files

contracts/enforcer/HolographERC20.sol:

563 function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
564: for (uint256 i = 0; i < wallets.length; i++) {
565 _mint(wallets[i], amounts[i]);

contracts/enforcer/HolographERC721.sol:

356 tokenIds = new uint256[](length);
357: for (uint256 i = 0; i < length; i++) {
358 tokenIds[i] = _ownedTokens[wallet][index + i];

715 tokenIds = new uint256[](length);
716: for (uint256 i = 0; i < length; i++) {
717 tokenIds[i] = _allTokens[index + i];

contracts/enforcer/PA1D.sol:

306 address payable value;
307: for (uint256 i = 0; i < length; i++) {
308 slot = keccak256(abi.encodePacked(i, slot));

322 address payable value;
323: for (uint256 i = 0; i < length; i++) {
324 slot = keccak256(abi.encodePacked(i, slot));

339 uint256 value;
340: for (uint256 i = 0; i < length; i++) {
341 slot = keccak256(abi.encodePacked(i, slot));

355 uint256 value;
356: for (uint256 i = 0; i < length; i++) {
357 slot = keccak256(abi.encodePacked(i, slot));

393 // uint256 sent;
394: for (uint256 i = 0; i < length; i++) {
395 sending = ((bps[i] * balance) / 10000);

413 //uint256 sent;
414: for (uint256 i = 0; i < length; i++) {
415 sending = ((bps[i] * balance) / 10000);

431 uint256 sending;
432: for (uint256 t = 0; t < tokenAddresses.length; t++) {
433 erc20 = ERC20(tokenAddresses[t]);

436 // uint256 sent;
437: for (uint256 i = 0; i < addresses.length; i++) {
438 sending = ((bps[i] * balance) / 10000);

453 address payable sender = payable(msg.sender);
454: for (uint256 i = 0; i < addresses.length; i++) {
455 if (addresses[i] == sender) {

473 uint256 totalBp;
474: for (uint256 i = 0; i < addresses.length; i++) {
475 totalBp = totalBp + bps[i];
```
**Description:**
In the use of for loops, this structure, which will reduce gas costs, can be preferred. It saves approximately 400 gas in an array with 6 members.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        address[] memory Inputs = new address[](6);
        Inputs[0] = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
        Inputs[1] = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
        Inputs[2] = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
        Inputs[3] = 0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB;
        Inputs[4] = 0x617F2E2fD72FD9D5503197092aC168c91465E7f2;
        Inputs[5] = 0x17F6AD8Ef982297579C203069C1DbfFE4348c372;
        
        c0.dummy(Inputs);
        c1.dummy(Inputs);
    }
}

contract Contract0 {
    function dummy(address[] memory test) public {
        for (uint256 i = 0; i < test.length; ++i) {
        }
    }
}

contract Contract1 { 
    function dummy(address[] memory test) public {
        for (uint256 i = 0; i < test.length; i = unchecked_inc(i)) {
            // do something that doesn't change the value of i
        }
    }
    
    function unchecked_inc(uint i) internal pure returns (uint) {
        unchecked {
            return i + 1;
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 113159                                    ┆ 597             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ dummy                                     ┆ 2044            ┆ 2044 ┆ 2044   ┆ 2044 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 92541                                     ┆ 494             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ dummy                                     ┆ 1666            ┆ 1666 ┆ 1666   ┆ 1666 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

### [G-10]  Use inline assembly for contract balance

**Context:**
```js
3 results 1 file

contracts/enforcer/HolographERC20.sol:

448 }
449: try ERC20(account).balanceOf(address(this)) returns (uint256 balance) {
450 require(balance >= amount, "ERC20: balance check failed");
contracts/enforcer/PA1D.sol:

409 ERC20 erc20 = ERC20(tokenAddress);
410: uint256 balance = erc20.balanceOf(address(this));
411 require(balance > 10000, "PA1D: Not enough tokens to transfer");

433 erc20 = ERC20(tokenAddresses[t]);
434: balance = erc20.balanceOf(address(this));
435 require(balance > 10000, "PA1D: Not enough tokens to transfer");
```

**Description:**
You  can use ```balance(address)``` instead of ```address.balance()``` when getting an contract’s balance of ETH.

**Proof Of Concept**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        
        c0._withdraw(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4);
        c1._withdraw(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4);
    }
}

contract Contract0 {
    
    function _withdraw(address addr) external {
        uint256 amount = address(this).balance;
        (bool sent, ) = addr.call{ value: amount }('');
    }
}

contract Contract1 {
    
    function _withdraw(address addr) external returns (uint256) {
        uint256 amount ;
        assembly {
            amount := selfbalance()
        }
        (bool sent, ) = addr.call{ value: amount }('');
    }
}

```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 55305                                     ┆ 308             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _withdraw                                 ┆ 2949            ┆ 2949 ┆ 2949   ┆ 2949 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 59511                                     ┆ 329             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _withdraw                                 ┆ 509             ┆ 509 ┆ 509    ┆ 509 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```