---
title: go语言中的导入与导出
tags:
  - go
categories:
  - go
date: 2024-06-10 12:42:21
---

#### 简介

* ***go***语言中引入与导出是通过包机制实现的

* ***package***是代码组织和封装的基本单位，每个Go文件都必须声明一个包，包名通常与所在目录名相同

#### 导出

* 标识符（变量、常量、类型、函数、结构体字段等））以大写字母开头则是导出，可以被其他包访问，小写只能内部访问

* go语言中是区分大小写的

* 例子如下：

  ```go
    var Con = "" // 外部可访问
    var con = "" // 只有包内部可访问
    
    func Test(){} // 外部可访问
    func test(){} // 只有包内部可访问
  ```

  
#### 导入

* 导入使用import关键字

* 可以多行import单独导入，也可以分组导入，如下：

  ```go
  // 单独导入
  import "fmt"
  import "math"
  
  // 分组导入
  import (
      "fmt"
      "math"
  )
  ```
  

* 别名导入（如果没有别名，默认的是取导入路径的最后一个单词作为包名，调用包内部暴露的标识）

  ```go
  // 1.别名导入
  import f "fmt"
  
  func main() {
      f.Println("Hello, world!")
  }
  
  // 2.默认导入
  import (
      "myproject/packages/package1"
  )
  
  func main() {
      package1.DoSomething()
  }
  
  ```

  
