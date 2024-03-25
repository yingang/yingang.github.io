---
layout: post
title:  "大型翻车丢人现场实录"
date:   2020-05-22 10:42:45 +0800
categories: c++
---

之前用 Python 写过一个解析研发日志的小工具，自用尚可，如果要分发的话，感觉还挺麻烦，需要带一个 Python 的运行环境，之前没有太多这方面的经验。只用过 PyInstaller，但在公司的信息安全体系下运行也有一些奇怪的问题。另外 Python 本身相对慢的运行速度也会稍微有点不太爽。

于是参照 Python 代码，用 C++ 重新撸了遍这个工具，在 C++ 标准库和 boost 类库的加持下，其实代码量倒没有太明显的差异，大概是两倍吧，好用的类库对于提升开发效率还是很重要的。

初始的喜悦结束后，开始掉坑。。。

* strftime

~~~cpp
auto nsec = boost::lexical_cast<int>(sec);
tm t = *localtime((time_t *)&nsec); // WRONG!!!
const int BUF_LEN = 80;
char buf[BUF_LEN] = {0};
auto size = strftime(buf, BUF_LEN - 1, "%y%m%d %H:%M:%S", &t);
~~~

~~用这个方法来将 Unix 的时间戳（从 1970 年开始的秒数）转换成人类可读的表示形式，其中提供给 strftime 的缓存大小，从格式字符串来看，最多十几个字节应该就够了，但日志解析时偶发会 crash 在这里，后来改成 80 后暂时没有碰到问题。而且偶发的 crash 仅发生在 release build（开启了优化）之后，debug build 无法复现，脑壳疼。~~

找到问题了（2020/05/27），学艺不精真是丢人。。。有问题的是前面调用 localtime 时的参数处理，int 指针强转 time_t 指针是有问题的，因为类型的位宽不同导致了未定义行为，并影响了后面的 strftime。正确的写法应该是类似于：

~~~cpp
time_t nsec = boost::lexical_cast<int>(sec);
tm t = *localtime(&nsec);
const int BUF_LEN = 32;
char buf[BUF_LEN] = {0};
auto size = strftime(buf, BUF_LEN - 1, "%y%m%d %H:%M:%S", &t);
~~~

* tellg

~~~cpp
std::ifstream fin(filepath, std::ios::binary);
fin.seekg(0, std::ios::end);
auto length = fin.tellg();  // BANG!
boost::shared_array<char> buf(new char[length]);
fin.seekg(0, std::ios::beg);
~~~

~~用这个方法来获取文件大小，在特定情形下（可重复）对于个别文件永远返回的是 -1（还好这个问题可以在 debug build 复现），后来换成 boost::filesystem::file_size 规避掉了。确实[网上也有人说 tellg 不保证能得到正确的文件大小](https://stackoverflow.com/questions/22984956/tellg-function-give-wrong-size-of-file/22986486#22986486)，本质的原因没太搞懂。~~

也找到问题了（2020/06/10），丢人again。。。似乎 Windows 上单个进程能同时打开的文件数量是有限制的（比如按[这里](https://stackoverflow.com/questions/870173/is-there-a-limit-on-number-of-open-files-in-windows)说的，基于 C 运行库的进程最多能打开 512 个文件）。代码层面的根本问题是没有对文件打开的结果做检查（因为要打开的文件都是本地刚遍历出来的，完全没想到还会有打不开的可能性。。。），正确的姿势应该是这样的：

~~~cpp
std::ifstream fin(filepath, std::ios::binary);
if (fin.is_open())
{
    ...
}
~~~

最后，在基本功能稳定之后，稍微优化了下（比如用基于 C++ 标准库的代码来替换 boost::format 和 boost::split 等，以及对最近解析过的时间戳做一些缓存），解析性能达到了 Python 版本的 5 倍，还不错。

### 题外话

事后想想，类似这样的一些问题场景，虽然直接原因都在我自己，但在 Rust 之类的比较新的编程语言中，可能会比在 C++ 中更容易避免，也不会受一些经验和习惯性思维的影响：
* 对于前一个类型转换的问题，有部分原因是对 `time_t` 这种不太常用的数据结构不太熟悉，比如它的大小，如果编程语言本身可以自动检查类型转换是否有风险，应该就可以避免了；
* 对于后一个操作失败的问题，主观上确实没想到过打开文件的数量是有限制的，但如果是用 Rust 这样的语言，强迫编程人员必须对打开文件操作的结果进行检查，应该也可以避免了。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>