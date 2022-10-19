 Solidity has a code size limit of 24 Kb. There is a simple trick to reduce the size of your code. Wrap code inside function modifier into a private / internal function.

***** THIS IS AN EXAMPLE FROM HolographBridge.sol CONTRACT *****

BEFORE:

 modifier onlyOperator() {
    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
    _;
  }

AFTER:

function _onlyOperator() private view {
    require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
  }
 
  modifier onlyOperator() {
    _onlyOperator;
    _;
  }

GAS OF DEPLOYMENT BEFORE: 2534115.

GAS OF DEPLOYMENT AFTER: 2485804. 

GAS SAVED: 48311.

This Trick will reduce gas of deployment, but the gas of execution may increase from calling a function.

 ***** This could be used for all contracts and will save tons of gas *****
