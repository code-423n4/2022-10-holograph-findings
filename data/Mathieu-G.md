# Unchecked Pattern In Loop
The `unchecked` pattern allows a reduction of gas consumption when incrementing the iteration integer. We can benefit from this pattern in the `PA1D.sol` contract in the following functions: `_setPayoutAddresses`, `_setPayoutBps`, `_payoutEth`, `_payoutToken`, `_payoutTokens` and `configurePayouts`.

Al of the loops are bounded by an array length, so there is no risk of an overflow error to throw.

### _setPayoutAddresses
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323:L329

    for (uint256 i = 0; i < length;) {
      slot = keccak256(abi.encodePacked(i, slot));
      value = addresses[i];
      assembly {
        sstore(slot, value)
      }
      unchecked { ++i; }
    }

### _setPayoutBps
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356:L362

    for (uint256 i = 0; i < length;) {
      slot = keccak256(abi.encodePacked(i, slot));
      value = bps[i];
      assembly {
        sstore(slot, value)
      }
      unchecked { ++i; }
    }

### _payoutEth
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394:L398

    for (uint256 i = 0; i < length;) {
      sending = ((bps[i] * balance) / 10000);
      addresses[i].transfer(sending);
      // sent = sent + sending;
      unchecked { ++i; }
    }

### _payoutToken
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414:L418

    for (uint256 i = 0; i < length;) {
      sending = ((bps[i] * balance) / 10000);
      require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
      // sent = sent + sending;
      unchecked { ++i; }
    }

### _payoutTokens
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432:L442

    for (uint256 t = 0; t < tokenAddresses.length;) {
      erc20 = ERC20(tokenAddresses[t]);
      balance = erc20.balanceOf(address(this));
      require(balance > 10000, "PA1D: Not enough tokens to transfer");
      // uint256 sent;
      for (uint256 i = 0; i < addresses.length;) {
        sending = ((bps[i] * balance) / 10000);
        require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
        // sent = sent + sending;
        unchecked { ++i; }
      }

      unchecked { ++t; }
    }

### configurePayouts
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474:L476

    for (uint256 i = 0; i < addresses.length;) {
      totalBp = totalBp + bps[i];
      unchecked { ++i; }
    }