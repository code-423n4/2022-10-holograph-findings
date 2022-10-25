### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
contracts/HolographFactory.sol:L252        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)

contracts/enforcer/HolographERC20.sol:L409    return (Holographable.bridgeOut.selector, abi.encode(from, to, amount, data));

contracts/enforcer/HolographERC20.sol:L471      abi.encode(

contracts/enforcer/HolographERC721.sol:L260        abi.encodeWithSignature("initPA1D(bytes)", abi.encode(address(this), uint256(contractBps)))

contracts/enforcer/HolographERC721.sol:L426    return (Holographable.bridgeOut.selector, abi.encode(from, to, tokenId, data));

```

### [G-02] Use assembly to check for zero address.


#### Impact
Save 6 gas when assembly is used to check for zero address.
e.g:
```
assembly {
            if iszero(_addr) {
                mstore(0x00, "zero address")
                revert(0x00, 0x20)
            }
```


#### Findings:
```

contracts/enforcer/HolographERC20.sol:L620    require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol:L621    require(spender != address(0), "ERC20: spender is zero address");

contracts/enforcer/HolographERC20.sol:L627    require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol:L695    require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol:L696    require(recipient != address(0), "ERC20: recipient is zero address");

contracts/enforcer/HolographERC721.sol:L419    require(to != address(0), "ERC721: zero address");

contracts/enforcer/HolographERC721.sol:L639    require(wallet != address(0), "ERC721: zero address");
```
### [G-03] Using bools for storage incurs overhead.


#### Impact
 
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 Use ```uint256(1)``` and ```uint256(2)``` for true/false to avoid a Gwarmaccess ([100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


#### Findings:
```
contracts/HolographOperator.sol:L198  mapping(bytes32 => bool) private _failedJobs;

contracts/enforcer/HolographERC721.sol:L196  mapping(address => mapping(address => bool)) private _operatorApprovals;

contracts/enforcer/HolographERC721.sol:L206  mapping(uint256 => bool) private _burnedTokens;

```

### [G-04] Cache Array Length Outside of Loop


#### Impact
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.


#### Findings:
```
contracts/HolographOperator.sol:L871        for (uint256 i = _operatorPods.length; i <= pod; i++) {

contracts/enforcer/PA1D.sol:L432    for (uint256 t = 0; t < tokenAddresses.length; t++) {

contracts/enforcer/PA1D.sol:L437      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L454      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L474    for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/HolographERC20.sol:L564    for (uint256 i = 0; i < wallets.length; i++) {

```

### [G-05] Use custom errors rather than ```revert()```/```require()``` string to save gas


#### Impact
Custom errors are available from solidity version 0.8.4, it saves around 50 gas each time they are hit by avoiding having to allocate and store the revert string.


#### Findings:
```
contracts/HolographOperator.sol:L241    require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/HolographOperator.sol:L309    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

contracts/HolographOperator.sol:L350        require(timeDifference > 0, "HOLOGRAPH: operator has time");

contracts/HolographOperator.sol:L354        require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

contracts/HolographOperator.sol:L368            require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

contracts/HolographOperator.sol:L415    require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

contracts/HolographOperator.sol:L446    require(msg.sender == address(this), "HOLOGRAPH: operator only call");

contracts/HolographOperator.sol:L485    require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

contracts/HolographOperator.sol:L591    require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");

contracts/HolographOperator.sol:L595    require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

contracts/HolographOperator.sol:L728    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

contracts/HolographOperator.sol:L739    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

contracts/HolographOperator.sol:L756    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

contracts/HolographOperator.sol:L829    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

contracts/HolographOperator.sol:L839    require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

contracts/HolographOperator.sol:L857    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

contracts/HolographOperator.sol:L863      require(current <= amount, "HOLOGRAPH: bond amount too small");

contracts/HolographOperator.sol:L881      require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

contracts/HolographOperator.sol:L889      require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

contracts/HolographOperator.sol:L903    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

contracts/HolographOperator.sol:L911      require(_isContract(operator), "HOLOGRAPH: operator not contract");

contracts/HolographOperator.sol:L915      require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

contracts/HolographOperator.sol:L932    require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");

contracts/HolographFactory.sol:L144    require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/HolographFactory.sol:L220    require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

contracts/HolographFactory.sol:L228    require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

contracts/HolographBridge.sol:L148    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

contracts/HolographBridge.sol:L163    require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/HolographBridge.sol:L214    require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

contracts/HolographBridge.sol:L233    require(doNotRevert, "HOLOGRAPH: reverted");

contracts/HolographBridge.sol:L270    require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

contracts/module/LayerZeroModule.sol:L159    require(!_isInitialized(), "HOLOGRAPH: already initialized");

contracts/module/LayerZeroModule.sol:L235    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

contracts/enforcer/PA1D.sol:L159    require(isOwner(), "PA1D: caller not an owner");

contracts/enforcer/PA1D.sol:L174    require(!_isInitialized(), "PA1D: already initialized");

contracts/enforcer/PA1D.sol:L190    require(initialized == 0, "PA1D: already initialized");

contracts/enforcer/PA1D.sol:L390    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

contracts/enforcer/PA1D.sol:L411    require(balance > 10000, "PA1D: Not enough tokens to transfer");

contracts/enforcer/PA1D.sol:L416      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

contracts/enforcer/PA1D.sol:L435      require(balance > 10000, "PA1D: Not enough tokens to transfer");

contracts/enforcer/PA1D.sol:L439        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

contracts/enforcer/PA1D.sol:L460      require(matched, "PA1D: sender not authorized");

contracts/enforcer/PA1D.sol:L472    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

contracts/enforcer/PA1D.sol:L477    require(totalBp == 10000, "PA1D: bps down't equal 10000");

contracts/enforcer/HolographERC20.sol:L192    require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");

contracts/enforcer/HolographERC20.sol:L204    require(msg.sender == sourceContract, "ERC20: source only call");

contracts/enforcer/HolographERC20.sol:L219    require(!_isInitialized(), "ERC20: already initialized");

contracts/enforcer/HolographERC20.sol:L241      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

contracts/enforcer/HolographERC20.sol:L349    require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol:L365    require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

contracts/enforcer/HolographERC20.sol:L387      require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

contracts/enforcer/HolographERC20.sol:L400      require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol:L427      require(newAllowance >= currentAllowance, "ERC20: increased above max value");

contracts/enforcer/HolographERC20.sol:L445    require(_isContract(account), "ERC20: operator not contract");

contracts/enforcer/HolographERC20.sol:L450      require(balance >= amount, "ERC20: balance check failed");

contracts/enforcer/HolographERC20.sol:L452      revert("ERC20: failed getting balance");

contracts/enforcer/HolographERC20.sol:L469    require(block.timestamp <= deadline, "ERC20: expired deadline");

contracts/enforcer/HolographERC20.sol:L482    require(signer == account, "ERC20: invalid signature");

contracts/enforcer/HolographERC20.sol:L505    require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");

contracts/enforcer/HolographERC20.sol:L529        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol:L539    require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");

contracts/enforcer/HolographERC20.sol:L599        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

contracts/enforcer/HolographERC20.sol:L620    require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol:L621    require(spender != address(0), "ERC20: spender is zero address");

contracts/enforcer/HolographERC20.sol:L627    require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol:L629    require(accountBalance >= amount, "ERC20: amount exceeds balance");

contracts/enforcer/HolographERC20.sol:L645        require(erc165support, "ERC20: no ERC165 support");

contracts/enforcer/HolographERC20.sol:L653              revert("ERC20: non ERC20Receiver");

contracts/enforcer/HolographERC20.sol:L661          revert("ERC20: eip-4524 not supported");

contracts/enforcer/HolographERC20.sol:L665          revert("ERC20: no ERC165 support");

contracts/enforcer/HolographERC20.sol:L684    require(to != address(0), "ERC20: minting to burn address");

contracts/enforcer/HolographERC20.sol:L695    require(account != address(0), "ERC20: account is zero address");

contracts/enforcer/HolographERC20.sol:L696    require(recipient != address(0), "ERC20: recipient is zero address");

contracts/enforcer/HolographERC20.sol:L698    require(accountBalance >= amount, "ERC20: amount exceeds balance");

contracts/enforcer/HolographERC721.sol:L212    require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

contracts/enforcer/HolographERC721.sol:L224    require(msg.sender == sourceContract, "ERC721: source only call");

contracts/enforcer/HolographERC721.sol:L239    require(!_isInitialized(), "ERC721: already initialized");

contracts/enforcer/HolographERC721.sol:L258      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

contracts/enforcer/HolographERC721.sol:L263      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

contracts/enforcer/HolographERC721.sol:L323    require(_exists(tokenId), "ERC721: token does not exist");

contracts/enforcer/HolographERC721.sol:L370    require(to != tokenOwner, "ERC721: cannot approve self");

contracts/enforcer/HolographERC721.sol:L371    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol:L388    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol:L404    require(!_exists(tokenId), "ERC721: token already exists");

contracts/enforcer/HolographERC721.sol:L408      require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

contracts/enforcer/HolographERC721.sol:L419    require(to != address(0), "ERC721: zero address");

contracts/enforcer/HolographERC721.sol:L420    require(_isApproved(sender, tokenId), "ERC721: sender not approved");

contracts/enforcer/HolographERC721.sol:L421    require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

contracts/enforcer/HolographERC721.sol:L458    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol:L484    require(to != msg.sender, "ERC721: cannot approve self");

contracts/enforcer/HolographERC721.sol:L513    require(!_burnedTokens[token], "ERC721: can't mint burned token");

contracts/enforcer/HolographERC721.sol:L622    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

contracts/enforcer/HolographERC721.sol:L639    require(wallet != address(0), "ERC721: zero address");

contracts/enforcer/HolographERC721.sol:L689    require(tokenOwner != address(0), "ERC721: token does not exist");

contracts/enforcer/HolographERC721.sol:L700    require(index < _allTokens.length, "ERC721: index out of bounds");

contracts/enforcer/HolographERC721.sol:L729    require(index < balanceOf(wallet), "ERC721: index out of bounds");

contracts/enforcer/HolographERC721.sol:L757    require(_isContract(_operator), "ERC721: operator not contract");

contracts/enforcer/HolographERC721.sol:L762      require(tokenOwner == address(this), "ERC721: contract not token owner");

contracts/enforcer/HolographERC721.sol:L764      revert("ERC721: token does not exist");

contracts/enforcer/HolographERC721.sol:L815    require(tokenId > 0, "ERC721: token id cannot be zero");

contracts/enforcer/HolographERC721.sol:L816    require(to != address(0), "ERC721: minting to burn address");

contracts/enforcer/HolographERC721.sol:L817    require(!_exists(tokenId), "ERC721: token already exists");

contracts/enforcer/HolographERC721.sol:L818    require(!_burnedTokens[tokenId], "ERC721: token has been burned");

contracts/enforcer/HolographERC721.sol:L869    require(_tokenOwner[tokenId] == from, "ERC721: token not owned");

contracts/enforcer/HolographERC721.sol:L870    require(to != address(0), "ERC721: use burn instead");

contracts/enforcer/HolographERC721.sol:L906    require(_exists(tokenId), "ERC721: token does not exist");

contracts/enforcer/Holographer.sol:L148    require(!_isInitialized(), "HOLOGRAPHER: already initialized");

contracts/enforcer/Holographer.sol:L166    require(success && selector == InitializableInterface.init.selector, "initialization failed");

contracts/abstract/ERC20H.sol:L117    require(msg.sender == holographer(), "ERC20: holographer only");

contracts/abstract/ERC20H.sol:L123      require(msgSender() == _getOwner(), "ERC20: owner only function");

contracts/abstract/ERC20H.sol:L125      require(msg.sender == _getOwner(), "ERC20: owner only function");

contracts/abstract/ERC20H.sol:L147    require(!_isInitialized(), "ERC20: already initialized");

contracts/abstract/ERC721H.sol:L117    require(msg.sender == holographer(), "ERC721: holographer only");

contracts/abstract/ERC721H.sol:L123      require(msgSender() == _getOwner(), "ERC721: owner only function");

contracts/abstract/ERC721H.sol:L125      require(msg.sender == _getOwner(), "ERC721: owner only function");

contracts/abstract/ERC721H.sol:L147    require(!_isInitialized(), "ERC721: already initialized");

```

### [G-06] Empty blocks should be removed or emit something


#### Impact
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.


#### Findings:
```
contracts/HolographOperator.sol:L233  constructor() {}

contracts/HolographOperator.sol:L1209  receive() external payable {}

contracts/HolographFactory.sol:L136  constructor() {}

contracts/HolographBridge.sol:L155  constructor() {}

contracts/module/LayerZeroModule.sol:L151  constructor() {}

contracts/enforcer/PA1D.sol:L166  constructor() {}

contracts/enforcer/HolographERC20.sol:L211  constructor() {}

contracts/enforcer/HolographERC20.sol:L251  receive() external payable {}

contracts/enforcer/HolographERC721.sol:L231  constructor() {}

contracts/enforcer/HolographERC721.sol:L962  receive() external payable {}

contracts/enforcer/Holographer.sol:L140  constructor() {}

contracts/enforcer/Holographer.sol:L223  receive() external payable {}

contracts/abstract/ERC20H.sol:L133  constructor() {}

contracts/abstract/ERC20H.sol:L212  receive() external payable {}

contracts/abstract/ERC721H.sol:L133  constructor() {}

contracts/abstract/ERC721H.sol:L212  receive() external payable {}

```

### [G-07] Using ```> 0``` costs more gas than ```!= 0``` when used on a uint in a ```require()``` statement.


#### Impact
When working with unsigned integer types, comparisons with != 0 are cheaper than with > 0 . This changes saves 6 gas per instance.


#### Findings:
```
contracts/HolographOperator.sol:L309    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

contracts/HolographOperator.sol:L350        require(timeDifference > 0, "HOLOGRAPH: operator has time");

contracts/enforcer/HolographERC721.sol:L815    require(tokenId > 0, "ERC721: token id cannot be zero");

```

### [G-08] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/HolographOperator.sol:L949  function setBridge(address bridge) external onlyAdmin {

contracts/HolographOperator.sol:L969  function setHolograph(address holograph) external onlyAdmin {

contracts/HolographOperator.sol:L989  function setInterfaces(address interfaces) external onlyAdmin {

contracts/HolographOperator.sol:L1009  function setMessagingModule(address messagingModule) external onlyAdmin {

contracts/HolographOperator.sol:L1029  function setRegistry(address registry) external onlyAdmin {

contracts/HolographOperator.sol:L1049  function setUtilityToken(address utilityToken) external onlyAdmin {

contracts/HolographFactory.sol:L280  function setHolograph(address holograph) external onlyAdmin {

contracts/HolographFactory.sol:L300  function setRegistry(address registry) external onlyAdmin {

contracts/HolographBridge.sol:L452  function setFactory(address factory) external onlyAdmin {

contracts/HolographBridge.sol:L472  function setHolograph(address holograph) external onlyAdmin {

contracts/HolographBridge.sol:L502  function setOperator(address operator) external onlyAdmin {

contracts/HolographBridge.sol:L522  function setRegistry(address registry) external onlyAdmin {

contracts/module/LayerZeroModule.sol:L320  function setBridge(address bridge) external onlyAdmin {

contracts/module/LayerZeroModule.sol:L340  function setInterfaces(address interfaces) external onlyAdmin {

contracts/module/LayerZeroModule.sol:L360  function setLZEndpoint(address lZEndpoint) external onlyAdmin {

contracts/module/LayerZeroModule.sol:L380  function setOperator(address operator) external onlyAdmin {

contracts/module/LayerZeroModule.sol:L441  function setBaseGas(uint256 baseGas) external onlyAdmin {

contracts/module/LayerZeroModule.sol:L470  function setGasPerByte(uint256 gasPerByte) external onlyAdmin {

contracts/enforcer/HolographERC20.sol:L380  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

contracts/enforcer/HolographERC20.sol:L415  function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {

contracts/enforcer/HolographERC20.sol:L549  function sourceBurn(address from, uint256 amount) external onlySource {

contracts/enforcer/HolographERC20.sol:L556  function sourceMint(address to, uint256 amount) external onlySource {

contracts/enforcer/HolographERC20.sol:L563  function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {

contracts/enforcer/HolographERC721.sol:L399  function bridgeIn(uint32 fromChain, bytes calldata payload) external onlyBridge returns (bytes4) {

contracts/enforcer/HolographERC721.sol:L500  function sourceBurn(uint256 tokenId) external onlySource {

contracts/enforcer/HolographERC721.sol:L508  function sourceMint(address to, uint224 tokenId) external onlySource {

contracts/enforcer/HolographERC721.sol:L520  function sourceGetChainPrepend() external view onlySource returns (uint256) {

contracts/enforcer/HolographERC721.sol:L577  function sourceTransfer(address to, uint256 tokenId) external onlySource {

```

### [G-09] ++i costs less gas than i++, especially when it's used in for loops.


#### Impact
Saves 6 gas per loop.


#### Findings:
```
contracts/HolographOperator.sol:L781    for (uint256 i = 0; i < length; i++) {

contracts/HolographOperator.sol:L871        for (uint256 i = _operatorPods.length; i <= pod; i++) {

contracts/enforcer/PA1D.sol:L307    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L323    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L340    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L356    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L394    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L414    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L432    for (uint256 t = 0; t < tokenAddresses.length; t++) {

contracts/enforcer/PA1D.sol:L437      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L454      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L474    for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/HolographERC20.sol:L564    for (uint256 i = 0; i < wallets.length; i++) {

contracts/enforcer/HolographERC721.sol:L357    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC721.sol:L716    for (uint256 i = 0; i < length; i++) {

```

### [G-10] Revert message greater than 32 bytes


#### Impact
Keep revert message lower than or equal to 32 bytes to save gas.


#### Findings:
```
contracts/enforcer/PA1D.sol:L411    require(balance > 10000, "PA1D: Not enough tokens to transfer");

contracts/enforcer/PA1D.sol:L435      require(balance > 10000, "PA1D: Not enough tokens to transfer");

```
           
### [G-11] Splitting ```require()``` statements that use && saves gas.


#### Impact
Consider splitting the ```require()``` statements to save gas.


#### Findings:
```
contracts/HolographOperator.sol:L857    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

contracts/enforcer/HolographERC721.sol:L263      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

contracts/enforcer/Holographer.sol:L166    require(success && selector == InitializableInterface.init.selector, "initialization failed");

```

### [G-12] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
contracts/HolographOperator.sol:L310    uint256 gasLimit = 0;

contracts/HolographOperator.sol:L311    uint256 gasPrice = 0;

contracts/HolographOperator.sol:L781    for (uint256 i = 0; i < length; i++) {

contracts/HolographBridge.sol:L380    uint256 fee = 0;

contracts/enforcer/PA1D.sol:L307    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L323    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L340    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L356    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L394    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L414    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L432    for (uint256 t = 0; t < tokenAddresses.length; t++) {

contracts/enforcer/PA1D.sol:L437      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L454      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L474    for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/HolographERC20.sol:L564    for (uint256 i = 0; i < wallets.length; i++) {

contracts/enforcer/HolographERC721.sol:L357    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC721.sol:L716    for (uint256 i = 0; i < length; i++) {

```

### [G-13] ```++i```/```i++``` should be ```unchecked{++i}```/```unchecked{i++}``` when it is not possible for them to overflow, as is the case when used in for- and while-loops.


#### Impact
The ```unchecked``` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.


#### Findings:
```
contracts/HolographOperator.sol:L781    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L307    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L323    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L340    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L356    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L394    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L414    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/PA1D.sol:L437      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L454      for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/PA1D.sol:L474    for (uint256 i = 0; i < addresses.length; i++) {

contracts/enforcer/HolographERC20.sol:L564    for (uint256 i = 0; i < wallets.length; i++) {

contracts/enforcer/HolographERC721.sol:L357    for (uint256 i = 0; i < length; i++) {

contracts/enforcer/HolographERC721.sol:L716    for (uint256 i = 0; i < length; i++) {

```


