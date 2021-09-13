# Домашняя работа 1
## Задание 1
![](https://raw.githubusercontent.com/Fawentus/fl-2021-hse-win/HW01/pictures/first.jpg)
## Задание 2
![](https://raw.githubusercontent.com/Fawentus/fl-2021-hse-win/HW01/pictures/second.jpg)
## Задание 3
Язык: c++
1. [Regular expressions library](https://en.cppreference.com/w/cpp/regex)  
Можно работать с регулярными выражениями: создавать регулярные выражения, искать совпадения с ними, находить нужные подпоследовательности, заменять их на другие символы и многое другое
```c++
const std::string fnames[] = {"foo.txt", "bar.txt", "baz.dat", "zoidberg"};
const std::regex txt_regex("[a-z]+\\.txt"); // само регулярное выражение
 
for (const auto &fname : fnames) {
    std::cout << fname << ": " << std::regex_match(fname, txt_regex) << '\n'; // выводит, соответствует ли строка регулярному выражению
}
```
2. [std::span](https://en.cppreference.com/w/cpp/container/span)  
std::span появился в C++20. Объекты этого класса ссылаются на некоторую непрерывную последовательность необязательно фиксированного размера. 
Это более удобная и безопасная альтернатива использования указателей для доступа к элементам последовательности.
```c++
template<class T, size_t Extent = dynamic_extent> // так размер последовательности будет указан во время выполнения
class span;
```
3. [std::all_of, std::any_of, std::none_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of)  
Эти функции позволяют проверять условие на некотором диапазоне
```c++
std::vector<int> v(10, 2);
if (std::all_of(v.cbegin(), v.cend(), [](int i){ return i % 2 == 0; })) {
    std::cout << "All numbers are even\n";
}
```
## Задание 4
Конечные автоматы похожи на орграфы, поэтому будем задавать их похожим образом: вершины - состояния, ориентированные ребра между ними - переходы, для каждого ребра
(вот тут есть отличия от графов) некоторая информация о нем - множество символов из алфавита.  
  
Для начала зададим алфавит (в данном случае он будет состоять из слов):
```
alphabet { "северный", "попугай", "синий", "нутрия", "олень" }
```
  
Затем нужно перечислить состояния, которые есть у автомата:
```
states { q0, q1, q2, q3, q4, q5 }
```
  
Нужно указать терминальные вершины:
```
terminal { q1, q5 }
```
  
Иногда состояний может быть слишком много, настолько, что название не способно отразить суть. Будут полезны описания состояний, при необходимости их можно добавить так:
```c++
state q1 { "два синих попугая и один северный олень" } // state <name_state> { <description> }
```
  
Затем нужно добавить переходы между состояниями, они характеризуются состоянием, из которого переходим, в которое переходим, и элементами из алфавита:
```c++
q0 ---> q5 ["северный", "олень"] // <out_state> ---> <in_state> [<elements_of_alphabet>]
```
  
Если автомат создан неверно (нет состояния, которое могло бы быть начальным, или их наобот несколько, не по всем элементам алфавита можно выйти из вершины и тд), то такой автомат создан не будет. Если все верно, то начальная вершина будет определена автоматически (это вершина, в которую не входит ни одно ребро)
  
Примеры:
1. Автомат из первого задания:
```
alphabet { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 }
states { q0, q1, q2, q3, scolo }
terminal { q2, q3 }
state q3 { "это 0" }
q0 ---> q1 [ 1, 2, 3, 4, 6, 7, 8, 9 ]
q0 ---> q2 [ 5 ]
q0 ---> q3 [ 0 ]
q1 ---> q1 [ 1, 2, 3, 4, 6, 7, 8, 9 ]
q1 ---> q2 [ 0, 5 ]
q2 ---> q1 [ 1, 2, 3, 4, 6, 7, 8, 9 ]
q2 ---> q2 [ 0, 5 ]
q3 ---> scolo [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

2. Грибникам сегодня повезло, в лесу после дождя их ждет огромный урожай, но нужно знать, где искать. Только если они хотя бы раз за весь свой путь пройдут 2 раза подряд вправо и хотя бы 1 раз вверх, то они найдут волшебную полянку, в любом другом случае наткнутся на не терминальные полянки:
```
alphabet { '←', '→', '↑', '↓' }
states { q0,  q1, q2, q3, q4}
terminal { q3 }
state q1 { "одна → подряд" }
state q2 { "две → подряд" }
state q3 { "две → и одна ↑ подряд найдены - нашли полянку" }
state q4 { "ноль → подряд" }
q0 ---> q1 [ '→' ]
q0 ---> q4 [ '←', '↑', '↓' ]
q1 ---> q2 [ '→' ]
q1 ---> q4 [ '←', '↑', '↓' ]
q2 ---> q2 [ '→' ]
q2 ---> q3 [ '↑' ]
q2 ---> q4 [ '←', '↓' ]
q3 ---> q3 [ '←', '→', '↑', '↓' ]
q4 ---> q1 [ '→' ]
q4 ---> q4 [ '←', '↑', '↓' ]
```

3. Один популярный блогер живет в стране, где говорят на языке, состоящем из букв 'a', 'b', 'c', 'd' и знаков препинания '!', '?'. Он хочет отобрать только экспрессивные комментарии (в конце стоит '!') или те, где есть хотя бы один вопрос (в комментарии есть '?'):
```
alphabet { 'a', 'b', 'c', 'd', '!', '?' }
states { q0, q1, q2, q3 }
terminal { q1, q2 }
state q1 { "есть ?" }
state q2 { "на конце !" }
q0 ---> q1 [ '?' ]
q0 ---> q2 [ '!' ]
q0 ---> q3 [ 'a', 'b', 'c', 'd' ]
q1 ---> q1 [ 'a', 'b', 'c', 'd', '!', '?' ]
q2 ---> q1 [ '?' ]
q2 ---> q2 [ '!' ]
q2 ---> q3 [ 'a', 'b', 'c', 'd' ]
q3 ---> q1 [ '?' ]
q3 ---> q2 [ '!' ]
q3 ---> q3 ['a', 'b', 'c', 'd' ]
```
## Задание 5
