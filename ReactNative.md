# ReactNative

## 原理及结构

* React Native原理其实跟Weex差不多，底层也会把React转换为原生API
* React Native和Weex区别在于跨平台上面，Weex只要写一套代码，React Native需要iOS，安卓都写，说明React Native底层解析原生API是分开实现的，iOS一套，安卓一套。

React Native如何把React转化成原生API？  
React Native会在一开始生成OC模块表，然后把这个模块表传入JS中，JS参照模块表，就能间接调用OC的代码。相当于买了一个机器人(OC)，对应一份说明书(模块表)，用户(JS)参照说明书去执行机器人的操作。

React Native是如何做到JS和OC交互的？  
iOS原生API有个JavaScriptCore框架，通过它就能实现JS和OC交互

1. 首先写好JSX代码(React框架就是使用JSX语法)
2. 把JSX代码解析成javaScript代码
3. OC读取JS文件
4. 把javaScript代码读取出来，利用JavaScriptCore执行
5. javaScript代码返回一个数组，数组中会描述OC对象，OC对象的属性，OC对象所需要执行的方法，这样就能让这个对象设置属性，并且调用方法。

React Navite启动流程  

1. 创建RCTRootView -> 设置窗口根控制器的View，把RN的View添加到窗口上显示。
2. 创建RCTBridge -> 桥接对象，管理JS和OC交互，做中转作用。
3. 创建RCTBatchedBridge -> 批量桥架对象，JS和OC交互具体实现都在这个类中。
4. 执行[RCTBatchedBridge loadSource] -> 加载JS源码
5. 执行[RCTBatchedBridge initModulesWithDispatchGroup] -> 创建OC模块表
6. 执行[RCTJSCExecutor injectJSONText] -> 往JS中插入OC模块表
7. 执行完JS代码，回调OC，调用OC中的组件
8. 完成UI渲染

## 和Flutter及JSPatch区别
