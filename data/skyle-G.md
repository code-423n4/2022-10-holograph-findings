G‑01 <array>.length should not be looked up in every loop of a for-loop
contracts/enforcer/PA1D.sol
Ln432 for (uint256 t = 0; t < tokenAddresses.length; t++) {
Ln454 for (uint256 i = 0; i < addresses.length; i++) {
Ln474 for (uint256 i = 0; i < addresses.length; i++) {

G‑02 ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops
contracts/enforcer/PA1D.sol
Ln307 for (uint256 i = 0; i < length; i++) {
Ln323 for (uint256 i = 0; i < length; i++) {
Ln340 for (uint256 i = 0; i < length; i++) {
Ln356 for (uint256 i = 0; i < length; i++) {
Ln394 for (uint256 i = 0; i < length; i++) {
Ln414 for (uint256 i = 0; i < length; i++) {
Ln432 for (uint256 t = 0; t < tokenAddresses.length; t++) {
Ln454 for (uint256 i = 0; i < addresses.length; i++) {
Ln474 for (uint256 i = 0; i < addresses.length; i++) {

G‑03 ++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)
contracts/enforcer/PA1D.sol
Ln307 for (uint256 i = 0; i < length; i++) {
Ln323 for (uint256 i = 0; i < length; i++) {
Ln340 for (uint256 i = 0; i < length; i++) {
Ln356 for (uint256 i = 0; i < length; i++) {
Ln394 for (uint256 i = 0; i < length; i++) {
Ln414 for (uint256 i = 0; i < length; i++) {
Ln432 for (uint256 t = 0; t < tokenAddresses.length; t++) {
Ln454 for (uint256 i = 0; i < addresses.length; i++) {
Ln474 for (uint256 i = 0; i < addresses.length; i++) {

[G-4] IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED.
contracts/enforcer/PA1D.sol
Ln307 for (uint256 i = 0; i < length; i++) {
Ln323 for (uint256 i = 0; i < length; i++) {
Ln340 for (uint256 i = 0; i < length; i++) {
Ln356 for (uint256 i = 0; i < length; i++) {
Ln394 for (uint256 i = 0; i < length; i++) {
Ln414 for (uint256 i = 0; i < length; i++) {
Ln432 for (uint256 t = 0; t < tokenAddresses.length; t++) {
Ln454 for (uint256 i = 0; i < addresses.length; i++) {
Ln474 for (uint256 i = 0; i < addresses.length; i++) {

contracts/HolographOperator.sol
Ln781 for (uint256 i = 0; i < length; i++) {

fix:
- for (uint256 i = 0; i < length; i++) {
+ for (uint256 i; i < length; i++) {


