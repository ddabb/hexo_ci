---
title: 委托在重构中的运用
toc: true
top: 100
mathjax: false
date: 2019-03-09 18:49:35
tags:
- 重构
categories:
- 代码世界
- 重构
---
# 序
最近在工作中的一次重构过程中运用到了委托，记录一下。

## 场景
有一段示例代码如下
```
public class ProjectAClass
{
    public static void CallMethod(bool isTrue)
    {
        if (isTrue)
        {
            ProjectB_StaticClass.DoSomething();
        }
    }

}

```
## 需求
后来因为业务的不断发展，需要把ProjectB从整个解决方案中移除掉。   
ProjectB_StaticClassB.DoSomething();需要替换成 ProjectC_StaticClassC.DoSomething();   
但是项目ProjectC需要引用ProjectA，故不能直接进行引用，否则会造成项目之间的循环依赖。  

## 解决方案
ProjectAClass添加一个委托事件,CallMethod函数添加一个**Dosometing dosometing**的参数。
```
public class ProjectAClass
{
    public delegate void Dosometing();

    public static void CallMethod(bool isTrue, Dosometing dosometing)
    {
        if (isTrue)
        {
            dosometing?.Invoke();
        }
    }
}

```
具体的调用代码处就变成了
```
ProjectAClass.CallMethod(true,()=>{ProjectC_StaticClassC.DoSomething();});
```

## 扩展
若ProjectAClass.CallMethod方法多地方出现，然而没有办法一下修改到位。可以通过参数默认值的方式来实现修改。
```
    public static void CallMethod(bool isTrue, Dosometing dosometing=null)
    {
        if (isTrue)
        {
            if(dosometing == null)
            {
                ProjectB_StaticClass.DoSomething();
            }
            else
            {
                dosometing.Invoke(); // or dosometing()
            }         
        }
    }
```
该方法可以避免原来的调用方法进行大面积的修改。

# 总结
重构之后的代码ProjectAClass只需要判断条件是否满足，满足则dosometing，而不需要知道dosometing这个委托方法中具体细节。