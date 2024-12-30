This article will introduce several coverage criteria in white box test.

# LOC Coverage

Provide test cases so that **every line of the code has been executed at least one time**.

> This is kind of weak coverage criteria.

# Decision Coverage (Branch Coverage)

- Cover all lines of code.
- Every branches of a condition node is triggered.

# Condition Coverage

- Cover all lines of code.
- Every condition elements in each condition is appeared.

## Examples

```
if A > 2 || B == 1:
  A -= 3
if A < 1 && B > 5:
  // do sth
```

```
Test cases:
A = 100, B = 1
A = 0,   B = 100
```

Here is explaination:

- `A=100, B=1`
	- In first conditional block:
		- `A = 100`, `B = 1`
		- Trigger: `A > 2`, `B ==1`
	- In second conditional block:
		- `A = 97`, `B = 1`
		- Trigger: `A >= 1`, `B <= 5`

- `A=0, B=100`
	- In first conditional block:
		- `A = 0`, `B = 100`
		- Trigger: `A <= 2`, `B !=1`
	- In second conditional block:
		- `A = 0`, `B = 100`
		- Trigger: `A < 1`, `B > 5`

## Difference Between Decision Coverage

Decision coverage and Branch coverage **both cares about conditional nodes**.

However:

**Decision coverage views one conditional node as a whole**, only cares about the final result `True/False` of that node.

**Condition coverage discuss every single elements** which consist of the the final conditional expression, **without concerning if each branch is finally covered**.

# Decision Condition Coverage

Satisfy Branch coverage and Condition converage at the same time.

# Condition Combination Coverage

For **each conditional node**, every combination of condition elements is triggered at least one time.

## Example

```
if A > 2 || B == 1:
  A -= 3
if A < 1 && B > 5:
  // do sth
```

Then:

```
Combination Requirements:

(For first conditional node)
A > 2,  B == 1
A > 2,  B != 1
A <= 2, B == 1
A <= 2, B != 1
(For second conditional node)
A < 1,  B > 5
A < 1,  B <= 5
A >= 1, B > 5
A >= 1, B <= 5
```

Basically, for a single conditional node, if it **has $n$ conditional elements, then it will require $2^n$ combinations**.

> Having n combination requirements **doesn’t essentially means you need n test cases**, and one single test cases could **cover more than one combination requirements in different conditional blocks**.

___

It could be proved that Condition Combination Coverage also **satisfy Decision Coverage and Condition Coverage**.

# Path Coverage

Every single possible path of the program from the beginning to the end has been tested.

Strong, but not feasible at most of the time as it requires huge number of test cases.
