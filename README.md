# Лабораторная работа 6

## Пояснения к заданию

Каждый алгоритм, который необходимо реализовать в задании, обладает шаблонным параметром `Compare` и аргументом этого типа `cmp`. Этот объект называется компаратором. Им нужно пользоваться для сравнения двух объектов. Например, реализуем функцию определения отсортирован ли массив.
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
Проверим, что массив отсортирован по возрастанию.
```cpp
std::vector<int> arr = {1, 2, 3, 4, 5, 6};
std::cout << std::boolalpha << is_sorted(arr.begin(), arr.end(), std::less<>{});
```

Проверим, что массив отсортирован по убыванию.
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

7. Реализуйте модульные тесты.

8. Сравните производительность всех реализованных алгоритмов сортировки (`merge_sort`, `inplace_merge_sort`, `heap_sort`, `quick_sort`, `insertion_sort`, `std::stable_sort`, `std::sort`).
Составьте отчет. Отчет должен включать в себя:
    * описание алгоритмов;
    * блок схемы алгоритмов;
    * зависимость времени работы алгоритмов от количества данных;
    * вывод.


