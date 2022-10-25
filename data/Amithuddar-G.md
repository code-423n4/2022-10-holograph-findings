1)++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP

File: 2022-10-holograph\contracts\HolographOperator.sol
  781,37:     for (uint256 i = 0; i < length; i++) {
  871,58:         for (uint256 i = _operatorPods.length; i <= pod; i++) {
  
File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  564,45:     for (uint256 i = 0; i < wallets.length; i++) {

File: 2022-10-holograph\contracts\enforcer\HolographERC721.sol
  357,37:     for (uint256 i = 0; i < length; i++) {

  716,37:     for (uint256 i = 0; i < length; i++) { 

File: 2022-10-holograph\contracts\enforcer\PA1D.sol
  307,37:     for (uint256 i = 0; i < length; i++) {
  323,37:     for (uint256 i = 0; i < length; i++) {
  340,37:     for (uint256 i = 0; i < length; i++) {
  356,37:     for (uint256 i = 0; i < length; i++) {
  394,37:     for (uint256 i = 0; i < length; i++) {
  414,37:     for (uint256 i = 0; i < length; i++) {
  432,5:     for (uint256 t = 0; t < tokenAddresses.length; t++) {  
  437,49:       for (uint256 i = 0; i < addresses.length; i++) {
  454,49:       for (uint256 i = 0; i < addresses.length; i++) {
  474,47:     for (uint256 i = 0; i < addresses.length; i++) {  
  
2)It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {  
 
File: 2022-10-holograph\contracts\HolographOperator.sol
  781,5:     for (uint256 i = 0; i < length; i++) {
  871,9:         for (uint256 i = _operatorPods.length; i <= pod; i++) {

File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  564,5:     for (uint256 i = 0; i < wallets.length; i++) {

File: 2022-10-holograph\contracts\enforcer\HolographERC721.sol
  357,5:     for (uint256 i = 0; i < length; i++) {

  716,5:     for (uint256 i = 0; i < length; i++) { 

File: 2022-10-holograph\contracts\enforcer\PA1D.sol
  307,5:     for (uint256 i = 0; i < length; i++) {
  323,5:     for (uint256 i = 0; i < length; i++) {
  340,5:     for (uint256 i = 0; i < length; i++) {
  356,5:     for (uint256 i = 0; i < length; i++) {
  394,5:     for (uint256 i = 0; i < length; i++) {
  414,5:     for (uint256 i = 0; i < length; i++) {
  432,5:     for (uint256 t = 0; t < tokenAddresses.length; t++) {
  437,7:       for (uint256 i = 0; i < addresses.length; i++) {
  454,7:       for (uint256 i = 0; i < addresses.length; i++) {
  474,5:     for (uint256 i = 0; i < addresses.length; i++) {

3)<ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
The overheads outlined below are PER LOOP, 

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

File: 2022-10-holograph\contracts\HolographOperator.sol

  871,9:         for (uint256 i = _operatorPods.length; i <= pod; i++) {

File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  564,5:     for (uint256 i = 0; i < wallets.length; i++) {
  
File: 2022-10-holograph\contracts\enforcer\PA1D.sol

  432,5:     for (uint256 t = 0; t < tokenAddresses.length; t++) {
  437,7:       for (uint256 i = 0; i < addresses.length; i++) {
  454,7:       for (uint256 i = 0; i < addresses.length; i++) {
  474,5:     for (uint256 i = 0; i < addresses.length; i++) {  

4)X = X + Y IS CHEAPER THAN X += Y

File: 2022-10-holograph\contracts\HolographFactory.sol
  328,9:       v += 27;

File: 2022-10-holograph\contracts\HolographOperator.sol
  382,36:         _bondedAmounts[msg.sender] += amount;
  
  834,32:       _bondedAmounts[operator] += amount;
 
  1177,15:       current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);

File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  685,18:     _totalSupply += amount;
  686,19:     _balances[to] += amount;
  702,26:     _balances[recipient] += amount;

5)Use custom errors rather than revert()/require() strings to save gas    

File: 2022-10-holograph\contracts\HolographBridge.sol
  148,5:     require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
  163,5:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
  203,5:     require(
                        _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
                     "HOLOGRAPH: not holographed"
                     );
  214,5:     require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");
  224,7:       require(
                         HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==
                         HolographERC20Interface.holographBridgeMint.selector,
                         "HOLOGRAPH: hToken mint failed"
                       );
  233,5:     require(doNotRevert, "HOLOGRAPH: reverted");
  255,5:     require(
                        HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==
                        HolographERC20Interface.holographBridgeMint.selector,
                        "HOLOGRAPH: hToken mint failed"
                     );
  270,5:     require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");
  352,5:     require(
                       _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
                      "HOLOGRAPH: not holographed"
                     );

File: 2022-10-holograph\contracts\HolographFactory.sol
  144,5:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
  220,5:     require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");
  228,5:     require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");
  250,5:     require(
                      InitializableInterface(holographerAddress).init(
                      abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
                     ) == InitializableInterface.init.selector,
                     "initialization failed"
             ); 

File: 2022-10-holograph\contracts\HolographOperator.sol
  241,5:     require(!_isInitialized(), "HOLOGRAPH: already initialized");
  309,5:     require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
  350,9:         require(timeDifference > 0, "HOLOGRAPH: operator has time");
  354,9:         require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
  368,13:             require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
  415,5:     require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");
  446,5:     require(msg.sender == address(this), "HOLOGRAPH: operator only call");
  485,5:     require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");
  591,5:     require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");
  595,5:     require(hlgFee < msg.value, "HOLOGRAPH: not enough value");
  728,5:     require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
  739,5:     require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
  756,5:     require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
  829,5:     require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
  839,5:     require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
  857,5:     require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
  863,7:       require(current <= amount, "HOLOGRAPH: bond amount too small");
  881,7:       require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");
  889,7:       require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
  903,5:     require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");
  911,7:       require(_isContract(operator), "HOLOGRAPH: operator not contract");
  915,7:       require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");
  932,5:     require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");

File: 2022-10-holograph\contracts\abstract\ERC20H.sol
  117,5:     require(msg.sender == holographer(), "ERC20: holographer only");
  123,7:       require(msgSender() == _getOwner(), "ERC20: owner only function");
  125,7:       require(msg.sender == _getOwner(), "ERC20: owner only function");
  147,5:     require(!_isInitialized(), "ERC20: already initialized");

File: 2022-10-holograph\contracts\abstract\ERC721H.sol
  117,5:     require(msg.sender == holographer(), "ERC721: holographer only");
  123,7:       require(msgSender() == _getOwner(), "ERC721: owner only function");
  125,7:       require(msg.sender == _getOwner(), "ERC721: owner only function");
  147,5:     require(!_isInitialized(), "ERC721: already initialized");

File: 2022-10-holograph\contracts\enforcer\Holographer.sol
  148,5:     require(!_isInitialized(), "HOLOGRAPHER: already initialized");
  166,5:     require(success && selector == InitializableInterface.init.selector, "initialization failed");

File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  192,5:     require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");
  204,5:     require(msg.sender == sourceContract, "ERC20: source only call");
  219,5:     require(!_isInitialized(), "ERC20: already initialized");
  241,7:       require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");
  328,7:       require(SourceERC20().beforeApprove(msg.sender, spender, amount));
  332,7:       require(SourceERC20().afterApprove(msg.sender, spender, amount));
  339,7:       require(SourceERC20().beforeBurn(msg.sender, amount));
  343,7:       require(SourceERC20().afterBurn(msg.sender, amount));
  349,5:     require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  354,7:       require(SourceERC20().beforeBurn(account, amount));
  358,7:       require(SourceERC20().afterBurn(account, amount));
  365,5:     require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");
  371,7:       require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
  375,7:       require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
  387,7:       require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");
  400,7:       require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  427,7:       require(newAllowance >= currentAllowance, "ERC20: increased above max value");
  430,7:       require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));
  434,7:       require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
  445,5:     require(_isContract(account), "ERC20: operator not contract");
  447,7:       require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));
  450,7:       require(balance >= amount, "ERC20: balance check failed");
  455,7:       require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));
  469,5:     require(block.timestamp <= deadline, "ERC20: expired deadline");
  482,5:     require(signer == account, "ERC20: invalid signature");
  484,7:       require(SourceERC20().beforeApprove(account, spender, amount));
  488,7:       require(SourceERC20().afterApprove(account, spender, amount));
  502,7:       require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));
  505,5:     require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");
  507,7:       require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));
  529,9:         require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  536,7:       require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));
  539,5:     require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");
  541,7:       require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));
  582,7:       require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));
  586,7:       require(SourceERC20().afterTransfer(msg.sender, recipient, amount));
  599,9:         require(currentAllowance >= amount, "ERC20: amount exceeds allowance");
  606,7:       require(SourceERC20().beforeTransfer(account, recipient, amount));
  610,7:       require(SourceERC20().afterTransfer(account, recipient, amount));
  620,5:     require(account != address(0), "ERC20: account is zero address");
  621,5:     require(spender != address(0), "ERC20: spender is zero address");
  627,5:     require(account != address(0), "ERC20: account is zero address");
  629,5:     require(accountBalance >= amount, "ERC20: amount exceeds balance");
  645,9:         require(erc165support, "ERC20: no ERC165 support");
  684,5:     require(to != address(0), "ERC20: minting to burn address");
  695,5:     require(account != address(0), "ERC20: account is zero address");
  696,5:     require(recipient != address(0), "ERC20: recipient is zero address");
  698,5:     require(accountBalance >= amount, "ERC20: amount exceeds balance");

File: 2022-10-holograph\contracts\enforcer\HolographERC721.sol
  212,5:     require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");
  224,5:     require(msg.sender == sourceContract, "ERC721: source only call");
  239,5:     require(!_isInitialized(), "ERC721: already initialized");
  258,7:       require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");
  263,7:       require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
  323,5:     require(_exists(tokenId), "ERC721: token does not exist");
  370,5:     require(to != tokenOwner, "ERC721: cannot approve self");
  371,5:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  373,7:       require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));
  378,7:       require(SourceERC721().afterApprove(tokenOwner, to, tokenId));
  388,5:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  391,7:       require(SourceERC721().beforeBurn(wallet, tokenId));
  395,7:       require(SourceERC721().afterBurn(wallet, tokenId));
  404,5:     require(!_exists(tokenId), "ERC721: token already exists");
  408,7:       require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");
  419,5:     require(to != address(0), "ERC721: zero address");
  420,5:     require(_isApproved(sender, tokenId), "ERC721: sender not approved");
  421,5:     require(from == _tokenOwner[tokenId], "ERC721: from is not owner");
  458,5:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  460,7:       require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));
  464,7:       require(
  473,7:       require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));
  484,5:     require(to != msg.sender, "ERC721: cannot approve self");
  486,7:       require(SourceERC721().beforeApprovalAll(to, approved));
  491,7:       require(SourceERC721().afterApprovalAll(to, approved));
  513,5:     require(!_burnedTokens[token], "ERC721: can't mint burned token");
  528,10:   //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
  532,12:   //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  534,12:   //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  543,10:   //     require(wallets.length == tokenIds.length, "ERC721: array length missmatch");
  544,10:   //     require(tokenIds.length < 1000, "ERC721: max batch size is 1000");
  549,12:   //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  566,12:   //       require(!_burnedTokens[token], "ERC721: can't mint burned token");
  622,5:     require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
  624,7:       require(SourceERC721().beforeTransfer(from, to, tokenId, data));
  628,7:       require(SourceERC721().afterTransfer(from, to, tokenId, data));
  639,5:     require(wallet != address(0), "ERC721: zero address");
  689,5:     require(tokenOwner != address(0), "ERC721: token does not exist");
  700,5:     require(index < _allTokens.length, "ERC721: index out of bounds");
  729,5:     require(index < balanceOf(wallet), "ERC721: index out of bounds");
  757,5:     require(_isContract(_operator), "ERC721: operator not contract");
  759,7:       require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));
  762,7:       require(tokenOwner == address(this), "ERC721: contract not token owner");
  767,7:       require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));
  815,5:     require(tokenId > 0, "ERC721: token id cannot be zero");
  816,5:     require(to != address(0), "ERC721: minting to burn address");
  817,5:     require(!_exists(tokenId), "ERC721: token already exists");
  818,5:     require(!_burnedTokens[tokenId], "ERC721: token has been burned");
  869,5:     require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
  870,5:     require(to != address(0), "ERC721: use burn instead");
  906,5:     require(_exists(tokenId), "ERC721: token does not exist");

File: 2022-10-holograph\contracts\enforcer\PA1D.sol
  159,5:     require(isOwner(), "PA1D: caller not an owner");
  174,5:     require(!_isInitialized(), "PA1D: already initialized");
  190,5:     require(initialized == 0, "PA1D: already initialized");
  390,5:     require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");
  411,5:     require(balance > 10000, "PA1D: Not enough tokens to transfer");
  416,7:       require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
  435,7:       require(balance > 10000, "PA1D: Not enough tokens to transfer");
  439,9:         require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
  460,7:       require(matched, "PA1D: sender not authorized");
  472,5:     require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
  477,5:     require(totalBp == 10000, "PA1D: bps down't equal 10000");  
  
6)Splitting require() statements that use && saves gas

File: 2022-10-holograph\contracts\HolographOperator.sol
 
  857,45:     require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

File: 2022-10-holograph\contracts\enforcer\Holographer.sol
  166,21:     require(success && selector == InitializableInterface.init.selector, "initialization failed");

File: 2022-10-holograph\contracts\enforcer\HolographERC721.sol
  263,23:       require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
     464:       require(
                 (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
                  ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
                  ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
                  ERC721TokenReceiver.onERC721Received.selector),
                  "ERC721: onERC721Received fail"
                  );

7)STATE VARIABLES CAN BE PACKED INTO FEWER STORAGE SLOTS
If variables occupying the same slot are both written the same function or by the constructor,
avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper

File: 2022-10-holograph\contracts\HolographFactory.sol
  323,5:     uint8 v,

File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  181,3:   uint8 private _decimals;
  229,7:       uint8 contractDecimals,
  235,50:     ) = abi.decode(initPayload, (string, string, uint8, uint256, string, string, bool, bytes));
  273,44:   function decimals() public view returns (uint8) {
  465,5:     uint8 v,

8)++i/i++ should be unchecked{++i}/unchecked{++i} when it is not possible for them to overflow,
as is the case when used in for- and while-loops

File: 2022-10-holograph\contracts\HolographOperator.sol
  781,5:     for (uint256 i = 0; i < length; i++) {
  871,9:         for (uint256 i = _operatorPods.length; i <= pod; i++) {

File: 2022-10-holograph\contracts\enforcer\HolographERC20.sol
  564,5:     for (uint256 i = 0; i < wallets.length; i++) {

File: 2022-10-holograph\contracts\enforcer\HolographERC721.sol
  357,5:     for (uint256 i = 0; i < length; i++) {

  716,5:     for (uint256 i = 0; i < length; i++) { 

File: 2022-10-holograph\contracts\enforcer\PA1D.sol
  307,5:     for (uint256 i = 0; i < length; i++) {
  323,5:     for (uint256 i = 0; i < length; i++) {
  340,5:     for (uint256 i = 0; i < length; i++) {
  356,5:     for (uint256 i = 0; i < length; i++) {
  394,5:     for (uint256 i = 0; i < length; i++) {
  414,5:     for (uint256 i = 0; i < length; i++) {
  432,5:     for (uint256 t = 0; t < tokenAddresses.length; t++) {
  437,7:       for (uint256 i = 0; i < addresses.length; i++) {
  454,7:       for (uint256 i = 0; i < addresses.length; i++) {
  474,5:     for (uint256 i = 0; i < addresses.length; i++) {

9)USING > 0 COSTS MORE GAS THAN != 0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

This change saves 6 gas per instance  

File: 2022-10-holograph\contracts\HolographOperator.sol
  309,33:     require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
  350,32:         require(timeDifference > 0, "HOLOGRAPH: operator has time");

File: 2022-10-holograph\contracts\HolographOperator.sol
  309,33:     require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
  350,32:         require(timeDifference > 0, "HOLOGRAPH: operator has time");

File: 2022-10-holograph\contracts\enforcer\HolographERC721.sol
  815,21:     require(tokenId > 0, "ERC721: token id cannot be zero");
