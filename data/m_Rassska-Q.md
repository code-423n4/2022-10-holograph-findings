# Table of contents

- **[[L-01] `.send()` is not recommended to use due to the limits of 2300 gas per call](L-01)**
- **[[L-02] `init()` could be front-runned](L-02)**
- **[[L-03] `approve()` could be front-runned](L-03)**
- **[[L-04] `_isContract()` is not safe to use and could be bypassed in some cases](L-04)**
- **[[L-05] `ERC721.transferFrom()` doesn't check for the case, where the receiver is a smart-contract without `ERC721.onCheckedReceived()` method](L-05)**


## **[L-01] `.send()` is not recommended to use due to the limits of 2300 gas per call**<a name="L-01"></a>

### ***Description:***
  - The paper from Consensys Diligence represents the whole point in a clear way. Make sure to check out. [Click](https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/)

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: src/HolographOperator.sol 
      .....................................
      
        // Lines: [541-548]
            messagingModule.send{value: msg.value - hlgFee}(
              gasLimit,
              gasPrice,
              toChain,
              msgSender,
              msg.value - hlgFee,
              encodedData
            );

### ***Recommendations:***
  - It's recommended to use standard `.call{}()`.

## **[L-02] `init()` could be front-runned**<a name="L-02"></a>

### ***Description:***
  - After deploying the contract, deployer invokes `init()`, but it could be front-run by mev bots or anyone to initialize the bridge. This will lead to redeploying contracts. 

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: src/HolographBridge.sol 
      .....................................
      
        // Lines: [63-78]
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

### ***Recommendations:***
  - Should be invoked internally. Otherwise, some modifiers to prevent anyone from invoking are fine as well for a short term. 

## **[L-03] `approve()` could be front-runned**<a name="L-03"></a>

### ***Description:***
  - The scenario described here: https://ethereum.stackexchange.com/questions/93717/how-can-we-stop-front-running-for-approve 

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: src/enforcer/HolographERC20.sol 
      .....................................
      
        // Lines: [516-525]
          function _approve(
            address account,
            address spender,
            uint256 amount
          ) private {
            require(account != address(0), "ERC20: account is zero address");
            require(spender != address(0), "ERC20: spender is zero address");
            _allowances[account][spender] = amount;
            emit Approval(account, spender, amount);
          }

### ***Recommendations:***
  - It's possible to use _safeApprove. See [SafeERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/utils/SafeERC20.sol) from Oz. 

## **[L-04] `_isContract()` is not safe to use and could be bypassed in some cases**<a name="L-04"></a>

### ***Description:***
  - Relaying on the bytecode size to identify whether the source is EOA or smart-contract is not decent, since calls might come directly from constructor of newly deployed smart-contract.  

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: src/HolographFactory.sol 
      .....................................
      
        // Lines: [210-216]
          function _isContract(address contractAddress) private view returns (bool) {
            bytes32 codehash;
            assembly {
              codehash := extcodehash(contractAddress)
            }
            return (codehash != 0x0 && codehash != precomputekeccak256(""));
          }
    ```
## **[L-05] `ERC721.transferFrom()` doesn't check for the case, where the receiver is a smart-contract without `ERC721.onCheckedReceived()` method **<a name="L-05"></a>


### ***Description:***
  - Therefore, the tokens sent to such contracts are lost forever 

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: src/enforcer/HolographERC20.sol 
      .....................................
      
        // Lines: [517-531]
          function transferFrom(
            address from,
            address to,
            uint256 tokenId,
            bytes memory data
          ) public payable {
            require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");
            if (_isEventRegistered(HolographERC721Event.beforeTransfer)) {
              require(SourceERC721().beforeTransfer(from, to, tokenId, data));
            }
            _transferFrom(from, to, tokenId);
            if (_isEventRegistered(HolographERC721Event.afterTransfer)) {
              require(SourceERC721().afterTransfer(from, to, tokenId, data));
            }
          }

### ***Recommendations:***
  - It's possible to use _safeApprove. See [SafeERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/utils/SafeERC20.sol) from Oz. 
