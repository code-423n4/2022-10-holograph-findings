## Init function can be called by any user

Contract:
https://github.com/holographxyz/holograph-protocol/blob/c4_audit/contracts/abstract/ERC20H.sol#L140
https://github.com/holographxyz/holograph-protocol/blob/c4_audit/contracts/abstract/ERC721H.sol#L140

Issue:
It was observed that init function can be called by any user bricking the contract owner. This will force the contract owner to redeploy this contract causing wastage of gas

Recommendation:
Only the owner (decided via constructor) should be allowed to call init function.
This need to be fixed for almost all contracts