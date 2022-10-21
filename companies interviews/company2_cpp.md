## Вопросы на собеседовании в компании 2 (cpp)

Это был тест по с++. Некоторые вопросы из него:

1. What is the output of the following code?

```cpp
#include <iostream>

enum class action {
    do_nothing, do_break, do_continue, do_return
};

void test(action a, char const * label) {
    std::cout << label << ":";
    do {
        switch(a) {
        case action::do_nothing:
            std::cout << " do_nothing";   
        case action::do_break:
            std::cout << " do_break"; break;  
        case action::do_continue:
            std::cout << " do_continue"; continue;
        case action::do_return:
            std::cout << " do_return"; return;    
        }
        std::cout << " after_switch";
    } while (false);
    std::cout << " after_do\n";
}

int main()
{
    test(action::do_nothing, "nothing");
    test(action::do_break, "break");
    test(action::do_continue, "continue");
    test(action::do_return, "return");
    
    return 0;
}
```

Правильный ответ: 
```cpp
nothing: do_nothing do_break after_switch after_do
break: do_break after_switch after_do
continue: do_continue after_do
return: do_return
```


2. What is the output of the following code?

```cpp
#include <iostream>

struct Base {
    virtual ~Base() = default;
    
    void f(char const * label) const { std::cout << label << ": Base\n"; }
    virtual void g(char const * label) const {std::cout << label << ": Base\n"; }
};

struct Derived: Base {
    void f(char const * label) const { std::cout << label << ": Derived\n"; }
    void g(char const * label) const {std::cout << label << ": Derived\n"; }
};

int main()
{
    Derived d;
    Base b1 = d;
    Base & b2 = d;
    
    d.f("d.f");
    d.g("d.g");
    b1.f("b1.f");
    b1.g("b1.g");
    b2.f("b2.f");
    b2.g("b2.g");
    
    return 0;
}
```

Правильный ответ: 
```cpp
d.f: Derived // всё понятно
d.g: Derived // всё понятно
b1.f: Base // для b1 вызывается Base, так как копировали по значению, по сути просто обрезали объект
b1.g: Base
b2.f: Base // вызвался Base, так как f не виртуальная
b2.g: Derived // вызвался Derived, так как g виртуальная и переопределена
```


3. Which of the following programs compile correctly?

a)
```cpp
int main()
{
    std::unique_ptr<int> x(new int(42));
    std::weak_ptr<int> y(x);
    return 0;
}
```
Ответ: ошибка компиляции, нельзя из unique_ptr сделать weak_ptr

b)
```cpp
int main()
{
    std::shared_ptr<int> x(new int(42));
    std::weak_ptr<int> y(x);
    return 0;
}
```
Ответ: здесь всё в порядке, без ошибок

c)
```cpp
int main()
{
    std::unique_ptr<int> x(new int(42));
    auto y = x;
    return 0;
}
```
Ответ: ошибка компиляции, так как у unique_ptr удален конструктор копирования

d)
```cpp
int main()
{
    std::shared_ptr<int> x(new int(42));
    auto y = x;
    return 0;
}
```
Ответ: здесь всё в порядке, без ошибок

4. What is the output of the following code?

```cpp
#define MAX(a, b) \
    a > b ? a : b

int main()
{
    int a = 1;
    int b = 2;
    int c = 3 + MAX(a, b) + 4;
    std::cout << c;
}
```
Ответ: в макросе нет скобок, поэтому получаем выражение ```3 + a > b ? a : b + 4```.
Расставим приоритеты с помощью скобок: ```((3 + a) > b) ? (a) : (b + 4)```.
Поэтому получаем ```c = 1```.

Подробнее про приоритеты: [тут](http://cppstudio.com/post/302/)


5. What is the output of the following code?

```cpp
#include <iostream>
#include <cctype>

template<typename Transformer>
void transform_and_print(Transformer t, std::string str) {
    t(str);
    std::cout<< str << '\n';
}

int main()
{
    auto toupper1 = [](auto str) { for(auto c: str) c = std::toupper(c); };
    auto toupper2 = [](auto& str) { for(auto c: str) c = std::toupper(c); };
    auto toupper3 = [](auto str) { for(auto& c: str) c = std::toupper(c); };
    auto toupper4 = [](auto& str) { for(auto& c: str) c = std::toupper(c); };
    
    transform_and_print(toupper1, "toupper1");
    transform_and_print(toupper2, "toupper2");
    transform_and_print(toupper3, "toupper3");
    transform_and_print(toupper4, "toupper4");
    
    return 0;
}
```
Ответ:

```
toupper1
toupper2
toupper3
TOUPPER4
```
Почему так: В ```toupper1``` и ```toupper3``` строка ```str``` передается по значению, 
поэтому мы изменяем ее локальную копию, сама же строка остается неизменной. 
Для ```toupper2``` строка берется по ссылке, но символы ```c``` передаются по значению,
поэтому изменяются локальные копии, но не сами символы.
И только для ```toupper4```, когда по ссылке берется и строка ```str```, и символы ```c```,
строка действительно изменяется.

6. What is the output of the following code?

```cpp
void a(int x) {
    x++;    
}

void b(int * x) {
    x += 2;
}

void c(int &x) {
    x += 4;
}


int main()
{
    int x = 0;
    a(x);
    b(&x);
    c(x);
    cout << x;
    return 0;
}
```

Ответ: 4

Почему так: в функцию ```a``` параметр передается по значению, изменяется 
только его копия. В ```b``` передается указатель, но команда ```x += 2```
изменяет сам указатель, а не значение 
(чтобы изменилось значение, нужно было бы писать ```*x += 2```). 
В ```c``` переменная передается по
ссылке, поэтому ее значение изменяется.

7. Given the following struct:

```cpp
struct person {
  std::string name;
  int age;
};
```

Что нужно добавить в код, чтобы код ниже работал как ожидается?
```cpp
int main()
{
    std::cout << person{"John", 32} << std::endl;
    return 0;
}
```

Ответ: 
```cpp
ostream & operator<<(ostream & os, const person& p) {
    os << p.name << " " << p.age << "\n";
    return os;
}
```

8. What is the output of the following code?

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

void f(map<string, int> map) {
    map["bar"] = 3;
}

int main() {
    map<string, int> map;
    map["foo"] = 5;
    f(map);
    map["baz"] = map["foo"] + map["qux"];
    for(auto i : map) {
        cout << i.first << " " << i.second << "\n";
    }
    return 0;
}
```

Ответ: 
```cpp
baz 5
foo 5
qux 0
```

Почему так: в ```f``` словарь подается по значению, поэтому изменяется 
только его копия. При вызове ```map["qux"]```, так как ключа ```"qux"``` еще нет
в словаре, такой ключ добавится в словарь со значением по умолчанию (то есть с 0). 
Кстати, такого поведения не было бы, если бы использовался метод ```at```
(в этом случае было бы исключение). 

9. What is the output of the following code?

```cpp
#include <iostream>
#include <memory>

using namespace std;

struct Base {
    Base() { f("Base::ctor");}
    virtual ~Base() {f("Base::dtor");}
    virtual void f(char const * label) const { std::cout << label << ": Base\n"; }
    void g() const { return f("Base::g"); }
};

struct Derived: Base {
    Derived() { f("Derived::ctor");}
    ~Derived() {f("Derived::dtor");}
    void f(char const * label) const { std::cout << label << ": Derived\n"; }
    void g() const { return f("Derived::g");}
};

int main()
{
    std::unique_ptr<Base> b(new Derived);
    b->g();
    return 0;
}
```

Ответ:
```cpp
Base::ctor: Base
Derived::ctor: Derived
Base::g: Derived
Derived::dtor: Derived
Base::dtor: Base
```

10. Which of the following calls to ```ohai()``` are valid?

```cpp
struct A {
    void ohai() {}
};

struct B : protected A {};

struct C: private A {
    friend int main();
};

struct D : B {
  void test() {
      ohai(); // 1
  }  
};

struct E: C {
  void test() {
      ohai(); // 2
  }  
};

int main()
{
    A().ohai(); // 3
    B().ohai(); // 4
    C().ohai(); // 5
    E().ohai(); // 6
}
```

Ответ:
```cpp
// 1 норм
// 2 ошибка, так как наследование C<-A приватное
// 3 норм
// 4 ошибка, так как при protected наследовании метод ohai стал protected
// 5 норм, так как мы сделали функцию main классом-другом класса C
// 6 ошибка, так как наследование C<-A приватное
```
