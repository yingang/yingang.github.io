---
layout: post
title:  "Named Mutex in boost C++ library"
date:   2019-05-25 18:09:45 +0800
categories: c++ boost
---

### 问题

* 系统中有一些资源，需要在不同进程之间互斥使用

### 方案

* 使用 boost::interprocess::sync 库里的 named_mutex 和 scoped_lock

### 新问题

* 如果应用在 lock mutex 期间 crash，下次再尝试 lock 的时候，会陷入无限等待

### 原因

* 刚开始以为是 named mutex 资源在进程退出的时候 OS 没有释放掉
* 后来发现应该是因为 boost 的 named_mutex，不支持对 abandoned 状态的 mutex 的处理
* 如果能自己编译 boost，应该有[一些办法](https://stackoverflow.com/questions/15772768/boost-interprocess-mutexes-and-checking-for-abandonment)可以规避这个问题，但这不是我的情况

### 新方案

* 听从[建议](https://stackoverflow.com/questions/1179685/how-do-i-take-ownership-of-an-abandoned-boostinterprocessinterprocess-mutex/1179766#1179766)，直接用 Windows 原生的 named mutex 吧

### 代码

~~~ cpp
class NamedMutex
{
public:
    NamedMutex(const std::string& name)
    {
        _mutex = ::CreateMutex(NULL, TRUE, ("Global\\" + name).c_str());
        if (::GetLastError() == ERROR_ALREADY_EXISTS)
            ::WaitForSingleObject(_mutex, INFINITE);
    }

    ~NamedMutex()
    {
        ::ReleaseMutex(_mutex);
        ::CloseHandle(_mutex);
    }

private:
    HANDLE _mutex;
}

int main(int argc, char** argv)
{
    ...
    {
        NamedMutex mutex("TheDarkKnight");
        ...
    }
    ...
}
~~~

### 其它

* [boost::interprocess::named_mutex vs CreateMutex](https://stackoverflow.com/questions/20379817/boostinterprocessnamed-mutex-vs-createmutex) -- 讲了讲两者的区别
* [Understanding the consequences of WAIT_ABANDONED](https://devblogs.microsoft.com/oldnewthing/?p=34253) -- 还好，这也不是我的情况
* 记住：OS 自己不会泄漏资源，一定是我哪里没用对！

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>