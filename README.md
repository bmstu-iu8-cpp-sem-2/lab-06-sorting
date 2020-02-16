# Лабораторная работа 6

## Цель
Цели работы:
* познакомиться с разными алгоритмами сортировки, узнать их различия, повысить навыки реализации различных алгоритмов, улучшить понимание `O` нотации и усвоить сложность алгоритмов сортировки.
* получить навыки написание обобщенного кода, научиться использовать компораторы и предикаты, повысить понимание "внутренностей" STL библиотеки.

## Теоретическая часть
На предыдущих занятиях были расмотрены алгоритмы STL, использвание компораторов и предикатов в алгоритмах.

Расмотрим два примера.

В первом примере, вектор строк сортируется в лексикографическом порядке, т.е. используется дефолтный оператор сравнения двух строк.
```cpp
std::vector<std::string> v = GetStrings();
std::sort(v.begin(), v.end());
```

Во втором примере, используется пользовательский компоратор (с использованием лямбда-функции). В этом пример вектор строк сортируется в порядке их длины.
```cpp
std::vector<std::string> v = GetStrings();
std::sort(v.begin(), v.end(), [](const std::string& a, const std::string& b) {
  return a.size() < b.size();
});
```

## Предикаты и компораторы
Рассмотрим, как создавать собственный код с использованием компоратора.

Во-первых, в большинстве случаем компораторы удобно определять как аргумент шаблона.

Во-вторых, компоратор надо представлять как обычную функцию и использовать её соответсвующим образом.

## Пример с использованием компоратора
Реализуем функцию определения отсортирован ли контейнер. В качестве двух первых аргументов передает диапазон контейнера `[first, last)`. Диапазон задается итераторами. Третий аргумент - это компоратор, который позволяет сравнивать элементы.

Компоратор возвращает `true`, если первый его аргумент "меньше" второго аргумента. Заметим, что "меньше" в кавычках. Потому что в зависимости от реализации компоратора, функция будет считать отсортированность по разному.

```cpp
template <class ForwardIt, class Compare=std::less<>>
bool is_sorted(ForwardIt first, ForwardIt last, Compare cmp=Compare{}) {
  if (first != last) {
    auto next = first;
    while (++next != last) {
      // Сравнение двух элементов.
      if (cmp(*next, *first)) {
        return false;
      }
      first = next;
    }
  }
  return true;
}
```

Проверим, что массив отсортирован по возрастанию. Для этого используем компоратор `std::less<>`:
```cpp
constexpr bool less()(const T& lhs, const T& rhs) const {
    return lhs < rhs;
}
```

```cpp
std::vector<int> arr = {1, 2, 3, 4, 5, 6};
std::cout << std::boolalpha << is_sorted(arr.begin(), arr.end(), std::less<>{});
```

Проверим, что массив отсортирован по убыванию. ля этого используем компоратор `std::greater<>`:
```cpp
constexpr bool greater()(const T& lhs, const T& rhs) const {
    return rhs < lhs; // внимание на порядок аргументов.
}
```

```cpp
std::vector<int> arr = {7, 5, 3, 1};
std::cout << std::boolalpha << is_sorted(arr.begin(), arr.end(), std::greater<>{});
```

Проверим, что массив отсортирован по убыванию с помощью лямбда-функции.
```cpp
std::vector<int> arr = {7, 5, 3, 1};
std::cout << std::boolalpha << is_sorted(arr.begin(), arr.end(),
  [](int a, int b) {
    return a > b;
  });
```

## Задание

1. Реализуйте слияние двух отсортированных массивов в один отсортированный. Алгоритм должен работать со сложностью по времени `O(N + M)`, где `N` и `M` длины массивов.
```cpp
template <class It, class Out, class Compare=std::less<>>
Out merge(It first1, It last1, It first2, It last2, Out out, Compare cmp=Compare{});
```

2. Реализуйте алгоритм сортировки слиянием.
```cpp
template <class It, class Out, class Compare=std::less<>>
Out merge_sort(It first, It last, Out out, Compare cmp=Compare{});
```

3. Реализуйте алгоритм сортировки слиянием без использования дополнительной памяти
```cpp
template <class It, class Compare=std::less<>>
void inplace_merge_sort(It first, It last, Compare cmp=Compare{});
```

4. Реализуйте пирамидальную сортировку.
Описание алгоритма ищите в книге Кормена "Алгоритмы. Построение и анализ (3-е издание)", стр 179, глава 6.
```cpp
template<class It, class Compare=std::less<>>
void heap_sort(It first, It last, Compare cmp=Compare{});
```

5. Реализуйте алгоритм быстрой сортировки.
```cpp
template <class It, class Compare=std::less<>>
void quick_sort(It first, It last, Compare cmp=Compare{});
```

6. Реализуйте сортировку вставками.
```cpp
template <class It, class Compare=std::less<>>
void insertion_sort(It first, It last, Compare cmp=Compare{});
```

7. Реализуйте модульные тесты. Миниальное покрытие кода должно превышать 85%.

8. Сравните производительность всех реализованных алгоритмов сортировки (`merge_sort`, `inplace_merge_sort`, `heap_sort`, `quick_sort`, `insertion_sort`, `std::stable_sort`, `std::sort`).
Составьте отчет. Отчет должен включать в себя:
    * описание алгоритмов;
    * блок схемы алгоритмов;
    * зависимость времени работы алгоритмов от количества данных;
    * вывод.
