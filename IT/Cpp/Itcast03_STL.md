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
- [25. list双向链表操作](#25-list双向链表操作)
- [26. priority_queue优先队列操作](#26-priority_queue优先队列操作)
- [27. iterator迭代器使用](#27-iterator迭代器使用)
- [28. 使用iterator迭代器在顺序容器中查找](#28-使用iterator迭代器在顺序容器中查找)
- [29. size_type类型](#29-size_type类型)
- [30. 顺序容器的insert操作](#30-顺序容器的insert操作)
- [31. 顺序容器的查找删除find/erase](#31-顺序容器的查找删除finderase)
- [32. 顺序容器的交换swap和赋值assign](#32-顺序容器的交换swap和赋值assign)
- [33. string字符串的查找](#33-string字符串的查找)
- [34. map/multimap操作](#34-mapmultimap操作)
- [35. set/multiset操作](#35-setmultiset操作)
- [36. 函数对象/class实例函数对象的使用](#36-函数对象class实例函数对象的使用)
- [37. count/count_if计数器操作](#37-countcount_if计数器操作)
- [38. max/min计算最大最小值](#38-maxmin计算最大最小值)
- [39. search_n查找算法](#39-search_n查找算法)
- [40. search/find_end在容器中查找另一个容器](#40-searchfind_end在容器中查找另一个容器)
- [41. 用find_first_of在ivec中查找searchList中的任一个对象](#41-用find_first_of在ivec中查找searchlist中的任一个对象)
- [42. adjacent_find查找2个连续的](#42-adjacent_find查找2个连续的)
- [43. 对已序区间进行查找binary_search/includes](#43-对已序区间进行查找binary_searchincludes)
- [44. lower_bound/upper_bound/equal_range 查找算法应用于顺序容器](#44-lower_boundupper_boundequal_range-查找算法应用于顺序容器)
- [45. for_each算法使用](#45-for_each算法使用)
- [46. 区间\(容器\)比较equal/mismatch/lexicographical_compare](#46-区间容器比较equalmismatchlexicographical_compare)
- [47. 复制元素copy/copy_backward算法](#47-复制元素copycopy_backward算法)
- [48. transform变换算法使用](#48-transform变换算法使用)
- [49. for_each与transform的比较](#49-for_each与transform的比较)
- [50. swap_ranges/swap算法使用](#50-swap_rangesswap算法使用)
- [51. fill/fill_n/generate/generate_n填充算法](#51-fillfill_ngenerategenerate_n填充算法)
- [52. replace/replace_if/replace_copy/replace_copy_if替换算法的使用](#52-replacereplace_ifreplace_copyreplace_copy_if替换算法的使用)
- [53. 删除算法使用](#53-删除算法使用)
- [54. reverse/rotate旋转算法](#54-reverserotate旋转算法)
- [55. next_permutation/prev_permutation排列组合算法](#55-next_permutationprev_permutation排列组合算法)

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
        //去掉最高,最低分
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

<a id="25-list双向链表操作"></a>
#### 25. list双向链表操作
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>

using namespace std;

void printIntListContent(const list<int> &listInput)
{
    cout << endl;
    cout << "{ ";
    list<int>::const_iterator iter;

    for (iter = listInput.begin(); iter != listInput.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << " }";
    cout << endl;

}

int main(int argc, char *argv[])
{

    list<int> l1;
    l1.push_back(7);
    l1.push_back(8);
    l1.push_back(9);
    l1.push_back(10);
    l1.push_front(2);
    l1.push_front(1);

    list<int>::iterator iter = l1.begin();
    iter++;

    l1.insert(iter, 88);

    l1.erase(l1.begin());

    list<int>::iterator itr;
    for (itr = l1.begin(); itr != l1.end(); ++itr)
    {
        cout << *itr << endl;
    }


    cout << "--------------" << endl;

    printIntListContent(l1);

    l1.reverse();
    printIntListContent(l1);

    l1.sort();
    printIntListContent(l1);

    return 0;
}

```

<a id="26-priority_queue优先队列操作"></a>
#### 26. priority_queue优先队列操作
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;

int main(int argc, char *argv[])
{

    priority_queue<int> pq1; // default vector
    priority_queue<int, deque<int>> pq2;

    pq1.push(20);
    pq1.push(88);
    pq1.push(-9);
    pq1.push(34);

    cout << pq1.top() << endl;

    priority_queue<int, vector<int>, greater<int> > pq3;

    return 0;
}

```

<a id="27-iterator迭代器使用"></a>
#### 27. iterator迭代器使用
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;

int main(int argc, char *argv[])
{

    vector<int> a;
    deque<int> b;
    list<int> c;

    a.push_back(1);
    a.push_back(2);
    a.push_back(3);
    a.push_back(4);
    a.push_back(5);

    vector<int>::iterator first = a.begin();
    vector<int>::iterator last = a.end(); // end() 指向最后一个的下一个
                                           // 目的: 为了循环条件设置方便,前包后不包 [ )

    while (first != last)
    {
        cout << *first << endl;
        first++;
    }

    vector<int>::iterator m = a.begin() + a.size() / 2;
    cout << "middle: " << *m << endl;

    return 0;
}

```

<a id="28-使用iterator迭代器在顺序容器中查找"></a>
#### 28. 使用iterator迭代器在顺序容器中查找
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;

//在begin和end的范围内找x, 返回迭代器位置, 如果找到begin!=end, 如果没找到begin==end
vector<int>::iterator findIntInVector(vector<int>::iterator begin,
                                      vector<int>::iterator end,
                                      int x)
{
    while (begin!=end)
    {
        if (*begin == x)
            break;
        else
            begin++;
    }
    return begin;
}


int main(int argc, char *argv[])
{

    vector<int> a;
    deque<int> b;
    list<int> c;

    a.push_back(1);
    a.push_back(2);
    a.push_back(3);
    a.push_back(4);
    a.push_back(5);

    vector<int>::iterator first = a.begin();
    vector<int>::iterator last = a.end(); 

    vector<int>::iterator result = findIntInVector(first, last, 5);
    if (result != last)
        cout << "find 5 in vec a." << endl;
    else
        cout << "not find 5 in vec a." << endl;

    return 0;
}

```

<a id="29-size_type类型"></a>
#### 29. size_type类型
所谓 size_type 就是这个“vector 的 size 的类型”。

size就是指vector有多少个元素，这个“多少个”也是个整型值，它的类型就是 size_type。

举个具体的例子：

假如有个vector，你调用size()来得到它有多少个元素，你要把这个值存在一个变量里，那么这个变量该声明为什么类型呢？

int 型吗？ 万一这个vector的元素数超过了 int 的范围呢？  要不就用 long？ 万一也不够用呢？？

现在你知道了 size_type 就是这个 size 值的类型，你只要声明一个 size_type 类型的变量就能存下“元素个数”的值。

顺便一提，在 C 语言中 size_t 指的是系统中最大的整型类型，一般在前述情况下都是使用 size_t 类型的。

<a id="30-顺序容器的insert操作"></a>
#### 30. 顺序容器的insert操作
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;


int main(int argc, char *argv[])
{
    string array[4] = { "Bill","Mary","Tom", "Jerry" };
    vector<string> a1;

    a1.push_back("begin");
    a1.push_back("end");

    vector<string>::iterator aBegin = a1.begin();
    vector<string>::iterator aEnd = a1.end();

    //insert
    aBegin++;
    a1.insert(aBegin, array+1, array + 2);

    for (vector<string>::iterator iter = a1.begin();iter != a1.end();++iter)
    {
        cout << *iter << " ";
    }

    cout << endl;

    return 0;
}
```

<a id="31-顺序容器的查找删除finderase"></a>
#### 31. 顺序容器的查找删除find/erase
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;


int main(int argc, char *argv[])
{
    list<string> slist;
    slist.push_back("apple");
    slist.push_back("boy");
    slist.push_back("cat");
    slist.push_back("dog");
    slist.push_back("egg");
    slist.push_back("fish");

    /*slist.pop_back();
    slist.pop_front();*/

    string s("dog");
    list<string>::iterator result = find(slist.begin(), slist.end(), s);
    if (result != slist.end())
    {
        slist.erase(result);
    }
    else
    {
        cout << "not find!" << endl;
    }


    for (list<string>::iterator iter = slist.begin(); iter != slist.end(); ++iter)
    {
        cout << *iter << endl;
    }

    return 0;
}

```

<a id="32-顺序容器的交换swap和赋值assign"></a>
#### 32. 顺序容器的交换swap和赋值assign
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;


int main(int argc, char *argv[])
{
    vector<int> a{ 10,20,30,40 };
    vector<int> b{ 100,200,300 };
    vector<int> c;
    vector<int> d;
    
    a.swap(b);

    for (vector<int>::iterator iter = a.begin(); iter != a.end(); ++iter)
    {
        cout << "a: " << *iter << endl;
    }
    for (vector<int>::iterator iter = b.begin(); iter != b.end(); ++iter)
    {
        cout << "b: " << *iter << endl;
    }

    c = b;
    for (vector<int>::iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << "c: " << *iter << endl;
    }

    d.assign(c.begin(), c.end());
    for (vector<int>::iterator iter = d.begin(); iter != d.end(); ++iter)
    {
        cout << "d: " << *iter << endl;
    }
    return 0;
}

```

<a id="33-string字符串的查找"></a>
#### 33. string字符串的查找
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <sstream>
#include <deque>
#include <algorithm>
#include <list>
#include <stack>
#include <queue>
#include <functional>

using namespace std;


int main(int argc, char *argv[])
{
    string s1("Har0394rdayP0349850#$#oter");

    string::size_type pos = s1.find("Pota");
    if (pos == string::npos)
    {
        cout << "not found!" << endl;
    }
    else
    {
        cout << "find pos: " << pos << endl;
    }

    string letters("abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ");
    pos = 0;
    while ((pos = s1.find_first_of(letters, pos)) != string::npos)
    {
        cout << s1[pos] << endl;
        ++pos;
    }


    return 0;
}

```

<a id="34-mapmultimap操作"></a>
#### 34. map/multimap操作
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <algorithm>
#include <functional>
#include <map>
#include <set>

using namespace std;


int main(int argc, char *argv[])
{
    map<int, string> m1;
    multimap<int, string> m2;

    m1.insert(map<int, string>::value_type(1, "one"));
    m1.insert(map<int, string>::value_type(2, "two"));
    m1.insert(map<int, string>::value_type(3, "three"));
    m1.insert(make_pair(4, "four"));
    m1.insert(pair<int, string>(5, "five"));
    m1[6] = "six";

    cout << "map size: " << m1.size() << endl;

    map<int, string>::iterator iter;
    for (iter = m1.begin(); iter != m1.end(); ++iter)
    {
        cout << "key: " << iter->first << " value: " << iter->second << endl;
    }

    cout << m1[3] << endl;

    m2.insert(multimap<int, string>::value_type(10, "ten"));
    m2.insert(multimap<int, string>::value_type(20, "twenty"));
    m2.insert(multimap<int, string>::value_type(30, "thirty"));
    m2.insert(make_pair(40, "fourty"));
    m2.insert(pair<int, string>(50, "fifty"));
    m2.insert(pair<int, string>(50, "fifty"));

    cout << "multimap size: " << m2.size() << endl;
    multimap<int, string>::const_iterator citer;
    for (citer = m2.begin(); citer != m2.end(); ++citer)
    {
        cout << "multimap key: " << citer->first << " " << " value: " << citer->second << endl;
    }

    cout << "have 1000:  " << m2.count(50) << endl;

    //find
    multimap<int, string>::const_iterator miter;
    miter = m2.find(50);
    if (miter==m2.end())
    {
        cout << "not found 50 in multimap." << endl;
    }
    else
    {
        cout << "found 50 in multimap." << endl;
        size_t n = m2.count(50);
        for (size_t i = 0; i < n; i++, miter++)
        {
            cout << i << " key: " << miter->first << " value: " << miter->second << endl;
        }
    }

    if (m2.erase(40) > 0)
    {
        cout << "m2 erase 40 success." << endl;
    }

    multimap<int, string>::iterator iFound = m2.find(40);
    if (iFound != m2.end())
    {
        cout << "m2 erase 40 success." << endl;
    }
    else
    {
        cout << "m2 erase 40 failed." << endl;
    }

    m2.erase(m2.lower_bound(50), m2.upper_bound(50));

    return 0;
}

```

<a id="35-setmultiset操作"></a>
#### 35. set/multiset操作
* set/multiset 是只读的, 不允许修改
* 插入数据是自动进行排序的
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <algorithm>
#include <functional>
#include <map>
#include <set>

using namespace std;

template <typename Container>
void printContents(const Container &c);

int main(int argc, char *argv[])
{
    set<int> s1;
    multiset<int> s2;

    s1.insert(2);
    s1.insert(-1);
    s1.insert(30);

    for (set<int>::const_iterator iter = s1.begin(); iter != s1.end(); ++iter)
    {
        cout << "s1: " << *iter << endl;
    }

    cout << endl;

    s2.insert(s1.begin(), s1.end());
    s2.insert(-1);
    s2.insert(-1);
    for (set<int>::const_iterator iter = s2.begin(); iter != s2.end(); ++iter)
    {
        cout << "s2: " << *iter << endl;
    }
    cout << "s2 have -1: " << s2.count(-1) << " times." << endl;

    printContents(s2);

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << endl;
    }
    cout << endl;
}
```

<a id="36-函数对象class实例函数对象的使用"></a>
#### 36. 函数对象/class实例函数对象的使用
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <set>
#include <algorithm>
#include <numeric>
#include <functional>

using namespace std;

template <typename Container>
void printContents(const Container &c);

void print(int x)
{
    cout << x << " ";
}

class Print
{
public:
    void operator() (int elem) { cout << elem << " "; }
};


int main(int argc, char *argv[])
{
    set<int, greater<int>> s1;
    set<int, less<int>> s2;
    

    s1.insert(2);
    s1.insert(-1);
    s1.insert(30);
    s1.insert(16);
    s1.insert(-16);

    printContents(s1);

    for_each(s1.begin(), s1.end(), print);
    cout << endl;
    for_each(s1.begin(), s1.end(), Print()); //将class实例作为函数对象(回调函数/函数指针)

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << endl;
    }
    cout << endl;
}
```

<a id="37-countcount_if计数器操作"></a>
#### 37. count/count_if计数器操作
* 关联容器(set/multiset/map/multimap)要使用自己的count
```
//一元谓词
bool isEven(int n)
{
    return n % 2 == 0;
}


int main(int argc, char *argv[])
{
    vector<int> vec1;
    for (int i = 0;i<10;++i)
    {
        vec1.push_back(i);
    }

    printContents(vec1);

    int num = count(vec1.begin(), vec1.end(), 4);
    cout << "have 4 num: " << num << " times." << endl;

    num = count_if(vec1.begin(), vec1.end(), isEven);
    int num2 = count_if(vec1.begin(), vec1.end(), bind2nd(modulus<int>(), 2));
    cout << "isEven num: " << num << endl;
    cout << "modulus num: " << num2 << endl;
    int num3 = count_if(vec1.begin(), vec1.end(), not1(bind2nd(modulus<int>(), 2)));
    cout << "odd num: " << num3 << endl;


    // bind2nd: 给第一个参数的函数对象greater<int>()绑定第二个参数4
    num = count_if(vec1.begin(), vec1.end(), bind2nd(greater<int>(), 4));
    cout << ">4: " << num << endl;

    cout << "---------------" << endl;

    multiset<int> ms;
    for (int i = 1;i<10;++i)
    {
        ms.insert(i);
    }
    ms.insert(2);
    ms.insert(7);

    printContents(ms);

    num = count(ms.begin(), ms.end(), 7);
    cout << "7 num: " << num << endl;

    // 比count()速度快(set/multiset已经排好序了)
    num = ms.count(7);
    cout << "7 num: " << num << endl;

    return 0;
}
```

<a id="38-maxmin计算最大最小值"></a>
#### 38. max/min计算最大最小值
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <deque>
#include <list>
#include <stack>
#include <algorithm>
#include <numeric>
#include <functional>

using namespace std;

template <typename Container>
void printContents(const Container &c);

bool absLess(int x, int y)
{
    return abs(x) < abs(y);
}

int main(int argc, char *argv[])
{
    deque<int> dq1;
    for (int i = 2; i < 8; ++i)
    {
        dq1.insert(dq1.end(), i);
    }
    for (int i = -3; i < 5; ++i)
    {
        dq1.insert(dq1.end(), i);
    }

    printContents(dq1);

    deque<int>::iterator min = min_element(dq1.begin(), dq1.end());
    cout << "min: " << *min << endl;

    deque<int>::iterator max = max_element(dq1.begin(), dq1.end());
    cout << "max: " << *max << endl;

    deque<int>::iterator absMin = min_element(dq1.begin(), dq1.end(), absLess);
    cout << "abs min: " << *absMin << endl;

    deque<int>::iterator absMax = max_element(dq1.begin(), dq1.end(), absLess);
    cout << "abs max: " << *absMax << endl;

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << endl;
    }
    cout << endl;
}
```

<a id="39-search_n查找算法"></a>
#### 39. search_n查找算法
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>

using namespace std;

template <typename Container>
void printContents(const Container &c);

int main(int argc, char *argv[])
{
    deque<int> dq;
    for (int i = 1;i<10;++i)
    {
        if (i == 3)
        {
            dq.insert(dq.end(), i);
            dq.insert(dq.end(), i);
            dq.insert(dq.end(), i);
        }
        else
        {
            dq.insert(dq.end(), i);
        }
    }

    printContents(dq);

    deque<int>::iterator pos;
    pos = search_n(dq.begin(), dq.end(), 3, 6, greater<int>());
    //pos = search_n_if(dq.begin(), dq.end(), 3, bind2nd(greater<int>(), 6)); 错误

    if (pos != dq.end())
    {
        cout << "find > 6: " << endl;
        for (;pos != dq.end();++pos)
        {
            cout << *pos << " ";
        }
        cout << endl;
    }
    else
    {
        cout << "not find > 6!" << endl;
    }

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " " ;
    }
    cout << endl;
}
```

<a id="40-searchfind_end在容器中查找另一个容器"></a>
#### 40. search/find_end在容器中查找另一个容器
* find_end (应该是search_end), 功能是从后往前找
```
//二元谓词: 检测是否为偶数时,isEven为true
bool checkEven(int elem, bool isEven)
{
    if (isEven)
    {
        return elem % 2 == 0;
    }
    else
    {
        return elem % 2 == 1;
    }
}

int main(int argc, char *argv[])
{
    deque<int> dq;
    list<int> li;

    for (int i = 1; i <= 7; ++i)
    {
        dq.insert(dq.end(), i);
    }
    for (int i = 1; i <= 7; ++i)
    {
        dq.insert(dq.end(), i);
    }
    for (int i = 1; i <= 7; ++i)
    {
        dq.insert(dq.end(), i);
    }

    for (int i = 3; i <= 6; ++i)
    {
        li.insert(li.end(), i);
    }

    printContents(dq);
    printContents(li);

    deque<int>::iterator pos;
    pos = search(dq.begin(), dq.end(), li.begin(), li.end());
    while(pos != dq.end())
    {
        cout << "find ok, pos: " << distance(dq.begin(), pos) + 1 << endl;
        ++pos;
        pos = search(pos, dq.end(), li.begin(), li.end());
    }

    cout << "find_end: " << endl;
    
    pos = find_end(dq.begin(), dq.end(), li.begin(), li.end());
    if(pos != dq.end())
    {
        cout << "find ok, pos: " << distance(dq.begin(), pos) + 1 << endl;
    }
    
    vector<int> vec;
    for (int i=1;i<=9;++i)
    {
        vec.push_back(i);
    }

    printContents(vec);

    bool checkEvenArgs[] = { true, false, true }; //find: even odd even
    //bool checkEvenArgs[] = { true, true, true }; //find: even even even

    vector<int>::iterator pos1;
    pos1 = search(vec.begin(), vec.end(), checkEvenArgs, checkEvenArgs + 3, checkEven);
    if (pos1 != vec.end())
    {
        cout << "found pos: " << distance(vec.begin(), pos1) + 1 << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }

    return 0;
}

```

<a id="41-用find_first_of在ivec中查找searchlist中的任一个对象"></a>
#### 41. 用find_first_of在ivec中查找searchList中的任一个对象
* 不存在find_last_of,需要用find_first_of加上逆向迭代器才能实现
```
/* 
功能: 用find_first_of,在ivec中查找searchList
*/
int main(int argc, char *argv[])
{
    vector<int> ivec;
    list<int> searchList{3, 6, 9}; //被查找的内容list

    for (int i = 1; i <= 11; ++i)
    {
        ivec.push_back(i);
    }
    printContents(ivec);
    printContents(searchList);

    vector<int>::iterator pos;
    pos = find_first_of(ivec.begin(), ivec.end(), searchList.begin(), searchList.end());
    if (pos != ivec.end())
    {
        cout << "find pos: " << distance(ivec.begin(), pos) + 1 << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }

    vector<int>::reverse_iterator rpos;
    rpos = find_first_of(ivec.rbegin(), ivec.rend(), searchList.begin(), searchList.end());
    cout << "find rpos: " << distance(ivec.begin(), rpos.base()) << endl;

    //在字符串里找数字
    string numeric("0123456789");
    string str("r3h8i9");
    string::size_type p = str.find_first_of(numeric);
    if (p != string::npos)
    {
        cout << "found num: " << p << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }

    p = str.find_last_not_of(numeric);
    if (p != string::npos)
    {
        cout << "found num: " << p << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }

    return 0;
}
运行结果:
1 2 3 4 5 6 7 8 9 10 11
3 6 9
find pos: 3
find rpos: 9
found num: 1
found num: 4
请按任意键继续. . .
```

<a id="42-adjacent_find查找2个连续的"></a>
#### 42. adjacent_find查找2个连续的
* 找2个相等的或符合谓词规则的
```
//二元谓词
bool doubled(int elem1, int elem2)
{
    return elem1*2 == elem2; //后面是前面的2倍
}

int main(int argc, char *argv[])
{
    vector<int> ivec{ 1,3,2,4,5,5,0 };
    printContents(ivec);

    vector<int>::iterator pos;
    pos = adjacent_find(ivec.begin(), ivec.end());
    if (pos != ivec.end())
    {
        cout << "found adjancent pos: " << distance(ivec.begin(), pos) + 1 << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }


    pos = adjacent_find(ivec.begin(), ivec.end(), doubled);
    if (pos != ivec.end())
    {
        cout << "found adjancent pos: " << distance(ivec.begin(), pos) + 1 << endl;
    }
    else
    {
        cout << "not found!" << endl;
    }


    return 0;
}

result:
1 3 2 4 5 5 0
found adjancent pos: 5
found adjancent pos: 3
请按任意键继续. . .
```

<a id="43-对已序区间进行查找binary_searchincludes"></a>
#### 43. 对已序区间进行查找binary_search/includes
* 要求容器先进行排序, 再查找, 速度快
* 可以使用谓词制定排序的规则
```
int main(int argc, char *argv[])
{
    list<int> iList;
    vector<int> iVec;

    for (int i = 1; i < 10; ++i)
    {
        iList.push_back(i);
    }
    printContents(iList);

    if (binary_search(iList.begin(), iList.end(), 5))
        cout << "find 5" << endl;
    else
        cout << "not found 5" << endl;

    iVec.push_back(3);
    iVec.push_back(4);
    iVec.push_back(7);
    // includes 全包括返回true, 顺序无所谓, iVec也需要排序
    if (includes(iList.begin(), iList.end(), iVec.begin(), iVec.end()))
    {
        cout << "includes 3 4 7" << endl;
    }
    else
    {
        cout << "not includes 3 4 7" << endl;

    }

    return 0;
}
```

<a id="44-lower_boundupper_boundequal_range-查找算法应用于顺序容器"></a>
#### 44. lower_bound/upper_bound/equal_range 查找算法应用于顺序容器
* lower_bound: 最先匹配
* upper_bound: 最后匹配
* equal_range: 相当于同时使用lower_bound 和 upper_bound, 返回2个迭代器
* 应用于顺序容器快, 关联容器慢(关联容器应该使用自身的成员函数)
```
int main(int argc, char *argv[])
{
    list<int> iList;
    for (int i = 1;i<10;++i)
    {
        iList.insert(iList.end(), i);
        //iList.insert(iList.end(), i);
    }
    iList.insert(iList.end(), 5);
    iList.insert(iList.end(), 5);
    iList.insert(iList.end(), 5);
    iList.insert(iList.end(), 5);

    iList.sort();
    printContents(iList);

    list<int>::iterator pos1;
    pos1 = lower_bound(iList.begin(), iList.end(), 5);
    if (pos1 != iList.end())
    {
        cout << "lower_bound found 5 pos: " << distance(iList.begin(), pos1)+1 << endl;
    }
    else
    {
        cout << "lower_bound not found 5!" << endl;
    }

    list<int>::iterator pos2;
    pos2 = upper_bound(iList.begin(), iList.end(), 5); //返回最后一个5的下一个位置
    if (pos2 != iList.end())
    {
        cout << "upper_bound found 5 pos: " << distance(iList.begin(), pos2) << endl;
    }
    else
    {
        cout << "upper_bound not found 5!" << endl;
    }

    iList.insert(upper_bound(iList.begin(), iList.end(), 5), 5);
    iList.insert(lower_bound(iList.begin(), iList.end(), 5), 5);
    printContents(iList);

    pair<list<int>::iterator, list<int>::iterator> range = equal_range(iList.begin(), iList.end(), 5);
    cout << distance(iList.begin(), range.first) << " " << distance(iList.begin(), range.second)  << endl;

    return 0;
}
result:
1 2 3 4 5 5 5 5 5 6 7 8 9
lower_bound found 5 pos: 5
upper_bound found 5 pos: 9
1 2 3 4 5 5 5 5 5 5 5 6 7 8 9
4 11
请按任意键继续. . .
```

<a id="45-for_each算法使用"></a>
#### 45. for_each算法使用
* for_each的返回值是一个函数对象
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>
#include <set>
#include <map>

using namespace std;

template <typename Container>
void printContents(const Container &c);

void print(int x)
{
    cout << x << " ";
}

template<class T>
class AddValue
{
public:
    AddValue(const T &v) :theValue(v) {}
    void operator()(T &elem) const
    {
        elem += theValue;
    }
private:
    T theValue;
};

class MeanValue
{
private:
    long num;
    long sum;
public:
    MeanValue() :num(0), sum(0) {}
    void operator()(int elem)
    {
        num++;
        sum += elem;
    }
    double value()
    {
        return static_cast<double>(sum) / static_cast<double>(num);
    }

    operator double()
    {
        return static_cast<double>(sum) / static_cast<double>(num);
    }
};


int main(int argc, char *argv[])
{
    vector<int> iVec;
    for (int i = 1; i < 10; ++i)
    {
        iVec.push_back(i);
    }

    for_each(iVec.begin(), iVec.end(), print);
    cout << endl;

    //使用函数对象为容器iVec所有对象+10
    for_each(iVec.begin(), iVec.end(), AddValue<int>(10));
    printContents(iVec);

    MeanValue mv = for_each(iVec.begin(), iVec.end(), MeanValue());
    cout << "MeanValue: " << mv.value() << endl;

    double mean = for_each(iVec.begin(), iVec.end(), MeanValue());
    cout << "MeanValue(double): " << mean << endl;

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;
}

result:
1 2 3 4 5 6 7 8 9
11 12 13 14 15 16 17 18 19
MeanValue: 15
MeanValue(double): 15
请按任意键继续. . .
```

<a id="46-区间容器比较equalmismatchlexicographical_compare"></a>
#### 46. 区间(容器)比较equal/mismatch/lexicographical_compare
* equal: 判断区间是否相等, 返回bool
* mismatch: 判断区间是否不相等, 返回一对iterator
* lexicographical_compare: 检查区间1是否小于区间2, 返回bool, true代表区间1<区间2, false表示区间1>=区间2
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>
#include <set>
#include <map>

using namespace std;

template <typename Container>
void printContents(const Container &c);

//二元谓词, 判定是否奇偶对应
bool bothEvenOrOdd(int elem1, int elem2)
{
    return elem1 % 2 == elem2 % 2;
}


bool lessForCollection(const list<int> &list1, const list<int> &list2)
{
    return lexicographical_compare(list1.begin(), list1.end(), list2.begin(), list2.end());
}

int main(int argc, char *argv[])
{
    //equal
    vector<int> ivec;
    list<int> ilist;

    for (int i = 1; i <= 7; ++i)
    {
        ivec.push_back(i);
    }
    for (int i = 3; i <= 9; ++i)
    {
        ilist.push_back(i);
    }
    printContents(ivec);
    printContents(ilist);

    if (equal(ivec.begin(), ivec.end(), ilist.begin()))
    {
        cout << "ivec == ilist" << endl;
    }
    else
    {
        cout << "ivec != ilist" << endl;
    }

    if (equal(ivec.begin(), ivec.end(), ilist.begin(), bothEvenOrOdd))
    {
        cout << "ivec correspond ilist Even Or Odd." << endl;
    }
    else
    {
        cout << "ivec not correspond ilist Even Or Odd." << endl;
    }

    //mismatch
    vector<int> ivec1;
    list<int> ilist1;

    for (int i = 1; i <= 6; ++i)
    {
        ivec1.push_back(i);
    }
    for (int i = 1; i <= 16; i *= 2)
    {
        ilist1.push_back(i);
    }
    ilist1.push_back(3);
    printContents(ivec1);
    printContents(ilist1);

    pair<vector<int>::iterator, list<int>::iterator> values;
    values = mismatch(ivec1.begin(), ivec1.end(), ilist1.begin());
    if (values.first == ivec1.begin())
    {
        cout << "no mismatch." << endl;
    }
    else
    {
        cout << "first mismatch: " << *values.first << " " << *values.second << endl;
    }

    //找第一个容器里大于第二个容器里的数
    values = mismatch(ivec1.begin(), ivec1.end(), ilist1.begin(), less_equal<int>());
    if (values.first == ivec1.end())
    {
        cout << "ivec1 <= ilist1." << endl;
    }
    else
    {
        cout << "found ivec1: " << *values.first << " less or equal ilist1: " << *values.second << endl;
    }
    cout << endl;

    //lexicographical_compare
    list<int> l1, l2, l3, l4;
    for (int i = 1;i<6;++i)
    {
        l1.push_back(i);
    }
    l4 = l3 = l2 = l1;
    l1.push_back(7);
    l3.push_back(2);
    l3.push_back(0);
    l4.push_back(2);

    cout << "l1: ";
    printContents(l1);
    cout << "l2: ";
    printContents(l2);
    cout << "l3: ";
    printContents(l3);
    cout << "l4: ";
    printContents(l4);

    if (lexicographical_compare(l4.begin(), l4.end(), l1.begin(), l1.end()))
    {
        cout << "l4 < l1" << endl;
    }
    else
    {
        cout << "l4 >= l1" << endl;
    }

    if (lexicographical_compare(l4.begin(), l4.end(), l3.begin(), l3.end()))
    {
        cout << "l4 < l3" << endl;
    }
    else
    {
        cout << "l4 >= l3" << endl;
    }
    cout << endl;

    //对vls中的8个list容器进行排序
    vector<list<int>> vls{ l1,l2,l3,l4,l3,l1,l4,l2 };
    for_each(vls.begin(), vls.end(), printContents<list<int>>);
    cout << endl;

    cout << "sorted: " << endl;
    sort(vls.begin(), vls.end(), lessForCollection);
    for_each(vls.begin(), vls.end(), printContents<list<int>>);

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;
}

result:
1 2 3 4 5 6 7
3 4 5 6 7 8 9
ivec != ilist
ivec correspond ilist Even Or Odd.
1 2 3 4 5 6
1 2 4 8 16 3
first mismatch: 3 4
found ivec1: 6 less or equal ilist1: 3

l1: 1 2 3 4 5 7
l2: 1 2 3 4 5
l3: 1 2 3 4 5 2 0
l4: 1 2 3 4 5 2
l4 < l1
l4 < l3

1 2 3 4 5 7
1 2 3 4 5
1 2 3 4 5 2 0
1 2 3 4 5 2
1 2 3 4 5 2 0
1 2 3 4 5 7
1 2 3 4 5 2
1 2 3 4 5

sorted:
1 2 3 4 5
1 2 3 4 5
1 2 3 4 5 2
1 2 3 4 5 2
1 2 3 4 5 2 0
1 2 3 4 5 2 0
1 2 3 4 5 7
1 2 3 4 5 7
请按任意键继续. . .
```

<a id="47-复制元素copycopy_backward算法"></a>
#### 47. 复制元素copy/copy_backward算法
* copy过程中不能修改元素
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>
#include <set>
#include <map>
#include <iterator>

using namespace std;

template <typename Container>
void printContents(const Container &c);


int main(int argc, char *argv[])
{
    //将list中的元素copy到vector中
    list<int> ilist;
    for (int i = 0; i < 10; ++i)
    {
        ilist.push_back(i);
    }
    printContents(ilist);

    vector<int> ivec(ilist.size() * 2);
    printContents(ivec);

    cout << "after copy: " << endl;
    copy(ilist.begin(), ilist.end(), ivec.begin());
    printContents(ivec);

    copy_backward(ilist.begin(), ilist.end(), ivec.end());
    printContents(ivec);

    cout << "back_inserter: " << endl;
    //back_inserter(ilist)插入迭代器: 用于向空的容器中插入数据
    vector<int> ivec1;
    list<int> ilist1;

    for (int i = 0; i < 10; ++i)
    {
        ivec1.push_back(i);
    }

    copy(ivec1.begin(), ivec1.end(), back_inserter(ilist1));
    printContents(ivec1);
    printContents(ilist1);
    cout << endl;

    copy(ivec1.begin(), ivec1.end(), ostream_iterator<int>(cout, " "));

    cout << "reverse copy: " << endl;
    copy(ivec1.rbegin(), ivec1.rend(), ilist1.begin());
    printContents(ilist1);

    //将source中的字母复制到source的后面
    vector<char> source(10, '.');
    for (char c = 'a'; c <= 'f'; ++c)
    {
        source.push_back(c);
    }
    source.insert(source.end(), 10, '.');
    printContents(source);

    vector<char> c1(source.begin(), source.end());
    copy(c1.begin() + 10, c1.begin() + 16, c1.begin() + 7);
    printContents(c1);

    vector<char> c2(source.begin(), source.end());
    copy_backward(c2.begin() + 10, c2.begin() + 16, c2.begin() + 19);
    printContents(c2);

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;
}

result:
0 1 2 3 4 5 6 7 8 9
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
after copy:
0 1 2 3 4 5 6 7 8 9 0 0 0 0 0 0 0 0 0 0
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
back_inserter:
0 1 2 3 4 5 6 7 8 9
0 1 2 3 4 5 6 7 8 9

0 1 2 3 4 5 6 7 8 9 reverse copy:
9 8 7 6 5 4 3 2 1 0
. . . . . . . . . . a b c d e f . . . . . . . . . .
. . . . . . . a b c d e f d e f . . . . . . . . . .
. . . . . . . . . . a b c a b c d e f . . . . . . .
请按任意键继续. . .
```

<a id="48-transform变换算法使用"></a>
#### 48. transform变换算法使用
* 如果目标容器是空的, 就必须使用插入迭代器back_inserter
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>
#include <set>
#include <map>
#include <iterator>

using namespace std;

template <typename Container>
void printContents(const Container &c);


int main(int argc, char *argv[])
{
    vector<int> ivec;
    list<int> ilist;

    for (int i = 1; i < 10; ++i)
    {
        ivec.push_back(i);
    }
    printContents(ivec);

    transform(ivec.begin(), ivec.end(), ivec.begin(), negate<int>());
    printContents(ivec);

    //将ivec中的每个元素x10, 然后插入到ilist中
    transform(ivec.begin(), ivec.end(), back_inserter(ilist), bind2nd(multiplies<int>(), 10));
    printContents(ilist);

    cout << endl;
    transform(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " "), negate<int>());
    

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;
}

result:
1 2 3 4 5 6 7 8 9
-1 -2 -3 -4 -5 -6 -7 -8 -9
-10 -20 -30 -40 -50 -60 -70 -80 -90

10 20 30 40 50 60 70 80 90 请按任意键继续. . .

int main(int argc, char *argv[])
{
    vector<int> ivec;
    list<int> ilist;

    for (int i = 1; i < 10; ++i)
    {
        ivec.push_back(i);
    }
    printContents(ivec);

    transform(ivec.begin(), ivec.end(), ivec.begin(), ivec.begin(), multiplies<int>());
    printContents(ivec);
    
    transform(ivec.begin(), ivec.end(), ivec.begin(), back_inserter(ilist), plus<int>());
    cout << "ilist: " << endl;
    printContents(ilist);

    transform(ivec.begin(), ivec.end(), ilist.begin(), ostream_iterator<int>(cout, " "), minus<int>());


    return 0;
}
result:
1 2 3 4 5 6 7 8 9
1 4 9 16 25 36 49 64 81
ilist:
2 8 18 32 50 72 98 128 162
-1 -4 -9 -16 -25 -36 -49 -64 -81 请按任意键继续. . .
```

<a id="49-for_each与transform的比较"></a>
#### 49. for_each与transform的比较
* for_each: 传参为引用, op函数无返回值 速度快, 相对不够灵活;
* transform: 传参为值拷贝, op函数有返回值, 速度慢, 相对灵活;
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>
#include <set>
#include <map>
#include <iterator>

using namespace std;

template <typename Container>
void printContents(const Container &c);

void square(int &elem)
{
    elem = elem * elem;
}

int square2(int elem)
{
    return elem = elem * elem;
}

int main(int argc, char *argv[])
{
    vector<int> a;
    vector<int> b;
    

    for (int i = 1; i < 10; ++i)
    {
        a.push_back(i);
        b.push_back(i);
    }
    printContents(a);
    printContents(b);

    for_each(a.begin(), a.end(), square);
    cout << "square: " << endl;
    printContents(a);

    transform(b.begin(), b.end(), b.begin(), square2);
    printContents(b);

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;
}
result:
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
square:
1 4 9 16 25 36 49 64 81
1 4 9 16 25 36 49 64 81
请按任意键继续. . .
```

<a id="50-swap_rangesswap算法使用"></a>
#### 50. swap_ranges/swap算法使用
* swap_ranges返回值为一个iterator, 指向第二个容器中没有被交换的位置
* swap: 容器全部进行交换
```
#define _CRT_SECURE_NO_WARNINGS

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <algorithm>
#include <numeric>
#include <functional>
#include <deque>
#include <set>
#include <map>
#include <iterator>

using namespace std;

template <typename Container>
void printContents(const Container &c);



int main(int argc, char *argv[])
{
    vector<int> ivec;
    deque<int> ideq;

    for (int i = 1; i < 10; ++i)
    {
        ivec.push_back(i);
    }
    for (int i = 11; i < 24; ++i)
    {
        ideq.push_back(i);
    }
    cout << "before swap: " << endl;
    cout << "ivec: ";
    printContents(ivec);
    cout << "ideq: ";
    printContents(ideq);

    deque<int>::iterator pos;
    pos = swap_ranges(ivec.begin(), ivec.end(), ideq.begin());
    if (pos != ideq.end())
    {
        cout << "first no swap number: " << *pos << endl;
    }

    cout << "after swap: " << endl;
    cout << "ivec: ";
    printContents(ivec);
    cout << "ideq: ";
    printContents(ideq);

    swap_ranges(ideq.begin(), ideq.begin() + 3, ideq.rbegin());
    printContents(ideq);

    vector<int> ivec1;
    vector<int> ivec2;
    for (int i = 1; i < 4; ++i)
    {
        ivec1.push_back(i);
    }
    for (int i = 10; i < 40; i+=10)
    {
        ivec2.push_back(i);
    }
    printContents(ivec1);
    printContents(ivec2);

    ivec1.swap(ivec2);

    printContents(ivec1);
    printContents(ivec2);

    return 0;
}

template <typename Container>
void printContents(const Container &c)
{
    for (Container::const_iterator iter = c.begin(); iter != c.end(); ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;
}

result:
before swap:
ivec: 1 2 3 4 5 6 7 8 9
ideq: 11 12 13 14 15 16 17 18 19 20 21 22 23
first no swap number: 20
after swap:
ivec: 11 12 13 14 15 16 17 18 19
ideq: 1 2 3 4 5 6 7 8 9 20 21 22 23
23 22 21 4 5 6 7 8 9 20 3 2 1
1 2 3
10 20 30
10 20 30
1 2 3
请按任意键继续. . .
```

<a id="51-fillfill_ngenerategenerate_n填充算法"></a>
#### 51. fill/fill_n/generate/generate_n填充算法
```
int main(int argc, char *argv[])
{
    list<string> slist;
    slist.push_back("hi");
    slist.push_back("good");
    slist.push_back("morning");

    printContents(slist);

    fill(slist.begin(), slist.end(), "hao");
    printContents(slist);

    list<string> slist2;
    fill_n(back_inserter(slist2), 9, "hello");
    printContents(slist2);

    fill_n(ostream_iterator<double>(cout, " "), 10, 7.7);
    cout << endl;

    list<int> ilist;
    generate_n(back_inserter(ilist), 5, rand);
    printContents(ilist);

    generate(ilist.begin(), ilist.end(), rand);
    printContents(ilist);

    return 0;
}
result:
hi good morning
hao hao hao
hello hello hello hello hello hello hello hello hello
7.7 7.7 7.7 7.7 7.7 7.7 7.7 7.7 7.7 7.7
41 18467 6334 26500 19169
15724 11478 29358 26962 24464
请按任意键继续. . .
```

<a id="52-replacereplace_ifreplace_copyreplace_copy_if替换算法的使用"></a>
#### 52. replace/replace_if/replace_copy/replace_copy_if替换算法的使用

```
int main(int argc, char *argv[])
{
    list<int> ilist;

    for (int i = 2; i < 8; ++i)
    {
        ilist.push_back(i);
    }
    for (int i = 4; i < 10; ++i)
    {
        ilist.push_back(i);
    }
    printContents(ilist);

    replace(ilist.begin(), ilist.end(), 6, 42);
    printContents(ilist);

    replace_if(ilist.begin(), ilist.end(), bind2nd(less<int>(), 5), 0);
    printContents(ilist);

    replace_copy(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " "), 5, 55);
    printContents(ilist);

    replace_copy_if(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " ") , bind2nd(less<int>(),5), 42);
    printContents(ilist);


    return 0;
}
result:
2 3 4 5 6 7 4 5 6 7 8 9
2 3 4 5 42 7 4 5 42 7 8 9
0 0 0 5 42 7 0 5 42 7 8 9
0 0 0 55 42 7 0 55 42 7 8 9 0 0 0 5 42 7 0 5 42 7 8 9
42 42 42 5 42 7 42 5 42 7 8 9 0 0 0 5 42 7 0 5 42 7 8 9
请按任意键继续. . .
```

<a id="53-删除算法使用"></a>
#### 53. 删除算法使用
* remove不是真正的删除, 而是将后面的元素往前移, 覆盖掉前面的元素, erase是真正的删除
* remove
* remove_if
```
int main(int argc, char *argv[])
{
    list<int> ilist;
    for (int i = 1;i<7;++i)
    {
        ilist.push_front(i);
        ilist.push_back(i);
    }
    printContents(ilist);

    list<int>::iterator end;

    end = remove(ilist.begin(), ilist.end(), 3); // end是删除后的逻辑终点
    printContents(ilist);

    cout << "一共删除了: " << distance(end, ilist.end()) << " 个 3." << endl;

    ilist.erase(end, ilist.end());
    printContents(ilist);

    //真正的删除
    vector<int> ivec;
    for (int i=2;i<7;++i)
    {
        ivec.push_back(i);
    }
    for (int i = 4; i < 10; ++i)
    {
        ivec.push_back(i);
    }
    for (int i = 1; i < 8; ++i)
    {
        ivec.push_back(i);
    }
    printContents(ivec);

    ivec.erase(remove(ivec.begin(), ivec.end(), 5), ivec.end());
    printContents(ivec);

    //删除掉ivec中所有<4的元素
    ivec.erase(remove_if(ivec.begin(), ivec.end(), bind2nd(less<int>(), 4)), ivec.end());
    printContents(ivec);

    return 0;
}
result:
6 5 4 3 2 1 1 2 3 4 5 6
6 5 4 2 1 1 2 4 5 6 5 6
一共删除了: 2 个 3.
6 5 4 2 1 1 2 4 5 6
2 3 4 5 6 4 5 6 7 8 9 1 2 3 4 5 6 7
2 3 4 6 4 6 7 8 9 1 2 3 4 6 7
4 6 4 6 7 8 9 4 6 7
请按任意键继续. . .
```
* remove_copy
* remove_copy_if
```
int main(int argc, char *argv[])
{
    list<int> ilist;
    for (int i = 1;i<7;++i)
    {
        ilist.push_back(i);
    }
    for (int i = 1; i < 10; ++i)
    {
        ilist.push_back(i);
    }
    printContents(ilist);

    multiset<int> iset;
    remove_copy_if(ilist.begin(), ilist.end(), inserter(iset, iset.end()), bind2nd(less<int>(), 4));
    printContents(iset);

    remove_copy(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " "), 3);
    cout << endl;
    remove_copy_if(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " "), bind2nd(greater<int>(), 4));

    return 0;
}
result:
1 2 3 4 5 6 1 2 3 4 5 6 7 8 9
4 4 5 5 6 6 7 8 9
1 2 4 5 6 1 2 4 5 6 7 8 9
1 2 3 4 1 2 3 4 请按任意键继续. . .
```
* unique
* unique_copy
```
bool differenceOne(int elem1, int elem2)
{
    return elem1 + 1 == elem2 || elem1 - 1 == elem2;
}

int main(int argc, char *argv[])
{
    int source[] = { 1,2,2,3,4,4,4,5,6,6,6,7,4,4 };
    int sourceNum = sizeof(source) / sizeof(source[0]);

    list<int> ilist;
    copy(source, source + sourceNum, back_inserter(ilist));
    printContents(ilist);

    list<int>::iterator pos;
    pos = unique(ilist.begin(), ilist.end());
    for (list<int>::iterator iter = ilist.begin(); iter != pos; ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;

    copy(source, source + sourceNum, ilist.begin());
    printContents(ilist);

    pos = unique(ilist.begin(), ilist.end(), greater<int>());
    for (list<int>::iterator iter = ilist.begin(); iter != pos; ++iter)
    {
        cout << *iter << " ";
    }
    cout << endl;

    copy(source, source + sourceNum, ilist.begin());
    printContents(ilist);

    unique_copy(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " "));
    cout << endl;

    unique_copy(ilist.begin(), ilist.end(), ostream_iterator<int>(cout, " "), differenceOne);

    return 0;
}
result:
1 2 2 3 4 4 4 5 6 6 6 7 4 4
1 2 3 4 5 6 7 4
1 2 2 3 4 4 4 5 6 6 6 7 4 4
1 2 2 3 4 4 4 5 6 6 6 7
1 2 2 3 4 4 4 5 6 6 6 7 4 4
1 2 3 4 5 6 7 4
1 3 5 7 4 4 请按任意键继续. . .
```

<a id="54-reverserotate旋转算法"></a>
#### 54. reverse/rotate旋转算法
```
int main(int argc, char *argv[])
{
    vector<int> ivec;
    for (int i = 1; i < 10; ++i)
    {
        ivec.push_back(i);
    }
    printContents(ivec);

    reverse(ivec.begin(), ivec.end());
    printContents(ivec);

    reverse_copy(ivec.begin() + 1, ivec.end() - 1, ostream_iterator<int>(cout, " "));
    cout << endl;

    reverse(ivec.begin(), ivec.end());
    printContents(ivec);

    //将ivec中的元素进行旋转, ivec.begin()+1为旋转后的开头元素
    rotate(ivec.begin(), ivec.begin() + 1, ivec.end());
    printContents(ivec);

    rotate(ivec.begin(), find(ivec.begin(), ivec.end(), 4), ivec.end());
    printContents(ivec);

    //rotate旋转
    set<int> iset;
    for (int i = 1; i < 10; ++i)
    {
        iset.insert(iset.end(), i);
    }
    printContents(iset);

    set<int>::iterator pos = iset.begin();
    advance(pos, 1); // set是双向迭代器, 只能用advance向前移动
    rotate_copy(iset.begin(), pos, iset.end(), ostream_iterator<int>(cout, " "));

    cout << endl;

    pos = iset.end();
    advance(pos, -2);
    rotate_copy(iset.begin(), pos, iset.end(), ostream_iterator<int>(cout, " "));
    cout << endl;

    rotate_copy(iset.begin(), iset.find(4), iset.end(), ostream_iterator<int>(cout, " "));

    return 0;
}
result:
1 2 3 4 5 6 7 8 9
9 8 7 6 5 4 3 2 1
2 3 4 5 6 7 8
1 2 3 4 5 6 7 8 9
2 3 4 5 6 7 8 9 1
4 5 6 7 8 9 1 2 3
1 2 3 4 5 6 7 8 9
2 3 4 5 6 7 8 9 1
8 9 1 2 3 4 5 6 7
4 5 6 7 8 9 1 2 3 请按任意键继续. . .
```

<a id="55-next_permutationprev_permutation排列组合算法"></a>
#### 55. next_permutation/prev_permutation排列组合算法
* 要先排序, 从小到大用next_permutation, 从大到小用prev_permutation
* next_permutation返回值为true代表还有下一个排列组合, false代表没有下一个排列组合
```
int main(int argc, char *argv[])
{
    vector<int> ivec;
    for (int i = 1; i < 4; ++i)
    {
        ivec.push_back(i);
    }
    printContents(ivec);

    while (next_permutation(ivec.begin(), ivec.end()))
    {
        printContents(ivec);

    }
    cout << endl;

    vector<int> ivec2;
    ivec2.push_back(3);
    ivec2.push_back(2);
    ivec2.push_back(1);

    while (prev_permutation(ivec2.begin(), ivec2.end()))
    {
        printContents(ivec2);
    }

    return 0;
}
result:
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1

3 1 2
2 3 1
2 1 3
1 3 2
1 2 3
请按任意键继续. . .
```

