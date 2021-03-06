---
title: c++ thread
tags:
  - C/C++
categories:
  - C/C++
date: 2021-07-02 07:55:17
---

```
std::thread thread_obj(func, params);

std::thread::join()  //wait for a thread 


 std::thread t1(callable);   // Start thread t1   
 t1.join(); // Wait for t1 to finish
 
 if(joinable()){
表示线程已结束
}
终止线程，需要先设置一个循环退出变量，然后检查是否joinable,然后用join等待退出
```

# sleep

```
  std::this_thread::sleep_for(std::chrono::seconds(1));
 std::this_thread::sleep_for(std::chrono::milliseconds(interval));
 
 
 auto start = std::chrono::high_resolution_clock::now(); //#include <chrono>
   std::this_thread::sleep_for(2000ms);  //Blocks the execution of the current thread
   auto end = std::chrono::high_resolution_clock::now();
   std::chrono::duration<double, std::milli> elapsed = end-start;
   std::cout << "Waited " << elapsed.count() << " ms\n";   //Waited 2000.12 ms
```



Sleep 函数告诉操作系统 “在未来的多少毫秒内我不参与 CPU 竞争”。

Thread.Sleep(0) 的作用是 “触发操作系统立刻重新进行一次 CPU 竞争”。竞争的结果也许是当前线程仍然获得 CPU 控制权，也许会换成别的线程获得 CPU 控制权。


1秒(s) ＝1000毫秒(ms)

1毫秒(ms)＝1000微秒 (us)

1微秒(us)＝1000纳秒 (ns)

1秒=10的9次方

[https://zh.wikipedia.org/zh-hans/%E6%95%B0%E9%87%8F%E7%BA%A7_(%E6%97%B6%E9%97%B4)](https://zh.wikipedia.org/zh-hans/数量级_(时间))

1ks千秒=10的3次方秒

1Ms兆秒=10的6次方秒

1吉秒Gs=10的9次方秒，约31.7年。



```
离开作用域就释放mutex了
std::lock_guard<std::mutex> lock(io_mutex); //lock_guard是一个类，是mutex的包装 https://en.cppreference.com/w/cpp/thread/lock_guard
std::unique_lock<std::mutex> locker(mutex_); 
```

https://www.cplusplus.com/reference/mutex/unique_lock/#:~:text=std%3A%3Aunique_lock&text=This%20class%20guarantees%20an%20unlocked,case%20an%20exception%20is%20thrown.



## detach

使用join(),线程运行完,main函数才能结束。

当使用detach()函数时，主调线程继续运行，被调线程驻留后台运行，主调线程无法再取得该被调线程的控制权。当主调线程结束时，由运行时库负责清理与被调线程相关的资源。使用detach(),main函数不用等待线程结束才能结束。有时候线程还没运行完，main函数就已经结束了。

## 参考

https://en.cppreference.com/w/cpp/thread/thread/detach

https://en.cppreference.com/w/cpp/thread/thread/joinable

https://www.cplusplus.com/reference/thread/thread/detach/

https://www.geeksforgeeks.org/multithreading-in-cpp/
