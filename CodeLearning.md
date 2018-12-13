# CodeLearning

记录一些代码的写法

从技术层面, 而非格式层面



## 1.设计模式

### 1.1 工厂设计模式

```c++
class RjpFactory
{
public:
    virtual ~RjpFactory(){}
    virtual RjpShape* createShape(QString filepath) = 0;
    static RjpFactory* getInstance();
protected:
    RjpFactory(){}
    static RjpFactory* _instance;

};

// Usage
// ESRIFactory::getInstance->createShape(filepath)
// the variable 'static RjpFactory* _instance'
// becomes ESRIFactory
class ESRIFactory : public RjpFactory{
public:
    ESRIFactory(){}
    virtual ~ESRIFactory(){}
    virtual RjpShape* createShape(QString filepath);
};

class StaticFactory{
public:
    static RjpShape* createShape(QString filepath);
};
```

子类的私有变量自动变为子类, 而非父类

此处也需要恶补C++知识

### 1.2 单例设计模式

```c++
// Usage
// SingletonShapes::getGlobalData()->function
class SingletonShapes{
public:
    static RjpShapes* getGlobalData();
    static void releaseGlobalData();
private:
    static RjpShapes* _global_data;
};
```

```c++
RjpShapes* SingletonShapes::getGlobalData(){
    if (!_global_data){	// Make sure only get one instance
        _global_data = new RjpShapes();
    }
    return _global_data;
}
```

只要包括了头文件或者使用命名空间, 就可以快速地在该出实例化出来数据

而且数据只会实例化一份

### 1.3 模板类

```c++
class ShapesObCommand{
public:
    virtual void execute() = 0;
protected:
    ShapesObCommand(){}
};

template<class Receiver>
class ShapesObCommandTemplate : public ShapesObCommand{
public:
    typedef void (Receiver::* Action)();
    ShapesObCommandTemplate(Receiver* r, Action a):
        _receiver(r),
        _action(a)
    {}
    virtual void execute();
private:
    Action _action;
    Receiver* _receiver;
};

template<class Receiver>
void ShapesObCommandTemplate<Receiver>::execute(){
    (_receiver->* _action)();
}
```

此处需要恶补C++基础知识

## 2.其他

### 2.1 回调函数中纯虚函数的调用

`ultrasound_can_parser` 是 `pthread_base` 的子类

```c++
// pthread_base.h
virtual void run() = 0;
int start();
static void *start_func(void *arg);

// pthread_base.cpp
void *ThreadBase::start_func(void *arg) {
    ThreadBase *ptr = (ThreadBase *) arg;   
    ptr->run();

    return NULL;
}

// pthread_base.cpp
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

```c++
// ultrasound_can_parser.h
 void run() override; 
 
// ultrasound_can_parser.cpp   
void UltraSoundCanPaser::run() {
   init();
   int m_run0=1;
   receive_func(&m_run0);	// listen to the port
}
```

```c++
// instance 
UltraSoundCanParser uscp;
uscp.start();
```

其过程为:

子类调用父类的 `start` 函数, 创建新的线程, 在回调函数中调用 `start_func`, 因为此时的子类调用, 所以我猜测在 `start_func` 实例化出来的父类指针指向的是一个子类实例, 调用子类的 `run` 函数, 实现监听端口数据