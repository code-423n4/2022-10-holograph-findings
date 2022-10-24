## [G-001] Functions guaranteed to revert when called by normal users can be marked payable

### Description:

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Findings:

Total:33

[HolographBridge.sol#L353](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L353)

```
353:    function setFactory(address factory) external onlyAdmin {
```

[HolographBridge.sol#L373](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L373)

```
373:    function setHolograph(address holograph) external onlyAdmin {
```

[HolographBridge.sol#L403](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L403)

```
403:    function setOperator(address operator) external onlyAdmin {
```

[HolographBridge.sol#L423](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L423)

```
423:    function setRegistry(address registry) external onlyAdmin {
```

[HolographFactory.sol#L181](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L181)

```
181:    function setHolograph(address holograph) external onlyAdmin {
```

[HolographFactory.sol#L201](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L201)

```
201:    function setRegistry(address registry) external onlyAdmin {
```

[HolographOperator.sol#L186](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L186)

```
186:    ) external onlyAdmin {
```

[HolographOperator.sol#L850](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L850)

```
850:    function setBridge(address bridge) external onlyAdmin {
```

[HolographOperator.sol#L870](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L870)

```
870:    function setHolograph(address holograph) external onlyAdmin {
```

[HolographOperator.sol#L890](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L890)

```
890:    function setInterfaces(address interfaces) external onlyAdmin {
```

[HolographOperator.sol#L910](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L910)

```
910:    function setMessagingModule(address messagingModule) external onlyAdmin {
```

[HolographOperator.sol#L930](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L930)

```
930:    function setRegistry(address registry) external onlyAdmin {
```

[HolographOperator.sol#L950](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L950)

```
950:    function setUtilityToken(address utilityToken) external onlyAdmin {
```

[HolographERC20.sol#L281](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L281)

```
281:    function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
```

[HolographERC20.sol#L297](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L297)

```
297:    ) external onlyBridge returns (bytes4 selector, bytes memory data) {
```

[HolographERC20.sol#L316](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L316)

```
316:    function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
```

[HolographERC20.sol#L450](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L450)

```
450:    function sourceBurn(address from, uint256 amount) external onlySource {
```

[HolographERC20.sol#L457](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L457)

```
457:    function sourceMint(address to, uint256 amount) external onlySource {
```

[HolographERC20.sol#L464](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L464)

```
464:    function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
```

[HolographERC20.sol#L477](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L477)

```
477:    ) external onlySource {
```

[HolographERC721.sol#L300](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L300)

```
300:    function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {
```

[HolographERC721.sol#L318](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L318)

```
318:    ) external onlyBridge returns (bytes4 selector, bytes memory data) {
```

[HolographERC721.sol#L401](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L401)

```
401:    function sourceBurn(uint256 tokenId) external onlySource {
```

[HolographERC721.sol#L409](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L409)

```
409:    function sourceMint(address to, uint224 tokenId) external onlySource {
```

[HolographERC721.sol#L478](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L478)

```
478:    function sourceTransfer(address to, uint256 tokenId) external onlySource {
```

[PA1D.sol#L372](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L372)

```
372:    function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
```

[PA1D.sol#L434](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L434)

```
434:    ) public onlyOwner {
```

[LayerZeroModule.sol#L221](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L221)

```
221:    function setBridge(address bridge) external onlyAdmin {
```

[LayerZeroModule.sol#L241](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L241)

```
241:    function setInterfaces(address interfaces) external onlyAdmin {
```

[LayerZeroModule.sol#L261](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L261)

```
261:    function setLZEndpoint(address lZEndpoint) external onlyAdmin {
```

[LayerZeroModule.sol#L281](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L281)

```
281:    function setOperator(address operator) external onlyAdmin {
```

[LayerZeroModule.sol#L342](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L342)

```
342:    function setBaseGas(uint256 baseGas) external onlyAdmin {
```

[LayerZeroModule.sol#L371](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L371)

```
371:    function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

## [G-002] Empty blocks should be removed or emit something

### Description:

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

### Findings:

Total:16

[HolographBridge.sol#L56](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographBridge.sol#L56)

```
56:    constructor() {}
```

[HolographFactory.sol#L37](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L37)

```
37:    constructor() {}
```

[HolographOperator.sol#L134](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L134)

```
134:    constructor() {}
```

[HolographOperator.sol#L1110](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L1110)

```
1110:    receive() external payable {}
```

[ERC20H.sol#L34](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L34)

```
34:    constructor() {}
```

[ERC20H.sol#L113](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L113)

```
113:    receive() external payable {}
```

[ERC721H.sol#L34](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L34)

```
34:    constructor() {}
```

[ERC721H.sol#L113](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L113)

```
113:    receive() external payable {}
```

[HolographERC20.sol#L112](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L112)

```
112:    constructor() {}
```

[HolographERC20.sol#L152](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L152)

```
152:    receive() external payable {}
```

[HolographERC721.sol#L132](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L132)

```
132:    constructor() {}
```

[HolographERC721.sol#L863](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L863)

```
863:    receive() external payable {}
```

[Holographer.sol#L41](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/Holographer.sol#L41)

```
41:    constructor() {}
```

[Holographer.sol#L124](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/Holographer.sol#L124)

```
124:    receive() external payable {}
```

[PA1D.sol#L67](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L67)

```
67:    constructor() {}
```

[LayerZeroModule.sol#L52](https://github.com/code-423n4/2022-10-holograph/tree/main/src/module/LayerZeroModule.sol#L52)

```
52:    constructor() {}
```

## [G-003] Not using the named return variables when a function returns, wastes deployment gas

### Description:

It is not necessary to have both a named return and a return statement.

### Findings:

Total:5

[HolographFactory.sol#L82](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L82)

```
82:    ) external pure returns (bytes4 selector, bytes memory data) {
```

[HolographOperator.sol#L618](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L618)

```
618:    function getTotalPods() external view returns (uint256 totalPods) {
```

[HolographOperator.sol#L705](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L705)

```
705:    function getBondedAmount(address operator) external view returns (uint256 amount) {
```

[HolographOperator.sol#L715](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L715)

```
715:    function getBondedPod(address operator) external view returns (uint256 pod) {
```

[HolographERC20.sol#L550](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L550)

```
550:    try ERC20Receiver(recipient).onERC20Received(address(this), account, amount, data) returns (bytes4 retval) {
```

## [G-004] Use Assembly to check for address(0)

### Description:

Saves 6 gas per instance if using assemlby to check for address(0)

### Findings:

Total:14

[HolographOperator.sol#L234](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographOperator.sol#L234)

```
234:    if (job.operator != address(0)) {
```

[HolographERC20.sol#L521](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L521)

```
521:    require(account != address(0), "ERC20: account is zero address");
```

[HolographERC20.sol#L522](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L522)

```
522:    require(spender != address(0), "ERC20: spender is zero address");
```

[HolographERC20.sol#L528](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L528)

```
528:    require(account != address(0), "ERC20: account is zero address");
```

[HolographERC20.sol#L585](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L585)

```
585:    require(to != address(0), "ERC20: minting to burn address");
```

[HolographERC20.sol#L596](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L596)

```
596:    require(account != address(0), "ERC20: account is zero address");
```

[HolographERC20.sol#L597](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L597)

```
597:    require(recipient != address(0), "ERC20: recipient is zero address");
```

[HolographERC721.sol#L320](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L320)

```
320:    require(to != address(0), "ERC721: zero address");
```

[HolographERC721.sol#L540](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L540)

```
540:    require(wallet != address(0), "ERC721: zero address");
```

[HolographERC721.sol#L558](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L558)

```
558:    return _tokenOwner[tokenId] != address(0);
```

[HolographERC721.sol#L590](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L590)

```
590:    require(tokenOwner != address(0), "ERC721: token does not exist");
```

[HolographERC721.sol#L717](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L717)

```
717:    require(to != address(0), "ERC721: minting to burn address");
```

[HolographERC721.sol#L771](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L771)

```
771:    require(to != address(0), "ERC721: use burn instead");
```

[HolographERC721.sol#L796](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L796)

```
796:    return tokenOwner != address(0);
```

## [G-005] Using storage instead of memory for structs/arrays saves gas

### Description:

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Goldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read.

### Findings:

Total:15

[PA1D.sol#L284](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L284)

```
284:    address payable[] memory addresses = _getPayoutAddresses();
```

[PA1D.sol#L285](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L285)

```
285:    uint256[] memory bps = _getPayoutBps();
```

[PA1D.sol#L307](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L307)

```
307:    address payable[] memory addresses = _getPayoutAddresses();
```

[PA1D.sol#L308](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L308)

```
308:    uint256[] memory bps = _getPayoutBps();
```

[PA1D.sol#L328](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L328)

```
328:    address payable[] memory addresses = _getPayoutAddresses();
```

[PA1D.sol#L329](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L329)

```
329:    uint256[] memory bps = _getPayoutBps();
```

[PA1D.sol#L353](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L353)

```
353:    address payable[] memory addresses = _getPayoutAddresses();
```

[PA1D.sol#L442](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L442)

```
442:    address[] memory receivers = new address[](1);
```

[PA1D.sol#L444](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L444)

```
444:    uint256[] memory bps = new uint256[](1);
```

[PA1D.sol#L460](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L460)

```
460:    uint256[] memory bps = new uint256[](1);
```

[PA1D.sol#L471](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L471)

```
471:    address payable[] memory receivers = new address payable[](1);
```

[PA1D.sol#L492](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L492)

```
492:    address payable[] memory receivers = new address payable[](1);
```

[PA1D.sol#L493](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L493)

```
493:    uint256[] memory bps = new uint256[](1);
```

[PA1D.sol#L506](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L506)

```
506:    address payable[] memory receivers = new address payable[](1);
```

[PA1D.sol#L507](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L507)

```
507:    uint256[] memory bps = new uint256[](1);
```

## [G-006] internal function not called by the contract should be removed to save deployment gas

### Description:

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword.

### Findings:

Total:2

[ERC20H.sol#L104](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC20H.sol#L104)

```
104:    function _setOwner(address ownerAddress) internal {
```

[ERC721H.sol#L104](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/ERC721H.sol#L104)

```
104:    function _setOwner(address ownerAddress) internal {
`