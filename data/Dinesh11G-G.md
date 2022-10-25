### Don't Initialize Variables with Default Value

#### Impact
Issue Information: Uninitialized variables are assigned with the type's default value.

Explicitly initializing a variable with its default value costs unnecessary gas.

#### Findings:
```
2022-10-holograph/contracts/HolographBridge.sol::380 => uint256 fee = 0;
2022-10-holograph/contracts/HolographInterfaces.sol::235 => for (uint256 i = 0; i < uriTypes.length; i++) {
2022-10-holograph/contracts/HolographInterfaces.sol::264 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographInterfaces.sol::286 => for (uint256 i = 0; i < interfaceIds.length; i++) {
2022-10-holograph/contracts/HolographOperator.sol::310 => uint256 gasLimit = 0;
2022-10-holograph/contracts/HolographOperator.sol::311 => uint256 gasPrice = 0;
2022-10-holograph/contracts/HolographOperator.sol::781 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::175 => for (uint256 i = 0; i < reservedTypes.length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::255 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/HolographRegistry.sol::322 => for (uint256 i = 0; i < hashes.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC20.sol::564 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::357 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::531 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::547 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::564 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/HolographERC721.sol::716 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::307 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::323 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::340 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::356 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::394 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::414 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::432 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::437 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::454 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/enforcer/PA1D.sol::474 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/contracts/library/Base64.sol::119 => uint256 i = 0;
2022-10-holograph/contracts/library/Base64.sol::120 => uint256 j = 0;
2022-10-holograph/contracts/library/Base64.sol::130 => uint8 la1 = 0;
2022-10-holograph/contracts/library/Bytes.sol::159 => uint256 length = 0;
2022-10-holograph/contracts/library/Strings.sol::114 => uint256 length = 0;
2022-10-holograph/contracts/library/Strings.sol::136 => for (uint256 i = 0; i < 20; i++) {
2022-10-holograph/contracts/mock/LZEndpointMock.sol::251 => for (uint256 i = 0; i < msgs.length - 1; i++) {
2022-10-holograph/contracts/mock/Mock.sol::114 => bool shouldFail = false;
2022-10-holograph/src/HolographBridge.sol::281 => uint256 fee = 0;
2022-10-holograph/src/HolographInterfaces.sol::136 => for (uint256 i = 0; i < uriTypes.length; i++) {
2022-10-holograph/src/HolographInterfaces.sol::165 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographInterfaces.sol::187 => for (uint256 i = 0; i < interfaceIds.length; i++) {
2022-10-holograph/src/HolographOperator.sol::211 => uint256 gasLimit = 0;
2022-10-holograph/src/HolographOperator.sol::212 => uint256 gasPrice = 0;
2022-10-holograph/src/HolographOperator.sol::682 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographRegistry.sol::76 => for (uint256 i = 0; i < reservedTypes.length; i++) {
2022-10-holograph/src/HolographRegistry.sol::153 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/HolographRegistry.sol::220 => for (uint256 i = 0; i < hashes.length; i++) {
2022-10-holograph/src/enforcer/HolographERC20.sol::465 => for (uint256 i = 0; i < wallets.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::258 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::432 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::448 => //     for (uint256 i = 0; i < tokenIds.length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::465 => //     for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/HolographERC721.sol::617 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::208 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::224 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::241 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::257 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::295 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::315 => for (uint256 i = 0; i < length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::333 => for (uint256 t = 0; t < tokenAddresses.length; t++) {
2022-10-holograph/src/enforcer/PA1D.sol::338 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::355 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/enforcer/PA1D.sol::375 => for (uint256 i = 0; i < addresses.length; i++) {
2022-10-holograph/src/library/Base64.sol::20 => uint256 i = 0;
2022-10-holograph/src/library/Base64.sol::21 => uint256 j = 0;
2022-10-holograph/src/library/Base64.sol::31 => uint8 la1 = 0;
2022-10-holograph/src/library/Bytes.sol::60 => uint256 length = 0;
2022-10-holograph/src/library/Strings.sol::15 => uint256 length = 0;
2022-10-holograph/src/library/Strings.sol::37 => for (uint256 i = 0; i < 20; i++) {
2022-10-holograph/src/mock/LZEndpointMock.sol::152 => for (uint256 i = 0; i < msgs.length - 1; i++) {
2022-10-holograph/src/mock/Mock.sol::15 => bool shouldFail = false;
```
#### Tools used
Manual