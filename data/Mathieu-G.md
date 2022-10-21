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


# Avoid Redundant Variable Casting


In the `PA1D.sol` contract we can see this pattern in the following functions: `_payoutToken` and `_payoutTokens` where the function's argument is an `address` that is manually re-casted to a `ERC20` in the function whereas it can be directly casted as `ERC20` in the argument definition. 

The recommendation is to remove this re-casting to save some gas, plus we are removing a useless variable by applying this.

### _payoutToken
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L405:L419

    function _payoutToken(ERC20 erc20token) private {
      address payable[] memory addresses = _getPayoutAddresses();
      uint256[] memory bps = _getPayoutBps();
      uint256 length = addresses.length;
      uint256 balance = erc20token.balanceOf(address(this));
      require(balance > 10000, "PA1D: Not enough tokens to transfer");
      uint256 sending;
      //uint256 sent;
      for (uint256 i = 0; i < length; i++) {
        sending = ((bps[i] * balance) / 10000);
        require(erc20token.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
        // sent = sent + sending;
      }
    }

### _payoutTokens
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L426:L443

    function _payoutTokens(ERC20[] memory erc20tokens) private {
      address payable[] memory addresses = _getPayoutAddresses();
      uint256[] memory bps = _getPayoutBps();
      ERC20 erc20;
      uint256 balance;
      uint256 sending;
      for (uint256 t = 0; t < erc20tokens.length; t++) {
        erc20 = erc20tokens[t];
        balance = erc20.balanceOf(address(this));
        require(balance > 10000, "PA1D: Not enough tokens to transfer");
        // uint256 sent;
        for (uint256 i = 0; i < addresses.length; i++) {
          sending = ((bps[i] * balance) / 10000);
          require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
          // sent = sent + sending;
        }
      }
    }

#### Additional Changes
For this new declarations to success, we must also changes the argument of these functions: `getTokenPayout` and `getTokensPayout`

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L507

    function getTokenPayout(ERC20 tokenAddress)

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L517

    function getTokensPayout(ERC20[] memory tokenAddresses)

The `PA1DInterface.sol` must be changed accordingly too:https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/interface/PA1DInterface.sol#L115:L117

    function getTokenPayout(ERC20 tokenAddress) external;

    function getTokensPayout(ERC20[] memory tokenAddresses) external;

And with the proper import at the beginning of the file:

    import "./ERC20.sol";