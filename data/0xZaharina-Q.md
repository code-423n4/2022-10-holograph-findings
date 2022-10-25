# Inaccurate comment

all three slot has comment bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1) but the hash is different,

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L136

```solidity
  /**
   * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
   */
  bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;
  /**
   * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
   */
  bytes32 constant _baseGasSlot = 0x1eaa99919d5563fbfdd75d9d906ff8de8cf52beab1ed73875294c8a0c9e9d83a;
  /**
   * @dev bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1)
   */
  bytes32 constant _gasPerByteSlot = 0x99d8b07d37c89d4c4f4fa0fd9b7396caeb5d1d4e58b41c61c71e3cf7d424a625;

```

only the first one map to bytes32(uint256(keccak256('eip1967.Holograph.operator')) - 1) hash.

```solidity
bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;
```

## Use payable(address).transfer to send ETH instead of address(target).call

https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/

transfer() uses a fixed amount of gas, which can result in revert.

Use call instead of transfer(). Example:
(bool succeeded, ) = _to.call{value: _amount}("");
require(succeeded, "Transfer failed.");

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L596

```
payable(hToken).transfer(hlgFee);
```

# better naming to indicate the function change state in PA1D.sol

In PA1D.sol,

```solidity
function getPayoutInfo()
```

is a read function

yet the function below are write function

```solidity
function getEthPayout() public
function getTokenPayout(address tokenAddress) public
function getTokensPayout(address[] memory tokenAddresses) public
```

can change the function name 

from getTokenPayout to claimTokenPayout in PA1D.sol
from getTokensPayout to claimTokensPayout in PA1D.sol
from getETHPayout to claimETHPayout in PA1D.sol

to imply that function performs write operation instead of read operation.

## the state changing should emit event

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L452


```solidity
  function setFactory(address factory) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L472


```solidity
  function setHolograph(address holograph) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L502


```solidity
  function setOperator(address operator) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L522


```solidity
  function setRegistry(address registry) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L278


```solidity
  function resetOperator(
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L949


```solidity
  function setBridge(address bridge) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L969


```solidity
  function setHolograph(address holograph) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L989


```solidity
  function setInterfaces(address interfaces) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1009


```solidity
  function setMessagingModule(address messagingModule) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1029


```solidity
  function setRegistry(address registry) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1049


```solidity
  function setUtilityToken(address utilityToken) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L280


```solidity
  function setHolograph(address holograph) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L300


```solidity
  function setRegistry(address registry) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L320


```solidity
  function setBridge(address bridge) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L340


```solidity
  function setInterfaces(address interfaces) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L360


```solidity
  function setLZEndpoint(address lZEndpoint) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L380


```solidity
  function setOperator(address operator) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L441


```solidity
  function setBaseGas(uint256 baseGas) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L470


```solidity
  function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L226


```solidity
  function _setDefaultReceiver(address receiver) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L246


```solidity
  function _setDefaultBp(uint256 bp) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L268


```solidity
  function _setReceiver(uint256 tokenId, address receiver) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L291


```solidity
  function _setBp(uint256 tokenId, uint256 bp) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L316


```solidity
  function _setPayoutAddresses(address payable[] memory addresses) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L349


```solidity
  function _setPayoutBps(uint256[] memory bps) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L372


```solidity
  function _setTokenAddress(string memory tokenName, address tokenAddress) private {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L529


```solidity
  function setRoyalties(
```

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L203


```solidity
  function _setOwner(address ownerAddress) internal {
```
            

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L203


```solidity
  function _setOwner(address ownerAddress) internal {
```
       