

## G01 Below state variables are only set in the ```init()``` function. They are also set in the ```resetOperator()``` function, but that function is commented to be not part of the final code.
Therefore they can be made constant and given their value at declaration.

158:   uint256 private _blockTime;   //@audit gas can be made constant. Only set in init() except resetOperator() which is not part of final code
168:   uint256 private _podMultiplier;   //@audit gas can be made constant. Only set in init() except resetOperator() which is not part of final code
173:   uint256 private _operatorThreshold;   //@audit gas can be made constant. Only set in init() except resetOperator() which is not part of final code
178:   uint256 private _operatorThresholdStep;   //@audit gas can be made constant. Only set in init() except resetOperator() which is not part of final code
183:   uint256 private _operatorThresholdDivisor;   //@audit gas can be made constant. Only set in init() except resetOperator() which is not part of final code


