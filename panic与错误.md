### panic与错误

错误有两种方式

- 可恢复的
- 不可恢复的（中断）

可恢复的是Result，不可恢复的是panic!

abort配置

中断分两种情况

- 沿着调用栈清理数据（默认）设置环境变量`RUST_BACKTRACE`获取详细调用栈回溯信息
- 直接中断，由操作系统清理数据`[profile.release]\npanic = 'abort'`

**Result**枚举

```rust
enum Result<T, E> {
  Ok(T),
  Err(r)
}
```

- unwrap用于自动恐慌（不需要太细致的错误检查）
- excpet用于指定恐慌信息（方便调试）

传播错误`?`规则
- 成功返回T
- 错误直接返回`Err(调用的错误)`

```rust
use std::{fs::File, io};
fn open_file(path: &str) -> Result<File, io::Error> {
    Ok(File::open(path)?)//?返回两种情况
}
```

- From trait：标准库的`use std::convert::From;`Trait用于隐式转换错误返回值
- 链式调用
```rust
let mut s = String::new();
File::open("hello")?.read_to_string(&mut s)?;
```
-  `Box<Error>`任何可能的错误对象

#### 使用panic的情况
- 演示
- 测试
- 原型开发
- 损坏和调用外部不可控代码以及无意义外部参数