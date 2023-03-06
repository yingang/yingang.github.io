---
layout: post
title:  "Rust 里的 Mock Objects"
date:   2022-01-26 21:48:00 +0800
categories: rust
---

事情的缘起，还是之前提到的那个[用 Rust 重造的轮子](https://yingang.github.io/rust/2021/07/21/log-parser-rewritten-in-rust.html)。最近有空的时候在逐步给它增加单元测试覆盖，不能因为是个小工具就不重视测试，毕竟日常还是偶尔需要改改的。有测试在，才能改得放心。

其中有一个类（`struct`）的用途是为了避免在日志解析的过程中不断打开文件、少量写入、关闭文件，而是在文件内容积累到一定量的时候再一次性写入。和 Rust 库自带的 `BufWriter` 不太一样，后者不涉及反复的文件打开、关闭。在我的场景下，因为系统不允许同时打开太多的文件，所以只能自己控制文件的写入了。

这个类中对单元测试不友好的地方也只是在最终的文件写入，所以很自然的想法，是把对文件系统的写入相关操作抽象出来，在创建类的时候作为参数传递进来，这样就可以在测试场景下引入特殊的写入逻辑（Mock Objects，后文将直接称为 Mock 对象）了。

    A test double is the general programming concept for a type used in place of another type during testing. Mock objects are specific types of test doubles that record what happens during a test so you can assert that the correct actions took place.

类似下面这样，提供两个构造函数（应该叫 Associated Function 吧），一个对外，一个对内（测试时传入 Mock 对象）：

~~~rust
impl Foo {
  pub fn new() -> Self {
    do_new(real_bar)
  }

  fn do_new(mock_bar) -> Self {
    Self {
      bar: mock_bar,
    }
  } 
}
~~~

刚开始先试了试函数指针，类似下面这样在结构体内保存传入的函数指针，执行倒是没问题，但从测试角度不能保存额外的信息（所谓 Mock 对象的用途），对我就没什么用了。

~~~rust
pub struct Foo {
  ...
  writer: fn(&PathBuf, &String, bool) -> io::Result<()>,
}
~~~

然后网上看了看，又尝试了下 Trait Objects（后文将直接称为 Trait 对象），应该算是类似于 C++ 里的动态分配（Dynamic Dispatch，即运行时多态）吧。

~~~rust
pub trait Writer {
  fn write(&mut self, filepath: &Path, content: &String, append: bool) -> io::Result<()>;
}

pub struct Foo {
  writer: Box<dyn Writer>,
}

impl MockWriter {
  pub fn new() -> Box<Self> {
    Box::new(MockWriter {})
  }
}

let mock_writer: Box<MockWriter> = MockWriter::new();
let foo = Foo::new(mock_writer.clone());  // 这里编译报错！
~~~

尝试给 Mock 对象相关类都加上 `#[derive(Clone)]`，编译倒是可以通过，但测试通不过。发现 `Clone` 前后的两个对象是完全独立的。。。算是深拷贝吧，想想也对，不然就违反了 Rust 的编译期借用规则检查。在没有想清楚的情况下，尝试了直接传递 `mock_writer` 或者 `&`、`&mut` 这几种方式，也都不行，各种奇奇怪怪的错误（后文有解释）。这时开始想到是不是应该要搞个共享可变引用（其实理解错了，应该叫不可变引用的内部可变性，即 Interior Mutability Pattern，详见后文）的东西出来，以前 [TRPL](https://doc.rust-lang.org/stable/book/) 书上看到过的那个什么机制？RefCell？

然后发现之前提到的微信读书上的《[Rust 权威指南](https://book.douban.com/subject/35081743/)》居然已经要付费卡无限用户才可以阅读全书了！直接看官网的吧。惊喜地发现书上居然就是用 Mock 对象的例子来讲 `RefCell` 的（[章节 15.5](https://doc.rust-lang.org/stable/book/ch15-05-interior-mutability.html)），真是读书一时爽，读后忘光光。。。看来方向似乎是对的，但我之前对相关机制的理解还是有问题的。

先说具体的处理方法，实际的代码可以看[书上的](https://doc.rust-lang.org/stable/book/ch15-05-interior-mutability.html#a-use-case-for-interior-mutability-mock-objects)，包括 `RefCell<T>` 的具体用法和运行机理：
* 没有用 `dyn` 或者说 Trait 对象，也就没有用 `Box<T>` 这些；
* 直接以 `&T` 的方式引用符合 Trait 的模板类型（Trait Bound），所以，对于 Mock Objects 的使用算是静态分配（Static Dispatch，即编译时多态，也就是泛型）了吧；
* 附带的一个影响就是不需要我前面说的那样搞两个构造函数了，都是在类的外部构建好之后再传递进来；
* 注意：是 `&T` 而不是 `&mut T`，后者没办法有多个引用同时存在（编译器的借用规则检查还是在的）；
* 对于 Mock Objects 的诉求，通过在 `T` 里包装 `RefCell<T>` 来解决在 Immutable 引用的基础上修改属性的需求（也就是通过运行时的借用规则检查来确保安全访问，有一定开销）。
* 以及，因为是对传入参数的引用，需要加上 Lifetime Specifier，可以在开发环境中根据提示自动处理，挺方便；
* 再以及，既然是 `&T`，我前面贴的 Trait 参数申明也需要修改为 `&self` 了。

~~~rust
pub trait Writer {
  fn write(&self, filepath: &Path, content: &String, append: bool) -> io::Result<()>;
}
~~~

关于几种智能指针，书上也有一些总结描述，应该能解释我前面瞎试的时候碰到的问题：
* `Box<T>` 只能单 Owner（类似于 C++ 里的 `unique_ptr`），这应该是我前面怎么处理都不对的原因；
* `Rc<T>` 可以多 Owners，但又都必须是 Immutable 的；
* `RefCell<T>` 也是单 Owner，但可以将借用规则检查推迟到运行期。

现在这个工具主要就只剩下一个多线程相关的处理机制没有抽象出来用单元测试覆盖了，下一个目标有了！

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>