### L-01 MISSING ZERO-ADDRESS CHECK IN THE SETTER FUNCTIONS

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

```
function setFactory(address factory) external onlyAdmin
function setHolograph(address holograph) external onlyAdmin 
function setOperator(address operator) external onlyAdmin
function setBridge(address bridge) external onlyAdmin
function setLZEndpoint(address lZEndpoint) external onlyAdmin
```

### L-02 MISSING EVENTS THROUGHOUT THE CONTRACT

All the important state changing functions should emit an event. Here, none of the importnat function emits anything.

### L-03 SAFEMINT SHOULD BE USED THROUGHOUT INSTEAD OF MINT FOR ERC721

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
406: _mint(to, tokenId);
514: _mint(to, token);
```

### L-04 PREFER SAFETRANSFER AND SAFETRANSFERFROM THROUGHOUT THE CONTRACT

Throughout the in-scope contract there are several occurences of `transferFom` which should be replaced with `safeTransferFrom` for both ERC20 as well as ERC 721

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```
589: transferFrom(msg.sender, to, tokenId, "");
604: transferFrom(from, to, tokenId, "");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
839: _utilityToken().transferFrom(msg.sender, address(this), amount)
889: _utilityToken().transferFrom(msg.sender, address(this), amount)
```


### L-05 ECRECOVER NOT CHECKED FOR ZERO RESULT

The solidity function `ecrecover` is used, however the error result of 0 is not checked for. See documentation: [https://docs.soliditylang.org/en/v0.8.9/units-and-global-variables.html?highlight=ecrecover#mathematical-and-cryptographic-functions](https://docs.soliditylang.org/en/v0.8.9/units-and-global-variables.html?highlight=ecrecover#mathematical-and-cryptographic-functions) “recover the address associated with the public key from elliptic curve signature or return zero on error. ”

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```
333: return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
      ecrecover(hash, v, r, s) == signer);
```


### N-01 Open TODOs

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```
701: // TODO: move the bit-shifting around to have it be sequential
```

### N-02 NATSPEC IS INCOMPLETE

@params are not well defined in the natspec throughout the in-scope contracts.
One such instance is:

```
function _popOperator(uint256 pod, uint256 operatorIndex) private { 
    /**
     * @dev only pop the operator if it's not a zero address
     */
    if (operatorIndex > 0) {
      unchecked {
        address operator = _operatorPods[pod][operatorIndex];
        /**
         * @dev mark operator as no longer bonded
         */
        _bondedOperators[operator] = 0;
        /**
         * @dev remove pod reference for operator
         */
        _operatorPodIndex[operator] = 0;
        uint256 lastIndex = _operatorPods[pod].length - 1;
        if (lastIndex != operatorIndex) {
          /**
           * @dev if operator is not last index, move last index to operator's current index
           */
          _operatorPods[pod][operatorIndex] = _operatorPods[pod][lastIndex];
          _operatorPodIndex[_operatorPods[pod][operatorIndex]] = operatorIndex;
        }
        /**
         * @dev delete last index
         */
        delete _operatorPods[pod][lastIndex];
        /**
         * @dev shorten array length
         */
        _operatorPods[pod].pop();
      }
    }
  }

```


### N-03 EVENT IS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fields

```
event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```

### N-04 CONSIDER MAKING CONTRACT PAUSABLE TO HAVE SOME PROTECTION AGAINST ONGOING EXPLOITS

```
contract HolographOperator is Admin, Initializable, HolographOperatorInterface
```
