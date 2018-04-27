# Introduction to Unit Testing	

## What is Unit Testing?

**Testing**, in general coding terms, is the practice of writing code (in addition to your main code) that runs and checks the output of your main code. A **unit** refers to a peice of code (i.e a module, class, or function) that is isolated from other code. So putthing this together, **Unit Testing** is the practice of writting code to test isolated pieces of code.

## Why Unit Testing?

You may be thinking, "Why should I waste my time writing more code to check my own code?" and it's a valid question. Here are a couple of the benefits:
1. Testing makes sure that your code works under a variety of conditions.
2. Testing allows you to make sure that recent changes did not affect existing functionality.
3. Testing gives you the oppurtunity to think of edge cases for your code, which may expose possible logical errors.

# Unit Testing in Python

`unittest` is the primary unit testing module used in python, and what we will be using to demonstrate unit testing practices. But before we write a test program, we have to have something to test.

## Our Example Code

Lets say we've written the following function designed to determine whether a number is prime :
```python
def is_prime(n) :
    """ Return True if n is prime, else returns False"""
    # For loop containing all numbers 0 to n
    for element in list(range(n)) :
        # if n is divisible by the element
        if not n % element :
            # Then the number is not prime, return false and terminate function
            return False
    # If we make it through the for loop, the number is prime
    return True
```

At this point, we're unsure whether this code has the desired behavior. Enter `unittest`.

## Our Test Case

It's common knowledge that 5 is prime, so it makes sense to write a test that confirms that our function returns `True` when 5 is passed to it.

```python
# importing unittest framework (similar to #include in C++)
import unittest

# This defines the test class inheriting from unittest.TestCase
class TestPrime(unittest.TestCase) :
    """ Test Class for is_prime """
    
    def test_is_prime_5(self) :
        """ Test that is_prime determines that 5 is prime """
        
        self.assertTrue(is_prime(5))
```

S
