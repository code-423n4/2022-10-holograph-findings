Anyone can call `bondUtilityToken` for an operator, there is no check on `msg.sender`

not a critical finding, but could be used as grieving attack to prevent an operator to bond to a particular pod by front-running them and bonding them to another pod (though the operator could still withdraw from that pod and join another one)

maybe a check on `msg.sender == operator` to be safe