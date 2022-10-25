# [01] The increment in for loop post condition can be made unchecked to save gas

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781

It can save 30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked).

# [02] x += y costs more gas than x = x + y for state variables

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382

Using the addition operator instead of plus-equals saves 113 [gas](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8).

# [03] Cache state variable reads

Caching of a state variable replace each Gwarmaccess (100 gas) with a cheaper stack read.

For example, on the following instances, `_bondedOperators[operator]` should be cached.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L903

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L928
