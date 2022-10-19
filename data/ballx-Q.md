
### Unsafe ERC20 Operation(s)

#### Impact
Issue Information: [L001](https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l001---unsafe-erc20-operations)

#### Findings:
```
contracts/HolographOperator.sol::400 => _utilityToken().transfer(job.operator, leftovers);
contracts/HolographOperator.sol::596 => payable(hToken).transfer(hlgFee);
contracts/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
contracts/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
contracts/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
contracts/enforcer/PA1D.sol::396 => addresses[i].transfer(sending);
contracts/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
contracts/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
contracts/faucet/Faucet.sol::141 => token.transfer(msg.sender, faucetDripAmount);
contracts/faucet/Faucet.sol::152 => token.transfer(_address, faucetDripAmount);
contracts/faucet/Faucet.sol::157 => token.transfer(_address, _amountWei);
contracts/faucet/Faucet.sol::162 => token.transfer(_receiver, token.balanceOf(address(this)));
contracts/faucet/Faucet.sol::168 => token.transfer(_receiver, _amountWei);
contracts/mock/ERC20Mock.sol::258 => ERC20(token).transfer(to, amount);
contracts/token/hToken.sol::206 => recipient.transfer(amount);
contracts/token/hToken.sol::224 => require(erc20.transferFrom(sender, address(this), amount), "hToken: ERC20 transfer failed");
contracts/token/hToken.sol::263 => erc20.transfer(recipient, adjustedAmount);
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

