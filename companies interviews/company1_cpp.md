## Вопросы на собеседовании в компании 1 (сpp)


1. Что делает следующий блок кода:

```cpp
int var = 10;

int* ptr = &var;
*ptr = 20;

int** ptr2 = &ptr;
(*(*ptr2)) = 30;
```

2. Какой размер у типа ```int``` и от чего зависит его размер?

3. Что выведет следующий блок кода: 

```
int main()
{
    BYTE array[] = { 0x0, 0x1, 0x2, 0x3, 0x4, 0x5 };
    size_t size_array;

    BYTE*   pByte = array;
    WORD* pWord = array;

    size_array = sizeof(array) / sizeof(BYTE);
    printf("size_array=%d \n", size_array);

    for (int i = 0; i < size_array; i++)
    {
        printf("%d ", pByte[i]);
    }
    printf("\n");

    size_array = sizeof(array) / sizeof(WORD);
    printf("size_array=%d \n", size_array);

    for (int i = 0; i < size_array; i++)
    {
        printf("%d ", pWord[i]);
    }
    return 0;
}
```

4. Что означают понятия big-endian и little-endian?

5. Виртуальное наследование: что это такое, пример, где может применяться 

6. Рассказать о правилах: "правило пяти", "правило трех"

7. Пусть есть такая иерархия классов:

```
class Animal {
public:
	int i;  
};

class Dog: public Animal {
  
};

class Cat: public Animal , Dog
{
  int i;
  int * data;
};
```

a) написать конструктор копирования для класса ```Cat```;

b) Какой размер будет у экземляров классов ```Animal``` и ```Cat```?

c) Пусть есть объект класса ```Cat```:

```c++
Cat cat;
```
Можно ли обратиться к полю ```i``` класса ```Animal```
через экземпляр ```cat```?
 
d) В каких из следующих случаев будет вызван 
копирующий оператор присваивания, а в каких - 
копирующий конструктор?

```
Cat c; // конструктор по умолчанию
Cat c1(c); // копирующий конструктор 
Cat c2 = c; // копирующий конструктор (тут суть в том, что 
            // объект c2 еще не создан, и поэтому именно 
            // конструктор вызывается, а не оператор присвания)
c1 = c2; // operator=
```

8. Привести пример возникновения исключений и их обработки

```cpp
std::string s = "11a";
try {
  int i = stoi(s);
} catch(...) {
  cout << "error with s = " << s;
}
```

9. Стек и куча, чем отличаются? 
Какие данные хранятся в стеке, а какие в куче?

10. Почему при рекурсии возникает stack overflow?

Ответ: у стека конечный размер, а при каждом рекурсивном 
вызове в стеке создается "стековый фрейм". Таким образом, 
stack overflow означает, что закончилось место в стеке.


11. Пусть есть рекурсивная функция ```f```:

```cpp
void f(i) {
  if (i == 0) {
    return;
  }
  f(i - 1);
}
```

Пусть также stack overflow наступает при вызове ```f(n)```,
где ```n > N```, ```N``` - какое-то фиксированное число. 
Как сделать так, чтобы stack overflow наступал раньше (то есть)
при ```n > N```?

Правильные ответы:

а) Добавить локальную переменную. Таким образом увеличивается
размер стекового фрейма, 
и место на стеке будет заканчиваться быстрее 

```cpp
void f(i) {
  if (i == 0) {
    return;
  }
  int i = 0; // добавили лок переменную
  f(i - 1);
}
```

b) делать больше вызовов: 

```cpp
void f(i) {
  if (i == 0) {
    return;
  }
  f(i - 2); // доп вызов f
  f(i - 1);
}
```

12. Виртуальные функции: 

а) зачем нужны виртуальные функции?

b) что такое "таблица виртуальных функций"?

c) что происходит при вызове виртуальной функции?

d) почему бы все функции не сделать виртуальными?


13. Из чего состоит библиотека stl, примеры контейнеров, алгоритмов и итераторов

14. Что можно передать в качестве третьего параметра функции ```std::sort```?

Ответ: функцию от двух переменных, лямбда-функцию или callable-объект.

15. Как передавать в лямбду по значению и по ссылке?
Ответ: 

Пусть есть такой код:
```bool isGreater = true;```

Передаем по значению, в этом случае после вызова ```f```
значение ```isGreater``` останется равным ```true```:

```cpp
auto f = [isGreater] (const intObj &lhs, const intObj& rhs)
      {
        isGreater = false;
        if(isGreater) {
          return lhs.GetValue() < rhs.GetValue();
        }
        return lhs.GetValue() > rhs.GetValue();;
      }
     );
```

Передаем по ссылке, в этом случае после вызова ```f```
значение ```isGreater``` изменится на ```false```.

```cpp
auto f = [&isGreater] (const intObj &lhs, const intObj& rhs)
      {
        isGreater = false;
        if(isGreater) {
          return lhs.GetValue() < rhs.GetValue();
        }
        return lhs.GetValue() > rhs.GetValue();;
      }
     );
```


16. Работа с потоками (```# include <thread>```), как запустить
функцию в отдельном потоке, как использовать методы ```join``` 
и ```detach``` у объектов ```std::thread```.

17. Рассказать про std::promise, std::future
18. Что делает этот код:

```
void initializer(std::promise<int> * promObj)
{
	std::cout<<"Inside Thread"<<std::endl;
	promObj->set_value(35);
}

int main()
{
    std::promise<int>		promiseObj;
    std::future<int>		futureObj = promiseObj.get_future();

    std::thread th(initializer, &promiseObj);
    std::cout << futureObj.get() << std::endl;

    th.join();
    return 0;
}
```

19. Умные указатели: для чего нужны, примеры использования

20. Паттерны проектирования: для чего нужны, примеры использования
  
21. Реализовать паттерн Singleton (одиночка)

Ответ:

```cpp
class Singleton
{
  static Singleton *  instance = nullptr;
  
  static Singleton * GetInstance() 
  {
    if(instance == nullptr) {
      // init
      instance = init();
      return instance;
    }
    return instance;
  }
};
```
22. В предыдущем вопросе (Singleton), можно ли сырой указатель заменить 
на один из умных указателей?

Ответ: можно заменить на std::weak_ptr 
(см. подробности [здесь](https://stackoverflow.com/questions/47558290/singleton-class-with-smart-pointers-and-destructor-being-called))