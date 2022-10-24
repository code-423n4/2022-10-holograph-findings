https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol
Line 148: change *require* to *revert* with custom Error to save gas.

Line 163: change *require* to *revert* with custom Error to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol
Line 144: change *require* to *revert* with custom Error to save gas.

Line 220: change *require* to *revert* with custom Error to save gas.

Line 228: change *require* to *revert* with custom Error to save gas.

Line 250: change *require* to *revert* with custom Error to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol
Line 682: change *i++* to *unchecked{++i}* to save gas.
Line 772: change *i++* to *unchecked{++i}* to save gas.

Line 241: change *require* to *revert* with custom Error to save gas.

Line 309: change *require* to *revert* with custom Error to save gas.

Line 350: change *require* to *revert* with custom Error to save gas.

Line 354: change *require* to *revert* with custom Error to save gas.

Line 368: change *require* to *revert* with custom Error to save gas.

Line 415: change *require* to *revert* with custom Error to save gas.

Line 446: change *require* to *revert* with custom Error to save gas.

Line 485: change *require* to *revert* with custom Error to save gas.

Line 591: change *require* to *revert* with custom Error to save gas.

Line 595: change *require* to *revert* with custom Error to save gas.

Line 728: change *require* to *revert* with custom Error to save gas.

Line 739: change *require* to *revert* with custom Error to save gas.

Line 756: change *require* to *revert* with custom Error to save gas.

Line 829: change *require* to *revert* with custom Error to save gas.

Line 839: change *require* to *revert* with custom Error to save gas.

Line 857: change *require* to *revert* with custom Error to save gas.

Line 863: change *require* to *revert* with custom Error to save gas.

Line 881: change *require* to *revert* with custom Error to save gas.

Line 889: change *require* to *revert* with custom Error to save gas.

Line 903: change *require* to *revert* with custom Error to save gas.

Line 911: change *require* to *revert* with custom Error to save gas.

Line 915: change *require* to *revert* with custom Error to save gas.

Line 932: change *require* to *revert* with custom Error to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographRegistry.sol
Line 169: change *require* to *revert* with custom Error to save gas.

Line 198: change *require* to *revert* with custom Error to save gas.

Line 202: change *require* to *revert* with custom Error to save gas.

Line 203: change *require* to *revert* with custom Error to save gas.

Line 219: change *require* to *revert* with custom Error to save gas.

Line 282: change *require* to *revert* with custom Error to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographTreasury.sol
Line 154: change *require* to *revert* with custom Error to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L143
change 
        bytes memory initPayload
to
      bytes calldata initPayload
to save gas

we are converting the following code to save gas, https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographRegistry.sol#L175-L177

the new code is
```
uint len = reservedTypes.length;
for (uint256 i; i < len ; unchecked{++i}) {
      _reservedTypes[reservedTypes[i]] = true;
 }
```
the gas saving is due to avoid unnecessary initialization of i, caching the result of reserveredTypes.length and the unchecked directive with replacment of ++i instead of i++.

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographGenesis.sol#L130
Use custom error if-revert statement instead of require-error-message to save gas. This suggestions go too all other
require-error-message statements. 


https://code4rena.com/contests/2022-10-holograph-contest/submit
change the following line 
require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");
to
if (_operatorJobs[hash] <> 0 ) revert HOLOGRAPHInvalid job();

That is, "<>" is cheapter than ">" and custom error is cheaper than string. 

similar suggestions to other require statements like line 350 and 354.

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L446
just change the function to *private* will do the same thing and save gas







