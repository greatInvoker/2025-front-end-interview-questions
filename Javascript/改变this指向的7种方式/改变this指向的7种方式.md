# 改变 this 指向的 7 种方式

| 方式                   | 特点                                                   |
| ---------------------- | ------------------------------------------------------ |
| call()                 | 调用函数，指定 this 和参数列表                         |
| apply()                | 调用函数，指定 this 和参数数组                         |
| bind()                 | 创建新函数，调用时将 this 和参数列表永久绑定到指定对象 |
| 箭头函数               | 不绑定自身的 this，它会捕获其所在上下文的 this         |
| new 关键字调用构造函数 | 函数内的 this 指向新创建的对象                         |
| 调用对象方法           | this 指向调用该方法的对象                              |
| 调用类方法             | 方法的 this 指向类的实例                               |
