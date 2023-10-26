## Project Selected: Redux

## I. Introduction
In this analysis, we are focusing on the testing techniques used within the Redux open-source project, particularly focusing on the `compose()` function. This function, chains functions from right to left, with the rightmost function providing the signature for the composite function. 

By examining two distinct test cases for this function, our objective is to discern the types of testing methods employed and give some description of the rationale behind the tests. We also will observe how test data is generated for the project's tests. Gathering this understanding will offer some insight into why tests are important as well providing examples of real test cases.

## 1. Test Data Generation
### A. Static Test Data

Generally in this project it appears that data used in tests is static and unchanging. Some examples can be seen here:
```TypeScript
it('skips non-function values in the passed object', () => {
    // as this is testing against invalid values, we will cast to unknown and then back to ActionCreator<any>
    // in a typescript environment this test is unnecessary, but required in javascript
    const boundActionCreators = bindActionCreators(
        {
        ...actionCreators,
        foo: 42,
        bar: 'baz',
        wow: undefined,
        much: {},
        test: null
        } as unknown as ActionCreator<any>,
        store.dispatch
    )
    expect(Object.keys(boundActionCreators)).toEqual(
        Object.keys(actionCreatorFunctions)
    )
})
```

```TypeScript
it('returns the first given argument if given no functions', () => {
    expect(compose<number>()(1, 2)).toBe(1)
    expect(compose()(3)).toBe(3)
    expect(compose()(undefined)).toBe(undefined)
})
```

```TypeScript
it('wraps dispatch method with middleware once', () => {
    function test(spyOnMethods: any): Middleware {
      return methods => {
        spyOnMethods(methods)
        return next => action => next(action)
      }
    }

    const spy = vi.fn()
    const store = applyMiddleware(test(spy), thunk)(createStore)(reducers.todos)

    store.dispatch(addTodo('Use Redux'))
    store.dispatch(addTodo('Flux FTW!'))

    expect(spy.mock.calls.length).toEqual(1)

    expect(spy.mock.calls[0][0]).toHaveProperty('getState')
    expect(spy.mock.calls[0][0]).toHaveProperty('dispatch')

    expect(store.getState()).toEqual([
      { id: 1, text: 'Use Redux' },
      { id: 2, text: 'Flux FTW!' }
    ])
})
```

In all three of these test cases, data that is being asserted on is explicitly and statically generated prior. In the first test case this can be seen when the object passed into `bindActionCreators()` is defined. In the second test case this can be seen in the arguments passed into `compose()`. And in the third, it can be seen in the lines:
```TypeScript
store.dispatch(addTodo('Use Redux'))
store.dispatch(addTodo('Flux FTW!'))
```

In all of these examples, the static generation of data used as input to functions is defined specifically so that the output of methods or expected state of the app can be explicitly asserted on without having to also dynamically change based on the input.