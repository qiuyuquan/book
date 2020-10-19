## 8.1 react 生命周期介绍
##### react V16.3 新增生命周期
  1. getDerivedStatFromProps
  2. getSnapshotBeforeUpdate

##### 逐渐废弃的生命周期
  1. componentWillMount
  2. componentWillReceiveProps
  3. componentWillUpdate

##### react v16.3以前的生命周期
  1. 组件装载（mount）组件第一次渲染到dom树 
  constructor（初始化state，绑定函数this环境，props本地化getDefaultProps）
  compontentWillMount
  render
  componentDidMount

  2. 组件更新（update）state，props引发的重新渲染 
   shouldComponentUpdate(true/false)
   compontentWillUpdate
   render
   compontentDidUpdate

  3. 组件卸载（ummount）组件从dom树删除 
   componentWillUnMount
      
##### react v16.3以后的生命周期
  1. getDerivedStatFromProps 在组件创建，props、state发生变化时触发，返回一个新对象作为新的state，返回null说明不需要更新state

  2. getSnapshotBeforeUpdate触发时间在update发生的时候，在render之后，组件dom渲染之前

  3. 挂载阶段
   constructor（实例化）
   static getDerivedStatFromProps（获取state）
   render（渲染）
   compontentDidMount（完成挂载）

  4. 更新阶段
   static getDerivedStatFromProps（获取state）
   shouldComponentUpdate（是否需要重绘）
   render（渲染）
   getSnapshotBeforeUpdate（获取快照）
   compontentDidUpdate

##### 参考文章
  [大话react生命周期2019：react-v16.3新生命周期总结](https://segmentfault.com/a/1190000020348448)


## 8.2 react 的 class 组件和函数组件有什么不同

## 8.3 react 的 setstate 过程


