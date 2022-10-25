1. HolographBridge.sol

Line 380:  uint256 fee = 0;

- do not need to initialize with a default value

2.  HolographInterfaces.sol
```
 function updateUriPrepends(TokenUriType[] calldata uriTypes, string[] calldata prepends) external onlyAdmin {
    for (uint256 i = 0; i < uriTypes.length; i++) {
      _prependURI[uriTypes[i]] = prepends[i];
    }
  }
```
- do not need to init 'i' with a default value
- need to cache uriTypes.length in a local variable
- use ++i instead of i++


```
  function updateChainIdMaps(
    ChainIdType[] calldata fromChainType,
    uint256[] calldata fromChainId,
    ChainIdType[] calldata toChainType,
    uint256[] calldata toChainId
  ) external onlyAdmin {
    uint256 length = fromChainType.length;
    for (uint256 i = 0; i < length; i++) {
      _chainIdMap[fromChainType[i]][fromChainId[i]][toChainType[i]] = toChainId[i];
    }
  }
```
- do not init 'i'
- use pre-increment for 'i'


```
function updateInterfaces(
    InterfaceType interfaceType,
    bytes4[] calldata interfaceIds,
    bool supported
  ) external onlyAdmin {
    for (uint256 i = 0; i < interfaceIds.length; i++) {
      _supportedInterfaces[interfaceType][interfaceIds[i]] = supported;
    }
  }
```
- do not init 'i' with 0
- use ++i
- cache interfaceIds.length in a local variable

3. HolographOperator.sol

Lines #310-311: 
```
  uint256 gasLimit = 0;
  uint256 gasPrice = 0;
```
- do not need to init with a default value


```
 function getPodOperators(
    uint256 pod,
    uint256 index,
    uint256 length
  ) external view returns (address[] memory operators) {
    require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");
    pod--;
    uint256 supply = _operatorPods[pod].length;
    if (index + length > supply) {
      length = supply - index;
    }
    operators = new address[](length);
    for (uint256 i = 0; i < length; i++) {
      operators[i] = _operatorPods[pod][index + i];
    }
  }
```

- do not need to init 'i' in  for loop
- use pre-increment for 'i'


```
 function bondUtilityToken(
    address operator,
    uint256 amount,
    uint256 pod
  ) external {
    unchecked {
      uint256 current = _getCurrentBondAmount(pod - 1);
      require(current <= amount, "HOLOGRAPH: bond amount too small");
      if (_operatorPods.length < pod) {
        for (uint256 i = _operatorPods.length; i <= pod; i++) {
          _operatorPods.push([address(0)]);
        }
      }
	...
}
```

- cache _operatorPods.length before a loop
- use pre-increment for 'i'
- i would suggest to create a local copy of _operatorPods array to prevent cold access


4. HolographRegistry.sol
```
function init(bytes memory initPayload) external override returns (bytes4) {
    require(!_isInitialized(), "HOLOGRAPH: already initialized");
    (address holograph, bytes32[] memory reservedTypes) = abi.decode(initPayload, (address, bytes32[]));
    ...
	for (uint256 i = 0; i < reservedTypes.length; i++) {
      _reservedTypes[reservedTypes[i]] = true;
    }
    _setInitialized();
    return InitializableInterface.init.selector;
  }
```
- cache _reservedTypes.length in a local variable
- do not init 'i' with default value
- use pre-increment for 'i'


```
function getHolographableContracts(uint256 index, uint256 length) external view returns (address[] memory contracts) {
    uint256 supply = _holographableContracts.length;
    if (index + length > supply) {
      length = supply - index;
    }
    contracts = new address[](length);
    for (uint256 i = 0; i < length; i++) {
      contracts[i] = _holographableContracts[index + i];
    }
  }
```

- do not init 'i' with default value
- use pre-increment for 'i'
- create a local copy of _holographableContracts array to prevent cold access gas fee


```
function setReservedContractTypeAddresses(bytes32[] calldata hashes, bool[] calldata reserved) external onlyAdmin {
    for (uint256 i = 0; i < hashes.length; i++) {
      _reservedTypes[hashes[i]] = reserved[i];
    }
  }
```

- do not init 'i'
- use pre-increment for 'i'


5. enforcer/HolographERC20.sol

```
function sourceMintBatch(address[] calldata wallets, uint256[] calldata amounts) external onlySource {
    for (uint256 i = 0; i < wallets.length; i++) {
      _mint(wallets[i], amounts[i]);
    }
  }
```

- cache wallets.length in a local variable
- do not init 'i' with a default value
- use pre-increment for 'i'

6. enforcer/HolographERC721.sol
```
function tokensOfOwner(
    address wallet,
    uint256 index,
    uint256 length
  ) external view returns (uint256[] memory tokenIds) {
    uint256 supply = _ownedTokensCount[wallet];
    if (index + length > supply) {
      length = supply - index;
    }
    tokenIds = new uint256[](length);
    for (uint256 i = 0; i < length; i++) {
      tokenIds[i] = _ownedTokens[wallet][index + i];
    }
  }
```
- do not init 'i' with 0
- use pre-increment for 'i


```
function tokens(uint256 index, uint256 length) external view returns (uint256[] memory tokenIds) {
    uint256 supply = _allTokens.length;
    if (index + length > supply) {
      length = supply - index;
    }
    tokenIds = new uint256[](length);
    for (uint256 i = 0; i < length; i++) {
      tokenIds[i] = _allTokens[index + i];
    }
  }
```
- do not init 'i' with default value
- use pre-increment for 'i'
- create a copy of _allTokens array to avoid cold access fee

7. enforcer/PA1D.sol
```
function _payoutToken(address tokenAddress) private {
    address payable[] memory addresses = _getPayoutAddresses();
    uint256[] memory bps = _getPayoutBps();
    uint256 length = addresses.length;
    ERC20 erc20 = ERC20(tokenAddress);
    uint256 balance = erc20.balanceOf(address(this));
    require(balance > 10000, "PA1D: Not enough tokens to transfer");
    uint256 sending;
    //uint256 sent;
    for (uint256 i = 0; i < length; i++) {
      sending = ((bps[i] * balance) / 10000);
      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
      // sent = sent + sending;
    }
```
- do not init 'i' with default value
- use pre-increment


```
function _payoutTokens(address[] memory tokenAddresses) private {
    address payable[] memory addresses = _getPayoutAddresses();
    uint256[] memory bps = _getPayoutBps();
    ERC20 erc20;
    uint256 balance;
    uint256 sending;
    for (uint256 t = 0; t < tokenAddresses.length; t++) {
      erc20 = ERC20(tokenAddresses[t]);
      balance = erc20.balanceOf(address(this));
      require(balance > 10000, "PA1D: Not enough tokens to transfer");
      // uint256 sent;
      for (uint256 i = 0; i < addresses.length; i++) {
        sending = ((bps[i] * balance) / 10000);
        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
        // sent = sent + sending;
      }
    }
  }
```
- do not init t and i with default value
- use pre-increment
- cache arrays lengths before for loops
- do not pass long (>32 bytes) strings to revert (lines 411, 435)


```
function _validatePayoutRequestor() private view {
    if (!isOwner()) {
      bool matched;
      address payable[] memory addresses = _getPayoutAddresses();
      address payable sender = payable(msg.sender);
      for (uint256 i = 0; i < addresses.length; i++) {
        if (addresses[i] == sender) {
          matched = true;
          break;
        }
      }
      require(matched, "PA1D: sender not authorized");
    }
  }
```
- dont init 'i' with default value
- use pre-increment
- cache address.length before loop


```
function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
    uint256 totalBp;
    for (uint256 i = 0; i < addresses.length; i++) {
      totalBp = totalBp + bps[i];
    }
    require(totalBp == 10000, "PA1D: bps down't equal 10000");
    _setPayoutAddresses(addresses);
    _setPayoutBps(bps);
  }
```
- cache address.length before loop
- do not init 'i' with zero
- use pre-increment for 'i'
- change arguments memory type to calldata

8. faucet/Faucet.sol 

Line 128: require(!_isInitialized(), "Faucet contract is already initialized");

- do not pass long strings to revert()

9.  library/Base64.sol
```
function encode(bytes memory _bs) internal pure returns (string memory) {
    uint256 rem = _bs.length % 3;

    uint256 res_length = ((_bs.length + 2) / 3) * 4 - ((3 - rem) % 3);
    bytes memory res = new bytes(res_length);

    uint256 i = 0;
    uint256 j = 0;

    for (; i + 3 <= _bs.length; i += 3) {
      (res[j], res[j + 1], res[j + 2], res[j + 3]) = encode3(uint8(_bs[i]), uint8(_bs[i + 1]), uint8(_bs[i + 2]));

      j += 4;
    
	...
}
```
- cache _bs.length in a local variable
- do not init 'i', 'j','la1' with default values

10.  library/Bytes.sol

```
function trim(bytes32 source) internal pure returns (bytes memory) {
    uint256 temp = uint256(source);
    uint256 length = 0;
    while (temp != 0) {
      length++;
      temp >>= 8;
    }
    return slice(abi.encodePacked(source), 32 - length, length);
  }
}
```
- do not need to init 'length'
- use pre-increment for 'length'

11. library/ECDSA.sol

Lines 31 and 33:
revert("ECDSA: invalid signature 's' value");
revert("ECDSA: invalid signature 'v' value");

- do not pass long strings to revert()

12. library/Strings.sol
```
function toHexString(uint256 value) internal pure returns (string memory) {
    if (value == 0) {
      return "0x00";
    }
    uint256 temp = value;
    uint256 length = 0;
    while (temp != 0) {
      length++;
      temp >>= 8;
    }
    return toHexString(value, length);
  }
```
- do not need to init 'length'
- use pre-increment for 'length'

```
function toAsciiString(address x) internal pure returns (string memory) {
    bytes memory s = new bytes(40);
    for (uint256 i = 0; i < 20; i++) {
      bytes1 b = bytes1(uint8(uint256(uint160(x)) / (2**(8 * (19 - i)))));
      bytes1 hi = bytes1(uint8(b) / 16);
      bytes1 lo = bytes1(uint8(b) - 16 * uint8(hi));
      s[2 * i] = char(hi);
      s[2 * i + 1] = char(lo);
    }
    return string(s);
  }
```
- do not init 'i' with default value
- use pre-increment for 'i'
- probably the whole loop could be put in unchecked block


