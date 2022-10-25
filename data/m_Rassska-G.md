# Disclaimer

- Dear, judges and sponsors! The gas optimization report consists of the general optimization patterns and unique optimization cases. I'll attach every single occurance of unique cases `[GQ-xx]`, buf for general optimization patterns `[G-xx]` - only the random one. This is done, cuz I wanna focus more on the quality side. Although, some `[GQ-xx]s` might be invalid, sorry about it! Enjoy!

# Table of contents

- **[[GQ-01] Use `calldataload` for read-only arguments in functions](#gq-01-use-calldataload-for-read-only-arguments-in-functions)**
- **[[GQ-02] Unused the named return variables](#gq-02-unused-the-named-return-variables)**
- **[[GQ-03] Consider `a = a + b` instead of `a += b`, in a case of `a` being a state variable](#gq-03-consider-a--a--b-instead-of-a--b-in-case-of-a-being-state-variable)**
- **[[GQ-04] Re-arrange `require/if` statements according to the gas usage](#gq-04-re-arrange-requireif-statements-according-to-the-gas-usage)**
- **[[GQ-07] Add `require()/reverts` before some computations](#gq-07-add-requirereverts-before-some-computations)**
- **[[GQ-10] Cache state variables into a memory to avoid extra warm-accessed `sload`](#gq-10-cache-state-variables-into-a-memory-to-avoid-extra-warm-accessed-sload)**
- **[[G-12] Try `++i/--i` instead of `i++/i--`](#g-12-try-i--i-instead-of-ii)**
- **[[G-13] Try `unchecked{++i}` instead of `i++` in loops](#g-13-try-uncheckedi-instead-of-ii-in-loops)**
- **[[G-14] Consider marking onlyOwner functions as payable](#g-14-consider-marking-onlyowner-functions-as-payable)**
- **[[G-15] Use the recent version of solidity](#g-15-use-the-recent-version-of-solidity)**
- **[[G-16] Possible names optimization](#g-16-possible-names-optimization)**
- **[[G-17] Breakdown complex conditions into multiple `if/require` statements](#g-17-breakdown-complex-conditions-into-multiple-ifrequire-statements)**
- **[[G-18] Use const values instead of computing `type(uint256).max` every time](#g-18-use-const-values-instead-of-computing-typeuint256max-every-time)**
- **[[G-19] Avoid memory expansions](#g-19-avoid-memory-expansions)**
- **[[G-xx] Overall analysis](#overall-analysis)**



## **[GQ-01] Use `calldataload` for read-only arguments in functions**<a name="GQ-01"></a>

### ***Description:***
 - In EIP-2029, gas consumption for non-zero byte read from calldata was dicreased from 68 to ~16, therefore, try to use it to save gas per each transaction and on the deployment stage as well. Here you need to explicitly specify `calldataload`, or replace `memory` with `calldata`, if args are not supposed to be modified.
 

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: contracts/HolographBridge.sol 
      ....................................
      
        // Lines: [63-78]
            // Comment: use calldata for `initPayload` 
              function init(bytes memory initPayload) external override returns (bytes4) {
                    require(!_isInitialized(), "HOLOGRAPH: already initialized");
                    (address factory, address holograph, address operator, address registry) = abi.decode(
                    initPayload,
                    (address, address, address, address)
                    );
                    assembly {
                    sstore(_adminSlot, origin())
                    sstore(_factorySlot, factory)
                    sstore(_holographSlot, holograph)
                    sstore(_operatorSlot, operator)
                    sstore(_registrySlot, registry)
                    }
                    _setInitialized();
                    return InitializableInterface.init.selector;
                }

        // Lines: [163-167]
            // Comment: use calldata instead of memory for `returnedPayload`, since it has not been modified.
                (bytes4 selector, bytes memory returnedPayload) = Holographable(holographableContract).bridgeOut(
                    toChain,
                    msg.sender,
                    bridgeOutPayload
                );
        // Lines: [197-230]
          function revertedBridgeOutRequest(
                address sender,
                uint32 toChain,
                address holographableContract,
                bytes calldata bridgeOutPayload
            ) external returns (string memory revertReason) {
                /**
                * @dev make a bridgeOut function call to the holographable contract inside of a try/catch
                */
                try Holographable(holographableContract).bridgeOut(toChain, sender, bridgeOutPayload) returns (
                bytes4 selector,
                bytes memory payload
                ) {
                /**
                * @dev ensure returned selector is bridgeOut function signature, to guarantee that the function was called and succeeded
                */
                if (selector != Holographable.bridgeOut.selector) {
                    /**
                    * @dev if selector does not match, then it means the request failed
                    */
                    return "HOLOGRAPH: bridge out failed";
                }
                assembly {
                    /**
                    * @dev the entire payload is sent back in a revert
                    */
                    revert(add(payload, 0x20), mload(payload))
                }
                } catch Error(string memory reason) {
                    return reason;
                } catch {
                    return "HOLOGRAPH: unknown error";
                }
            }

    ```
## **[GQ-02] Unused the named return variables**<a name="GQ-02"></a>

### ***Description:***

- Unnecessary gas usage on a deployment side.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/HolographBridge.sol 
      ...............................

        // Lines: [197-202]
            // Comment: `revertReason` has not been used. 
              function revertedBridgeOutRequest(
                address sender,
                uint32 toChain,
                address holographableContract,
                bytes calldata bridgeOutPayload
            ) external returns (string memory revertReason) {}

    ``` 

## **[GQ-03] Consider `a = a + b` instead of `a += b`, in case of `a` being state variable**<a name="GQ-03"></a>

### ***Description:***

- It has an impact on the deployment side and on each distinct transaction as well, since in a case of `a += b` some extra copies of `a` are performed.

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographOperator.sol 
      ...............................

        // Lines: [279-283]
            _bondedAmounts[job.operator] -= amount;
            /**
            * @dev the slashed amount is sent to current operator
            */
            _bondedAmounts[msg.sender] += amount;

        // Lines: [735-735]
            _bondedAmounts[operator] += amount;

    ```
  
    ```Solidity
      file: src/enforcer/HolographERC20.sol 
      .....................................

        // Lines: [586-587]
            _totalSupply += amount;
            _balances[to] += amount;

        // Lines: [603-603]
            _balances[recipient] += amount;

        // Lines: [584-584]
            _totalSupply -= amount;


    ```
## **[GQ-04] Re-arrange `require/if` statements according to the gas usage**<a name="GQ-04"></a>

### ***Description:***

  - Let's take an example: 
    ```Solidity
        if (!creationUnlocked && msg.sender != _owner) 
            revert LBFactory__FunctionIsLockedForUsers(msg.sender);
    ```
    The tx should revert in any other cases: except `true && true`, but checking `msg.sender != _owner` first and then checking other part costs less gas in a cases, where the second part fails. Therefore, it's reasonable to put lightest conditions first.

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographBridge.sol 
      ...............................
      
        // Lines: [156-159]
            // Comments: put the second condition first, since it cheaper
                require(
                    _registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
                    "HOLOGRAPH: not holographed"
                );



## **[GQ-10] Cache state variables into a memory to avoid extra warm-accessed `sload`**<a name="GQ-10"></a>

### ***Description:***

- `mload` costs only 3 units of gas, `sload`(warm access) is about 100 units. Therefore, cache, when it's possible.

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographOperator.sol 
      .........................................
      
        // Lines: [202-312]
            // Comment: it's possible to cache `_operatorPods[pod].length` here
              function executeJob(bytes calldata bridgeInRequestPayload) external payable {
                /**
                * @dev derive the payload hash for use in mappings
                */
                bytes32 hash = keccak256(bridgeInRequestPayload);
                /**
                * @dev check that job exists
                */
                require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
                uint256 gasLimit = 0;
                uint256 gasPrice = 0;
                assembly {
                /**
                * @dev extract gasLimit
                */
                gasLimit := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x40))
                /**
                * @dev extract gasPrice
                */
                gasPrice := calldataload(sub(add(bridgeInRequestPayload.offset, bridgeInRequestPayload.length), 0x20))
                }
                /**
                * @dev unpack bitwise packed operator job details
                */
                OperatorJob memory job = getJobDetails(hash);
                /**
                * @dev to prevent replay attacks, remove job from mapping
                */
                delete _operatorJobs[hash];
                /**
                * @dev check that a specific operator was selected for the job
                */
                if (job.operator != address(0)) {
                /**
                * @dev switch pod to index based value
                */
                uint256 pod = job.pod - 1;
                /**
                * @dev check if sender is not the selected primary operator
                */
                if (job.operator != msg.sender) {
                    /**
                    * @dev sender is not selected operator, need to check if allowed to do job
                    */
                    uint256 elapsedTime = block.timestamp - uint256(job.startTimestamp);
                    uint256 timeDifference = elapsedTime / job.blockTimes;
                    /**
                    * @dev validate that initial selected operator time slot is still active
                    */
                    require(timeDifference > 0, "HOLOGRAPH: operator has time");
                    /**
                    * @dev check that the selected missed the time slot due to a gas spike
                    */
                    require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");
                    /**
                    * @dev check if time is within fallback operator slots
                    */
                    if (timeDifference < 6) {
                    uint256 podIndex = uint256(job.fallbackOperators[timeDifference - 1]);
                    /**
                    * @dev do a quick sanity check to make sure operator did not leave from index or is a zero address
                    */
                    if (podIndex > 0 && podIndex < _operatorPods[pod].length) {
                        address fallbackOperator = _operatorPods[pod][podIndex];
                        /**
                        * @dev ensure that sender is currently valid backup operator
                        */
                        require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");
                    }
                    }
                    /**
                    * @dev time to reward the current operator
                    */
                    uint256 amount = _getBaseBondAmount(pod);
                    /**
                    * @dev select operator that failed to do the job, is slashed the pod base fee
                    */
                    _bondedAmounts[job.operator] -= amount;
                    /**
                    * @dev the slashed amount is sent to current operator
                    */
                    _bondedAmounts[msg.sender] += amount;
                    /**
                    * @dev check if slashed operator has enough tokens bonded to stay
                    */
                    if (_bondedAmounts[job.operator] >= amount) {
                    /**
                    * @dev enough bond amount leftover, put operator back in
                    */
                    _operatorPods[pod].push(job.operator);
                    _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
                    _bondedOperators[job.operator] = job.pod;
                    } else {
                    /**
                    * @dev slashed operator does not have enough tokens bonded, return remaining tokens only
                    */
                    uint256 leftovers = _bondedAmounts[job.operator];
                    if (leftovers > 0) {
                        _bondedAmounts[job.operator] = 0;
                        _utilityToken().transfer(job.operator, leftovers);
                    }
                    }
                } else {
                    /**
                    * @dev the selected operator is executing the job
                    */
                    _operatorPods[pod].push(msg.sender);
                    _operatorPodIndex[job.operator] = _operatorPods[pod].length - 1;
                    _bondedOperators[msg.sender] = job.pod;
                }
            }
    ```


## **[G-12] Try `++i/--i` instead of `i++/i--`**<a name="G-12"></a>

### ***Description:***

  - In case of `i++`, the compiler has to to create a temp variable to return `i` (if there's a need) and then `i` gets incremented.  
  - In case of `++i`, the compiler just simply returns already incremented value.

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographOperator.sol 
      ................................
      
        // Lines: [682-684]
            for (uint256 i = 0; i < length; i++) {
                operators[i] = _operatorPods[pod][index + i];
            }
    ```

## **[G-13] Try `unchecked{++i};` instead of `i++;`/`++i;` in loops**<a name="G-13"></a>

### ***Description:***

  - Since [Solidity 0.8.0](https://github.com/ethereum/solidity/releases/tag/v0.8.0), all arithmetic operations revert on over- and underflow by default Therefore, `unchecked` box can be used to prevent all unnecessary checks, if it's no a way to get a reversion.
  
  - There are ~1e80 atoms in the universe, so 2^256 is closed to that number, therefore it's no a way to be overflowed, because of the gas limit as well. 

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographOperator.sol 
      ................................
      
        // Lines: [682-684]
            for (uint256 i = 0; i < length; i++) {
                operators[i] = _operatorPods[pod][index + i];
            }
    ```


## **[G-14] Consider marking onlyOwner functions as payable**<a name="G-14"></a>

### ***Description:***

- This one is a bit questionable, but you can try that out. So, the compiler adds some extra conditions in case of non-payable, but we know that `onlyOwner` modifier will be reverted, if the user invoke following methods.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/enforcer/PA1D.sol 
      ...............................
      
        // Lines: [372-381]
          function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
                require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
                uint256 totalBp;
                for (uint256 i = 0; i < addresses.length; i++) {
                totalBp = totalBp + bps[i];
                }
                require(totalBp == 10000, "PA1D: bps down't equal 10000");
                _setPayoutAddresses(addresses);
                _setPayoutBps(bps);
            }
        ```

## **[G-15] Use the recent version of solidity**<a name="G-15"></a>

### ***Description:***

  - Version >= 0.8.2 provides some automatic inlining features
  - Version >= 0.8.3 comes with better struct packing
  - Version >= 0.8.10 allows to ignore contract existence checks for external calls, if it has some return value


## **[G-16] Possible names optimization**<a name="G-16"></a>

### ***Description:***

  -  The compiler sorts in alphabetic order all ids of methods (state var's generated getters) to retrieve them when invokes happen. Therefore, it's possible to change the name of a frequently-called function to get some leading zeroes in the beginning of a needed selector. 
    - `function execTransaction` -> `0x6a761202`
    - `function execTransaction32785586` -> `0x00000081`
  which will the first in a view-table after sorting, thus saves some gas avoiding extra jums each time.
  - The downside of it is code polution. Ans also, it's worth doing for a contracts with a huge amount of instances.

## **[G-17] Breakdown complex conditions into multiple `if/require` statements**<a name="G-17"></a>

### ***Description:***

  -  It's possible to optimize by simply removing `AND` `OR` opcodes.

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographBridge.sol 
      ..............................
      
        // Lines: [282-285]
            if (gasPrice < type(uint256).max && gasLimit < type(uint256).max) {
                (uint256 hlgFee, ) = _operator().getMessageFee(toChain, gasLimit, gasPrice, bridgeOutPayload);
                fee = hlgFee;
            }
    ```


## **[G-18] Use const values instead of computing `type(uint256).max` every time**<a name="G-18"></a>

### ***Description:***

  - Using already predefined const variables instead of computing them in every single tx might not be sufficient. But I agree, there is a balance between readability and optimization. 

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographBridge.sol 
      ..............................
      
        // Lines: [282-285]
            if (gasPrice < type(uint256).max && gasLimit < type(uint256).max) {
                (uint256 hlgFee, ) = _operator().getMessageFee(toChain, gasLimit, gasPrice, bridgeOutPayload);
                fee = hlgFee;
            }
    ```



## **[G-19] Avoid memory expansions**<a name="G-19"></a>

### ***Description:***

- The memory cost function is linear up to 724 bytes of memory used, at which point additional memory costs substantially more. 
 

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/HolographFactory.sol     
      ...............................
      
        // Lines: [125-125]
            // Comments: if the bytecode of created contract is huge, that would lead in significant amount of gas needed to push into the memory. 
                bytes memory holographerBytecode = type(Holographer).creationCode;
    ```

## Conclusion

  - All of the following contracts within the scope were optimized pretty well. Huge kudos for devs of `Holograph protocol` for taking that much effort on that.