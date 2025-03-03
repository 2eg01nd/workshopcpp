# Задачи

## Динамический массив. Часть 1

У нас есть класс:
```
#include <iostream>

class VectorInt
{
public:
    VectorInt() = default;

    ~VectorInt()
    {
        delete[] m_data;
        m_size = 0;
        m_capacity = 0;
    }

    void push_back(int value)
    {
        if (m_size >= m_capacity) {
            reallocate(m_capacity == 0 ? 4 : m_capacity * 2);
        }
        m_data[m_size++] = value;
    }

    void printVector()
    {
        for (size_t i = 0; i < m_size; ++i) {
            std::cout << m_data[i] << " ";
        }
        std::cout << std::endl;
    }

    size_t size() const { return m_size; } 

private:
    void reallocate(size_t newCapacity);

private:
    int* m_data = nullptr;
    size_t m_capacity = 0;
    size_t m_size = 0;
};

int main(int argc, char** argv)
{
    VectorInt vec;
    vec.push_back(1);
    vec.push_back(4);
    vec.push_back(5);

    vec.printVector();

    return 0;
}

```
Задача, нужно реализовать метод reallocate, чтобы получить вывод:
```
1 4 5
```

Решение:
```
void reallocate(size_t newCapacity)
{
    auto newData = new int[newCapacity];
    const auto newSize = std::min(m_size, newCapacity);
    for (size_t i = 0; i < m_size; ++i) {
        newData[i] = m_data[i];
    }
    delete[] m_data;
    m_data = newData;
    m_capacity = newCapacity;
    m_size = newSize;
}
```

## Динамический массив. Часть 2

Усложним предыдущую задачу, и реализуем правило трех на нашего класса. 
Потребуется реализовать конструктор копирования и оператор копирования, чтобы 
следующий код:
```
int main(int argc, char** argv)
{
    VectorInt vec1;
    vec1.push_back(3);
    vec1.push_back(6);
    vec1.push_back(9);

    VectorInt vec2(vec1);

    VectorInt vec3;
    vec3.push_back(50);
    vec3 = vec1;
    vec3.push_back(10);

    vec1.printVector();
    vec2.printVector();
    vec3.printVector();

    return 0;
}
```
Вывел:
```
3 6 9
3 6 9
3 6 9 10
```

Решение:
```
class VectorInt
{
public:
    VectorInt() = default;

    VectorInt(const VectorInt& other)
        : m_capacity(other.m_capacity)
        , m_size(other.m_size)
    {
        m_data = new int[m_capacity];
        for (auto i = 0; i < m_size; ++i) {
            m_data[i] = other.m_data[i];
        }
    }

    VectorInt& operator= (const VectorInt& other)
    {
        if (this != &other) {
            VectorInt tmp(other);
            std::swap(m_data, tmp.m_data);
            std::swap(m_size, tmp.m_size);
            std::swap(m_capacity, tmp.m_capacity);
        }
        return *this;
    }

    // остальная реализация
};
```

## Динамический массив. Часть 3

Реализуем для нашего класса также конструктор перемещения и оператор
перемещения, чтобы следующий код:
```
int main(int argc, char** argv)
{
    VectorInt vec1;
    vec1.push_back(3);
    vec1.push_back(6);

    VectorInt vec2(std::move(vec1));

    VectorInt vec3;
    vec3.push_back(50);
    vec3 = std::move(vec2);

    std::cout << "size vec1: " << vec1.size() << std::endl;
    std::cout << "size vec2: " << vec2.size() << std::endl;
    std::cout << "size vec3: " << vec3.size() << std::endl;
    vec3.printVector();

    return 0;
}
```
Вывел:
```
size vec1: 0
size vec2: 0 
size vec3: 2
3 6
```
Подсказка: конструктор и оператор перемещения не должен выбрасывать исключений, 
потому что он просто передаёт владение ресурсами, не выделяя новую память.


Решение:
```
class VectorInt
{
public:
    // остальная реализация

    VectorInt(VectorInt&& other) noexcept
        : m_capacity(other.m_capacity)
        , m_size(other.m_size)
        , m_data(other.m_data)
    {
        other.m_capacity = 0;
        other.m_size = 0;
        other.m_data = nullptr;
    }

    VectorInt& operator= (VectorInt&& other) noexcept
    {
        if (this != &other) {
            delete[] m_data;
            m_data = other.m_data;
            m_capacity = other.m_capacity;
            m_size = other.m_size;
            other.m_data = nullptr;
            other.m_capacity = 0;
            other.m_size = 0;
        }
        return *this;
    }

    // остальная реализация
};
```