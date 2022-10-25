### [G-01] ++i costs less gas compared to i++

++i costs **about 5 gas less per iteration** compared to i++ for unsigned integer.
This statement is true even with the optimizer enabled.
Summarized my results where i is used in a loop, is unsigned integer, and you safely can be changed to ++i without changing any behavior,

I've found 20 locations in 4 files:

```
src/HolographOperator.sol:
  420        if (podSize > 1) {
  421:         podSize--;
  422        }

  660       */
  661:     pod--;
  662      /**
  
  681       */
  682:     for (uint256 i = 0; i < length; i++) {
  683        operators[i] = _operatorPods[pod][index + i];

  771           */
  772:         for (uint256 i = _operatorPods.length; i <= pod; i++) {
  773            /**

src/enforcer/HolographERC20.sol:
  464    function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
  465:     for (uint256 i = 0; i < wallets.length; i++) {
  466        _mint(wallets[i], amounts[i]);

  613      current = _nonces[account];
  614:     _nonces[account]++;
  615    }

src/enforcer/HolographERC721.sol:
  257      tokenIds = new uint256[](length);
  258:     for (uint256 i = 0; i < length; i++) {
  259        tokenIds[i] = _ownedTokens[wallet][index + i];

  616      tokenIds = new uint256[](length);
  617:     for (uint256 i = 0; i < length; i++) {
  618        tokenIds[i] = _allTokens[index + i];

  679      _ownedTokensIndex[tokenId] = _ownedTokensCount[to];
  680:     _ownedTokensCount[to]++;
  681      _ownedTokens[to].push(tokenId);
  
  742      _removeTokenFromAllTokensEnumeration(tokenId);
  743:     _ownedTokensCount[from]--;
  744      uint256 lastTokenIndex = _ownedTokensCount[from];

src/enforcer/PA1D.sol:
  207      address payable value;
  208:     for (uint256 i = 0; i < length; i++) {
  209        slot = keccak256(abi.encodePacked(i, slot));

  223      address payable value;
  224:     for (uint256 i = 0; i < length; i++) {
  225        slot = keccak256(abi.encodePacked(i, slot));

  240      uint256 value;
  241:     for (uint256 i = 0; i < length; i++) {
  242        slot = keccak256(abi.encodePacked(i, slot));

  256      uint256 value;
  257:     for (uint256 i = 0; i < length; i++) {
  258        slot = keccak256(abi.encodePacked(i, slot));

  294      // uint256 sent;
  295:     for (uint256 i = 0; i < length; i++) {
  296        sending = ((bps[i] * balance) / 10000);

  314      //uint256 sent;
  315:     for (uint256 i = 0; i < length; i++) {
  316        sending = ((bps[i] * balance) / 10000);

  332      uint256 sending;
  333:     for (uint256 t = 0; t < tokenAddresses.length; t++) {
  334        erc20 = ERC20(tokenAddresses[t]);

  337        // uint256 sent;
  338:       for (uint256 i = 0; i < addresses.length; i++) {
  339          sending = ((bps[i] * balance) / 10000);

  354        address payable sender = payable(msg.sender);
  355:       for (uint256 i = 0; i < addresses.length; i++) {
  356          if (addresses[i] == sender) {

  374      uint256 totalBp;
  375:     for (uint256 i = 0; i < addresses.length; i++) {
  376        totalBp = totalBp + bps[i];

```

---------------------------------------------------------------------------

### [G-02] An arrays length should be cached to save gas in for-loops

An array’s length should be cached to save gas in for-loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset).
Caching the array length in the stack saves around **3 gas per iteration**.

I've found 6 locations in 4 files:

```
src/HolographOperator.sol:
  771           */
  772:         for (uint256 i = _operatorPods.length; i <= pod; i++) {
  773            /**

src/enforcer/HolographERC20.sol:
  464    function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
  465:     for (uint256 i = 0; i < wallets.length; i++) {
  466        _mint(wallets[i], amounts[i]);

src/enforcer/PA1D.sol:
  332      uint256 sending;
  333:     for (uint256 t = 0; t < tokenAddresses.length; t++) {
  334        erc20 = ERC20(tokenAddresses[t]);

  337        // uint256 sent;
  338:       for (uint256 i = 0; i < addresses.length; i++) {
  339          sending = ((bps[i] * balance) / 10000);

  354        address payable sender = payable(msg.sender);
  355:       for (uint256 i = 0; i < addresses.length; i++) {
  356          if (addresses[i] == sender) {

  374      uint256 totalBp;
  375:     for (uint256 i = 0; i < addresses.length; i++) {
  376        totalBp = totalBp + bps[i];
```

---------------------------------------------------------------------------


### [G-03] Using default values is cheaper than assignment

If a variable is not set/initialized, it is assumed to have the default value 0 for uint, and false for boolean.
Explicitly initializing it with its default value is an anti-pattern and wastes gas.
For example: ```uint8 i = 0;``` should be replaced with ```uint8 i;```

I've found 17 locations in 5 files:

```
src/HolographBridge.sol:
  280       */
  281:     uint256 fee = 0;
  282      if (gasPrice < type(uint256).max && gasLimit < type(uint256).max) {

src/HolographOperator.sol:
  210      require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
  211:     uint256 gasLimit = 0;
  212:     uint256 gasPrice = 0;
  213      assembly {

  681       */
  682:     for (uint256 i = 0; i < length; i++) {
  683        operators[i] = _operatorPods[pod][index + i];

src/enforcer/HolographERC20.sol:
  464    function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
  465:     for (uint256 i = 0; i < wallets.length; i++) {
  466        _mint(wallets[i], amounts[i]);

src/enforcer/HolographERC721.sol:
  257      tokenIds = new uint256[](length);
  258:     for (uint256 i = 0; i < length; i++) {
  259        tokenIds[i] = _ownedTokens[wallet][index + i];

  616      tokenIds = new uint256[](length);
  617:     for (uint256 i = 0; i < length; i++) {
  618        tokenIds[i] = _allTokens[index + i];

src/enforcer/PA1D.sol:
  207      address payable value;
  208:     for (uint256 i = 0; i < length; i++) {
  209        slot = keccak256(abi.encodePacked(i, slot));

  223      address payable value;
  224:     for (uint256 i = 0; i < length; i++) {
  225        slot = keccak256(abi.encodePacked(i, slot));

  240      uint256 value;
  241:     for (uint256 i = 0; i < length; i++) {
  242        slot = keccak256(abi.encodePacked(i, slot));

  256      uint256 value;
  257:     for (uint256 i = 0; i < length; i++) {
  258        slot = keccak256(abi.encodePacked(i, slot));

  294      // uint256 sent;
  295:     for (uint256 i = 0; i < length; i++) {
  296        sending = ((bps[i] * balance) / 10000);

  314      //uint256 sent;
  315:     for (uint256 i = 0; i < length; i++) {
  316        sending = ((bps[i] * balance) / 10000);

  332      uint256 sending;
  333:     for (uint256 t = 0; t < tokenAddresses.length; t++) {
  334        erc20 = ERC20(tokenAddresses[t]);

  337        // uint256 sent;
  338:       for (uint256 i = 0; i < addresses.length; i++) {
  339          sending = ((bps[i] * balance) / 10000);

  354        address payable sender = payable(msg.sender);
  355:       for (uint256 i = 0; i < addresses.length; i++) {
  356          if (addresses[i] == sender) {

  374      uint256 totalBp;
  375:     for (uint256 i = 0; i < addresses.length; i++) {
  376        totalBp = totalBp + bps[i];

```

---------------------------------------------------------------------------

### [G-04] != 0 is cheaper than > 0

!= 0 costs less gas compared to > 0 for unsigned integers even when optimizer enabled
All of the following findings are uint (E&OE) so >0 and != have exactly the same effect.
** saves 6 gas ** each

I've found 7 locations in 3 files:

```
src/HolographBridge.sol:
  118       */
  119:     if (hTokenValue > 0) {
  120        /**

src/HolographOperator.sol:
   209       */
   210:     require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
   211      uint256 gasLimit = 0;

   250           */
   251:         require(timeDifference > 0, "HOLOGRAPH: operator has time");
   252          /**

   263             */
   264:           if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
   265              address fallbackOperator = _operatorPods[pod][podIndex];

   298            uint256 leftovers = _bondedAmounts[job.operator];
   299:           if (leftovers > 0) {
   300              _bondedAmounts[job.operator] = 0;

  1026       */
  1027:     if (operatorIndex > 0) {
  1028        unchecked {

src/enforcer/HolographERC721.sol:
  715    function _mint(address to, uint256 tokenId) private {
  716:     require(tokenId > 0, "ERC721: token id cannot be zero");
  717      require(to != address(0), "ERC721: minting to burn address");
```

---------------------------------------------------------------------------

### [G-05] Custom errors save gas

From solidity 0.8.4 and above,
Custom errors from are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
Source: https://blog.soliditylang.org/2021/04/21/custom-errors/:
```Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.```

I've found 111 locations in 10 files:

```
src/HolographBridge.sol:
   48    modifier onlyOperator() {
   49:     require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
   50      _;

   63    function init(bytes memory initPayload) external override returns (bytes4) {
   64:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
   65      (address factory, address holograph, address operator, address registry) = abi.decode(

  114       */
  115:     require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
  116      /**

  133       */
  134:     require(doNotRevert, "HOLOGRAPH: reverted");
  135    }

  170       */
  171:     require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
  172      /**

src/HolographFactory.sol:
   44    function init(bytes memory initPayload) external override returns (bytes4) {
   45:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
   46      (address holograph, address registry) = abi.decode(initPayload, (address, address));

  120       */
  121:     require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
  122      /**

  128      );
  129:     require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
  130      /**

src/HolographOperator.sol:
  141    function init(bytes memory initPayload) external override returns (bytes4) {
  142:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
  143      (address bridge, address holograph, address interfaces, address registry, address utilityToken) = abi.decode(

  209       */
  210:     require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
  211      uint256 gasLimit = 0;

  250           */
  251:         require(timeDifference > 0, "HOLOGRAPH: operator has time");
  252          /**

  254           */
  255:         require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
  256          /**

  268               */
  269:             require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
  270            }

  315       */
  316:     require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
  317      /**

  346    function nonRevertingBridgeCall(address msgSender, bytes calldata payload) external payable {
  347:     require(msg.sender == address(this), "HOLOGRAPH: operator only call");
  348      assembly {

  385    function crossChainMessage(bytes calldata bridgeInRequestPayload) external payable {
  386:     require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
  387      /**

  491    ) external payable {
  492:     require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
  493      CrossChainMessageInterface messagingModule = _messagingModule();

  495      address hToken = _registry().getHToken(_holograph().getHolographChainId());
  496:     require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
  497      payable(hToken).transfer(hlgFee);

  628    function getPodOperatorsLength(uint256 pod) external view returns (uint256) {
  629:     require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
  630      return _operatorPods[pod - 1].length;

  639    function getPodOperators(uint256 pod) external view returns (address[] memory operators) {
  640:     require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
  641      operators = _operatorPods[pod - 1];

  656    ) external view returns (address[] memory operators) {
  657:     require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
  658      /**

  729       */
  730:     require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
  731      unchecked {

  739       */
  740:     require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
  741    }

  757       */
  758:     require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
  759      unchecked {

  763        uint256 current = _getCurrentBondAmount(pod - 1);
  764:       require(current <= amount, "HOLOGRAPH: bond amount too small");
  765        /**

  781         */
  782:       require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
  783        _operatorPods[pod - 1].push(operator);

  789         */
  790:       require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
  791      }

  803       */
  804:     require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
  805      /**

  811         */
  812:       require(_isContract(operator), "HOLOGRAPH: operator not contract");
  813        /**

  815         */
  816:       require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
  817      }

  832       */
  833:     require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
  834    }

src/abstract/ERC20H.sol:
  17    modifier onlyHolographer() {
  18:     require(msg.sender == holographer(), "ERC20: holographer only");
  19      _;

  23      if (msg.sender == holographer()) {
  24:       require(msgSender() == _getOwner(), "ERC20: owner only function");
  25      } else {
  26:       require(msg.sender == _getOwner(), "ERC20: owner only function");
  27      }

  47    ) internal returns (bytes4) {
  48:     require(!_isInitialized(), "ERC20: already initialized");
  49      address _holographer = msg.sender;

src/abstract/ERC721H.sol:
  17    modifier onlyHolographer() {
  18:     require(msg.sender == holographer(), "ERC721: holographer only");
  19      _;

  23      if (msg.sender == holographer()) {
  24:       require(msgSender() == _getOwner(), "ERC721: owner only function");
  25      } else {
  26:       require(msg.sender == _getOwner(), "ERC721: owner only function");
  27      }

  47    ) internal returns (bytes4) {
  48:     require(!_isInitialized(), "ERC721: already initialized");
  49      address _holographer = msg.sender;

src/enforcer/Holographer.sol:
  48    function init(bytes memory initPayload) external override returns (bytes4) {
  49:     require(!_isInitialized(), "HOLOGRAPHER: already initialized");
  50      (bytes memory encoded, bytes memory initCode) = abi.decode(initPayload, (bytes, bytes));

  66      bytes4 selector = abi.decode(returnData, (bytes4));
  67:     require(success && selector == InitializableInterface.init.selector, "initialization failed");
  68      _setInitialized();

src/enforcer/HolographERC20.sol:
   92    modifier onlyBridge() {
   93:     require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");
   94      _;

  104      }
  105:     require(msg.sender == sourceContract, "ERC20: source only call");
  106      _;

  119    function init(bytes memory initPayload) external override returns (bytes4) {
  120:     require(!_isInitialized(), "ERC20: already initialized");
  121      InitializableInterface sourceContract;

  141      if (!skipInit) {
  142:       require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");
  143      }

  249      uint256 currentAllowance = _allowances[account][msg.sender];
  250:     require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  251      unchecked {

  265      uint256 currentAllowance = _allowances[msg.sender][spender];
  266:     require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
  267      uint256 newAllowance;

  287      if (_isEventRegistered(HolographERC20Event.bridgeIn)) {
  288:       require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");
  289      }

  300        uint256 currentAllowance = _allowances[from][sender];
  301:       require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  302        unchecked {

  327      unchecked {
  328:       require(newAllowance >= currentAllowance, "ERC20: increased above max value");
  329      }

  345    ) public returns (bytes4) {
  346:     require(_isContract(account), "ERC20: operator not contract");
  347      if (_isEventRegistered(HolographERC20Event.beforeOnERC20Received)) {

  350      try ERC20(account).balanceOf(address(this)) returns (uint256 balance) {
  351:       require(balance >= amount, "ERC20: balance check failed");
  352      } catch {

  369    ) public {
  370:     require(block.timestamp <= deadline, "ERC20: expired deadline");
  371      bytes32 structHash = keccak256(

  382      address signer = ECDSA.recover(hash, v, r, s);
  383:     require(signer == account, "ERC20: invalid signature");
  384      if (_isEventRegistered(HolographERC20Event.beforeApprove)) {

  405      _transfer(msg.sender, recipient, amount);
  406:     require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");
  407      if (_isEventRegistered(HolographERC20Event.afterSafeTransfer)) {

  429          uint256 currentAllowance = _allowances[account][msg.sender];
  430:         require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  431          unchecked {

  439      _transfer(account, recipient, amount);
  440:     require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");
  441      if (_isEventRegistered(HolographERC20Event.afterSafeTransfer)) {

  499          uint256 currentAllowance = _allowances[account][msg.sender];
  500:         require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  501          unchecked {

  520    ) private {
  521:     require(account != address(0), "ERC20: account is zero address");
  522:     require(spender != address(0), "ERC20: spender is zero address");
  523      _allowances[account][spender] = amount;

  527    function _burn(address account, uint256 amount) private {
  528:     require(account != address(0), "ERC20: account is zero address");
  529      uint256 accountBalance = _balances[account];
  530:     require(accountBalance >= amount, "ERC20: amount exceeds balance");
  531      unchecked {

  545        try ERC165(recipient).supportsInterface(ERC165.supportsInterface.selector) returns (bool erc165support) {
  546:         require(erc165support, "ERC20: no ERC165 support");
  547          // we have erc165 support

  584    function _mint(address to, uint256 amount) private {
  585:     require(to != address(0), "ERC20: minting to burn address");
  586      _totalSupply += amount;

  595    ) private {
  596:     require(account != address(0), "ERC20: account is zero address");
  597:     require(recipient != address(0), "ERC20: recipient is zero address");
  598      uint256 accountBalance = _balances[account];
  599:     require(accountBalance >= amount, "ERC20: amount exceeds balance");
  600      unchecked {

src/enforcer/HolographERC721.sol:
  112    modifier onlyBridge() {
  113:     require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");
  114      _;

  124      }
  125:     require(msg.sender == sourceContract, "ERC721: source only call");
  126      _;

  139    function init(bytes memory initPayload) external override returns (bytes4) {
  140:     require(!_isInitialized(), "ERC721: already initialized");
  141      InitializableInterface sourceContract;

  158      if (!skipInit) {
  159:       require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
  160        (bool success, bytes memory returnData) = _royalties().delegatecall(

  163        bytes4 selector = abi.decode(returnData, (bytes4));
  164:       require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
  165      }

  223    function tokenURI(uint256 tokenId) external view returns (string memory) {
  224:     require(_exists(tokenId), "ERC721: token does not exist");
  225      ERC721Metadata sourceContract;

  270      address tokenOwner = _tokenOwner[tokenId];
  271:     require(to != tokenOwner, "ERC721: cannot approve self");
  272:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  273      if (_isEventRegistered(HolographERC721Event.beforeApprove)) {

  288    function burn(uint256 tokenId) external {
  289:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  290      address wallet = _tokenOwner[tokenId];

  304      );
  305:     require(!_exists(tokenId), "ERC721: token already exists");
  306      delete _burnedTokens[tokenId];

  308      if (_isEventRegistered(HolographERC721Event.bridgeIn)) {
  309:       require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");
  310      }

  319      (address from, address to, uint256 tokenId) = abi.decode(payload, (address, address, uint256));
  320:     require(to != address(0), "ERC721: zero address");
  321:     require(_isApproved(sender, tokenId), "ERC721: sender not approved");
  322:     require(from == _tokenOwner[tokenId], "ERC721: from is not owner");
  323      if (_isEventRegistered(HolographERC721Event.bridgeOut)) {

  358    ) public payable {
  359:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  360      if (_isEventRegistered(HolographERC721Event.beforeSafeTransfer)) {

  384    function setApprovalForAll(address to, bool approved) external {
  385:     require(to != msg.sender, "ERC721: cannot approve self");
  386      if (_isEventRegistered(HolographERC721Event.beforeApprovalAll)) {

  413      uint256 token = uint256(bytes32(abi.encodePacked(_chain(), tokenId)));
  414:     require(!_burnedTokens[token], "ERC721: can't mint burned token");
  415      _mint(to, token);

  522    ) public payable {
  523:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  524      if (_isEventRegistered(HolographERC721Event.beforeTransfer)) {

  539    function balanceOf(address wallet) public view returns (uint256) {
  540:     require(wallet != address(0), "ERC721: zero address");
  541      return _ownedTokensCount[wallet];

  589      address tokenOwner = _tokenOwner[tokenId];
  590:     require(tokenOwner != address(0), "ERC721: token does not exist");
  591      return tokenOwner;

  600    function tokenByIndex(uint256 index) external view returns (uint256) {
  601:     require(index < _allTokens.length, "ERC721: index out of bounds");
  602      return _allTokens[index];

  629    function tokenOfOwnerByIndex(address wallet, uint256 index) external view returns (uint256) {
  630:     require(index < balanceOf(wallet), "ERC721: index out of bounds");
  631      return _ownedTokens[wallet][index];

  657    ) external returns (bytes4) {
  658:     require(_isContract(_operator), "ERC721: operator not contract");
  659      if (_isEventRegistered(HolographERC721Event.beforeOnERC721Received)) {

  662      try HolographERC721Interface(_operator).ownerOf(_tokenId) returns (address tokenOwner) {
  663:       require(tokenOwner == address(this), "ERC721: contract not token owner");
  664      } catch {

  715    function _mint(address to, uint256 tokenId) private {
  716:     require(tokenId > 0, "ERC721: token id cannot be zero");
  717:     require(to != address(0), "ERC721: minting to burn address");
  718:     require(!_exists(tokenId), "ERC721: token already exists");
  719:     require(!_burnedTokens[tokenId], "ERC721: token has been burned");
  720      _tokenOwner[tokenId] = to;

  769    ) private {
  770:     require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
  771:     require(to != address(0), "ERC721: use burn instead");
  772      _clearApproval(tokenId);

  806    function _isApproved(address spender, uint256 tokenId) private view returns (bool) {
  807:     require(_exists(tokenId), "ERC721: token does not exist");
  808      address tokenOwner = _tokenOwner[tokenId];

src/enforcer/PA1D.sol:
   59    modifier onlyOwner() override {
   60:     require(isOwner(), "PA1D: caller not an owner");
   61      _;

   74    function init(bytes memory initPayload) external override returns (bytes4) {
   75:     require(!_isInitialized(), "PA1D: already initialized");
   76      assembly {

   90      }
   91:     require(initialized == 0, "PA1D: already initialized");
   92      (address receiver, uint256 bp) = abi.decode(initPayload, (address, uint256));

  290      uint256 balance = address(this).balance;
  291:     require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
  292      balance = balance - gasCost;

  311      uint256 balance = erc20.balanceOf(address(this));
  312:     require(balance > 10000, "PA1D: Not enough tokens to transfer");
  313      uint256 sending;

  316        sending = ((bps[i] * balance) / 10000);
  317:       require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
  318        // sent = sent + sending;

  335        balance = erc20.balanceOf(address(this));
  336:       require(balance > 10000, "PA1D: Not enough tokens to transfer");
  337        // uint256 sent;

  339          sending = ((bps[i] * balance) / 10000);
  340:         require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
  341          // sent = sent + sending;

  360        }
  361:       require(matched, "PA1D: sender not authorized");
  362      }

  372    function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
  373:     require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
  374      uint256 totalBp;

  377      }
  378:     require(totalBp == 10000, "PA1D: bps down't equal 10000");
  379      _setPayoutAddresses(addresses);

src/module/LayerZeroModule.sol:
   59    function init(bytes memory initPayload) external override returns (bytes4) {
   60:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
   61      (address bridge, address interfaces, address operator, uint256 baseGas, uint256 gasPerByte) = abi.decode(

  135    ) external payable {
  136:     require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
  137      LayerZeroOverrides lZEndpoint;

```


---------------------------------------------------------------------------

### [G-06] Upgrade pragma to 0.8.16 to save gas?

Across the whole solution, the declared pragma is 0.8.13
Upgrading to 0.8.15 has many benefits, cheaper on gas is one of them.
The following information is regarding 0.8.15 over 0.8.14. Assume that over 0.8.13 the save is higher!
```
According to the release note of 0.8.15: https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/
The benchmark shows saving of 2.5-10% Bytecode size,
Saving 2-8% Deployment gas,
And saving up to 6.2% Runtime gas.
```

