## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Instances|
|-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 9 |
| [GAS&#x2011;2](#GAS&#x2011;2) | Empty Blocks Should Be Removed Or Emit Something | 6 |
| [GAS&#x2011;3](#GAS&#x2011;3) | `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables | 9 |
| [GAS&#x2011;4](#GAS&#x2011;4) | `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops | 15 |
| [GAS&#x2011;5](#GAS&#x2011;5) | Splitting `require()` Statements That Use `&&` Saves Gas | 3 |
| [GAS&#x2011;6](#GAS&#x2011;6) | `abi.encode()` Is Less Efficient Than `abi.encodepacked()` | 5 |
| [GAS&#x2011;7](#GAS&#x2011;7) | Help The Optimizer By Saving A Storage Variable’s Reference Instead Of Repeatedly Fetching It | 2 |
| [GAS&#x2011;8](#GAS&#x2011;8) | Use calldata instead of memory for function parameters | 17 |
| [GAS&#x2011;9](#GAS&#x2011;9) | Use assembly to check for `address(0)` | 1 |
| [GAS&#x2011;10](#GAS&#x2011;10) | Using `10**X` for constants isn't gas efficient  | 3 |
| [GAS&#x2011;11](#GAS&#x2011;11) | Public Functions To External | 48 |
| [GAS&#x2011;12](#GAS&#x2011;12) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 3 |
| [GAS&#x2011;13](#GAS&#x2011;13) | Optimize names to save gas | 9 |

Total: 130 instances over 13 issues

## Gas Optimizations

### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```
218: mapping(address => uint256) private _bondedOperators;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L218

```
223: mapping(address => uint256) private _operatorPodIndex;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L223

```
228: mapping(address => uint256) private _bondedAmounts;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L228

```
156: mapping(address => uint256) private _balances;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L156

```
161: mapping(address => mapping(address => uint256)) private _allowances;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L161

```
186: mapping(address => uint256) private _nonces;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L186

```
185: mapping(address => uint256) private _ownedTokensCount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L185

```
190: mapping(address => uint256[]) private _ownedTokens;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L190

```
196: mapping(address => mapping(address => bool)) private _operatorApprovals;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L196



### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Empty Blocks Should Be Removed Or Emit Something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})

#### <ins>Proof Of Concept</ins>

```
1209: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1209

```
212: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol#L212

```
212: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol#L212

```
223: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L223

```
251: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L251

```
962: receive() external payable {}
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L962






### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

#### <ins>Proof Of Concept</ins>


```
378: _bondedAmounts[job.operator] -= amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L378

```
382: _bondedAmounts[msg.sender] += amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L382

```
834: _bondedAmounts[operator] += amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L834

```
1175: position -= threshold;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1175

```
1177: current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L1177

```
633: _totalSupply -= amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L633

```
685: _totalSupply += amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L685

```
686: _balances[to] += amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L686

```
702: _balances[recipient] += amount;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L702





### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

#### <ins>Proof Of Concept</ins>


```
781: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L781

```
871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L871

```
564: for (uint256 i = 0; i < wallets.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L564

```
357: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L357

```
716: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L716

```
307: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L307

```
323: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L323

```
340: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L340

```
356: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L356

```
394: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L394

```
414: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L414

```
432: for (uint256 t = 0; t < tokenAddresses.length; t++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L432

```
437: for (uint256 i = 0; i < addresses.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L437

```
454: for (uint256 i = 0; i < addresses.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L454

```
474: for (uint256 i = 0; i < addresses.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L474



### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Splitting `require()` Statements That Use `&&` Saves Gas

See https://github.com/code-423n4/2022-01-xdefi-findings/issues/128 which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

#### <ins>Proof Of Concept</ins>

```
857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L857

```
166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L166

```
263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L263





### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> `abi.encode()` Is Less Efficient Than `abi.encodepacked()`

#### <ins>Proof Of Concept</ins>


```
252: abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L252

```
409: return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L409

```
471: abi.encode(
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L471

```
260: abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L260

```
426: return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L426





### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> Help The Optimizer By Saving A Storage Variable's Reference Instead Of Repeatedly Fetching It

To help the optimizer, declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array.
The effect can be quite significant.
As an example, instead of repeatedly calling someMap[someIndex], save its reference like this: SomeStruct storage someStruct = someMap[someIndex] and use it.

#### <ins>Proof Of Concept</ins>


```
782: operators[i] = _operatorPods[pod][index + i];
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L782


```
358: tokenIds[i] = _ownedTokens[wallet][index + i];
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L358






### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> Use calldata instead of memory for function parameters

In some cases, having function arguments in calldata instead of
memory is more optimal.

Consider the following generic example:
```
contract C {
	function add(uint[] memory arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above example, the dynamic array arr has the storage location
memory. When the function gets called externally, the array values are
kept in calldata and copied to memory during ABI decoding (using the
opcode calldataload and mstore). And during the for loop, arr[i]
accesses the value in memory using a mload. However, for the above
example this is inefficient. Consider the following snippet instead:
```
contract C {
	function add(uint[] calldata arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above snippet, instead of going via memory, the value is directly
read from calldata using calldataload. That is, there are no
intermediate memory operations that carries this value.

Gas savings: In the former example, the ABI decoding begins with
copying value from calldata to memory in a for loop. Each iteration
would cost at least 60 gas. In the latter example, this can be
completely avoided. This will also reduce the number of instructions and
therefore reduces the deploy time cost of the contract.

In short, use calldata instead of memory if the function argument
is only read.

Note that in older Solidity versions, changing some function arguments
from memory to calldata may cause "unimplemented feature error".
This can be avoided by using a newer (0.8.*) Solidity compiler.

Examples
Note: The following pattern is prevalent in the codebase:
```
function f(bytes memory data) external {
	(...) = abi.decode(data, (..., types, ...));
}
```
Here, changing to bytes calldata will decrease the gas. The total
savings for this change across all such uses would be quite
significant.

#### <ins>Proof Of Concept</ins>


```
function getPodOperators(uint256 pod) external view returns (address[] memory operators) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L738

```
function getPodOperators(
    uint256 pod,
    uint256 index,
    uint256 length
  ) external view returns (address[] memory operators) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L738

```
function tokensOfOwner(address wallet) external view returns (uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L336

```
function tokensOfOwner(
    address wallet,
    uint256 index,
    uint256 length
  ) external view returns (uint256[] memory tokenIds) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L336

```
function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L710

```
function _getPayoutAddresses() private view returns (address payable[] memory addresses) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L298

```
function _setPayoutAddresses(address payable[] memory addresses) private {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L316

```
function _getPayoutBps() private view returns (uint256[] memory bps) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L332

```
function _setPayoutBps(uint256[] memory bps) private {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L349

```
function _payoutTokens(address[] memory tokenAddresses) private {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L426

```
function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L471

```
function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L488

```
function getTokensPayout(address[] memory tokenAddresses) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L517

```
function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L558

```
function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L569

```
function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L585

```
function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L604






### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Use assembly to check for `address(0)`

Save 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
	if iszero(_addr) {
		mstore(0x00, "AddressZero")
		revert(0x00, 0x20)
	}
}
```

#### <ins>Proof Of Concept</ins>


```
function balanceOf(address wallet) public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L638






### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> Using `10**X` for constants isn't gas efficient 

In Solidity, a constant expression in a variable will compute the expression everytime the variable is called. It's not the result of the expression that is stored, but the expression itself.

As Solidity supports the scientific notation, constants of form 10**X can be rewritten as 1eX to save the gas cost from the calculation with the exponentiation operator **.

#### <ins>Proof Of Concept</ins>


```
256: _baseBondAmount = 100 * (10**18); // one single token unit * 100
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L256

```
274: return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L274

```
293: return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol#L293



#### <ins>Recommended Mitigation Steps</ins>

Replace 10**X with 1eX



### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```
function getJobDetails(bytes32 jobHash) public view returns (OperatorJob memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L690

```
function getHolographEnforcer() public view returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol#L192

```
function decimals() public view returns (uint8) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L273

```
function allowance(address account, address spender) public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L297

```
function balanceOf(address account) public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L301

```
function DOMAIN_SEPARATOR() public view returns (bytes32) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L306

```
function name() public view returns (string memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L310

```
function nonces(address account) public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L314

```
function symbol() public view returns (string memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L318

```
function totalSupply() public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L322

```
function approve(address spender, uint256 amount) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L326

```
function burn(uint256 amount) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L337

```
function burnFrom(address account, uint256 amount) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L347

```
function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L363

```
function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L420

```
function onERC20Received(
    address account,
    address sender,
    uint256 amount,
    bytes calldata data
  ) public returns (bytes4) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L439

```
function permit(
    address account,
    address spender,
    uint256 amount,
    uint256 deadline,
    uint8 v,
    bytes32 r,
    bytes32 s
  ) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L460

```
function safeTransfer(address recipient, uint256 amount) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L492

```
function safeTransfer(
    address recipient,
    uint256 amount,
    bytes memory data
  ) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L492

```
function safeTransferFrom(
    address account,
    address recipient,
    uint256 amount
  ) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L512

```
function safeTransferFrom(
    address account,
    address recipient,
    uint256 amount,
    bytes memory data
  ) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L512

```
function transfer(address recipient, uint256 amount) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L580

```
function transferFrom(
    address account,
    address recipient,
    uint256 amount
  ) public returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L591

```
function owner() public view override returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L740

```
function safeTransferFrom(
    address from,
    address to,
    uint256 tokenId,
    bytes memory data
  ) public payable {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L436

```
function transferFrom(
    address from,
    address to,
    uint256 tokenId
  ) public payable {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L599

```
function transferFrom(
    address from,
    address to,
    uint256 tokenId,
    bytes memory data
  ) public payable {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L599

```
function balanceOf(address wallet) public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L638

```
function burned(uint256 tokenId) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L643

```
function exists(uint256 tokenId) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L656

```
function owner() public view override returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L935

```
function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L471

```
function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L488

```
function getEthPayout() public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L497

```
function getTokenPayout(address tokenAddress) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L507

```
function getTokensPayout(address[] memory tokenAddresses) public {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L517

```
function setRoyalties(
    uint256 tokenId,
    address payable receiver,
    uint256 bp
  ) public onlyOwner {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L529

```
function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L549

```
function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L558

```
function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L569

```
function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L585

```
function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L604

```
function tokenCreator(
    address,
    
    uint256 tokenId
  ) public view returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L621

```
function calculateRoyaltyFee(
    address,
    
    uint256 tokenId,
    uint256 amount
  ) public view returns (uint256) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L634

```
function marketContract() public view returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L649

```
function tokenCreators(uint256 tokenId) public view returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L655

```
function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L665

```
function getTokenAddress(string memory tokenName) public view returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L683






### <a href="#Summary">[GAS&#x2011;12]</a><a name="GAS&#x2011;12"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contractâ€™s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```
208: uint32 private _operatorTempStorageCounter;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol#L208

```
181: uint8 private _decimals;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L181

```
160: uint16 private _bps;
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L160



### <a href="#Summary">[GAS&#x2011;13]</a><a name="GAS&#x2011;13"> Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://github.com/enzosv/solidity-optimize-name) link for an example of how it works. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### <ins>Proof Of Concept</ins>

```
File: \2022-10-holograph\contracts\HolographFactory.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol

```
File: \2022-10-holograph\contracts\HolographOperator.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographOperator.sol

```
File: \2022-10-holograph\contracts\abstract\ERC20H.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC20H.sol

```
File: \2022-10-holograph\contracts\abstract\ERC721H.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/ERC721H.sol

```
File: \2022-10-holograph\contracts\enforcer\Holographer.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/Holographer.sol

```
File: \2022-10-holograph\contracts\enforcer\HolographERC20.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol

```
File: \2022-10-holograph\contracts\enforcer\HolographERC721.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol

```
File: \2022-10-holograph\contracts\enforcer\PA1D.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol

```
File: \2022-10-holograph\contracts\module\LayerZeroModule.sol
```

https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/module/LayerZeroModule.sol

