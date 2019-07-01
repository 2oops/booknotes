# Vuex

1. 组件不允许直接修改store实例的state，而应执行action来dispatch事件通知store去改变 =>`Flex架构`，好处是能够记录所有 store 中发生的 state 改变，同时实现能做到记录变更 (mutation)、保存状态快照、历史回滚/时光旅行。

2. Vuex 允许开发者通过 dispatch 派发一个异步的 action，通过 commit 提交一个同步的 mutation

   之所以区分异步和同步是为了能够更加准确的追踪状态的变化，因为就像无法准确知道一个响应何时会收到一样，异步操作并不能准确的知道何时修改的数据，所以不能将修改 state 的操作放在 action 中，但是我们可以在异步完成后通过提交一个 commit 的形式**同步**的修改 state ，同步的特点使得任何状态的变化都能够确切知道执行前后 state 的状态，以便完成一些高级操作。

