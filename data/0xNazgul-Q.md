## [NAZ-L] Value Range Validity for Setters
**Severity** Low
**Context**: [`HolographOperator.sol#L278`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L278), [`LayerZeroModule.sol#L441`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441), [`LayerZeroModule.sol#L470`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470)

**Description**:
These functions doesn't have any checks to ensure that the variables being set is within some kind of value range.

**Recommendation**:
Each variable input parameter updated should have it's own value range checks to ensure their validity.


## [NAZ-L] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`HolographBridge.sol#L452`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452), [`HolographBridge.sol#L472`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472), [`HolographBridge.sol#L502`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502), [`HolographBridge.sol#L522`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522), [`HolographOperator.sol#L278`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L278), [`HolographOperator.sol#L949`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949), [`HolographOperator.sol#L969`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969), [`HolographOperator.sol#L989`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989), [`HolographOperator.sol#L1009`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009), [`HolographOperator.sol#L1029`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029), [`HolographOperator.sol#L1049`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049), [`HolographFactory.sol#L280`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280), [`HolographFactory.sol#L300`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300), [`LayerZeroModule.sol#L320`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320), [`LayerZeroModule.sol#L340`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340), [`LayerZeroModule.sol#L360`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360), [`LayerZeroModule.sol#L380`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380), [`LayerZeroModule.sol#L441`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441), [`LayerZeroModule.sol#L470`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L] Missing Time locks
**Severity**: Low 
**Context**: [`HolographBridge.sol#L452`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452), [`HolographBridge.sol#L472`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472), [`HolographBridge.sol#L502`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502), [`HolographBridge.sol#L522`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522), [`HolographOperator.sol#L278`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L278), [`HolographOperator.sol#L949`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949), [`HolographOperator.sol#L969`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969), [`HolographOperator.sol#L989`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989), [`HolographOperator.sol#L1009`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009), [`HolographOperator.sol#L1029`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029), [`HolographOperator.sol#L1049`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049), [`HolographFactory.sol#L280`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280), [`HolographFactory.sol#L300`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300), [`LayerZeroModule.sol#L320`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320), [`LayerZeroModule.sol#L340`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340), [`LayerZeroModule.sol#L360`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360), [`LayerZeroModule.sol#L380`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380), [`LayerZeroModule.sol#L441`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441), [`LayerZeroModule.sol#L470`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470)

**Description**:
When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert: (1) users and give them a chance to engage/exit protocol if they are not agreeable to the changes (2) team in case of compromised owner(s) and give them a chance to perform incident response.

**Recommendation**:
Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorised owners have no time for any planned incident response.



## [NAZ-L] Missing Zero-address Validation
**Severity**: Low
**Context**: [`HolographBridge.sol#L452`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452), [`HolographBridge.sol#L472`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472), [`HolographBridge.sol#L502`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502), [`HolographBridge.sol#L522`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522), [`HolographOperator.sol#L949`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949), [`HolographOperator.sol#L969`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969), [`HolographOperator.sol#L989`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989), [`HolographOperator.sol#L1009`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009), [`HolographOperator.sol#L1029`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029), [`HolographOperator.sol#L1049`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049), [`HolographFactory.sol#L280`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280), [`HolographFactory.sol#L300`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300), [`LayerZeroModule.sol#L320`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320), [`LayerZeroModule.sol#L340`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340), [`LayerZeroModule.sol#L360`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360), [`LayerZeroModule.sol#L380`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L] Lack of Event Emission For Critical Functions
**Severity**: Low
**Context**: [`HolographBridge.sol#L452`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452), [`HolographBridge.sol#L472`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472), [`HolographBridge.sol#L502`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502), [`HolographBridge.sol#L522`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522), [`HolographOperator.sol#L278`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L278), [`HolographOperator.sol#L949`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949), [`HolographOperator.sol#L969`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969), [`HolographOperator.sol#L989`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989), [`HolographOperator.sol#L1009`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009), [`HolographOperator.sol#L1029`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029), [`HolographOperator.sol#L1049`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049), [`HolographFactory.sol#L280`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280), [`HolographFactory.sol#L300`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300), [`LayerZeroModule.sol#L320`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320), [`LayerZeroModule.sol#L340`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340), [`LayerZeroModule.sol#L360`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360), [`LayerZeroModule.sol#L380`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380), [`LayerZeroModule.sol#L441`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441), [`LayerZeroModule.sol#L470`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470)

**Description**:
Several functions update critical parameters that are missing event emission. These should be performed to ensure tracking of changes of such critical parameters.

**Recommendation**:
Consider adding events to functions that change critical parameters.


## [NAZ-L] `receive()` Function Should Emit An Event
**Severity**: Low
**Context**: [`HolographOperator.sol#L1209`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209), [`Holographer.sol#L223`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223), [`HolographERC721.sol#L962`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962), [`HolographERC20.sol#L251`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251), [`ERC721H.sol#L212`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212), [`ERC20H.sol#L212`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L212)

**Description**:
Consider emitting an event inside this function with `msg.sender` and `msg.value` as the parameters. This would make it easier to track incoming ether transfers.

**Recommendation**:
Consider adding events to the `receive()` functions. 


## [NAZ-L] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`HolographBridge.sol#L162`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162), [`HolographOperator.sol#L240`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240), [`HolographFactory.sol#L158`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L158), [`Holographer.sol#L147`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L147), [`PA1D.sol#L173`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L173), [`HolographERC721.sol#L238`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L238), [`HolographERC20.sol#L218`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L218), [`ERC721H.sol#L144`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L144), [`ERC20H.sol#L144`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L144)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N] Commented Out Code
**Severity**: Informational
**Context**: [`HolographOperator.sol#L438`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L438), [`PA1D.sol#L579-L587`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L579-L587), [`HolographERC721.sol#L524-L570`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L524-L570)

**Description**:
There is commented code that makes the code messy and unneeded. 

**Recommendation**:
Consider removing the commented out code.


## [NAZ-N] Function States Named Return But Doesn't Use It
**Severity** Informational
**Context**: [`HolographOperator.sol#L804`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L804), [`HolographOperator.sol#L814`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L814), [`HolographFactory.sol#L181`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L181)

**Description**:
The linked function(s) have a named return that is not used. Using both named returns and a return statement isn't necessary. 

**Recommendation**:
Removing one of those can improve code clarity.


## [NAZ-N] Array length mismatch
**Severity**: Informational
**Context**: [`HolographERC20.sol#L563`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L563)

**Description**:
These fail to perform input validation on arrays to verify the lengths match. A mismatch could lead to an exception or undefined behavior.

**Recommendation**:
Perform input validation on the arrays to verify that the lengths match.


## [NAZ-N] Missing Explicit Visibility of State 
**Severity**: Informational
**Context**: [`HolographBridge.sol#L126-L142`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L126-L142), [`HolographOperator.sol#L129-L153`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L129-L153), [`HolographFactory.sol#L127-L131`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L127-L131), [`LayerZeroModule.sol#L126-L146`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L126-L146), [`Holographer.sol#L119-L135`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L119-L135), [`PA1D.sol#L124-L144`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L124-L144), [`HolographERC721.sol#L136-L140`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L136-L140), [`HolographERC20.sol#L142-L146`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L142-L146), [`ERC721H.sol#L110-L114`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L110-L114), [`ERC20H.sol#L110-L114`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L110-L114)

**Description**:
All state variables should have explicit visibility.

**Recommendation**:
Consider adding explicit visibility to the listed state variables.


## [NAZ-N] Line Length
**Severity**: Informational
**Context**: [`HolographBridge.sol#L120`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L120), [`HolographBridge.sol#L188`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L188), [`HolographBridge.sol#L212`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L212), [`HolographBridge.sol#L231`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L231), [`HolographBridge.sol#L243`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L243), [`HolographBridge.sol#L268`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L268), [`HolographBridge.sol#L294`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L294), [`HolographBridge.sol#L310`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L310), [`HolographBridge.sol#L339`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L339), [`HolographOperator.sol#L608`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L608), [`HolographFactory.sol#L121`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L121), [`Holographer.sol#L113`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L113), [`Holographer.sol#L172`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L172), [`Holographer.sol#L212`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L212), [`PA1D.sol#L117-L118`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L117-L118), [`PA1D.sol#L149`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L149), [`PA1D.sol#L151`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L151), [`PA1D.sol#L524`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L524), [`PA1D.sol#L618-L619`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L618-L619), [`HolographERC721.sol#L723`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L723), [`HolographERC20.sol#L289`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L289)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N] Code Contains Empty Blocks
**Severity**: Informational
**Context**: [`HolographBridge.sol#L155`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L155), [`HolographOperator.sol#L233`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L233), [`HolographOperator.sol#L1209`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209), [`HolographFactory.sol#L136`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L136), [`LayerZeroModule.sol#L151`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L151), [`Holographer.sol#L140`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L140), [`Holographer.sol#L223`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223), [`PA1D.sol#L166`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L166), [`HolographERC721.sol#L231`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L231), [`HolographERC721.sol#L962`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962), [`HolographERC20.sol#L211`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L211), [`HolographERC20.sol#L251`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251), [`ERC721H.sol#L133`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L133), [`ERC721H.sol#L212`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212), [`ERC20H.sol#L133`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L133), [`ERC20H.sol#L212`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L212)

**Description**:
It's best practice that when there is an empty block, to add a comment in the block explaining why it's empty.

**Recommendation**:
Consider adding `/* Comment on why */` to the empty blocks.


## [NAZ-N] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`PA1D.sol#L205`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L205), [`HolographERC721.sol#L922`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L922), [`HolographERC20.sol#L727`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L727), [`ERC721H.sol#L159`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L159), [`ERC20H.sol#L159`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L159)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`HolographBridge.sol#L452`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452), [`HolographOperator.sol#L717`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L717), [`HolographFactory.sol#L192`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L192), [`LayerZeroModule.sol#L310`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L310), [`Holographer.sol#L205`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L205), [`PA1D.sol#L226`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L226), [`HolographERC721.sol#L368`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L368), [`HolographERC20.sol#L251`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251), [`ERC721H.sol#L168`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L168), [`ERC20H.sol#L168`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L168)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N] Use Underscores for Number Literals
**Severity**: Informational
**Context**: [`HolographOperator.sol#L261`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L261), [`PA1D.sol#L388`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L388), [`PA1D.sol#L390`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390), [`PA1D.sol#L395`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L395), [`PA1D.sol#L411`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411), [`PA1D.sol#L415`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L415), [`PA1D.sol#L435`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435), [`PA1D.sol#L438`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L438), [`PA1D.sol#L477`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L477), [`PA1D.sol#L551`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L551), [`PA1D.sol#L553`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L553), [`PA1D.sol#L641`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L641), [`PA1D.sol#L643`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L643)

**Description**:
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

**Recommendation**:
Consider using underscores for number literals to improve its readability.


## [NAZ-N] TODOs Left In The Code
**Severity**: Informational
**Context**: [`HolographOperator.sol#L701`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701)

**Description**:
There should never be any TODOs in the code when deploying.

**Recommendation**:
Consider finishing the TODOs before deploying.


## [NAZ-N] Spelling Errors
**Severity**: Informational
**Context**: [`HolographBridge.sol#L160 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L160), [`HolographBridge.sol#L415-L416 (destiantion => destination)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L415-L416), [`HolographBridge.sol#L575 (transfered => transferred)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L575), [`HolographOperator.sol#L238 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L238), [`HolographOperator.sol#L515 (accomodate => accommodate)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L515), [`HolographOperator.sol#L553 (rever => revert)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L553), [`HolographOperator.sol#L661-L662 (destiantion => destination)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L661-L662), [`HolographOperator.sol#L997 (adress => address)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L997), [`HolographFactory.sol#L141 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L141), [`HolographFactory.sol#L188 (deployement => deployment)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L188), [`HolographFactory.sol#L338 (transfered => transferred)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L338), [`LayerZeroModule.sol#L156 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L156), [`LayerZeroModule.sol#L414 (transfered => transferred)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L414), [`Holographer.sol#L145 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L145), [`PA1D.sol#L156 (accesible => accessible)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L156), [`PA1D.sol#L171 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L171), [`PA1D.sol#L299 (optimizaion => optimization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L299), [`PA1D.sol#L387 (accomodate => accommodate)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L387), [`PA1D.sol#L446 (authorised => authorized)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L446), [`PA1D.sol#L447 (tranaction => transaction)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L447), [`PA1D.sol#L472 (missmatched => mismatched)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L472), [`PA1D.sol#L472 (lenghts => lengths)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L472), [`HolographERC721.sol#L194 (utilised => utilized)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L194), [`HolographERC721.sol#L236 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L236), [`HolographERC721.sol#L263 (coud => could)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263), [`HolographERC721.sol#L543 (missmatch => mismatch)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L543), [`HolographERC721.sol#L575 (transfered => transferred)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L575), [`HolographERC721.sol#L662 (authorisations => authorization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L662), [`HolographERC721.sol#L900 (usecases => use cases)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L900), [`HolographERC20.sol#L216 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L216), [`ERC721H.sol#L138 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L138), [`ERC20H.sol#L138 (initilaization => initialization)`](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L138)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.

## [NAZ-N] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.
