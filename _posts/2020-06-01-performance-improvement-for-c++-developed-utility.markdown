---
layout: post
title:  "对 C++ 开发的小工具进行性能优化"
date:   2020-06-01 21:21:00 +0800
categories: c++ vs2019
---

继续[前前一个帖子](https://yingang.github.io/strftime/tellg/2020/05/22/strftime-and-tellg.html)和[前一个帖子](https://yingang.github.io/vs2019/c++17/2020/05/27/hello-vs2019-and-c++17.html)中提到的日志解析小工具。 既然都改用 C++ 了，又抽空对性能做了一些优化，目前速度基本是受限于磁盘 IO 了，解析速度大概是最早 Python 版本的十倍左右，具体的优化点主要涉及如下几个方面：

* 性能分析

  VS 自带的性能分析还是挺好用的，可以精确地给出 CPU 时间都消耗在哪里了。这个是起点，不然就是瞎优化了。

* [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)

  之前代码中涉及到比较多的内存拷贝，比如先将每个读取到的日志文件按行分隔符进行分拆，然后每一行再按特定分隔符进行分拆，等等。这个过程中很多时候并不会修改其分拆前的内容，这应该是使用 std::string_view（应该是 C++ 17 才有的吧）的合适场景。仅在需要修改的时候才转换成 std::string（显式转换）。

  比如上次提到的 split 方法，也可以对应改成：

~~~cpp
  void split(std::string_view input, char delim, std::vector<std::string_view>& tokens)
  {
    std::string::size_type start = 0;
    for (;;)
    {
      auto to = input.find(delim, start);
      tokens.push_back(input.substr(start, to - start));
      if (to == std::string::npos)
        break;

      start = to + 1;
    }
  }
~~~

* [std::string::reserve](http://www.cplusplus.com/reference/string/string/reserve/) 和 [std::vector::reserve](http://www.cplusplus.com/reference/vector/vector/reserve/)

  这个是老调重弹了，根据实际的业务提前预留好空间，对性能的帮助还是挺大的，可以避免大量不必要的内存申请和拷贝。当然，很大一个前提是前面性能分析的结果，有的放矢。

  包括[之前提到过的手工撸的 boost::join](https://yingang.github.io/vs2019/c++17/2020/05/27/hello-vs2019-and-c++17.html)，用 std::stringstream 和直接将一堆 std::string 加在一起的性能都不太好，最后还是先 reserve 好最终输出的 std::string 空间，然后一个个将素材 append 上去的性能是更快的。

上面这些做完之后，CPU 时间的分布比之前更平均一些，相对突出的几个，除了文件读写，主要是下面这两个，暂时没有动力进一步优化了：

* 输出结果的拼接（指对原始日志进行解析、转换后的结果进行拼接，在上面提到的 reserve + append 之后，仍然是主要的性能消耗点）；
* 时间戳的格式化（除了转换本身，从性能分析结果来看，还涉及到底层运行库在不同字符集之间的转换，比如 Multi Byte 和 Wide 之间的来回转换）。

题外：C++ 也可以用类似于 1'000'000 的方式来定义整数字面量（[Integer Literal](https://en.cppreference.com/w/cpp/language/integer_literal)），好像是 C++ 14 后就支持了，赞！

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>