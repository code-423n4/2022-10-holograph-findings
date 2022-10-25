 --- 

 ## `10e18` is more gas efficient than `10**18` 
 ### Files Found: 
 - File: contracts/HolographOperator.sol - line 256 

 ### Mitigation: 
 Use Scientific Notation for easier readability 
 --- 

 --- 
 ## `Abi.Encode()` is less efficient than `abi.Encodepacked()` 
 ### Files Found: 
 There are 1 instances of this issue. 
 - File: contracts/HolographFactory.sol - line 252 
 
 ### Mitigation: 
 Use Abi.encode packed where necessary 
 --- 

 --- 

 ## Unused/empty `receive()/fallback()` function 
 ### Files Found: 

- File: contracts/HolographOperator.sol - line 1209 
 
 ### Mitigation: 
 Either implement, emit or remove the empty callback 
 
--- 

