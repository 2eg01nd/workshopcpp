# Задачи

## Поиск элемента в векторе 

Дан отсортированный `std::vector` с некоторым набором целочисленных `int` значений. Задача написать функцию для поиска индекса `findIndex(const std::vector<int>& vec, int target)`, которая принимает в аргументы вектор и значение которое нужно найти?
Значения vec и target уже заданы нужно только реализовать метод. Если значение удалось найти вывести `Search index in vector is {INDEX}`, если не удалось, то `Vector has no such element`.

Пример на входе:
```
std::vector<int> vec = { 1, 2, 4, 5, 7, 8, 9 };
int target = 7;
```
На выходе вывести
```
Searched index in vector is 4
```

Решение:
```
#include <vector>
#include <algorithm>
#include <iostream>

void findIndex(const std::vector<int>& vec, int target) {
    const auto it = std::find(vec.begin(), vec.end(), target);
    if (it != vec.end()) {
        std::cout << "Search index in vector is " << std::distance(vec.begin(), it) << std::endl;
    } else {
        std::cout << "Vector has no such element" << std::endl;
    }
}
```

## Сдвинуть список на опредленного количество элементов

Дан `std::list` с некоторым набором целочисленных `int` значений.
Нужно написать метод `shiftList(const std::list<int>& list, int n)`, которая принимает список и значение на сколько элементов нужно сдвинуть список. 
Сдвинуть список можно только вправо к концу контейнера, то есть значение `n` всегда положительное.

Пример 1. На входе:
```
std::list<int> list = {1, 2, 3, 4, 5};
int n = 2;
```
на выходе:
```
{4, 5, 1, 2, 3}
```

Пример 2. На входе:
```
std::list<int> list = {1, 2, 3, 4, 5};
int n = 5
```
на выходе:
```
{1, 2, 3, 4, 5}
```

Решение:
```
#include <list>

void shiftList(std::list<int>& list, int n) {
    if (list.empty() || n == 0) 
        return;

    n %= list.size();
    auto it = list.begin();
    std::advance(it, list.size() - n);
    list.splice(list.begin(), list, it, list.end());
}

```

## Удаление дубликатов из вектора

Дан неотсортированный вектор элементов. Используя итераторы, отсортировать элементыи удалить те, которые будут дублироваться.
Вектор vec уже задан нужно только реализовать метод `sortedUniqueVec`.
Задача вывести получившийся массив через пробел?

Пример 1. На входе:
```
std::vector<int> vec = {6, 2, 1, 4, 1, 2, 2, 5};
```
на выходе:
```
1 2 4 5 6
```

Пример 1. На входе:
```
std::vector<int> vec = {1, 5, 5, 7, 2, 8, 8, 8, 9};
```
на выходе:
```
1 2 5 7 8 9
```

Решение:
```
void sortedUniqueVec(std::vector<int>& vec)
{
    std::sort(vec.begin(), vec.end());
    auto last = std::unique(vec.begin(), vec.end());
    vec.erase(last, vec.end());
    for (const auto& it : vec) {
        std::cout << it << " ";
    }
    std::cout << std::endl;
}

```
