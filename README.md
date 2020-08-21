[Computed data shouldn't be stored in the state]. Instead, it should be computed as-needed as part of the render method. [I assume we can employ techniques like memoization to improve performance or maybe React has a built-in solution for this][Memoization and PureComponents in React].

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

---

[Computed data shouldn't be stored in the state]: https://stackoverflow.com/questions/25145857/react-js-having-state-based-on-other-state
[Memoization and PureComponents in React]: https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization
[Never use `this.state.something` inside a `this.state()` call]: https://stackoverflow.com/questions/25145857/react-js-having-state-based-on-other-state#comment85652520_40900154
[state updates may be asynchronous]: https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous
