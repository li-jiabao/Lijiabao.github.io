---
title: aggregation operation
author: lijiabao
date: 2020-11-22 01:01:16.585649900 +0800
category: JavaNotebook
categories: JavaNotebook
tags: JavaNotebook
---

### lambda expression
you can use it to complements functionality interface with a simple declaration
`InterfaceName varName = (a, b) -> a + b;`
in (), these contents are paraeters, after -> it is a expression which has a result, this result was return value, you also can use {} to include return clause.Like this `() -> {return "hello"}`

### method reference
- static method reference：ClassName::StaticMethodName
- instance method reference：ClassObj::MethodName
- type cast method reference：TypeCast::MthodName
- constructor reference：ClassName::new

### functionality interface
it is a interface with just one method.

### stream
it represents a sequences of objects from a source, which supports aggragate operation. We can use stream(),parallelStream() method to get stream. The former is a popular method. The later is to get stream which support parallel processing.

### pipeline
pipelines is a sequence of aggregate operation.aggregate operation is like as forEach()、filter()

pipeline component:
- A source: could be a collection、array、generator function or a I/O channel。
- zero or more intermediate operation：use it can get a new stream
- terminal operation：use it to get a non-stream value--- primitive value、collection、or forEach（no value)