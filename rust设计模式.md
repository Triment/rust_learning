rust设计模式

- 减小耦合（使逻辑简单）
- 避免逻辑错误（减小耦合就可以简单）
- 代码复用

SOLID原则

- single单一原则，一个对象一个功能
- open/close开闭原则，扩展开放，修改封闭
- Liskov里氏替换，派生类可以替代超类
- Iterface Segregation 接口隔离（细分），拆分大接口为小接口
- Dependence Inversion 依赖倒置，依赖抽象而不是具体
- Law of Demeter 迪米特法则，两个实体不需要直接通信就不需要发生直接调用

single是一个高度概念贯穿始终

开闭原则是一个功能设计范式

里氏就和依赖倒置理解为**面向接口**

迪米特是减小耦合的一种哲学，和single是同层级不同概念

1. 使用struct替换基础变量

rust的0成本抽象大部分情况不会增加负担，可以使用结构体替换对应的谓词或者信号类型



```rust
enum NetworkLayer {
  Segment(4)
  Packet(3)
  Frame(2)
}

struct IpAddr(String);

#[derive(PartialEq)]
enum Test { Pass, Fail }
if ( Test::Pass == result ) {
  ...
}
```

