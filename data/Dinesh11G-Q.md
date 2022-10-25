### Unsafe ERC20 Operation(s)

#### Impact
Issue Information: ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

To circumvent ERC20's approve functions race-condition vulnerability use OpenZeppelin's SafeERC20 library's safe{Increase|Decrease}Allowance functions.

In case the vulnerability is of no danger for your implementation, provide enough documentation explaining the reasonings.

Example
🤦 Bad:

IERC20(token).transferFrom(msg.sender, address(this), amount);
🚀 Good (using OpenZeppelin's SafeERC20):

import {SafeERC20} from "openzeppelin/token/utils/SafeERC20.sol";

// ...

IERC20(token).safeTransferFrom(msg.sender, address(this), amount);
🚀 Good (using require):

bool success = IERC20(token).transferFrom(msg.sender, address(this), amount);
require(success, "ERC20 transfer failed");

#### Findings:
```
2022-10-holograph/contracts/HolographOperator.sol::400 => _utilityToken().transfer(job.operator, leftovers);
2022-10-holograph/contracts/HolographOperator.sol::596 => payable(hToken).transfer(hlgFee);
2022-10-holograph/contracts/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/contracts/enforcer/PA1D.sol::396 => addresses[i].transfer(sending);
2022-10-holograph/contracts/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/contracts/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/contracts/faucet/Faucet.sol::141 => token.transfer(msg.sender, faucetDripAmount);
2022-10-holograph/contracts/faucet/Faucet.sol::152 => token.transfer(_address, faucetDripAmount);
2022-10-holograph/contracts/faucet/Faucet.sol::157 => token.transfer(_address, _amountWei);
2022-10-holograph/contracts/faucet/Faucet.sol::162 => token.transfer(_receiver, token.balanceOf(address(this)));
2022-10-holograph/contracts/faucet/Faucet.sol::168 => token.transfer(_receiver, _amountWei);
2022-10-holograph/contracts/mock/ERC20Mock.sol::258 => ERC20(token).transfer(to, amount);
2022-10-holograph/contracts/token/hToken.sol::206 => recipient.transfer(amount);
2022-10-holograph/contracts/token/hToken.sol::224 => require(erc20.transferFrom(sender, address(this), amount), "hToken: ERC20 transfer failed");
2022-10-holograph/contracts/token/hToken.sol::263 => erc20.transfer(recipient, adjustedAmount);
2022-10-holograph/src/HolographOperator.sol::301 => _utilityToken().transfer(job.operator, leftovers);
2022-10-holograph/src/HolographOperator.sol::497 => payable(hToken).transfer(hlgFee);
2022-10-holograph/src/HolographOperator.sol::740 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/src/HolographOperator.sol::790 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/src/HolographOperator.sol::833 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
2022-10-holograph/src/enforcer/PA1D.sol::297 => addresses[i].transfer(sending);
2022-10-holograph/src/enforcer/PA1D.sol::317 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/src/enforcer/PA1D.sol::340 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
2022-10-holograph/src/faucet/Faucet.sol::42 => token.transfer(msg.sender, faucetDripAmount);
2022-10-holograph/src/faucet/Faucet.sol::53 => token.transfer(_address, faucetDripAmount);
2022-10-holograph/src/faucet/Faucet.sol::58 => token.transfer(_address, _amountWei);
2022-10-holograph/src/faucet/Faucet.sol::63 => token.transfer(_receiver, token.balanceOf(address(this)));
2022-10-holograph/src/faucet/Faucet.sol::69 => token.transfer(_receiver, _amountWei);
2022-10-holograph/src/mock/ERC20Mock.sol::159 => ERC20(token).transfer(to, amount);
2022-10-holograph/src/token/hToken.sol::107 => recipient.transfer(amount);
2022-10-holograph/src/token/hToken.sol::125 => require(erc20.transferFrom(sender, address(this), amount), "hToken: ERC20 transfer failed");
2022-10-holograph/src/token/hToken.sol::164 => erc20.transfer(recipient, adjustedAmount);
```
#### Tools used
Manual