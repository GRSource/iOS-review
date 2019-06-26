# Block

## Block本质

## Block截获变量

## __block和__weak

## Block内存管理

## Block循环引用

## Block里面self、weakSelf、strongSelf使用

1.不成环，并且想让block代码什么情况下都执行：两种方式：A.全部使用self就行；B.外面定义weak，block里面用strong，也行，多次一举。

2.不成环，并且想让block代码在当前VC释放的情况下不执行：两种方式：A.外面定义weak，里面使用weak，然后注意nil可能会crash的地方（加判断）；B.外面定义weak，block里面使用strong（或者直接使用self），自己加if判断，否是在当前页面，不在当前页面不执行。

3.成环，想让block代码无论如何都执行：必用weak。block里面用strong。

4.成环，想让block代码在当前VC释放的情况下不执行：两种方式：A.必用weak，block里面用strong，则自己加if判断不在当前页面就不执行；B.block里面使用weak，注意nil可能导致crash的地方。
