# code review 总结(1)
## 代码规范
1. useState和useRef分别放在一起。
2. useState一定是set/get都要使用，否则就不能发挥useState作用。
3. 变量之后空一行。
4. 容易异常的代码私用try catch。
5. 统一使用箭头函数。
6. 使用const和let替代var。
## hooks
1. 使用useMemo来进行性能优化。
### useState/useMemo/useCallback的区别
#### DEMO

<iframe height="265" style="width: 100%;" scrolling="no" title="difference of useState, useMemo and useCallback" src="https://codepen.io/jonsam-ng/embed/JjXvVQN?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/jonsam-ng/pen/JjXvVQN'>difference of useState, useMemo and useCallback</a> by jonsam
  (<a href='https://codepen.io/jonsam-ng'>@jonsam-ng</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

#### 比较
- useState：状态机，用于组件的状态管理。state的改变会粗发render。
- useEffect: 在组件mounted或者render的之后执行副作用。
-  useMemo: 可以用作性能优化，只有在观测值发生变化时执行，相当于Vue中的computed，具有缓存功能。可以返回计算值，也可以执行计算函数。useMemo会自动执行。
-  useCallback: 可以用作性能优化，也具有缓存作用，不同的是useCallback将函数进行缓存，不会自动执行。
