# Heap动态数据类型

### 向量

vec!宏`vec![1,2,3]`

**溢出访问**

```rust
方式1会恐慌
fn main() {
    let acc = vec![1,2,3,4];
    let b = &acc[5];
    println!("{}", b);
}
方式2
fn main() {
    let acc = vec![1,2,3,4];
    match acc.get(5) {
        Some(value) => println!("{}", value),
        None => println!("None"),
    }
}
```

引用规则

- 符合可变和不可变引用不可同时出现
- 引用就是刻舟求剑，heap数据类型修改可能会导致整体迁移到别处，所以局部引用和局部修改不能同时出现

解引用修改数据

```rust
fn main() {
    let mut acc = vec![1,2,3,4];
    for x in  acc.iter_mut() {
        *x += 1;//解引用
    }
    for x in acc.iter() {
        println!("{}", x);
    }
}
```

vec可以放枚举类型，使得一个向量可以把多种数据类型放在一起



**String**

- String是Vec<u8>，但是不能像数组一样获取特定位置数据

```rust
fn main() {
    let s = "hello".to_string();
    let s2 = "hello wp".to_string();
    let s3 = s + &s2;//add函数获取了self（这里是s）的所有权，&s2的只读引用
    println!("{}", s);//报错
  	println!("{}", s3.get(2..4).unwrap());//无法通过[index]访问
}
```



- 切片

  ```rust
  let s = "细化";
  let b = &s[0..1];//恐慌，必须是在字符边界处切割
  ```

as_bytes()是u8，as_chars()是编码后的字符

**HashMap**

- 数据在堆上

- K，V统一数据类型

  ```rust
  let a = vec![1, 2, 3, 4, 5];
  let b = vec![ 2, 3, 4, 5, 6];
  let c = a.iter().zip(b.iter()).collect::<HashMap<_, _>>();//两个数组或tuple创建hashmap 
  println!("{:#?}", c); 
  ```

- 元组数组转Hashmap、两个数组转Hashmap、遍历

  ```rust
  let mut a = vec![("ah".to_string(), 2)];
  let c = a.into_iter().collect::<HashMap<_,_>>(); 
  println!("{:#?}", c); /*
  {
      "ah": 2,
  }*/
  //两个数组转hashmap
  let a = vec![1, 2, 3, 4, 5];
  let b = vec![ 2, 3, 4, 5, 6];
  let c = a.iter().zip(b.iter()).collect::<HashMap<_, _>>(); 
  println!("{:#?}", c); 
  //遍历
  for (k, v) in c {
        println!("{}: {}", k, v);
  }
  ```

- HashMap**的所有权**

  - 实现了Copy的会复制值
  - 对于在堆上的值，值被移动，所有权移动到hash内部
  - 插入值的引用，值不移动，可以再次引用
  - - hashmap中引用的值必须存活，不能有悬垂指针

HashMap**插入操作**

- Key存在
  * 替换Value
  
  * 保留现有Value，忽略新Value
  
  * 合并新旧Value
  
- Key不存在
  * 直接插入新值
`scores.entry(String::from("key")).or_insert(1);`
`scores.entry(String::from("key"))`返回&mut 类型