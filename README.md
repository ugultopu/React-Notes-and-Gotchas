[Computed data shouldn't be stored in the state]. Instead, it should be computed as-needed as part of the render method. [I assume we can employ techniques like memoization to improve performance or maybe React has a built-in solution for this][Memoization and PureComponents in React]. In fact, AFAICT the current and the right way to memoize an expensive calculation is to use the [`useMemo()` hook]. The reason is that whenever I go to [React's website] and type "memoiz" (beginning for "memoization" or "memoize"), all search results are about either the `useMemo()` hook, [`React.memo` higher order component] or something like that. Reading more about hooks in React would be very helpful to understand more about this.

[Computed data shouldn't be stored in the state]: https://stackoverflow.com/questions/25145857/react-js-having-state-based-on-other-state
[Memoization and PureComponents in React]: https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization
[`useMemo()` hook]: https://reactjs.org/docs/hooks-faq.html#how-to-memoize-calculations
[`React.memo` higher order component]: https://reactjs.org/docs/react-api.html#reactmemo

---

[Never use `this.state.something` inside a `this.state()` call]. The reason is that [state updates may be asynchronous]. That is, React may batch multiple `setState()` calls into a single update for performance. Since `this.props` and `this.state` may be updated asynchronously, _you should not rely on their values for calculating the next state_.

For example, following is a wrong way to do a state update:

    // Wrong
    this.setState({
      counter: this.state.counter + this.props.increment,
    });

To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time of the update is applied as the second argument:

    // Correct
    this.setState((state, props) => ({
      counter: state.counter + props.increment
    }));

Or, even shorter:

    // Correct
    this.setState( ({counter}, {increment}) => ({
      counter: counter + increment
    }) );

[React's website]: https://reactjs.org
[Never use `this.state.something` inside a `this.state()` call]: https://stackoverflow.com/questions/25145857/react-js-having-state-based-on-other-state#comment85652520_40900154
[state updates may be asynchronous]: https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous

---

If you observe that a call to `setState` is being invoked twice, whereas it should have been invoked only once, you are most likely running React in Strict Mode. Strict Mode in React is a tool for highlighting potential problems in an application. [It intentionally invokes a set of methods twice][GitHub Issue Comment About React Strict Mode], in order to break the application if these methods hold state inside them (whereas they shouldn't have). One example is [invoking twice (double-invoking) state updater functions (the first argument to setState)][React Strict Mode].

[More information][StackOverflow Question About React Strict Mode].

[GitHub Issue Comment About React Strict Mode]: https://github.com/facebook/react/issues/12856#issuecomment-390206425
[React Strict Mode]: https://reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects
[StackOverflow Question About React Strict Mode]: https://stackoverflow.com/questions/53183362/what-is-strictmode-in-react
