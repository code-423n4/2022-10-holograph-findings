# Missing events 

### Events are missing despite that the function is changing the state and it is best practice this to be registrated via an event emitting.

#### File: HolographGenesis.sol

1. function deploy(
    uint256 chainId,
    bytes12 saltHash,
    bytes memory sourceCode,
    bytes memory initCode
  ) external onlyDeployer {
    require(chainId == block.chainid, "HOLOGRAPH: incorrect chain id");
    bytes32 salt = bytes32(abi.encodePacked(msg.sender, saltHash));
    address contractAddress = address(
      uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, keccak256(sourceCode)))))
    );
    require(!_isContract(contractAddress), "HOLOGRAPH: already deployed");
    assembly {
      contractAddress := create2(0, add(sourceCode, 0x20), mload(sourceCode), salt)
    }
    require(_isContract(contractAddress), "HOLOGRAPH: deployment failed");
    require(
      InitializableInterface(contractAddress).init(initCode) == InitializableInterface.init.selector,
      "HOLOGRAPH: initialization failed"
    );

1.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographGenesis.sol#L124


========================================

2.function approveDeployer(address newDeployer, bool approve) external onlyDeployer {
    _approvedDeployers[newDeployer] = approve;
  }
2.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographGenesis.sol#L146


# Code Style: constants should be named in all caps

### Here are some examples that the code style does not follow the best practices: 

#### File: HolographBridge.sol

1.bytes32 constant _factorySlot = 0xa49f20855ba576e09d13c8041c8039fa655356ea27f6c40f1ec46a4301cd5b23;
1.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L126

#### Consider changing the name to: FACTORY_SLOT



#### File: HolographTreasury.sol
2.bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
2.https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographTreasury.sol#L133

#### Consider changing the name to: HOLOGRAPH_SLOT
