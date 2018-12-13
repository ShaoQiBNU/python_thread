python多线程
===========

* 参考链接 *
https://morvanzhou.github.io/tutorials/python-basic/threading/4-queue/
https://www.jianshu.com/p/f58ff94ec92b


```python
######################### load packages ########################
import threading
from queue import Queue

######################### job ########################
def job(l,q):
    ## 多线程调用的函数不能用return返回值，可以用队列承接 ##
    
    for i in range(len(l)):
        l[i] = l[i]**2
    
    q.put(l)

######################### multi threading ########################
def multithreading(data, num_thread):
    ###### 队列存放返回值，代替return的返回值 #####
    q =Queue()

    ###### 多线程列表 #####
    threads = []

    ###### 结果值 #####
    results = []

    ###### 开启num_thread个线程 #####
    for i in range(0, len(data)-num_thread+1, num_thread):

        ###### 定义num_thread个线程 #####
        for j in range(i, i+num_thread):
            # 被调用的job函数没有括号，只是一个索引，参数在后面
            t = threading.Thread(target=job, args=(data[j], q))

            # 启动线程
            t.start()

            # 将每个线程append进线程列表
            threads.append(t)

        ###### 将num_thread个线程join到主线程 #####
        for thread in threads:
            thread.join()

        ###### 获取结果 #####
        for _ in range(num_thread):
            results.append(q.get())
    return results

######################### main ########################
if __name___=='__main__':

    ########## generate data ##########
    data = [[1, 2, 3], [3, 4, 5], [5, 6, 7], [7, 8, 9], [9, 10, 11], [11, 12, 13]]

    ########## multi threading ##########
    result = multithreading(data, num_thread=2)
    print(result)
```
