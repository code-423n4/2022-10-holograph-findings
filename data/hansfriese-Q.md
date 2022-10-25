### [L-01] Use of Solidity version 0.8.13 which has known issues
- [All contracts use this version](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L102)

The version 0.8.13 has some issues like [Vulnerability related to ABI-encoding](https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/) and [Vulnerability related to 'Optimizer Bug Regarding Memory Side Effects of Inline Assembly'](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/).

Many contracts contain the encoding logics and there are many assemblies in the protocol also.

Recommend using the latest solidity version like 0.8.17.

### [L-02] Wrong gas limit
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L388

```solidity
    // accommodating the 2300 gas stipend
    // adding 1x for each item in array to accomodate rounding errors
    uint256 gasCost = (23300 * length) + length;
```

Currently, it distrubute smaller eth than expected and we should change `23300` to `2300` like the comment.

### [L-03] We should check if `bps[i] > 0` in `configurePayouts()`
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L474-L476

```solidity
  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
    uint256 totalBp;
    for (uint256 i = 0; i < addresses.length; i++) {
      totalBp = totalBp + bps[i];
    }
    require(totalBp == 10000, "PA1D: bps down't equal 10000"); //@audit-N5
    _setPayoutAddresses(addresses);
    _setPayoutBps(bps);
  }
```

If `bps[i] == 0`, it will just increase the length and produce unnecessary transfers in [_payoutEth()](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L382) and [_payoutToken()](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L405).

Also, with the [revert-on-zero-value-tranfer](https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers) tokens, `_payoutToken()` will revert [here](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L416).

```solidity
  function _payoutToken(address tokenAddress) private {
    address payable[] memory addresses = _getPayoutAddresses();
    uint256[] memory bps = _getPayoutBps();
    uint256 length = addresses.length;
    ERC20 erc20 = ERC20(tokenAddress);
    uint256 balance = erc20.balanceOf(address(this));
    require(balance > 10000, "PA1D: Not enough tokens to transfer");
    uint256 sending;
    //uint256 sent;
    for (uint256 i = 0; i < length; i++) {
      sending = ((bps[i] * balance) / 10000);
      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
      // sent = sent + sending;
    }
  }
```

### [L-04] There should be an upper limit of `addresses.length` in `configurePayouts()`
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L471-L480

```solidity
  function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {
    require(addresses.length == bps.length, "PA1D: missmatched array lenghts");
    uint256 totalBp;
    for (uint256 i = 0; i < addresses.length; i++) {
      totalBp = totalBp + bps[i];
    }
    require(totalBp == 10000, "PA1D: bps down't equal 10000"); //@audit-N5
    _setPayoutAddresses(addresses);
    _setPayoutBps(bps);
  }
```

Currently, `addresses.length` can be 10000 at most(when we check if bps > 0) and [_payoutEth()](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L382) and [_payoutToken()](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L405) will revert because of block gas limit.

We should check an upper limit like 50.

### [L-05] Check address(0) for `addresses` in `_setPayoutAddresses()`
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L316-L330

```solidity
  function _setPayoutAddresses(address payable[] memory addresses) private {
    bytes32 slot = _payoutAddressesSlot;
    uint256 length = addresses.length;
    assembly {
      sstore(slot, length)
    }
    address payable value;
    for (uint256 i = 0; i < length; i++) {
      slot = keccak256(abi.encodePacked(i, slot));
      value = addresses[i];
      assembly {
        sstore(slot, value)
      }
    }
  }
```

Currently, there is no validation of address(0) and the tokens might be lost in [_payoutEth()](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L382) and [_payoutToken()](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L405).

### [N-01] Typo
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L477

```solidity
require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

Change `down't` to `doesn't`.
