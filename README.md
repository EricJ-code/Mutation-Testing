# Mutation-Testing
## Introduction
This is an assignment that introduces the idea of mutation testing. I used Mutpy to automate this process.
## List of Defined Mutation Operations
- Arithmetic Operator Swapping:
    - Replace arithmetic operators like addition, subtraction, multiplication and division with one another to test if the test suite detects these changes.

- Assignment Operator Swapping:
    - Change assignment operators such as equals, plus equals, minus equals to check if the tests catch variable assignment mutations.

- Comparison Operator Swapping:
    - Swap comparison operators such as less than, greater than, equals, and not equals to evaluate if the tests identify modifications to condition checks.

- Decorator Removal:
    - Remove decorators to assess detection of missing decorators.

- Exception Handling Changes:
    - Modify exception handling code to evaluate detection of errors in error-handling logic.

- Flow Control Modification:
    - Alter break and continue statements to test the ability to detect changes in loop control flow.

- Logical Connector Swapping:
    - Modify logical connectors including and, or and not operators to assess if the tests catch alterations to compound conditional logic.

- Return Value Changing:
    - Modify function return values to evaluate if the tests identify changes in output.

- Statement Removal:
    - Remove statements to assess if the test suite detects missing code.

- Unary Operator Swapping:
    - Modify unary operators including positive, negative and bitwise NOT to evaluate if the tests detect mutations in unary expressions.


## Summary of Mutation Survival and Killing
Note: these are the final results
```
[*] Mutation score [10.57238 s]: 69.6%
   - all: 89
   - killed: 32 (36.0%)
   - survived: 14 (15.7%)
   - incompetent: 43 (48.3%)
   - timeout: 0 (0.0%)
```
## Description of Changes Made
### First Pass of Mutpy
#### Changes for this pass
none
#### Results
```
[*] Mutation score [10.74934 s]: 54.3%
   - all: 89
   - killed: 25 (28.1%)
   - survived: 21 (23.6%)
   - incompetent: 43 (48.3%)
   - timeout: 0 (0.0%)
```
### Second Pass of Mutpy
#### Changes for this pass
```
def test_get_derivitive_coefficents():
    poly = Polynomial([1, 0, -2, 0])  # Represents x^3 - 2x
    derivitive = poly.get_derivative_coefficients()
    assert derivitive == [i * coeff for (i, coeff) in enumerate(list(reversed(poly.coefficients))[:-1])]
```
#### Results
```
[*] Mutation score [10.88514 s]: 67.4%
   - all: 89
   - killed: 31 (34.8%)
   - survived: 15 (16.9%)
   - incompetent: 43 (48.3%)
   - timeout: 0 (0.0%)
```

### Third Pass of Mutpy
#### Changes for this pass
```
def test_evaluate():
    poly = Polynomial([3, 0, 2])
    result = 0
    for i, coef in enumerate(poly.coefficients):
        result += coef * (1 ** (len(poly.coefficients) - i - 1))
    test = poly.evaluate(x=1)
    assert test == result
```
#### Results
```
[0.10573 s] killed by PolyTest.py::test_get_derivitive_coefficents
[*] Mutation score [10.42915 s]: 69.6%
   - all: 89
   - killed: 32 (36.0%)
   - survived: 14 (15.7%)
   - incompetent: 43 (48.3%)
   - timeout: 0 (0.0%)
```
## Analysis
As seen above the overall Mutation Score was 69.6% meaning that the total effectivness is the same, 69.6%.

## Recommendations
A recomendation I have would be a test for the change of operator on line 93.
I attempted a test but was unsuccessful. 
Bleow is just one example of this going untested.
```
--------------------------------------------------------------------------------
   89:             c = (a + b) / 2
   90:             print(c)
   91:             if abs(self.evaluate(c)) < epsilon:
   92:                 return c
-  93:             if self.evaluate(c) * self.evaluate(a) < 0:
+  93:             if self.evaluate(c) * self.evaluate(a) <= 0:
   94:                 b = c
   95:             else:
   96:                 a = c
   97:         
--------------------------------------------------------------------------------
```
There is another example like this on line 85.
```
--------------------------------------------------------------------------------
   81:             :param epsilon: Tolerance for the root approximation.
   82:             :param max_iterations: Maximum number of iterations.
   83:             :return: Approximated root within the specified interval.
   84:             '''
-  85:         if self.evaluate(a) * self.evaluate(b) > 0:
+  85:         if self.evaluate(a) * self.evaluate(b) >= 0:
   86:             raise ValueError('The chosen interval does not contain a root.')
   87:         
   88:         for _ in range(max_iterations):
   89:             c = (a + b) / 2
--------------------------------------------------------------------------------
```
## Conclusion
In conclusion this test suite needs more testing for embedded "ifs" and loops.