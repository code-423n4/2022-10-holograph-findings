
### [G-01] `delete` operation could be avoided
[Here](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L1049) the array is `pop()` anyway so there is no need to manually delete the storage


### [G-02] Use a mapping instead of arrays for `_operatorPods`

In `HolographOperator`, `_operatorPods` is a Multi-dimensional array. But it is used as a stack, and successful length modifications (`pop` then `push`) will set the storage back to 0, and then to non 0, which costs overall ~20k gas.

Instead, you could use a `mapping` to represent the array and store the length of the array separately. This way, when popping an element you don't need to clear the storage and every sequence of `pop` then `push` will save you 10k gas.


### [G-03] Useless getters

What are the utility of all getters like [`getOperator`](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L492)? If it's only for off-chain use, you could read from the storage slot directly and save deployment costs.


### [G-05] Some private variables could be constants

Assuming [`resetOperator`](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L278) is not part of the final code at it seems to be the intent of the dev team, some private variables could be switch to constants to save numerous cold `SLOAD`(2100 gas). 

At least the following ones in `HolographOperator`: `_blockTime`, `_baseBondAmount`, `_podMultiplier`, `_operatorThreshold`, `_operatorThresholdStep`, `_operatorThresholdDivisor`.


### [G-05] Remove `blockTimes` from `OperatorJob`.

In `HolographOperator`, `job.blockTimes` is always equal to `_blockTime` so the entry could be removed from the struct. This could be combined with the previous gas optimization for even more savings.


### [G-06] Useless check and external call `require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");`.

In `HolographFactory`, at this [line](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographFactory.sol#L129) there is no need to check if the contract has already been deployed per EIP-684. See [here](https://eips.ethereum.org/EIPS/eip-1014).

Quoting it: "If a contract creation is attempted, due to either a creation transaction or the CREATE (or future CREATE2) opcode, and the destination address already has either nonzero nonce, or nonempty code, then the creation throws immediately, with exactly the same behavior as would arise if the first byte in the init code were an invalid opcode. This applies retroactively starting from genesis."
