# Задачи

## Анаграммы

Является ли заданные строки `str1` и `str2` анаграммами. Попробовать реализовать
алгоритм, который будет это делать за линейное время, то есть отсортировать
строки нельзя?
Подсказка: попробуйте использовать контейнеры вместо сортировки

На входе
```
str1 = listen;
str2 = silent;
```
На выходе
```
true
```

На входе
```
str1 = rat;
str2 = car;
```
На выходе
```
false
```

Решение:
```
bool isAnagram(std::string str1, std::string str2) {
    if (str1.length() != str2.length()) {
        return false;
    }
    std::unordered_map<char, int> freq;

    for (const auto& c : str1) {
        freq[c]++;
    }
    for (const auto& c : str2) {
        freq[c]--;
    }

    for (const auto& pair : freq) {
        if (pair.second != 0) {
            return false;
        }
    }
    return true;
}
```

## Удаление дубликатов из отсортированной строки

Дана отсортированная строка, например `ABBBCCDEEF...`. Строка состоит только 
из заглавных символов. Задача удалить из строки все дубликаты не используя
дополнительную память?


На входе
```
ABBBCCDEEFFFGHHIJKK
```
На выходе
```
ABCDEFGHIJK
```

На входе
```
UVWXYZZ
```
На выходе
```
UVWXYZ
```
Подсказка: нужно использовать два счетчика: один для чтения, другой для записи.

Решение:
```
void removeDuplicates(std::string& str)
{
    if (str.empty()) 
        return;
    int wIndex = 1;
    for (int rIndex = 1; rIndex < str.size(); ++rIndex) {
        if (str[rIndex] != str[rIndex - 1]) {
            str[wIndex] = str[rIndex];
            ++wIndex;
        }
    }
    str.resize(wIndex);
}
```

## Найти первый уникальный символ в строке

Задача. Дана произвольная строка `HelloWorldHe`, нужно вернуть индекс символа, 
который не повторяется в строке? Из ограничений, нужно сделать это за линейное
время. Использовать `std::set` не получится.  
Если уникальных символов нет вернуть -1.

На входе
```
HelloWorldHe
```
На выходе
```
5
```
На входе
```
aabbccdd
```
На выходе
```
-1
```
Подсказка: подсчитайте символы в строке и помните, для линейной сложности
нельзя использовать вложенные циклы

Решение:
```
int findUniqueChar(const std::string& str)
{
    std::unordered_map<char, int> charCount;
    for (const auto& c : str) {
        charCount[c]++;
    }
    for (size_t i = 0; i < str.size(); ++i) {
        if (charCount[str[i]] == 1) {
            return i;
        }
    }
    return -1;
}
```
