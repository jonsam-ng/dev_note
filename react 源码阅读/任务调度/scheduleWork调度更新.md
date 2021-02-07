# scheduleWork调度更新
## scheduleWork()方法
`scheduleWork(fiber, expirationTime)`传入fiber 和 expirationTime，可见 fiber 更新的调度是根据 expirationTime 来处理的。

在 `react-reconciler/src/ReactFiberWorkLoop.js` 中函数 `scheduleUpdateOnFiber()`实际上就是scheduleWork。

1. checkForNestedUpdates() 检查嵌套更新

如果`nestedUpdateCount > NESTED_UPDATE_LIMIT` 会被判定为嵌套更新然后报`Maximum update depth exceeded`的错误。其中 `NESTED_UPDATE_LIMIT` 为50，这是为了防止嵌套更新的死循环。

2. 嵌套更新的层级数是如何计算的？

```ts
let nestedUpdateCount: number = 0;
let rootWithNestedUpdates: FiberRoot | null = null;
```

在函数`commitRootImpl()`中有如下代码来更新 `nestedUpdateCount`：

```ts
if (remainingExpirationTime === Sync) {
  // Count the number of times the root synchronously re-renders without
  // finishing. If there are too many, it indicates an infinite update loop.
  if (root === rootWithNestedUpdates) {
	  nestedUpdateCount++;
  } else {
	  nestedUpdateCount = 0;
	  rootWithNestedUpdates = root;
  }
} else {
  nestedUpdateCount = 0;
}
```
3. 