# Gas Optimization

## ++i costs less gas than i++ and i+=1 (--i/i--/i-=1 too).

    File: contracts/HolographOperator.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781

    File: contracts/HolographOperator.sol

        for (uint256 i = _operatorPods.length; i <= pod; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L871

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L307

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L323

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L340

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L356

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L394

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L414

    File: contracts/enforcer/PA1D.sol

    for (uint256 t = 0; t < tokenAddresses.length; t++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L432

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L437

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L454

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474

    File: contracts/enforcer/HolographERC721.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L357

    File: contracts/enforcer/HolographERC721.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L716

    File: contracts/enforcer/HolographERC721.sol

    _ownedTokensCount[to]++;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L779

    File: contracts/enforcer/HolographERC20.sol

    for (uint256 i = 0; i < wallets.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L564

    File: contracts/enforcer/HolographERC20.sol

    _nonces[account]++;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L713

    File: contracts/HolographOperator.sol

        podSize--;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L520

    File: contracts/HolographOperator.sol

    pod--;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L520

    File: contracts/enforcer/HolographERC721.sol

    _ownedTokensCount[from]--;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L842


## Initializing a variable to its default value costs unnecessary gas.

    File: contracts/HolographBridge.sol

    uint256 fee = 0;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L380

    File: contracts/HolographBridge.sol

    uint256 gasLimit = 0;
    uint256 gasPrice = 0;

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L310-L311

    File: contracts/HolographOperator.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L307

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L323

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L340

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L356

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L394

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L414

    File: contracts/enforcer/PA1D.sol

    for (uint256 t = 0; t < tokenAddresses.length; t++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L432

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L437

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L454

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474

    File: contracts/enforcer/HolographERC721.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L357

    File: contracts/enforcer/HolographERC721.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L716

    File: contracts/enforcer/HolographERC20.sol

    for (uint256 i = 0; i < wallets.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L564


## Array length should not be looked up in every loop. Instead, use a variable to store the length before the loop starts.

    File: contracts/enforcer/PA1D.sol

    for (uint256 t = 0; t < tokenAddresses.length; t++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L432

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L437

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L454

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474

    File: contracts/enforcer/HolographERC20.sol

    for (uint256 i = 0; i < wallets.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L564


## Variable increment(e.g.++i/i++) for looping should be unchecked{++i} when they are not possible to overflow, to remove overflow checking to save gas.

    File: contracts/HolographOperator.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L781

    File: contracts/HolographOperator.sol

        for (uint256 i = _operatorPods.length; i <= pod; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L871

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L307

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L323

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L340

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L356

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L394

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L414

    File: contracts/enforcer/PA1D.sol

    for (uint256 t = 0; t < tokenAddresses.length; t++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L432

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L437

    File: contracts/enforcer/PA1D.sol

      for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L454

    File: contracts/enforcer/PA1D.sol

    for (uint256 i = 0; i < addresses.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474

    File: contracts/enforcer/HolographERC721.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L357

    File: contracts/enforcer/HolographERC721.sol

    for (uint256 i = 0; i < length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L716

    File: contracts/enforcer/HolographERC20.sol

    for (uint256 i = 0; i < wallets.length; i++) {

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L564


## Use custom error instead of string in require() to save gas.

    File: contracts/HolographBridge.sol

    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L148

    File: contracts/HolographBridge.sol

    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L163

    File: contracts/HolographBridge.sol

    require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L203-L206

    File: contracts/HolographBridge.sol

    require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L214

    File: contracts/HolographBridge.sol

      require(
        HolographERC20Interface(hToken).holographBridgeMint(hTokenRecipient, hTokenValue) ==
          HolographERC20Interface.holographBridgeMint.selector,
        "HOLOGRAPH: hToken mint failed"
      );

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L224-L227

    File: contracts/HolographBridge.sol

    require(doNotRevert, "HOLOGRAPH: reverted");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L233

    File: contracts/HolographBridge.sol

    require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L255-L258

    File: contracts/HolographBridge.sol

    require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L270

    File: contracts/HolographBridge.sol

    require(
      _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
    );

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L352-L355

    File: contracts/HolographOperator.sol

    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L241

    File: contracts/HolographOperator.sol

    require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L309

    File: contracts/HolographOperator.sol

        require(timeDifference > 0, "HOLOGRAPH: operator has time");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L350

    File: contracts/HolographOperator.sol

        require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L354

    File: contracts/HolographOperator.sol

            require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L368

    File: contracts/HolographOperator.sol

    require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L415

    File: contracts/HolographOperator.sol

    require(msg.sender == address(this), "HOLOGRAPH: operator only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L446

    File: contracts/HolographOperator.sol

    require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L485

    File: contracts/HolographOperator.sol

    require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L591

    File: contracts/HolographOperator.sol

    require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L595

    File: contracts/HolographOperator.sol

    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
    
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L728

    File: contracts/HolographOperator.sol

    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L739

    File: contracts/HolographOperator.sol

    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L756

    File: contracts/HolographOperator.sol

    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L829

    File: contracts/HolographOperator.sol

    require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L839

    File: contracts/HolographOperator.sol

    require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L857

    File: contracts/HolographOperator.sol

      require(current <= amount, "HOLOGRAPH: bond amount too small");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L863

    File: contracts/HolographOperator.sol

      require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L881

    File: contracts/HolographOperator.sol

      require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L889

    File: contracts/HolographOperator.sol

    require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L903

    File: contracts/HolographOperator.sol

      require(_isContract(operator), "HOLOGRAPH: operator not contract");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L911
    
    File: contracts/HolographOperator.sol

      require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L915

    File: contracts/HolographOperator.sol

    require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L932

    File: contracts/HolographFactory.sol

    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L144

    File: contracts/HolographFactory.sol

    require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L220

    File: contracts/HolographFactory.sol

    require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L228

    File: contracts/HolographFactory.sol

    require(
      InitializableInterface(holographerAddress).init(
        abi.encode(abi.encode(config.chainType, holograph, config.contractType, sourceContractAddress), config.initCode)
      ) == InitializableInterface.init.selector,
      "initialization failed"
    );

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L250-L255

    File: contracts/module/LayerZeroModule.sol

    require(!_isInitialized(), "HOLOGRAPH: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L159

    File: contracts/module/LayerZeroModule.sol

    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L235

    File: contracts/enforcer/Holographer.sol

    require(!_isInitialized(), "HOLOGRAPHER: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L148

    File: contracts/enforcer/Holographer.sol

    require(success && selector == InitializableInterface.init.selector, "initialization failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L166

    File: contracts/enforcer/PA1D.sol

    require(isOwner(), "PA1D: caller not an owner");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L159

    File: contracts/enforcer/PA1D.sol

    require(!_isInitialized(), "PA1D: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L174

    File: contracts/enforcer/PA1D.sol

    require(initialized == 0, "PA1D: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L190

    File: contracts/enforcer/PA1D.sol

    require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L390

    File: contracts/enforcer/PA1D.sol

    require(balance > 10000, "PA1D: Not enough tokens to transfer");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L411

    File: contracts/enforcer/PA1D.sol

      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L416

    File: contracts/enforcer/PA1D.sol

      require(balance > 10000, "PA1D: Not enough tokens to transfer");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L435

    File: contracts/enforcer/PA1D.sol

        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L439

    File: contracts/enforcer/PA1D.sol

      require(matched, "PA1D: sender not authorized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L460

    File: contracts/enforcer/PA1D.sol

    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L472

    File: contracts/enforcer/PA1D.sol

    require(totalBp == 10000, "PA1D: bps down't equal 10000");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L477

    File: contracts/enforcer/HolographERC721.sol

    require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L212

    File: contracts/enforcer/HolographERC721.sol

    require(msg.sender == sourceContract, "ERC721: source only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L224

    File: contracts/enforcer/HolographERC721.sol

    require(!_isInitialized(), "ERC721: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L239

    File: contracts/enforcer/HolographERC721.sol

      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L258

    File: contracts/enforcer/HolographERC721.sol

      require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L263

    File: contracts/enforcer/HolographERC721.sol

    require(_exists(tokenId), "ERC721: token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L323

    File: contracts/enforcer/HolographERC721.sol

    require(to != tokenOwner, "ERC721: cannot approve self");
    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L370-L371

    File: contracts/enforcer/HolographERC721.sol

    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L388

    File: contracts/enforcer/HolographERC721.sol

    require(!_exists(tokenId), "ERC721: token already exists");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L404

    File: contracts/enforcer/HolographERC721.sol

      require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L408

    File: contracts/enforcer/HolographERC721.sol

    require(to != address(0), "ERC721: zero address");
    require(_isApproved(sender, tokenId), "ERC721: sender not approved");
    require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L419-L421

    File: contracts/enforcer/HolographERC721.sol

    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L458

    File: contracts/enforcer/HolographERC721.sol

      require(
        (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
          ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
          ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
          ERC721TokenReceiver.onERC721Received.selector),
        "ERC721: onERC721Received fail"
      );

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L464-L470

    File: contracts/enforcer/HolographERC721.sol

    require(to != msg.sender, "ERC721: cannot approve self");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L484

    File: contracts/enforcer/HolographERC721.sol

    require(!_burnedTokens[token], "ERC721: can't mint burned token");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L513

    File: contracts/enforcer/HolographERC721.sol

    require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L622

    File: contracts/enforcer/HolographERC721.sol

    require(wallet != address(0), "ERC721: zero address");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L639

    File: contracts/enforcer/HolographERC721.sol

    require(tokenOwner != address(0), "ERC721: token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L689

    File: contracts/enforcer/HolographERC721.sol

    require(index < _allTokens.length, "ERC721: index out of bounds");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L700

    File: contracts/enforcer/HolographERC721.sol

    require(index < balanceOf(wallet), "ERC721: index out of bounds");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L729

    File: contracts/enforcer/HolographERC721.sol

    require(_isContract(_operator), "ERC721: operator not contract");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L757

    File: contracts/enforcer/HolographERC721.sol

      require(tokenOwner == address(this), "ERC721: contract not token owner");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L762

    File: contracts/enforcer/HolographERC721.sol

    require(tokenId > 0, "ERC721: token id cannot be zero");
    require(to != address(0), "ERC721: minting to burn address");
    require(!_exists(tokenId), "ERC721: token already exists");
    require(!_burnedTokens[tokenId], "ERC721: token has been burned");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L815-L818

    File: contracts/enforcer/HolographERC721.sol

    require(_tokenOwner[tokenId] == from, "ERC721: token not owned");
    require(to != address(0), "ERC721: use burn instead");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L869-L870

    File: contracts/enforcer/HolographERC721.sol

    require(_exists(tokenId), "ERC721: token does not exist");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L906

    File: contracts/enforcer/HolographERC20.sol

    require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L192

    File: contracts/enforcer/HolographERC20.sol

    require(msg.sender == sourceContract, "ERC20: source only call");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L204

    File: contracts/enforcer/HolographERC20.sol

    require(!_isInitialized(), "ERC20: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L219

    File: contracts/enforcer/HolographERC20.sol

      require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L241

    File: contracts/enforcer/HolographERC20.sol

    require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L349

    File: contracts/enforcer/HolographERC20.sol

    require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L365

    File: contracts/enforcer/HolographERC20.sol

      require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L387

    File: contracts/enforcer/HolographERC20.sol

      require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L400

    File: contracts/enforcer/HolographERC20.sol

      require(newAllowance >= currentAllowance, "ERC20: increased above max value");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L427

    File: contracts/enforcer/HolographERC20.sol

    require(_isContract(account), "ERC20: operator not contract");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L445

    File: contracts/enforcer/HolographERC20.sol

      require(balance >= amount, "ERC20: balance check failed");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L450

    File: contracts/enforcer/HolographERC20.sol

    require(block.timestamp <= deadline, "ERC20: expired deadline");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L469

    File: contracts/enforcer/HolographERC20.sol

    require(signer == account, "ERC20: invalid signature");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L482

    File: contracts/enforcer/HolographERC20.sol

    require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L505

    File: contracts/enforcer/HolographERC20.sol

        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L529

    File: contracts/enforcer/HolographERC20.sol

    require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L539

    File: contracts/enforcer/HolographERC20.sol

        require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L599

    File: contracts/enforcer/HolographERC20.sol

    require(account != address(0), "ERC20: account is zero address");
    require(spender != address(0), "ERC20: spender is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L620-L621

    File: contracts/enforcer/HolographERC20.sol

    require(account != address(0), "ERC20: account is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L627

    File: contracts/enforcer/HolographERC20.sol

    require(accountBalance >= amount, "ERC20: amount exceeds balance");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L629

    File: contracts/enforcer/HolographERC20.sol

        require(erc165support, "ERC20: no ERC165 support");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L645

    File: contracts/enforcer/HolographERC20.sol

    require(to != address(0), "ERC20: minting to burn address");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L684

    File: contracts/enforcer/HolographERC20.sol

    require(account != address(0), "ERC20: account is zero address");
    require(recipient != address(0), "ERC20: recipient is zero address");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L695-L696

    File: contracts/enforcer/HolographERC20.sol

    require(accountBalance >= amount, "ERC20: amount exceeds balance");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L698

    File: contracts/abstract/ERC721H.sol

    require(msg.sender == holographer(), "ERC721: holographer only");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L117

    File: contracts/abstract/ERC721H.sol

      require(msgSender() == _getOwner(), "ERC721: owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L123

    File: contracts/abstract/ERC721H.sol

      require(msg.sender == _getOwner(), "ERC721: owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L125

    File: contracts/abstract/ERC721H.sol

    require(!_isInitialized(), "ERC721: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L147


    File: contracts/abstract/ERC20H.sol

    require(msg.sender == holographer(), "ERC20: holographer only");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L117

    File: contracts/abstract/ERC20H.sol

      require(msgSender() == _getOwner(), "ERC20: owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L123

    File: contracts/abstract/ERC20H.sol

      require(msg.sender == _getOwner(), "ERC20: owner only function");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L125

    File: contracts/abstract/ERC20H.sol

    require(!_isInitialized(), "ERC20: already initialized");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L147

