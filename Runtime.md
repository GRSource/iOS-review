# Runtime

## Runtime数据结构

## 类对象和元类对象

## 方法缓存查找

## 消息转发

当向Objective-C对象发送一个消息，但runtime在当前类及父类中找不到此selector对应的方法时，消息转发(message forwarding)流程开始启动。

动态方法解析(Dynamic Method Resolution或Lazy method resolution)
向当前类(Class)发送resolveInstanceMethod:(对于类方法则为resolveClassMethod:)消息，如果返回YES,则系统认为请求的方法已经加入到了，则会重新发送消息。
快速转发路径(Fast forwarding path)
若果当前target实现了forwardingTargetForSelector:方法,则调用此方法。如果此方法返回除nil和self的其他对象，则向返回对象重新发送消息。
慢速转发路径(Normal forwarding path)
首先runtime发送methodSignatureForSelector:消息查看Selector对应的方法签名，即参数与返回值的类型信息。如果有方法签名返回，runtime则根据方法签名创建描述该消息的NSInvocation，向当前对象发送forwardInvocation:消息，以创建的NSInvocation对象作为参数；若methodSignatureForSelector:无方法签名返回，则向当前对象发送doesNotRecognizeSelector:消息,程序抛出异常退出。

## Method-Swizzling

## 动态添加方法

## 动态方法解析
