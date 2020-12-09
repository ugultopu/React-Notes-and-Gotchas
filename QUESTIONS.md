On [React setState API documentation], it is stated:

> `setState()` will always lead to a re-render unless `shouldComponentUpdate()` returns `false`. If mutable objects are being used and conditional rendering logic cannot be implemented in `shouldComponentUpdate()`, _calling `setState()` only when the new state differs from the previous state will avoid unnecessary re-renders_.

I'm wondering how calling `setState()` "only when the new state differs from the previous state" is possible. As far as I know, React batches state updates. Hence, `this.state` is unreliable and might not reflect the actual state of a component, since there might be batched state updates waiting to be applied. For this reason, it is recommended to use the `setState(oldState => {doSomeOperationsAndReturnNewState}` method when the new state depends on the old state. Since we actually need to call `setState` to get the actual (real) value of the current state, how can we compare the next state with the current state before calling `setState`, check if they are different and _only then_ call `setState`?


Also, isn't React smartly comparing the old state to the new state and not performing any rendering if the old and the new state are the same? Isn't this the whole point of React? So why is the programmer supposed to check if the state has been changed and _only then_ call `setState()`?

[React setState API documentation]: https://reactjs.org/docs/react-component.html#setstate

Potential Answer: I think the keyword is _if mutable objects are being used_. In that case, then that quote makes sense. However, as far as I know, we should always use immutable objects when using React. Using mutable objects is the exception rather than the norm AFAIK. Heck, even `React.StrictMode`'s whole purpose is this AFAIK. That is, `React.StrictMode`'s whole purpose is to break your program if your program uses mutable objects, forcing you to re-write your program using immutable objects.
