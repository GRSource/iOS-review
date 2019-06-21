# UI视图

## 一、UI事件传递

问1：UIView和CALayer的区别是什么？  
1.UIView可以响应事件，CALayer不可以，因为UIView继承自UIResponsder，而CALayer直接继承自NSObject，并没有相应的处理事件的接口。  
2.View的frame、center和bounds只是简单的返回Layer的frame、center和bounds  
3.UIView主要对显示内容的管理，而CALayer主要侧重显示内容的绘制  
4.CALayer修改属性时默认会产生隐式动画，时间为0.25s，但在为UIView当中的CALayer做动画时，View作为Layer的代理，Layer需要通过actionForLayer:forKey:向View请求相应的动画action。  

问2：UI事件的传递机制是什么样的？  
1.当iOS程序发生触摸事件后，系统会将事件加入到UIApplication管理的一个任务队列中  
2.UIApplication将处于任务队列最前端的事件向下分发给UIWindow  
3.UIWindow将事件下发给UIView  
4.UIView通过hitTest:withEvent:来判断自己是否能处理事件(即触点是否在自身范围之内)，如果能，那么继续采用倒序的方式遍历它的子视图，一直循环下去，直到找到最终能处理事件的视图  
5.如果遍历完之后仍然未找到处理事件的子视图，那么自己就是事件的处理者  
6.如果自己也不能处理，那么将不做任何处理。  
(UIView不接受事件处理的情况有三种：1.alpha < 0.01 2.userInteractionEnabled = NO 3.hidden = YES)

## 二、UI事件响应

问1：UI事件的响应机制是什么样的？  
通过hitTest:withEvent:找第一响应者之后，如果该响应者没有处理该事件，那么事件会沿着响应链向上传递，依次为父视图、视图控制器、UIWindow、UIApplication，如果UIApplication也不处理，那就将该事件丢弃

## 三、UITableView卡顿原因及优化方案

问1：卡顿原因是什么及如何优化？  
1.cell没有重用，在数据量比较大时，如果没有重用就会堆积很多cell  
2.避免cell重新布局  
3.提前计算并缓存cell的属性及内容  
4.减少cell中控件的数量  
5.不要使用ClearColor，渲染耗时比较长  
6.使用局部刷新  
7.加载网络数据、下载图片，使用异步加载并缓存  
8.少使用addSubView给cell动态添加view  
9.不要实现无用的代理方法  
10.缓存行高  
11.不要做多余的绘制工作，比如在drawRect范围之外绘制  
12.采用异步绘制  

## 四、异步绘制

实现displayLayer:代理方法，自己生成对应的bitmap，将bitmap设置为layer.contents属性的值

## **离屏渲染**

问1：什么是在屏渲染？  
在屏渲染指的是GPU在当前用于显示的屏幕缓冲区中进行渲染操作  
问2：什么是离屏渲染？离屏渲染是怎么造成的？  
离屏渲染指的是GPU在当前屏幕缓冲区以外开辟一个新的缓存区进行渲染操作。
当我们指定了UI视图的某些属性，标记为它在位域合成之前不能用于当前屏幕直接显示的时候，就会触发离屏渲染。比如说圆角和maskToBounds结合使用时、蒙版、阴影、光栅化。  
问3：为何要避免？  
触发离屏渲染的时候会增加GPU的工作量，而增加了GPU的工作量很有可能导致CPU和GPU总耗时加起来超过16.7毫秒，那么就可能导致UI的卡顿和掉帧，所以我们需要避免离屏渲染。
