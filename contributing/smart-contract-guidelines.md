---
description: >-
  When creating a new smart contract project, use the guidelines below to ensure
  your project meets our basic standards of quality.
---

# 📐 Smart Contract Guidelines

## Security

### Re-entrancy

Any external or public functions need to be analyzed to determine whether the contract can be attacked if a user re-enters that function or another.

### Safe ERC20 Usage

ERC20 token interactions should always use SafeERC20 safeApprove and safeTransferFrom in the OpenZeppelin library.

### Math Overflow and Underflow

Any math operations need to be checked for overflow and underflow conditions, using SafeMath or similar.

### Trusted External Calls

The contract should minimize trust in external calls.

## Optimizations

* All external / public functions should return a value when possible \(to save gas\)
* Structs must be tightly packed
* Hardcoded integers and strings should be constants
* Contract members that don't change should be immutable

## Conventions

### Typed Arguments

All arguments, whether to an event or function, must be typed when possible. `address` types should be avoided in favour of contract or interface types

### Logs Emitted

Events must be emitted for any significant state change or event.

### Grammar

Names and documentation must be spelled correctly with no grammatical errors.

## Documentation

### Readme

A complete readme must be included with the project. It needs:

* a complete description
* usage
* setup instructions
* all test commands
* deployment instructions and information
* information on any additional scripts

### Natspec

Contracts must have \(at minimum\):

* `@title`: short title.  shouldn't repeat the description
* `@notice`: descriptive enough so that the reader understands the intent of the contract.

Functions must have \(at minimum\):

* `@notice`: explain what the function does
* `@param`: explain each parameter
* `@return`: explain the return param\(s\)

Events must have \(at minimum\):

* `@notice`: describe when the event is emitted
* `@param`: describe each of the args

## Testing

### Unit Tests

* There must be a test suite for each contract
* Each unit test should mock out contract dependencies using Waffle or Smock
* Contract functions should be tested in isolation
* Unit tests must run **locally; i.e. they do not connect to a remote node.**

### Code Coverage

* Coverage must exceed 95%

### Fork Test

* A fork test script tests the contracts in the real world
* At a minimum the test must execute the "happy path" for the code.

### Repository Badges

* Coverage badge must be included \(we use Coveralls\)
* Github Workflow badge for the fork test 
* Github Workflow badge for the unit tests

