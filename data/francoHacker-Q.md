*[G-01]Cheaper For Loops - 25 to 80 gas per instance 

You can get cheaper for loops (at least 25 gas, however can be up to 80 gas under certain conditions), by rewriting:

        for (uint256 i = 0; i < length; /** NOTE: Removed i++ **/ ) {
                // Do the thing
                // Unchecked pre-increment is cheapest
                unchecked { ++i; }
        }      

INSTANCES: 
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781-L871
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307-L323-L340-L356-L394-L414-L432-L437-L454-L474
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357-L716
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564



*[G-02]MORE GAS IS USED WHEN THE VARIABLE IS INITIALIZED TO 0 IN A FOR LOOP

It is more efficient '''for (uint i; i < xxx; ++i)''' than if we initialize the variable to 0 '''for (uint i = 0; i < xxx; ++i)''' It is recommended not to initialize the variable, just declare it, since it is more efficient in gas consumption.

INSTANCES:
ALL FOR LOOPS IN CONTEST


*[G-03] '''> 0''' IS LESS EFFICIENT THAN '''!= 0''' FOR UNSIGNED INTEGERS

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

INSTANCES:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309-L350
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815



*[G‑04] EMPTY BLOCKS SHOULD BE REMOVED OR EMIT SOMETHING

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}}). Empty receive()/fallback() payable functions that are not used, can be removed to save deployment gas.

INSTANCES:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251
