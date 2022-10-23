
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


