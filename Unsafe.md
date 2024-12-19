**unsafe代码**四个能力

- 解引用原始指针
- 调用unsafe函数或方法
- 访问修改可变的静态变量
- 实现unsafe trait

原始指针：

- 同时具有n个可变不可变指针指向同一个位置
- 允许为null
- 不实现任何自动清理

```rust
let mut s = "hello";
let r1 = &s as *const &str;
let r2 = &mut s as *mut &str;
unsafe {
    *r2 = "world";
    println!("{}", *r1);
    println!("{}", *r2);
}
```

函数定义和调用

```rust
unsafe fn test(){
    let mut s = "hello";
    let r1 = &s as *const &str;
    let r2 = &mut s as *mut &str;
    unsafe {
        *r2 = "world";
        println!("{}", *r1);
        println!("{}", *r2);
    }
}

fn main(){
    unsafe {
        test();
    }
}
```

- 语法
- - 通过as进行类型转换（安全转不安全指针）
  - unsafe分离安全和不安全区域
  - 函数、代码块、trait前面加unsafe

**extern函数**

简化FFI（外部函数接口）

ABI：定义函数在汇编层调用方式

```rust
extern "C" {//C语言的ABI
  
}
```

其他语言调用Rust函数

```rust
#[no_mangle]//防止Rust修改编译后的函数名
pub extern "C" fn call_from_c() {//不需要unsafe 
  println!("called from c");
}
```

 

##### 静态变量和常量

- 静态指的是生命周期，本质还是变量（但是修改和访问可变静态变量需要放进unsafe）
- 常量
