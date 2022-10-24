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

