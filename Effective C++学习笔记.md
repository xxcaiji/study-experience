## Effective C++
####1.认为C++是联邦语言
(1)C++以C为基础 

(2)面向对象编程C++ , class、封装、继承、多态、虚拟函数 

(3)泛型编程C++ 

(4)STL函数库

####2.用const,enum,inline替换#define
尽量用const来替代define来定义常量

首先define无法定义作用域

当编译器不支持允许static成员声明初值时，可以利用enum来进行初值设定。例如enum{NumTurns = 5}

对于定义函数的宏，最好用inline替换定义函数例：

    #define CALL_WITH_MAX(A,B) F((a) > (b) ? (a) : (b))
可以替换成下述定义

    template<typename T>
    inline void callWithMax(const T& a , const T& b) 
    {
        f(a > b ? a : b);
    }

####3.尽量使用const
const在指针星号左边表示指针所指物为常量，在指针星号右边则表示这个指针为常量不能改变。

const的bitwise constness约束可以通过mutable释放掉，即可以修改此变量中的某一位。

####4.初始化对象
1.内置型对象进行手工初始化

2.构造函数使用成员初值列

3.避免跨编译单元的初始化次序问题，以local static对象替换non_local static对象。

####5.了解C++编写且调用那些函数
编译器可以自主为class创建default、copy构造函数和操作符，以及析构函数。

1.空类：

class Empty{};等价于

    class Empty {
    public:
        Empty(){...}                                 //default构造函数
        Empty(const Empty& rhs) {...}                //copy构造函数
        ~Empty() {...}                               //析构函数
        Empty& operator=(const Empty& rhs) {...}     //copy assignment操作符
    };

####6.不想使用编译器自动生成的函数要明确拒绝
1.为避免编译器自动创建你不允许出现的函数，需要自己编译此函数，然后将其设置为private

2.使用像Uncopyable这样的base class。

    class Uncopyable{
    protected:
        Uncopyable(){}
        ~Uncopyable(){}
    private:
        Uncopyable(const Uncopyable&);
        Uncopyable& operator = (const Uncopyable&);
    };

    class HomeForSale : private Uncopyabsle{
    ....
    ;

####7.为多态基类声明virtual析构函数

1.带有多态性质的base classes应该声明一个virtual的析构函数，如果class中存在virtual函数，那么析构函数就应该是virtual。

2.class的设计目的不是作为base class使用，或者不是为了具备多态性，那么就不应该有virtual析构函数。

####8.别让异常逃离析构函数
1.析构函数不能吐出异常，如果一个被析构函数调用的函数可能出现异常，析构函数应该要捕捉异常，然后吞下。

2.如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么class应该提供一个普通函数执行该操纵。

####9.绝不再构造和析构过程中调用virtual函数。
1.在构造和析构期间不要调用virtual函数，因为这类调用从不下降至derived class

####10.令 operator=返回一个reference to * this


####11.在 operator=中处理自我赋值
1.确保当对象自我赋值时 operator=有良好行为

2.确定任何函数如果操作一个以上的对象，其中多个对象是同一个对象时，其行为仍然正确。

####12.复制对象时勿忘其每一个成分
1.copying函数应该确保复制对象内的所有成员变量及所有base class成分

2.不要尝试以某个copying函数实现另一个copying函数，应该将共同技能放进第三个函数中提供给复制函数调用。

####13.以对象管理资源

1.为防止子园泄漏，使用RAII对象(资源取得时机便是初始化时机)，在构造函数中获得资源并在析构函数中释放资源。

2.两个常用的RAII class类分别是trl::shared_ptr 和auto_ptr。前者通常是较佳的选择，copy行为比较直观。后者的复制动作会使它被指向null。

####14.在资源管理类中小心coping行为。

1.复制RAII对象必须一起复制他所管理的资源，所以资源的copying行为决定了RAII对象的copying行为。

2.普遍而常见的RAII类的复制行为是抑制copying，施行引用计数法。

####15.在资源管理类中提供对原始资源的访问。
1.APIs往往要求访问原始资源，所以每一个RAII class应该提供一个取得其所管理之资源的办法

2.对原始资源的访问可能经由显示转换或隐士转换。一般而言显式转换比较安全，但隐式转换对客户比较方便。

####16.成对使用new和delete时要采用相同的形式。
1.如果在new表达式中使用了[]，在delete中也必须使用[]。这表示数组

####17.以独立语句将newed对象置入智能指针
1.以独立语句将newed对象置入智能指针内，否则，一旦异常被抛出，有可能导致难以察觉的资源泄露。

####18.让接口容易被正确使用，不易被误用

1.接口一致性，以及与内置类型的行为兼容。
2.阻止误用的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任