# hooks基础
----
## hooks API
> Basic Hooks
> - useState
> - useEffect
> - useContext

> Additional Hooks
> - useReducer
> - useCallback
> - useMemo
> - useRef
> - useImperativeHandle
> - useLayoutEffect
> - useDebugValue

 ## hooks rules
 - Only Call Hooks at the `Top Level`, Don’t call Hooks inside loops, conditions, or nested functions.
- Only Call Hooks from `React Functions`, Don’t call Hooks from regular JavaScript functions.

## useState
### usage
```js
const [state, setState] = useState(initialState);
```
### deatils
- initialState: `initial value` in first render.
- state: return the `most recent stateful value`.
- setState: accept a new stateful value to update the state, enqueues a re-render of the component.

### Functional updates
```js
setState(prevState => {
  return {...prevState, ...updatedValues};
});
```
The function will receive the previous value, and return an updated value.

### Lazy initial state
```js
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```
If the initial state is the result of an `expensive computation`, you may provide a function instead.

## useEffect
### usage
```js
useEffect(
  () => {
    doSideEffect;
    return () => {
      doCleanUp;
    };
  },
  [dependencies],
);
```
### details
- function:  do `side effects` after the render is committed to the screen `every time`.(used for side effect that can be `deferred`).
- cleaning up an effect: return a `clean-up function`, runs before the component is removed from the UI.
- dependencies: an effect will be recreated if one of the dependencies changes.

### note
- if you use dependencies, make sure the dependencies array includes all values from the `component scope` (such as `props and state`) that change over time and that are used by the effect.
- If you want to run an effect and clean it up only `once` (on `mount` and `unmount`), you can pass an `empty array` ([]) as a second argument.Then the props and state inside the effect are initial values.This is closed to `componentDidMount` and `componentWillUnmount` mental model.
- every value referenced inside the effect function should also appear in the dependencies array.

## useContext
### usage
```js
const value = useContext(ContextObject);
```
### params
- **ContextObject**: the value returned from `React.createContext`.
- **value**: the current context value, determined by the value prop of the `nearest` ``<MyContext.Provider>`` above the calling component in the tree.
### note
- When the nearest Provider above the component updates, this Hook will trigger a rerender with the latest context value even if an ancestor uses `React.memo` or `shouldComponentUpdate`.that means: 
	**A component calling useContext will always re-render when the context value changes**.
	- useContext() is equivalent to *`static contextType = MyContext`* in a class, or to *`<MyContext.Consumer>`*.

## useReducer
### usage
```js
const [state, dispatch] = useReducer(reducer, initialState, init);
```
### params
- reducer: a reducer of type *`(state, action) => newState`*.
- initialState: the initial state for the reducer.
- init: init function to create the initial state lazily.
- state: the current state.
- dispatch: a dispatch state to mutate the state.
### Lazy initialization
- init function: extract the logic for calculating the initial state outside the reducer.
- a live demo with lazy initialized useReducer: [[#lazy initialized useReducer]].

### note
- Bailing out of a dispatch: If you return the same value from a Reducer Hook as the current state, React will bail out without rendering the children or firing effects.

## useCallback
### usage
```js
const memoizedCallback = useCallback(
  () => {
    memoizedLogicByDependences
  },
  [dependences],
);
```
### params
- dependences: an optional array of dependencies.
- memoizedCallback: a memoized version of the callback that only changes if one of the dependencies has changed.


### note
- *`useCallback(fn, deps)`* is equivalent to *`useMemo(() => fn, deps)`*.
- every value referenced inside the callback should also appear in the dependencies array.

## useMemo
### usage
```js
const memoizedValue = useMemo(
	() => computeExpensiveValue, 
	[dependences]
);
```
### params
- dependences: an optional array of dependencies.
- memoizedValue: return a memoized value that will `recompute`  when one of the dependencies has changed to avoid expensive calculations on every render.
### note
- Don't do side effects in useMemo or useCallback, because they run during rendering.
- If no array is provided, a new value will be computed on every render.
-  every value referenced inside the function should also appear in the dependencies array. 

## useRef
useRef is like a “box” that can hold a mutable value in its .current property, include a DOM node .
### usage
```js
const ref = useRef(initialValue);
```
### params
- **initialValue**: initial value for ref.
- **ref**: a `mutable ref object` whose `.current` property is initialize to initialValue.
### note
- useRef will give you the same ref object on every render though it creates a plain JavaScript object.
-  Mutating the .current property doesn’t cause a re-render and also doesn’t notify you.

## useImperativeHandle
useImperativeHandle `customizes the instance value` that is exposed to parent components when using ref and it should be used with `forwardRef`.
### usage
```js
useImperativeHandle(ref, () => ({
   handles
  }),
  [dependences]
);
```
### params
- ref: ref that use with forwardRef.
- handles: handles exposed to parent components imperatively.
- dependences: an optional array of dependences.

## useLayoutEffect
read layout from the DOM and `synchronously re-render`.
### usage
```js
useLayoutEffect(
  () => {
    doLayoutEffectSynchronously;
    return () => {
      doCleanUp;
    };
  },
  [dependencies],
);
```
### note
- Updates scheduled inside useLayoutEffect will be flushed synchronously, before the browser has a chance to paint.（在浏览器有机会绘制之前，在useLayoutEffect内部计划的更新将被同步刷新。）
- Prefer the standard useEffect when possible to `avoid blocking visual updates`.

## useDebugValue
useDebugValue can be used to display a label for custom hooks in React DevTools.
### usage
```js
useDebugValue(value, formatter)
```
### params
- **value**: the label that should display.
- **formatter**: useDebugValue accepts a formatting function as an optional second parameter when formatting a value for display might be an expensive operation. **This function is only called if the Hooks are inspected. **It receives the debug value as a parameter and should return a formatted display value.

## some topic
### What is side effect in hooks?
### What's the difference between useEffect hook and useLayoutEffect hook?
### What's the difference between useMemo hook and useCallback hook?
see the demo here: [[#difference between useMemo ang useCallback]]
### What's the difference between useState hook and useReducer hook?
An alternative to useState
useReducer is usually preferable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one.
### How to migrate from class component live methods to hooks?

## some live demos
### difference between useMemo ang useCallback
<iframe height="265" style="width: 100%;" scrolling="no" title="difference of useState, useMemo and useCallback" src="https://codepen.io/jonsam-ng/embed/JjXvVQN?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/jonsam-ng/pen/JjXvVQN'>difference of useState, useMemo and useCallback</a> by jonsam
  (<a href='https://codepen.io/jonsam-ng'>@jonsam-ng</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

###  lazy initialized useReducer
### a theme provider demo
### simple Redux