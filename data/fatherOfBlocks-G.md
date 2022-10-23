**HolographBridge**
- L148/163/203/214/224/233/255/270/352 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L204/256/353 - It is less expensive to validate address(_factory()) == holographableContract, than to query another contract in validation. Therefore, it is less expensive to put the second validation first in the require.

- L218 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L380 - When we initialize a variable and we want to set its default value, it is not necessary to set it, since it has that value by default.

- L382/383 - Instead of creating a uint256 hlgFee variable, you can directly destruct the "fee" variable.


**HolographFactory**
- L145 - Instead of requesting bytes as input, two addresses could be directly requested as inputs, in order to simplify the parameter request.

- L144/220/228/250 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**enforcer/Holographer.sol**
- L148/166 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**abstract/ERC721H.sol**
- L117/123/125/147 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**abstract/ERC20H.sol**
- L117/123/125/147 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**HolographOperator**
- L309/350/363/398/1126 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L310/311/781 - When we initialize a variable and we want to set its default value, it is not necessary to set it, since it has that value by default.

- L241/309/350/354/368/415/446/485/591/595/739/756/829/839/857/863/881/889/903/911/915/932 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L337/359/520/729/740/781/782/794/795/862/871/881/882/883 - It is less expensive to do ++i or --i, rather than i++, i-- or i - 1.

- L316/320/363/391/408/640/645/867/871/881/883 - When a storage variable or a struct variable is used more than once, it is less expensive to create a variable in memory and use that variable.


**module/LayerZeroModule.sol**
- L159/235 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**enforcer/PA1D.sol**
- L159/174/190/390/411/416/435/439/460/472/477 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L307/323/340/356/394/414/437/454/474 - It is less expensive to do ++i or --i, rather than i++, i-- or i - 1.

- L307/323/340/356/394/414/437/454/474 - When we initialize a variable and we want to set its default value, it is not necessary to set it, since it has that value by default.

- L432/437/454/472/474 - When a storage variable or a struct variable is used more than once, it is less expensive to create a variable in memory and use that variable.

- L390/391 - When we first validate a subtraction operation to be > 1000, we know that it will not underflow, therefore the operation in L391 can be unchecked.


**enforcer/HolographERC721.sol**
- L212/224/239/258/263/323/370/371/373/378/388/391/395/404/408/419/420/421/458/460/464/473/484/486/491/513/622/624/628/639/689/700/729/757/759/762/767/815/816/817/818/869/570/906 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L357/716 - It is less expensive to do ++i or --i, rather than i++, i-- or i - 1.

- L357/716 - When we initialize a variable and we want to set its default value, it is not necessary to set it, since it has that value by default.

- L815 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L894/895 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.


**enforcer/HolographERC20.sol**
- L204/219/241/328/332/339/343/349/354/358/365/371/375/387/400/427/430/434/445/447/450/455/469/482/484/488/502/505/507/529/536/539/541/582/586/599/606/610/620/621/627/629/645/684/695/696/698 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.
