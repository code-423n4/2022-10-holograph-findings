# > 0 is less efficient than != 0 for unsigned integers
The gas cost can be reduced by using != 0 instead of > 0 in the following locations:

|File|Line|Code|
|---|---|---|
|HolographERC721.sol|815|require(tokenId > 0, "ERC721: token id cannot be zero");|
|HolographOperator.sol|309|require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");|
|HolographOperator.sol|350|require(timeDifference > 0, "HOLOGRAPH: operator has time");|
|HolographOperator.sol|363|if (podIndex > 0 && podIndex < _operatorPods[pod].length) {|
|HolographOperator.sol|398|if (leftovers > 0) {|
|HolographOperator.sol|1126|if (operatorIndex > 0) {|
|HolographBridge.sol|218|if (hTokenValue > 0) {|

# i++ is less efficient than ++i
The gas cost can be reduced by using ++i or i += 1 instead of i++ in the following locations:

|File|Line|Code|
|---|---|---|
|HolographERC721.sol|357|for (uint256 i = 0; i < length; i++) {|
|HolographERC721.sol|716|for (uint256 i = 0; i < length; i++) {|
|HolographERC20.sol|564|for (uint256 i = 0; i < wallets.length; i++) {|
|PA1D.sol|307|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|323|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|340|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|356|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|394|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|414|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|432|for (uint256 t = 0; t < tokenAddresses.length; t++) {|
|PA1D.sol|437|for (uint256 i = 0; i < addresses.length; i++) {|
|PA1D.sol|454|for (uint256 i = 0; i < addresses.length; i++) {|
|PA1D.sol|474|for (uint256 i = 0; i < addresses.length; i++) {|
|HolographOperator.sol|781|for (uint256 i = 0; i < length; i++) {|
|HolographOperator.sol|871|for (uint256 i = _operatorPods.length; i <= pod; i++) {|

# Initialising variables to 0 uses unecessary gas
Uninitialsed variables default to 0x0. Hence, assigning them 0 uses gas unecessarily.

|File|Line|Code|
|---|---|---|
|HolographERC721.sol|357|for (uint256 i = 0; i < length; i++) {|
|HolographERC721.sol|716|for (uint256 i = 0; i < length; i++) {|
|HolographERC20.sol|564|for (uint256 i = 0; i < wallets.length; i++) {|
|PA1D.sol|307|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|323|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|340|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|356|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|394|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|414|for (uint256 i = 0; i < length; i++) {|
|PA1D.sol|432|for (uint256 t = 0; t < tokenAddresses.length; t++) {|
|PA1D.sol|437|for (uint256 i = 0; i < addresses.length; i++) {|
|PA1D.sol|454|for (uint256 i = 0; i < addresses.length; i++) {|
|PA1D.sol|474|for (uint256 i = 0; i < addresses.length; i++) {|
|HolographOperator.sol|310|uint256 gasLimit = 0;|
|HolographOperator.sol|311|uint256 gasPrice = 0;|
|HolographOperator.sol|781|for (uint256 i = 0; i < length; i++) {|
|HolographBridge.sol|380|uint256 fee = 0;|
