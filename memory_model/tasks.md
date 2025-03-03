# Задачи

## Выделение и освобождение памяти

Дан такой кусок кода:
```
#include <iostream>

class SomeClass
{
public:
    SomeClass(const std::string& str)
        : m_str(str)
    {
        std::cout << "Constructor SomeClass with " << m_str << std::endl;
    }

    ~SomeClass()
    {
        std::cout << "Destructor SomeClass with " << m_str << std::endl;
    }

private:
    std::string m_str;
};

SomeClass someClass("Global ABCD");

void func()
{
    static SomeClass sClass("Static BACD");
}

int main(int argc, char** argv)
{
    std::cout << "start main()" << std::endl;
    SomeClass someClass1("Local Stack ABCD");
    SomeClass *someClass2 = new SomeClass("Local Heap ABCD");
    func();
    delete someClass2;
    std::cout << "end main()" << std::endl;
    return 0;
}
```
Используя только `std::cout` и `std::endl`, попытайтесь воспроизвести вывод?

Решение:
```
    std::cout << "Constructor SomeClass with Global ABCD" << std::endl;
    std::cout << "start main()" << std::endl;
    std::cout << "Constructor SomeClass with Local Stack ABCD" << std::endl;
    std::cout << "Constructor SomeClass with Local Heap ABCD" << std::endl;
    std::cout << "Constructor SomeClass with Static BACD" << std::endl;
    std::cout << "Destructor SomeClass with Local Heap ABCD" << std::endl;
    std::cout << "end main()" << std::endl;
    std::cout << "Destructor SomeClass with Local Stack ABCD" << std::endl;
    std::cout << "Destructor SomeClass with Static BACD" << std::endl;
    std::cout << "Destructor SomeClass with Global ABCD" << std::endl;
```

