更加细节的部分都写到了注释里面

### controlcan部分

`controlcan.h` 和`libcontrolcan.so` 是已经集成在CAN2USB中的程序

在所给的pdf文档中, 各个函数的注释非常清楚



### pthread_base.h部分

吴睿学长所写, 得把它看明白

把 `pthread.h` 自己封装了一遍



觉得吴睿好像把 `pthread_join` 函数的用法搞错了

有点望文生义的味道

`pthread_join` 是等待线程结束的函数, 可在他的 `cout` 里面, 成了加入的意思

2333333



### ultrasound_can_parser部分

`init` 函数: `rospublisher` 初始化, 硬件的初始化



`receive_func` 函数: 

 	1. 开启线程, 接收*原始数据*, 使用集成在硬件内部的函数
 	2. 调用 `sendRangeMsg` 函数, 把接收到的 *原始数据* 给 `parseCanData` 函数
 	3. `parseCanData` 函数处理接收的 *原始数据* , 转换为 *可发出数据*
 	4. `generateRangeMsg` 函数调用`publisher_`把 *可发出数据* 发出 



### main函数部分

调用的方法全是父类的方法

但是却隐含的调用了子类的方法



在父类声明 `virtual = 0` 的纯虚函数 `run`

在子类中定义的 `run` 中调用 `init` 和 `receive_func`

这两个函数可以实现所有需要的功能



### 不懂的地方

使用 `pthread_create` 创造一个新的线程

新的线程调用 `start_func` 函数

```c++
int ThreadBase::start() {
    cout << "Start a new thread" << endl;
    if (pthread_create(&m_tid, NULL, start_func, (void *) this) != 0) {
        cout << "Start a new thread failed!" << endl;
        return -1;
    }

    cout << "Start a new thread success! tid=" << m_tid << endl;
    m_isAlive = true;
    return 0;
}
```

`start_func` 的参数是根据 `pthread_create` 的参数定

但是不懂的地方出现了

```c++
void *ThreadBase::start_func(void *arg) {
    ThreadBase *ptr = (ThreadBase *) arg;   
    ptr->run();

    return NULL;
}
```

注意 `run` 函数

```c++
virtual void run() = 0;
```



所以现在的逻辑是

有一个 `Class Rjp` , 中有一个纯虚函数 `run` , 还有一个普通函数 `work`

现在在 `work` 里面实例化了一个 `Rjp` , 还调用了 `run` 函数



这就是我没有看懂的地方 