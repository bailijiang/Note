<!-- MarkdownTOC -->

- [1. STL 容器 迭代器 算法](#1-stl-容器-迭代器-算法)
- [2. string:](#2-string)
- [3. vector:](#3-vector)
- [4. deque vector sort 平均分练习](#4-deque-vector-sort-平均分练习)
- [5. stack queue](#5-stack-queue)
- [6. list 链表](#6-list-链表)
- [7. set/multiset 二叉树/红黑树](#7-setmultiset-二叉树红黑树)
- [8. pair:  lower_bound / upper_bound / equal_range](#8-pair-lower_bound--upper_bound--equal_range)
- [9. map/multimap](#9-mapmultimap)
- [10. vector multimap 员工分组练习](#10-vector-multimap-员工分组练习)
- [11. 容器中对象的浅拷贝问题](#11-容器中对象的浅拷贝问题)
- [12. class对象 函数化\(伪函数\)](#12-class对象-函数化伪函数)
- [13. 一元函数对象, 二元函数对象, 一元谓词, 二元谓词](#13-一元函数对象-二元函数对象-一元谓词-二元谓词)
- [14. class匿名对象](#14-class匿名对象)
- [15. `functional plus minus...`](#15-functional-plus-minus)
- [16. 函数对象适配器](#16-函数对象适配器)
- [17. 常用查找算法](#17-常用查找算法)
- [18. 常用排序算法](#18-常用排序算法)
- [19. lambda expression](#19-lambda-expression)
- [20. 常用拷贝替换算法](#20-常用拷贝替换算法)
- [21. 常用数字生成算法 numeric](#21-常用数字生成算法-numeric)
- [22. 常用集合算法\(交集/并集\)](#22-常用集合算法交集并集)
- [23. distance 两个迭代器的距离](#23-distance-两个迭代器的距离)
- [24. 演讲比赛练习](#24-演讲比赛练习)

<!-- /MarkdownTOC -->

<a id="1-stl-容器-迭代器-算法"></a>
#### 1. STL 容器 迭代器 算法
* STL中容器与算法是分离的
* 容器 ---迭代器---> 算法
* 容器提供iterator, 算法调用iterator
* 容器可以嵌套容器

<a id="2-string"></a>
#### 2. string: 
    - at()  越界会抛异常
    - find() 没找到 返回-输出

<a id="3-vector"></a>
#### 3. vector:
* 动态数组, 是单口容器, 在尾部插入删除效率高
* end() 不是指向最后一个元素, 他是指向最后一个元素的下一个地址
* swap小技巧: capacity适配
```
    #define _CRT_SECURE_NO_WARNINGS

    #include<iostream>
    #include<string>
    #include<vector>

    using namespace std;


    void printV(vector<int> &v) {
        for each (int x in v)
        {
            cout << x << " ";
        }
        cout << endl;
    }

    void test01() {
        vector<int> v;
        v.resize(100);
        v.clear();
        v.push_back(2);
        v.push_back(3);
        cout << "size: " << v.size() << endl;
        cout << "capacity: " << v.capacity() << endl;

        vector<int>(v).swap(v);
        cout << "size: " << v.size() << endl;
        cout << "capacity: " << v.capacity() << endl;

    }


    int main(void)
    {
        test01();

        return 0;
    }
```
```
size: 2
capacity: 100
size: 2
capacity: 2
请按任意键继续. . .
```

<a id="4-deque-vector-sort-平均分练习"></a>
#### 4. deque vector sort 平均分练习
```
#define _CRT_SECURE_NO_WARNINGS

#include<iostream>
#include<string>
#include<vector>
#include<deque>
#include<algorithm>

using namespace std;

class Player
{
public:
    Player(string name, int score): name(name), score(score){}
    int score;
    string name;
};

void createPlayer(vector<Player> &pList) {
    char nameseed[] = "ABCDE";
    for (int i = 0; i < 5; i++) {
        string playername = "player";
        playername += nameseed[i];
        Player p = Player(playername, 0);
        pList.push_back(p);
    }
}

bool myCompare(int a, int b) {
    if (a > b) {
        return true;
    }
    else
    {
        return false;
    }
}

void setScore(vector<Player> &pList) {
    for (vector<Player>::iterator it = pList.begin(); it != pList.end(); it++) {
        int avgScore = 0;
        int totalScore = 0;

        deque<int> dscore;
        for (int i = 0; i < 10; i++) {
            int score = 50 + rand() % 50;
            dscore.push_back(score);
        }
        sort(dscore.begin(), dscore.end(), myCompare);
        dscore.pop_front();
        dscore.pop_back();

        //for (int i = 0; i < (int)dscore.size(); i++) {
        //  totalScore += dscore[i];
        //}
        for (deque<int>::iterator it = dscore.begin(); it != dscore.end(); it++) {
            totalScore += *it;
        }
        avgScore = totalScore / dscore.size();


        it->score = avgScore;
    }

}

void printV(vector<Player> &v) {
    for (vector<Player>::iterator it = v.begin();it != v.end(); it++) {
        cout << "name: " << it->name <<" ";
        cout << "score: " << it->score << endl;
    }
}

int main(void)
{
    vector<Player> pList;
    createPlayer(pList);
    setScore(pList);
    printV(pList);

    return 0;
}
```

<a id="5-stack-queue"></a>
#### 5. stack queue
* stack 后进先出 LIFO
* queue 先进先出 FIFO
* stack/queue 无迭代器

<a id="6-list-链表"></a>
#### 6. list 链表
* 单向链表, 双向链表
* 非连续, 动态分配内存
* 添加/插入/删除 效率高, 查找/遍历效率低
```
#define _CRT_SECURE_NO_WARNINGS

#include<iostream>
#include<string>
#include<vector>
#include<deque>
#include<algorithm>
#include<stack>
#include<queue>
#include<list>

using namespace std;

class Teacher {
    friend bool compare_Teacher(Teacher &t1, Teacher &t2);
public:
    Teacher(string name, int age) :name(name), age(age) {}
    
    friend ostream& operator<<(ostream &os, Teacher &t) {
        os << "name: " << t.name << " age: " << t.age << endl;
        return os;
    }
private:
    string name;
    int age;
};

template<class T>
void printList_t(list<T> &l) {
    for (T n : l) {
        cout << n;
    }
    cout << endl;
}

void printList(list<int> &l) {
    for (int n : l) {
        cout << n;
    }
    cout << endl;
}

bool myCompare(int a, int b) {
    return a > b;
}

bool compare_Teacher(Teacher &t1, Teacher &t2) {
    return t1.age > t2.age;
}

void test01() {
    list<int> l = { 3,5,8,9,7,2,1 };
    l.push_back(11);
    l.push_front(22);
    printList_t<int>(l);

    l.pop_back();
    l.pop_front();
    printList_t<int>(l);
    list<int>::iterator it = l.begin();
    it++;
    l.insert(it, 44);
    printList_t<int>(l);

    l.remove(44);
    printList_t<int>(l);

    l.sort();
    printList_t<int>(l);
    l.sort(myCompare);
    printList_t<int>(l);

    list<Teacher> tList;
    Teacher t1("zhang3", 30), t2("li4", 23), t3("wang5", 28);
    tList.push_back(t1);
    tList.push_back(t2);
    tList.push_back(t3);
    printList_t<Teacher>(tList);

    tList.sort(compare_Teacher);
    printList_t<Teacher>(tList);

}


int main(void)
{
    test01();

    return 0;
}
```

<a id="7-setmultiset-二叉树红黑树"></a>
#### 7. set/multiset 二叉树/红黑树
* set 查找效率高
* multiset 可以插入重复值
* set 排序自定义数据类型
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include <set>
#include<string>

using namespace std;

struct Teacher
{
    int id;
    int age;
    string name;
    Teacher(int id, int age, string name): id(id), age(age), name(name){}
};
// 伪函数
class SortTeacher
{
public:
    bool operator()(const Teacher &t1, const Teacher &t2) const {
        if (t1.age > t2.age) {
            return true;
        }
        else
        {
            return false;
        }
    }
};


int main()
{
    
    set<Teacher, SortTeacher> teachers;
    Teacher t1(1, 30, "范冰冰"), t2(2, 19, "张柏芝"), t3(3, 38, "陈冠希");
    teachers.insert(t1);
    teachers.insert(t2);
    teachers.insert(t3);

    set<Teacher, SortTeacher>::iterator iter;
    iter = teachers.find(t2);
    if (iter == teachers.end()) {
        cout << "not found" << endl;
    }
    else
    {
        cout << iter->name << endl;
    }


    return 0;
}
```

<a id="8-pair-lower_bound--upper_bound--equal_range"></a>
#### 8. pair:  lower_bound / upper_bound / equal_range
* lower_bound: 包括  ]
* upper_bound: 不包括  (
* equal_range: (lower_bound, upper_bound) ==> ( ]
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include <set>
#include<string>

using namespace std;


void test01() {
    set<int> s1;
    s1.insert(10);
    s1.insert(5);
    s1.insert(3);
    s1.insert(6);
    s1.insert(2);

    set<int>::iterator pos = s1.lower_bound(5);
    if (pos == s1.end()) {
        cout << "not found" << endl;
    }
    else
    {
        cout << *pos << endl;
        
    }
    cout << "----" << endl;
    pos = s1.upper_bound(5);
    if (pos == s1.end()) {
        cout << "not found" << endl;
    }
    else
    {
        cout << *pos << endl;
    }
    cout << "-----" << endl;
    pair<set<int>::iterator, set<int>::iterator> ret;
    ret = s1.equal_range(5);
    cout << *ret.first << endl;
    cout << *ret.second << endl;

}

int main()
{
    
    test01();

    return 0;
}
```


<a id="9-mapmultimap"></a>
#### 9. map/multimap 
* 基于红黑树
* map 4种insert方式:
```
void test01() {
    map<int, int> map1;
    
    map1.insert(pair<int, int>(4, 5));
    map1.insert(make_pair(2, 10));
    map1.insert(map<int, int>::value_type(3, 12));
    map1[4] = 20; // key重复时会修改value值

    for (map<int, int>::iterator it = map1.begin(); it != map1.end(); it++)
    {
        cout << "key: " << it->first << " value: " << it->second << endl;
    }

    pair<map<int, int>::iterator, bool> ret;
    ret = map1.insert(make_pair(2, 10));
    if (ret.second) {
        cout << "map1 insert success" << endl;
    }
    else
    {
        cout << "map1 insert failed" << endl;
    }
}
```


```
Example (key: mymap['d'])
// accessing mapped values
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main ()
{
  map<char,string> mymap;
  map<char,string>::iterator it;

  mymap['a']="an element";
  mymap['b']="another element";
  mymap['c']=mymap['b'];

  cout << "mymap['a'] is " << mymap['a'] << endl;
  cout << "mymap['b'] is " << mymap['b'] << endl;
  cout << "mymap['c'] is " << mymap['c'] << endl;
  cout << "mymap['d'] is " << mymap['d'] << endl;

  cout << "mymap now contains " << (int) mymap.size() << " elements." << endl;

  return 0;
}
 

Notice how the last access (to element 'd') inserts a new element in the set with that key and initialized to its default value (an empty string) even though it is accessed only to retrieve its value. Member function map::find does not produce this effect.
Output:
mymap['a'] is an element
mymap['b'] is another element
mymap['c'] is another element
mymap['d'] is
mymap now contains 4 elements. 
```

<a id="10-vector-multimap-员工分组练习"></a>
#### 10. vector multimap 员工分组练习
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include <map>
#include<string>
#include<vector>

using namespace std;

enum Department
{
    Sale, Develop, Finacial
};
struct Employee
{
    string name;
    int age;
    string phone;
    double salary;
    void printEmp() {
        cout << "name: " << name << " age: " << age << " phone: " << phone << " salary: " << salary << endl;
    }
};

// create 5 emp
void createEmp(vector<Employee> &emps) {
    struct Employee e;
    char nameseed[] = "ABCDE";
    for (int i = 0; i < 5; i++) {
        e.age = 20 + rand() % 20;
        e.name = "emp";
        e.name += nameseed[i];
        e.phone = "13888888888";
        e.salary = 10000 + rand() % 20000;

        emps.push_back(e);
    }
}

// assign department
void assignDepartment(vector<Employee> &emps, multimap<int, Employee> &department_group) {
    int depa_id = -1;
    for each (Employee e in emps)
    {
        e.printEmp();

        while (true)
        {
            cout << "Please input department ID(0 Sale, 1 Develop, 2 Finacial): ";
            cin >> depa_id;
            if (depa_id < 0 || depa_id>2) {
                continue;
            }
            break;
        }

        switch (depa_id)
        {
        case Sale:
            department_group.insert(make_pair(Sale, e));
            break;
        case Develop:
            department_group.insert(make_pair(Develop, e));
            break;
        case Finacial:
            department_group.insert(make_pair(Finacial, e));
            break;
        default:
            break;
        }
    }
}

void show_emp_info(multimap<int, Employee> &department_group) {
    int depa_id = -1;
    int count_depa_emp = 0;
    while (true)
    {
        cout << "Please input department ID(0 Sale, 1 Develop, 2 Finacial): ";
        cin >> depa_id;
        if (depa_id < 0 || depa_id>2) {
            continue;
        }
        break;
    }

    multimap<int, Employee>::iterator pos = department_group.find(depa_id);
    count_depa_emp = department_group.count(depa_id);
    for (int i = 0; i < count_depa_emp; i++) {
        (pos->second).printEmp();
        pos++;
    }

}


int main()
{
    vector<Employee> emps;
    createEmp(emps);

    multimap<int, Employee> department_group;
    assignDepartment(emps, department_group);

    show_emp_info(department_group);

    return 0;
}
```

<a id="11-容器中对象的浅拷贝问题"></a>
#### 11. 容器中对象的浅拷贝问题
* 容器中的对象如果有在堆上开辟空间的元素(指针), push_back操作时就会产生浅拷贝问题
    - 解决方法:
        + 需要手动提供拷贝构造函数实现深拷贝
        + 并且实现operator=号操作符重载实现深拷贝
        + 显示提供析构函数手动释放内存delete[]

<a id="12-class对象-函数化伪函数"></a>
#### 12. class对象 函数化(伪函数)
* 在 algorithm的 for_each 中使用, 可作为参数 和 返回值, 记录对象运行状态, __从而避免定义全局变量__
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<vector>
#include<algorithm>

using namespace std;

class printT {
public:
    printT(): count(0){}
    void operator()(int v) {
        count++;
        cout << v << " ";
    }
    int count;
};

void test01() {
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
    }
    printT p1;
    printT p2 = for_each(v.begin(), v.end(), p1);
    cout << "p1 count: " << p1.count << endl;
    cout << "p2 count: " << p2.count << endl;
}

int main()
{
    test01();

    return 0;
}

0 1 2 3 4 5 6 7 8 9 p1 count: 0
p2 count: 10
请按任意键继续. . .
```

<a id="13-一元函数对象-二元函数对象-一元谓词-二元谓词"></a>
#### 13. 一元函数对象, 二元函数对象, 一元谓词, 二元谓词
* __主要针对 for_each / find_if / transform / sort__
* 函数对象: class实例, 实现operator()重载的伪函数
* 函数对象接收一个参数，那么就叫 一元函数对象: __for_each__ 中使用
* 函数对象接收两个参数，那么叫 二元函数对象

* 谓词：
    - 一元函数对象：函数参数1个
    - 二元函数对象：函数参数2个
    - 一元谓词 函数参数1个，函数返回值是bool类型，可以作为一个判断式
    - 谓词可以使一个仿函数，也可以是一个回调函数
* 函数对象或者普通函数(回调函数)接收一个参数，并且返回值是Bool，叫一元谓词: __算法中需要进行 bool判定的辅助函数__, __find_if__中使用
* 函数对象或者普通函数(回调函数)接收二个参数，并且返回值是Bool，叫二元谓词
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<vector>
#include<algorithm>

using namespace std;

class MyCompare {
public:
    bool operator()(int v) {
        if (v > 7) {
            return true;
        }
        else
        {
            return false;
        }
    }
};

void test01() {
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
    }

    vector<int>::iterator it;
    it = find_if(v.begin(), v.end(), MyCompare());
    if (it == v.end()) {
        cout << "not found" << endl;
    }
    else
    {
        cout << "found: " << *it << endl;
    }
    
}

int main()
{
    test01();

    return 0;
}
```
* transform
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<vector>
#include<algorithm>

using namespace std;


class Add
{
public:
    int operator()(int a, int b) {
        return a + b;
    }
};

void test_transform() {
    vector<int> v1, v2, v3;
    for (int i = 0; i < 10; i++) {
        v1.push_back(i);
        v2.push_back(i + 1);
    }
    v3.resize(v1.size());
    vector<int>::iterator it;
    
    transform(v1.begin(), v1.end(), v2.begin(), v3.begin(), Add());

    for each (int x in v3)
    {
        cout << x << " ";
    }
    cout << endl;
}

int main()
{
    test_transform();

    return 0;
}
```



<a id="14-class匿名对象"></a>
#### 14. class匿名对象
```
class A {...};
A a;
A(); // 匿名对象
```

<a id="15-functional-plus-minus"></a>
#### 15. `functional plus<T> minus<T>...`
* 预定义操作符实现函数对象重载(仿函数)
* 自定义class对象, 使用 functional 中的 `plus<T>, minus<T>` 等
* `plus<T> 需要重载 operator()(...), friend operator+(...)`
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<vector>
#include<algorithm>
#include<string>
#include<functional>

using namespace std;
/*
// TEMPLATE STRUCT plus
template<class _Ty = void>
struct plus
{   // functor for operator+
typedef _Ty first_argument_type;
typedef _Ty second_argument_type;
typedef _Ty result_type;

constexpr _Ty operator()(const _Ty& _Left, const _Ty& _Right) const
{   // apply operator+ to operands
return (_Left + _Right);
}
};
*/

class Student :public binary_function<const Student, const Student, Student>
{
public:
    friend Student operator+(const Student &s1, const Student &s2);

    Student(int age, string name): age(age), name(name){}
    void printStu() {
        cout << "age: " << age << " name: " << name << endl;
    }
    Student operator()(const Student &s1, const Student &s2) {
        return s1 + s2;
    }
private:
    int age;
    string name;
};

Student operator+(const Student &s1, const Student &s2) {
    Student temp = Student(s1.age + s2.age, s1.name + s1.name);
    return temp;
}


void test_plusT() {

    Student stu1(20, "xiaoming");
    Student stu2(22, "xiaowang");

    plus<Student> stuPlus;
    
    Student temp = stuPlus(stu1, stu2);
    temp.printStu();
}

int main()
{
    test_plusT();

    return 0;
}
```

<a id="16-函数对象适配器"></a>
#### 16. 函数对象适配器
* functional, binary_function, bind1st, bind2nd
    - bind1st: 将实参绑定到第1个参数上, 迭代器对象绑定到第2个参数
    - bind2st: 将实参绑定到第2个参数上, 迭代器对象绑定到第1个参数
    - bind1st bind2nd绑定适配器  调用绑定适配器，统统变成一元函数对象
* unary_function, not1, not2
    - not1: 1个参数取反 
    - not2: 2个参数取反 
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<vector>
#include<algorithm>
#include<string>
#include<functional>

using namespace std;

class Print: public binary_function<int, int, bool> 
{
public:
    bool operator()(int v1, int v2) const{
        //cout << "v1: " << v1 << " v2: " << v2 << endl;

        // bind2nd
        if (v1 > v2) {
            cout << v1 << " ";
            return true;
        }
        else
        {
            return false;
        }
    }
};

void test_bind() {
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
    }
    Print p;
    /*cout << "bind1st:" << endl;
    for_each(v.begin(), v.end(), bind1st(p, 5));*/
    cout << "bind2nd" <<  endl;
    for_each(v.begin(), v.end(), bind2nd(p, 5));
    cout << endl;
}

struct MyCompare: public unary_function<int, bool> {
    bool operator()(int v) const{
        return v < 5;
    }
};

void test_not() {
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
    }

    vector<int>::iterator pos;
    pos = find_if(v.begin(), v.end(), not1(MyCompare()));
    if (pos != v.end()) {
        cout << "found: " << *pos << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }

    cout << "---sort---" << endl;
    sort(v.begin(), v.end(), not2(greater<int>()));
    for each (int val in v)
    {
        cout << val << " ";
    }
    cout << endl;


}

int main()
{
    //test_bind();
    test_not();

    return 0;
}
```
* 对普通函数进行绑定参数
    - 需要将普通函数转化为函数对象 ptr_fun
    - 可以用bind1st/bind2nd适配器了
```
void print_ptr(int v1, int v2) {
    //cout << "v1: " << v1 << " v2: " << v2 << endl;
    if (v1 > v2) {
        cout << v1 << " ";
    }
}

void test_ptr_func() {
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
    }

    for_each(v.begin(), v.end(), bind2nd(ptr_fun(print_ptr), 5));
    cout << endl;
}
```

* mem_fun / mem_fun_ref
    - 容器中放的是类对象实体& 用mem_fun_ref
    - 容器中放的是类对象指针* 用mem_fun
```
class Teacher 
{
public:
    Teacher(int id, int age): id(id), age(age){}
    int age;
    int id;
    void printT() {
        cout << this->id << " " << this->age << endl;
    }
};

void test_mem_fun() {
    vector<Teacher> v;
    Teacher t1(1, 30), t2(2, 26), t3(3, 33);
    v.push_back(t1);
    v.push_back(t2);
    v.push_back(t3);

    for_each(v.begin(), v.end(), mem_fun_ref(&Teacher::printT));
    cout << "----" << endl;

    vector<Teacher*> v2;
    Teacher *tp1 = &t1;
    Teacher *tp2 = &t2;
    Teacher *tp3 = &t3;
    v2.push_back(tp1);
    v2.push_back(tp2);
    v2.push_back(tp3);

    for_each(v2.begin(), v2.end(), mem_fun(&Teacher::printT));

}
```

<a id="17-常用查找算法"></a>
#### 17. 常用查找算法
* find : 查找对象时, 要重载 operator==(){...} 操作符
* adjacent_find: 查找相邻重复对象, 返回第一个
* binary_find: 二分查找, 需要先排序

<a id="18-常用排序算法"></a>
#### 18. 常用排序算法
* 需要支持随机访问: vector, map/multimap
* merge : 2in1 需要先排序(相同原则)
* random_shuffle:
```
void test_random_shuffle() {
    vector<int> v;
    v.push_back(3);
    v.push_back(2);
    v.push_back(4);
    v.push_back(7);
    v.push_back(1);
    for_each(v.begin(), v.end(), [](int v)->void {cout << v << " "; });
    cout << endl;
    sort(v.begin(), v.end(), greater<int>());
    for_each(v.begin(), v.end(), [](int v)->void {cout << v <<" "; });
    cout << endl;
    random_shuffle(v.begin(), v.end());
    for_each(v.begin(), v.end(), [](int v)->void {cout << v << " "; });
    cout << endl;
}
```

<a id="19-lambda-expression"></a>
#### 19. lambda expression
* `[capture list] (parameter list) -> return type {function body}`
```
[](int v)->void{cout << v << endl;};
```

<a id="20-常用拷贝替换算法"></a>
#### 20. 常用拷贝替换算法
* copy / replace / replace_if / swap

<a id="21-常用数字生成算法-numeric"></a>
#### 21. 常用数字生成算法 numeric
* accumulate / fill
```
void test_accumulate() {
    vector<int> v;
    for (int i = 0; i <= 100; i++) {
        v.push_back(i);
    }

    int n = accumulate(v.begin(), v.end(), 100);
    cout << "n: " << n << endl;
}

void test_fill(){
    vector<int> v;
    v.resize(10);
    fill(v.begin(), v.end(), 100);
    cout << "size: " << v.size() << endl;
    for_each(v.begin(), v.end(), [](int v)->void {cout << v << " "; });
    cout << endl;
}
```

<a id="22-常用集合算法交集并集"></a>
#### 22. 常用集合算法(交集/并集)
* set_intersection
```
void test_set_intersection() {
    vector<int> v1, v2, v3;
    for (int i = 0; i <= 10; i++) {
        v1.push_back(i);
    }
    for (int i = 0; i <= 15; i++) {
        v2.push_back(i);
    }
    v3.resize(min(v1.size(), v2.size()));
    vector<int>::iterator v3end = set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
    for_each(v3.begin(), v3end, [](int v)->void {cout << v << " "; });
}
```
* set_union:
```
void test_set_union() {
    vector<int> v1, v2, v3;
    for (int i = 0; i <= 10; i++) {
        v1.push_back(i);
    }
    for (int i = 0; i <= 15; i++) {
        v2.push_back(i);
    }
    v3.resize(v1.size() + v2.size());
    vector<int>::iterator v3end = set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
    for_each(v3.begin(), v3end, [](int v)->void {cout << v << " "; });
}
```
* set_difference:
```
void test_set_difference()
{
    vector<int> v1, v2, v3;
    for (int i = 0; i < 10; i++) {
        v1.push_back(i);
    }

    for (int i = 5; i < 15; i++) {
        v2.push_back(i);
    }
    v3.resize(v1.size());
    vector<int>::iterator v3end = set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
    for_each(v3.begin(), v3end, [](int v)->void {cout << v << " "; });
}
```

<a id="23-distance-两个迭代器的距离"></a>
#### 23. distance 两个迭代器的距离
```
void test_distance() {
    vector<int> v;
    for (int i = 10; i < 15; i++) {
        v.push_back(i);
    }
    int dis = distance(v.begin(), v.end());
    cout << "dis:" << dis << endl;
}
```

<a id="24-演讲比赛练习"></a>
#### 24. 演讲比赛练习
* 需求:
    - 24个选手, 每6个一组进行打分, 共比赛3轮, 每轮的每组前3名晋级到下一轮
    - 10个裁判打分, 去掉最高,最低分,算平均分
    - 每个选手, 保存3轮比赛的平均分
    - 第1轮: 共4组, 晋级12人
    - 第2轮: 共2组, 晋级6人
    - 第3轮: 共1组, 晋级3人
    - 容器: vector, map, multimap, deque
    - 算法: sort, random_shuffle, greater
    - include: vector, map, deque, algorithm, numeric, functional, string
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<vector>
#include<algorithm>
#include<string>
#include<functional>
#include<numeric>
#include<map>
#include<deque>

using namespace std;

struct Speaker {
    string name;
    int score[3]; // 0: 1st  1: 2nd  2: 3rd
};

void create_speakers(map<int, Speaker> &speakers, vector<int> &v) {
    string nameseed = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    for (int i = 0; i < 24; i++) {
        Speaker s;
        s.name = "speaker";
        s.name += nameseed[i];
        s.score[0] = 0;
        s.score[1] = 0;
        s.score[2] = 0;
        int s_ID = 100 + i;
        v.push_back(s_ID);
        speakers.insert(make_pair(s_ID, s));
    }
}

void random_no(vector<int> &v) {
    random_shuffle(v.begin(), v.end());
}

void speaker_match(int round, vector<int> &v_in, map<int, Speaker> &speakers, vector<int> &v_out) {
    multimap<int, int, greater<int>> score_group; // key: s_ID, value: avg_score
    int s_num = 0;
    int s_group = 0;

    for (vector<int>::iterator it = v_in.begin(); it != v_in.end(); it++) {
        s_num++;
        deque<int> scores;
        int score = 0;
        int total_score = 0;
        int avg_score = 0;
        int top3_count = 0;

        for (int i = 0; i < 10; i++) {
            score = 50 + rand() % 50;
            scores.push_back(score);
        }
        sort(scores.begin(), scores.end());
        scores.pop_back();
        scores.pop_front();
        total_score = accumulate(scores.begin(), scores.end(), 0);
        avg_score = total_score / scores.size();

        speakers[*it].score[round] = avg_score;
        score_group.insert(make_pair(*it, avg_score));

        if (s_num % 6 == 0) {
            s_group++;
            cout << "Group: " << s_group << endl;

            for (multimap<int, int, greater<int>>::iterator it = score_group.begin(); it != score_group.end(); it++) {
                top3_count++;
                if (top3_count <= 3) {
                    v_out.push_back(it->first);
                cout << "name: " << speakers[it->first].name << " " << " score: " << speakers[it->first].score[round] << endl;
                }
            }
            score_group.clear();

        }

    }
}


void show_result(int round, vector<int> &v, map<int, Speaker> &speakers) {
    cout << "round: " << round << endl;
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << "No: " << *it << " ";
        cout << "name: " << speakers[*it].name << " ";
        cout << "score: " << speakers[*it].score[round] << endl;
    }
}

int main()
{
    // create 24 speaker
    map<int, Speaker> speakers;
    vector<int> v;
    vector<int> v1; // speaker No in 1st round result 
    vector<int> v2;
    vector<int> v3;

    create_speakers(speakers, v);

    // 1st round match 
    random_no(v);
    speaker_match(0, v, speakers, v1);
    show_result(0, v1, speakers);
    

    // 2nd round match 
    random_no(v1);
    speaker_match(1, v1, speakers, v2);
    show_result(1, v2, speakers);

    // 3rd round match 
    random_no(v2);
    speaker_match(2, v2, speakers, v3);
    show_result(2, v3, speakers);

    return 0;
}
```





