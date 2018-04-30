# Introduction to Unit Testing	

## What is Unit Testing?

**Testing**, in general coding terms, is the practice of writing code (in addition to your main code) that runs and checks the output of your main code. A **unit** refers to a peice of code (i.e a module, class, or function) that is isolated from other code. So puting this together, **Unit Testing** is the practice of writting code to test isolated pieces of code.

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
        
if __name__ == '__main__' :
    unittest.main()
```

There are a couple of important things to note about our test class
1. `import unittest`
    * This line allows us access to the unittest module. Without it, the code would not run.
2. `class TestPrime(unittest.TestCase) :`
    * This line declares our test case class. It inherits several class methods from the `unittest.TestCase` class that enable it to work with the rest of the `unittest` framework.
3. `def test_is_prime_5(self) :`
    * This line declares a test function. In order for a function to be catagorized as a test by `unittest`, it must begin with `test_`. `unittest` then collects all of these tests and runs them. (Note: The `self` argument has more to do with class structure in python than unittesting, so for our purposes, it's enough to know that it's necessary for the function to work.)
4. `self.assertTrue(is_prime(5))`
    * This line checks (or asserts) that `is_prime(5)` is `True`. `unittest` has several different "assert" methods, including `self.assertEqual` and `self.assertNotEqual`. A complete list can be found in `unittest`'s documentation [here](https://docs.python.org/3/library/unittest.html "unittest documentation").
    
## Running Our Test

Running our test yeilds the following output :

```
E
======================================================================
ERROR: test_is_prime_5 (__main__.TestPrime)
Test that is_prime determines that 5 is prime
----------------------------------------------------------------------
Traceback (most recent call last):
File "/Users/Sam/sp18/me615/unittest_demo.py", line 24, in test_is_prime_5
self.assertTrue(is_prime(5))
File "/Users/Sam/sp18/me615/unittest_demo.py", line 8, in is_prime
if not n % element :
ZeroDivisionError: integer division or modulo by zero

----------------------------------------------------------------------
Ran 1 test in 0.002s

FAILED (errors=1)
```

Oh no! We've failed out test! But, thanks to unittest, we have some useful feedback. Parsing through our results, we see our failure was due to an error. The header also lists the test function, the test class, and the docstring (the phrase in tripple quotes under the declaration) of our test function. Beneath that is a traceback with a ZeroDivisionError that occured on the `if not n % element :` line of our code. 

Looking back at our function, we can realize that our loop starts at 0. So we can edit our code to start at 2 (starting at 1 would yeild prime for every number), and we end up with the following:

```python
def is_prime(n) :
    """ Return True if n is prime, else returns False"""
    # For loop containing all numbers 2 to n
    for element in list(range(2, n)) :
        # if n is divisible by the element
        if not n % element :
            # Then the number is not prime, return false and terminate function
            return False
    # If we make it through the for loop, the number is prime
    return True
```

# Second Times the Charm

After our changes, running our tests yeids the following output: 

```
.
----------------------------------------------------------------------
Ran 1 test in 0.001s

OK
```

Great! We've passed our first test! So what happens now? Do we delete it and write another test to take its place? Not quite. Remember one of the benefits of unit testing is to ensure that functionality remains intact when making changes, so it is rarely a good idea to delete a test, whether you're passing it or not. One of the other benefits was to check that our code works for weird edge cases, so writing more tests is definitely the direction to go.

## Our Test 2.0

So far we've tested that our function is correct for one prime number, which doesn't come close to covering the range of arguments that could be passed to our function. While its unrealistic to test for every case, with some inteligent design, we can make sure that our code works for a good variety of cases. So let's list the different groups of numbers that could be tested:
1. Normal Primes (we've already got a test for this!)
2. Normal Non-Prime Numbers
3. Zero 
4. Negatives 
    
This is by no means an exhaustive list of the things we could test for, but they're a start. So let's update our test class

```python
import unittest

class TestPrime(unittest.TestCase) :
    """ Test Class for is_prime """

    def test_is_prime_5(self) :
        """ Test that is_prime determines that 5 is prime """

        self.assertTrue(is_prime(5))

    def test_is_four_non_prime(self):
        """Is four correctly determined not to be prime?"""

        self.assertFalse(is_prime(4))

    def test_is_zero_not_prime(self):
        """Is zero correctly determined not to be prime?"""

        self.assertFalse(is_prime(0))  

    def test_negative_number(self):
        """Is a negative number correctly determined not to be prime?"""

        for index in range(-1, -10, -1):
            self.assertFalse(is_prime(index), msg = '{} is not prime!'.format(index))

if __name__ == '__main__' :
    unittest.main()
```

Most of these test should be pretty easy to read given what we've learned so far, but the negative number has something a little different. Our assert function is nested inside of a for loop, which is okay, even beneficial. This notation allows us to test the function for the numbers from -1 to -10 with only two lines of code!

## Our Results 2.0

Let's take a look at the results we get with our new tests!

```
..FF
======================================================================
FAIL: test_is_zero_not_prime (__main__.TestPrime)
Is zero correctly determined not to be prime?
----------------------------------------------------------------------
Traceback (most recent call last):
File "/Users/Sam/sp18/me615/unittest_demo.py", line 38, in test_is_zero_not_prime
self.assertFalse(is_prime(0))
AssertionError: True is not false

======================================================================
FAIL: test_negative_number (__main__.TestPrime)
Is a negative number correctly determined not to be prime?
----------------------------------------------------------------------
Traceback (most recent call last):
File "/Users/Sam/sp18/me615/unittest_demo.py", line 44, in test_negative_number
self.assertFalse(is_prime(index), msg = '{} is not prime!'.format(index))
AssertionError: True is not false : -1 is not prime!

----------------------------------------------------------------------
Ran 4 tests in 0.005s

FAILED (failures=2)
```

Yikes!  Looks like we've got a couple of failures. Before we look at these, a couple of details about the output.
* At the top, theres a `..FF`. This is a breif summary of the test results. A "." indicates a passed test, a "F" a failure, and an "E" an error or exception (like we saw when we ran our first test case for the first time).
* The body of the output stays basically the same with the test name, test docstring, and the test class its from in a header.
* At the bottom, we see the number of tests, the test time, and the number of failures (and errors, had there been any).

Looking more at our actual results, we can see that our function does not produce the desired result for zero and negative numbers, so we need to address that in our code. Fortunately, it's fairly easy to address both of these with one if statement (two birds, one stone).

```python
def is_prime(n) :
    """ Return True if n is prime, else returns False"""
    
    # Any number less than one is NOT prime
    if n <= 1 :
        return False

    for element in list(range(2, n)) :
        if not n % element :
            return False
    return True
```

And running our tests we see...

```
....
----------------------------------------------------------------------
Ran 4 tests in 0.004s

OK
```

We're good!

# Final Words on Unit Testing

Using unit testing on your code is a very useful and practical way to manage your code and ensure it continues to function properly as your library grows. Running unit tests after each development can ensure that small tweaks did not have unintended side effects. However, just because your code passes all of the tests, does not mean it's perfect. The code is only as good as the tests, so it falls on the coder to be thoughtful and think outside the box when testing their code.  
    


