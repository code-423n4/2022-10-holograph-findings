    1.  Increment 'i' at the end of the loop in 'unchecked' so save gas. 'unchecked { ++i; }' at end of loop instead of 'for(uint i; i < reservedTypes.length, i++)'. (found line 175 of ./HolographRegistry.sol).
    2.  Do not use '+=' ('_totalSupply += amount', found line 685 ./HolographERC20.sol). Instead use "_totalSupply =  _totalSupply + amount" in order to save some gas.
    3.  Using uint smaller than 32 bytes incurs overhead. Each operation involving a uint8 costs an extra 22-28 gas (found line 229  in HolographERC20.sol).
