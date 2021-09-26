# HW03

## Задание 1
### 7\) (ba|a)* | (bb|a)*

Самые короткие различные строки:
  1) ε
  2) a
  3) ba

Cтрока abbab не принадлежит языку. Мы не можем получить на конце b, перед которой идет a.

Строка bababa принадлежит языку. Из (ba|a)* выбираем три раза ba.

## Задание 2
### 11\) {a·b·ω | ω ∈ {0, 1}\*, a ∈ {0, 1}, b ∈ {0, 1}, a = b}
![](https://raw.githubusercontent.com/Fawentus/fl-2021-hse-win/HW03/2.jpg)
Проверила, что это минимальный автомат так, как мы это делали на практике

## Задание 3
### 3\) {α·001·β | α, β ∈ {0, 1}\*} ∩ {γ·000·δ | γ, δ ∈ {0, 1}\*}
Этому языку принадлежат слова, у которых есть подстрока 001 и подстрока 000.

Этот язык описывается регулярным выражением: ([01]\*000[01]\*001[01]\*)|([01]\*001[01]\*000[01]\*)|([01]\*0001[01]\*)

Kонечный автомат, описывающий этот язык: ![](https://raw.githubusercontent.com/Fawentus/fl-2021-hse-win/HW03/3.jpg)

Регулярная грамматика: S → 1S | S1 | 0S | S0 | {1}* 001 {1}* 000 {0, 1}* | {1}* 000 {0}* 1 {0, 1}*

## Задание 4
### 7\) {aᵐ·ω | 1 ⩽ |ω|<sub>b</sub> ⩽ m}

Этот язык не является регулярным: ![](https://raw.githubusercontent.com/Fawentus/fl-2021-hse-win/HW03/4.jpg)

## Задание 5
### 7\) (ba|a)* | (bb|a)*
![](https://raw.githubusercontent.com/Fawentus/fl-2021-hse-win/HW03/5.jpg)
