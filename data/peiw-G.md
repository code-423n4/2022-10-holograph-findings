- `++i` costs less gas than `i++`, especially when in FOR loops (`--i` / `i--` too).
	- `++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled. `i++` increments i and returns the initial value of i. Which means: `uint i = 1; i++; // == 1 but i == 2` But `++i` returns the actual incremented value: `uint i = 1; ++i; // == 2 and i == 2` too, so no need for a temporary variable In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2.
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L779
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L713
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781


- Using unchecked blocks to save gas - increments in for loop can be unchecked (SAVE 30-40 GAS PER LOOP ITERATION).
	- The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop .
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781



- `<ARRAY>.length` should not be looked up in every loop of a for-loop.
	- The overheads PER LOOP, excluding the first loop: 1). storage arrays incur a Gwarmaccess (100 gas); 2). memory arrays use MLOAD (3 gas); 3). calldata arrays use CALLDATALOAD (3 gas). Caching the length changes each of these to a `DUP<N>` (3 gas), and gets rid of the extra `DUP<N>` needed to store the stack offset.
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564



- It costs more gas to initialize `NON-CONSTANT/NON-IMMUTABLE` variables to zero than to let the default of zero be applied.
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781


- `x += y` cost more gas than `x = x + y` for state variables.
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L685
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L686
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L702
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382


- Splitting `REQUIRE()` statements that use `&&` saves gas.
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L470

- `!=` is more efficient than `>` in require() (6 gas less).
	- Instance:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350


- Use custom errors rather than `REVERT()/REQUIRE()` strings to save gas.
	- Instances:
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L117
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L123
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L125
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L147
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L117
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L123
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L125
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L147
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L159
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L174
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L190
		- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390
