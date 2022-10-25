# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Multiple address mappings can be combined into a single mapping of an address to a struct     | 3      |
| 2      | Variables inside struct should be packed to save gas  | 1    |
| 3      | Use `unchecked` blocks to save gas  |  2 |
| 4      | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  |  7 |
| 5      | Use `calldata` instead of `memory` for function parameters type  |  5 |
| 6      | Splitting `require()` statements that uses && saves gas  |  4 |
| 7      | `++i/i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops |  13  |
| 8      | Using `private` rather than `public` for constants, saves gas |  40 |

## Findings

### 1- Multiple address mappings can be combined into a single mapping of an address to a struct :

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

There are 3 instances of this issue :

```
File: contracts/HolographOperator.sol

218      mapping(address => uint256) private _bondedOperators;
223      mapping(address => uint256) private _operatorPodIndex;
228      mapping(address => uint256) private _bondedAmounts;   
```

These mappings could be refactored into the following struct and mapping for example :

```
struct Operator {
    uint256 _bondedOperators;
    uint256 _operatorPodIndex;
    uint256 _bondedAmount;
}
    
mapping(address => Operator) private _operators;
```

### 2- Variables inside `struct` should be packed to save gas :

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

There is 1 instance of this issue:

File: contracts/struct/OperatorJob.sol [Line 104-111](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/struct/OperatorJob.sol#L104-L111)

```
struct OperatorJob {
    uint8 pod;
    uint16 blockTimes;
    address operator;
    uint40 startBlock;
    uint64 startTimestamp;
    uint16[5] fallbackOperators;
}
```

The struct should be modified as follow to save gas :

```
struct OperatorJob {
    address operator;
    uint8 pod;
    uint16 blockTimes;
    uint40 startBlock;
    uint64 startTimestamp;
    uint16[5] fallbackOperators;
}
```

### 3- Use `unchecked` blocks to save gas :

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

There are 2 instances of this issue:

File: contracts/enforcer/PA1D.sol [Line 391](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L391)

```
balance = balance - gasCost;
```

The above operation cannot underflow due to the check [require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390) which means that balance is always greater than gasCost. It should be modified as follows :

```
unchecked {
    balance = balance - gasCost;
}
```

File: contracts/HolographOperator.sol [Line 1175](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1175)

```
position -= threshold;
```

The above operation cannot underflow due to the check [if (position > threshold)](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1174) thus it should be modified as follows :

```
unchecked {
    position -= threshold;
}
```

### 4- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

Using the addition operator instead of plus-equals saves **113 gas** as explained [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)

There are 7 instances of this issue:

```
File: contracts/HolographOperator.sol

378     _bondedAmounts[job.operator] -= amount;
382     _bondedAmounts[msg.sender] += amount;
834     _bondedAmounts[operator] += amount;

File: contracts/enforcer/HolographERC20.sol

633     _totalSupply -= amount;
685     _totalSupply += amount;
686     _balances[to] += amount;
702     _balances[recipient] += amount;
```

### 5- Use `calldata` instead of `memory` for function parameters type :

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

There are 5 instances of this issue :

```
File: contracts/enforcer/PA1D.sol

316     function _setPayoutAddresses(address payable[] memory addresses)
349     function _setPayoutBps(uint256[] memory bps)
426     function _payoutTokens(address[] memory tokenAddresses)
471     function configurePayouts(address payable[] memory addresses, uint256[] memory bps)
517     function getTokensPayout(address[] memory tokenAddresses)
```

### 6- Splitting `require()` statements that uses && saves gas :

There are 4 instances of this issue :

```
File: contracts/enforcer/HolographERC721.sol

263     require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");
464     require(
            (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&
            ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
            ERC721TokenReceiver(to).onERC721Received(address(this), from, tokenId, data) ==
            ERC721TokenReceiver.onERC721Received.selector),
            "ERC721: onERC721Received fail"
        );

File: contracts/enforcer/Holographer.sol

166     require(success && selector == InitializableInterface.init.selector, "initialization failed");

File: contracts/HolographOperator.sol

857     require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

### 7- `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops :

There are 13 instances of this issue:
 
```
File: contracts/enforcer/HolographERC20.sol

564     for (uint256 i = 0; i < wallets.length; i++)

File: contracts/enforcer/Holographer.sol

357     for (uint256 i = 0; i < length; i++)

File: contracts/enforcer/PA1D.sol

307     for (uint256 i = 0; i < length; i++)
323     for (uint256 i = 0; i < length; i++)
356     for (uint256 i = 0; i < length; i++)
394     for (uint256 i = 0; i < length; i++)
414     for (uint256 i = 0; i < length; i++)
432     for (uint256 t = 0; t < tokenAddresses.length; t++)
437     for (uint256 i = 0; i < addresses.length; i++)
454     for (uint256 i = 0; i < addresses.length; i++)
474     for (uint256 i = 0; i < addresses.length; i++)

File: contracts/HolographOperator.sol

781     for (uint256 i = 0; i < length; i++)
871     for (uint256 i = _operatorPods.length; i <= pod; i++)
```

### 8- Using `private` rather than `public` for constants, saves gas :

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

There are 40 instances of this issue:
 
```
File: contracts/abstract/ERC20H.sol

110     bytes32 constant _holographerSlot = 0xe9fcff60011c1a99f7b7244d1f2d9da93d79ea8ef3654ce590d775575255b2bd;
114     bytes32 constant _ownerSlot = 0xb56711ba6bd3ded7639fc335ee7524fe668a79d7558c85992e3f8494cf772777;

File: contracts/abstract/ERC721H.sol

110     bytes32 constant _holographerSlot = 0xe9fcff60011c1a99f7b7244d1f2d9da93d79ea8ef3654ce590d775575255b2bd;
114     bytes32 constant _ownerSlot = 0xb56711ba6bd3ded7639fc335ee7524fe668a79d7558c85992e3f8494cf772777;

File: contracts/enforcer/HolographERC20.sol

142     bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
146     bytes32 constant _sourceContractSlot = 0x27d542086d1e831d40b749e7f5509a626c3047a36d160781c40d5acc83e5b074;

File: contracts/enforcer/HolographERC721.sol

136     bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
140     bytes32 constant _sourceContractSlot = 0x27d542086d1e831d40b749e7f5509a626c3047a36d160781c40d5acc83e5b074;

File: contracts/enforcer/Holographer.sol

119     bytes32 constant _originChainSlot = 0xd49ffd6af8249d6e6b5963d9d2b22c6db30ad594cb468453047a14e1c1bcde4d;
123     bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
127     bytes32 constant _contractTypeSlot = 0x0b671eb65810897366dd82c4cbb7d9dff8beda8484194956e81e89b8a361d9c7;
131     bytes32 constant _sourceContractSlot = 0x27d542086d1e831d40b749e7f5509a626c3047a36d160781c40d5acc83e5b074;
135     bytes32 constant _blockHeightSlot = 0x9172848b0f1df776dc924b58e7fa303087ae0409bbf611608529e7f747d55de3;

File: contracts/enforcer/PA1D.sol

124     bytes32 constant _defaultBpSlot = 0x3ab91e3c2ba71a57537d782545f8feb1d402b604f5e070fa6c3b911fc2f18f75;
128     bytes32 constant _defaultReceiverSlot = 0xfd430e1c7265cc31dbd9a10ce657e68878a41cfe179c80cd68c5edf961516848;
132     bytes32 constant _initializedPaidSlot = 0x33a44e907d5bf333e203bebc20bb8c91c00375213b80f466a908f3d50b337c6c;
136     bytes32 constant _payoutAddressesSlot = 0x700a541bc37f227b0d36d34e7b77cc0108bde768297c6f80f448f380387371df;
140     bytes32 constant _payoutBpsSlot = 0x7a62e8104cd2cc2ef6bd3a26bcb71428108fbe0e0ead6a5bfb8676781e2ed28d;
142     string constant _bpString = "eip1967.Holograph.PA1D.bp";
143     string constant _receiverString = "eip1967.Holograph.PA1D.receiver";
144     string constant _tokenAddressString = "eip1967.Holograph.PA1D.tokenAddress";

File: contracts/module/LayerZeroModule.sol

126     bytes32 constant _bridgeSlot = 0xeb87cbb21687feb327e3d58c6c16d552231d12c7a0e8115042a4165fac8a77f9;
130     bytes32 constant _interfacesSlot = 0xbd3084b8c09da87ad159c247a60e209784196be2530cecbbd8f337fdd1848827;
134     bytes32 constant _lZEndpointSlot = 0x56825e447adf54cdde5f04815fcf9b1dd26ef9d5c053625147c18b7c13091686;
138     bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;
142     bytes32 constant _baseGasSlot = 0x1eaa99919d5563fbfdd75d9d906ff8de8cf52beab1ed73875294c8a0c9e9d83a;
146     bytes32 constant _gasPerByteSlot = 0x99d8b07d37c89d4c4f4fa0fd9b7396caeb5d1d4e58b41c61c71e3cf7d424a625;

File: contracts/HolographBridge.sol

126     bytes32 constant _factorySlot = 0xa49f20855ba576e09d13c8041c8039fa655356ea27f6c40f1ec46a4301cd5b23;
130     bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
134     bytes32 constant _jobNonceSlot = 0x1cda64803f3b43503042e00863791e8d996666552d5855a78d53ee1dd4b3286d;
138     bytes32 constant _operatorSlot = 0x7caba557ad34138fa3b7e43fb574e0e6cc10481c3073e0dffbc560db81b5c60f;
142     bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;

File: contracts/HolographFactory.sol

127     bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
131     bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;

File: contracts/HolographOperator.sol

129     bytes32 constant _bridgeSlot = 0xeb87cbb21687feb327e3d58c6c16d552231d12c7a0e8115042a4165fac8a77f9;
133     bytes32 constant _holographSlot = 0xb4107f746e9496e8452accc7de63d1c5e14c19f510932daa04077cd49e8bd77a;
137     bytes32 constant _interfacesSlot = 0xbd3084b8c09da87ad159c247a60e209784196be2530cecbbd8f337fdd1848827;
141     bytes32 constant _jobNonceSlot = 0x1cda64803f3b43503042e00863791e8d996666552d5855a78d53ee1dd4b3286d;
145     bytes32 constant _messagingModuleSlot = 0x54176250282e65985d205704ffce44a59efe61f7afd99e29fda50f55b48c061a;
149     bytes32 constant _registrySlot = 0xce8e75d5c5227ce29a4ee170160bb296e5dea6934b80a9bd723f7ef1e7c850e7;
152     bytes32 constant _utilityTokenSlot = 0xbf76518d46db472b71aa7677a0908b8016f3dee568415ffa24055f9a670f9c37;
```