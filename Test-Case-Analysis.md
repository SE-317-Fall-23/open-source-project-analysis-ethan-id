# Assignment Submission

## Project Selected: Redux

## I. Introduction
The two test cases I have selected are testing some expected behaviors of a `compose()` function. This is the description of the function according to the documentation:
```javascript
/**
 * Composes single-argument functions from right to left. The rightmost
 * function can take multiple arguments as it provides the signature for the
 * resulting composite function.
 *
 * @param funcs The functions to compose.
 * @returns A function obtained by composing the argument functions from right
 *   to left. For example, `compose(f, g, h)` is identical to doing
 *   `(...args) => f(g(h(...args)))`.
 */
 ```

## II. Test Case 1: 'returns the first argument if given no functions'
### A. Description
This test case is validating that if the `compose()` function is given no functions as parameters after its first argument, it should return the first argument.
### B. Gherkin Syntax (if applicable)
Feature: `compose()`
Given `compose()` is provided with no functions to compose,
But is given at least one argument to process,
When `compose()` is called,
Then it should return the first argument provided.
### C. Test Steps
This test is quite short and only contains `expect()` statements asserting that the expected behavior happens given various inputs.
### D. Code Segments Under Test
This is what the `compose()` function looks like in the source code of Redux:
```TypeScript
export default function compose(...funcs: Function[]) {
  if (funcs.length === 0) {
    // infer the argument type so it is usable in inference down the line
    return <T>(arg: T) => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce(
    (a, b) =>
      (...args: any) =>
        a(b(...args))
  )
}
```
The function is exported multiple times in the file essentially overloading it for multiple cases of expected inputs; I've omitted these for simplicity.

Here is the full test case:

```TypeScript
it('returns the first given argument if given no functions', () => {
    expect(compose<number>()(1, 2)).toBe(1)
    expect(compose()(3)).toBe(3)
    expect(compose()(undefined)).toBe(undefined)
})
```

These `expect()` statements are testing that when `compose()` is called without any functions, but is given arguments, it should return the first argument. They are specifically asserting that the first if statement, `if(funcs.length === 0)`, is entered as expected and the code within, `return <T>(arg: T) => arg`, is returning the first argument.

### E. Initial State
The initial state of this test is essentially just that the project has been compiled and the `compose()` function is available to be called.
### F. Transition
The action occuring in this test is the execution of the `compose()` function.
### G. Expected State
The expecting outcome of the test is the value returned from the execution of `compose()`

## III. Test Case 2: 'composes from right to left'
### A. Description
This test case asserting that when the `compose()` function is called with multiple functions to compose, it runs them from right to left, passing the output of each to the function it should be composed with. For example, if I do `compose(f, g, h)`, it should be equivalent to `(...args) => f(g(h(...args)))`.
### B. Gherkin Syntax (if applicable)
Feature: `compose()`
Given `compose()` is provided with multiple functions to compose,
When `compose()` is called,
Then it should process the provided functions from right to left.
### C. Test Steps
- Enumerate the sequence of steps or actions involved in this test case.
### D. Code Segments Under Test
- Identify specific code segments or functions that are being tested in this case.

Here is the `compose()` function again, but with only the code being tested:
```TypeScript
export default function compose(...funcs: Function[]) {
  // ... other logic not being tested in this test case

  return funcs.reduce(
    (a, b) =>
      (...args: any) =>
        a(b(...args))
  )
}
```

And here is the test case itself:
```TypeScript
it('composes from right to left', () => {
    const double = (x: number) => x * 2
    const square = (x: number) => x * x
    expect(compose(square)(5)).toBe(25)
    expect(compose(square, double)(5)).toBe(100)
    expect(compose(double, square, double)(5)).toBe(200)
})
```

This test case is asserting with various inputs, that the functions inputted to `compose()` are processed right to left. This is verified in the test case mathematically by defining two simple functions, `double()`, and `square()`, then passing them in different orders and asserting that the output from `compose()` is as expected.

### E. Initial State
The conditions of this test are that there are functions defined that will be passed into `compose()`.
### F. Transition
The event occurring in the test is the execution of `compose()` with a specific order of defined functions.
### G. Expected State
The expected outcome of the test is the value returned by compose is what it should be, in this test case, that the value is as expected mathematically.
