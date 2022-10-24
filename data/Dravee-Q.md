## `HolographFactory.deployHolographableContract()`: There's no check that `signer != address(0)`

`signer == address(0)` is a valid input and would pass the require statement if `ecrecover()` fails, as it returns `address(0)` when it fails:

```solidity
File: HolographFactory.sol
192:   function deployHolographableContract(
193:     DeploymentConfig memory config,
194:     Verification memory signature,
195:     address signer //@audit can be address(0)
196:   ) external {
...
206:     bytes32 hash = keccak256(
207:       abi.encodePacked(
208:         config.contractType,
209:         config.chainType,
210:         config.salt,
211:         keccak256(config.byteCode),
212:         keccak256(config.initCode),
213:         signer //@audit contains the signer which can be address(0)
214:       )
215:     );
...
220:     require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature"); //@audit-issue this is valid wih signer == address(0) and any signature as the hash shouldn't match anything
...
320:   function _verifySigner(
321:     bytes32 r,
322:     bytes32 s,
323:     uint8 v,
324:     bytes32 hash,
325:     address signer
326:   ) private pure returns (bool) {
...
333:     return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer || //@audit returns true with signer == address(0)
334:       ecrecover(hash, v, r, s) == signer);
335:   }
```

Consider adding a check that `signer != address(0)`

## Non-standard extract of `msg.sender` via `calldata`

The usual syntax is `shr(96,calldataload(sub(calldatasize(),20)))`, meaning the last bytes of msg.data give the sender address.

However, in this project, the syntax is the following:

```solidity
contracts/abstract/ERC20H.sol:
  156    /**
  157     * @dev The Holographer passes original msg.sender via calldata. This function extracts it.
  158     */
  159:   function msgSender() internal pure returns (address sender) {
  160      assembly {
  161        sender := calldataload(sub(calldatasize(), 0x20))
  162      }
  163    }
```

```solidity
contracts/abstract/ERC721H.sol:
  156    /**
  157     * @dev The Holographer passes original msg.sender via calldata. This function extracts it.
  158     */
  159:   function msgSender() internal pure returns (address sender) {
  160      assembly {
  161        sender := calldataload(sub(calldatasize(), 0x20))
  162      }
  163    }
```

Unless it's certain that the only calldata will always only be the sender, consider using the full syntax.

Sources :

- <https://github.com/OpenZeppelin/openzeppelin-contracts/blob/2dc086563f2e51620ebc43d2237fc10ef201c4e6/contracts/metatx/ERC2771Context.sol#L29>

```solidity
    function _msgSender() internal view virtual override returns (address sender) {
        if (isTrustedForwarder(msg.sender)) {
            // The assembly code is more direct than the Solidity version using `abi.decode`.
            /// @solidity memory-safe-assembly
            assembly {
                sender := shr(96, calldataload(sub(calldatasize(), 20)))
            }
        } else {
            return super._msgSender();
        }
    }
```

- <https://eips.ethereum.org/EIPS/eip-2771#:~:text=signer%20%3A%3D%20shr(96%2Ccalldataload(sub(calldatasize>

```solidity
    function _msgSender() internal view returns (address payable signer) {
        signer = msg.sender;
        if (msg.data.length>=20 && isTrustedForwarder(signer)) {
            assembly {
                signer := shr(96,calldataload(sub(calldatasize(),20)))
            }
        }    
    }
```
