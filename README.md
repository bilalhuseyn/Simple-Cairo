# Simple-Cairo
That is explaining Cairo
cairo-repo/
  |
  +-- README.md
  |
  +-- LICENSE
  |
  +-- src/
  |     |
  |     +-- my_contract.cairo
  |     |
  |     +-- utils.cairo
  |
  +-- tests/
  |     |
  |     +-- test_my_contract.cairo
  |
  +-- build.sh
// Define contract state variables
felt public balance;

// Function to deposit funds
felt deposit(felt amount) {
  balance += amount;
  emit DepositEvent(amount);
  return balance;
}

// Function to withdraw funds
felt withdraw(felt amount) {
  require(amount <= balance, "Insufficient funds");
  balance -= amount;
  emit WithdrawalEvent(amount);
  return balance;
}

// Define events
event DepositEvent(felt amount);
event WithdrawalEvent(felt amount);
// Utility function to check if a value is even
felt is_even(felt x) {
  return x % 2 == 0;
}
// Test deposit function
felt test_deposit() {
  felt starting_balance = balance;
  felt deposit_amount = 10;
  deposit(deposit_amount);
  assert(balance == starting_balance + deposit_amount);
}

// Test withdraw function
felt test_withdraw() {
  felt starting_balance = balance;
  felt withdraw_amount = 5;
  withdraw(withdraw_amount);
  assert(balance == starting_balance - withdraw_amount);
}
#!/bin/bash

cairo-compile --no-debug-info --proof_mode src/my_contract.cairo -o my_contract.json
cairo-run --ignore-missing-libraries my_contract.json
