### *Степанов Александр ИУ7-33Б*

# Лабораторная работа 1: *Обработка больших чисел.*

## Описание условия задачи

Смоделировать операцию деления действительного числа в форме ±m.n Е ±K, где суммарная длина мантиссы (m+n) - до 30 значащих цифр, а величина порядка K - до 5 цифр, на целое число длиной до 30 десятичных цифр. Результат выдать в форме ±0.m1 Е ±K1, где m1 - до 30 значащих цифр, а K1 - до 5 цифр. 

## Техническое задание:

### *Исходные данные*

* Первое число вводится строкой вида ±m.n E ±K, где m+n не превышает 30, а K не больше 5 знаков. Допускается ввод запятой и точки, любого регистра E и отсутствие порядка мантиссы, знаков числа и порядка и точки.
* Второе число вводится строкой вида ±S, где S не превышает 30 цифр.

### *Результат*

Результат выводится строкой вида ±0.m ±K, где m не превышет 30 цифр, а K – пяти.

### *Описание задачи*

Деление вещественного числа на целое. Если в ответе получается больше 30 знаков, происходит округление, то есть если цифра больше или равна 5, предыдущая цифра увеличивается на 1, в противном случае не меняется.

### *Аварийные ситуации*

* Некорректный ввод чисел (превышение допустимого количества символов, использование неразрешенных символов). Вывод сообщения на экран.
* Деление на ноль. Вывод соощения на экран.
* Переполнение порядка. Вывод сообщения на экран.

## Структуры данных

Для хранения чисел используется массив целых чисел. Порядок и знак хранятся в целочисленых переменных.

## Алгоритм

1. Проверка корректоности входных данных. Если входные данных некорректны, то возвращается код ошибки INPUT_ERROR. Данные проверяются на переполнение и использование неразрешенных символов.
2. Вещественное число приводится к виду ±m e±K, то есть происходит избавление от запятой и соответсвенно уменьшение порядка. Также в отдельные целочисленные переменные записывается знак каждого из чисел.
3. Проверка делимости на ноль, в таком случае возвращается код ошибки DIVISION_BY_ZERO.
4. Деление реализовано алгоритмом деления в столбик. К делителю добавляются нули в конец, чтобы был одинаковый размер с делимым. Из делимого циклически вычитается делитель и получается следующая цифра ответа. Когда делимое становится меньше делителя, у делителя убирается ноль.

## Тесты

### *Проверки на ввод-вывод*

#### Больше 30 цифр в числе

```
--Template input--: ±______________________________e±_____
Input float number: 1234567890123456789012345678901

Input error!
```

```
--Template input--: ±______________________________e±_____
Input float number: 12

---Template input---: ±______________________________
Input integer number: 1234567890123456789012345678901

Input error!
```

#### Больше 5 цифр в порядке

```
--Template input--: ±______________________________e±_____
Input float number: 12e+123456

Input error!
```

#### Недопустимые символы

```
--Template input--: ±______________________________e±_____
Input float number: 123w2

Input error!
```

### *Граничные значения*

```
--Template input--: ±______________________________e±_____
Input float number: +999999999999999999999999999999e+99999

---Template input---: ±______________________________
Input integer number: 1

Overflow!
```

### *Корректные случаи*

#### Деление на ноль

```
--Template input--: ±______________________________e±_____
Input float number: 12

---Template input---: ±______________________________
Input integer number: 0

Division by zero!
```

#### Деление нуля

```
--Template input--: ±______________________________e±_____
Input float number: 0

---Template input---: ±______________________________
Input integer number: 12

+0.0e-1
```

#### Деление отрицательных

```
--Template input--: ±______________________________e±_____
Input float number: -12e+3

---Template input---: ±______________________________
Input integer number: -12

+0.1e+4
```

```
--Template input--: ±______________________________e±_____
Input float number: +12e+3

---Template input---: ±______________________________
Input integer number: -12

-0.1e+4
```

```
--Template input--: ±______________________________e±_____
Input float number: -12e+3

---Template input---: ±______________________________
Input integer number: +12

-0.1e+4
```

#### Округление

```
--Template input--: ±______________________________e±_____
Input float number: +999999999999999999999999999999e+9999 

---Template input---: ±______________________________
Input integer number: 2

+0.50000000000000000000000000000e+10029
```

```
--Template input--: ±______________________________e±_____
Input float number: 1

---Template input---: ±______________________________
Input integer number: 6

+0.16666666666666666666666666666e+0
```

## Контрольные вопросы

1. **Каков возможный диапазон чисел, представляемых в ПК?**
    ```
    Диапазон чисел зависит от выбранного типа данных. 
    Int, 4 байта: [-2 147 483 648, +2 147 483 647]
    Unsigned Int, 4 байта: [0, +4 294 967 295]
    Long int: [−2 147 483 648, +2 147 483 647]
    Unsigned long int: [0, +4 294 967 295]
    Float: [-3.4 *10^38, + 3.4*10^38]
    Double: [-1.7*10^308, +1.7*10^308]
    ```

2. **Какова возможная точность представления чисел, чем она определяется?**<br>Точность представления чисел с плавающей запятой зависит от длины мантиссы, которая определяется типом данных переменной. При выходе за пределы этой длины происходит округление. У double на порядок выделяется 11 бит, на мантиссу 52 бита. Для вещественных чисел диапазон представления в памяти шире, мантисса обрезается после 24 разряда для float.

3. **Какие стандартные операции возможны над числами?**<br>Сложение, вычитание, умножение, деление, сравнение.

4. **Какой тип данных может выбрать программист, если обрабатываемые числа превышают возможный диапазон представления чисел в ПК?**<br>Програмист может выбрать тип массив, в котором будет хранить каждую цифры этого числа, для порядка и знака можно использовать целочисленный тип.

5. **Как можно осуществить операции над числами, выходящими за рамки машинного представления?**<br>Последовательно выполнять операции на каждой цифрой числа.

## Вывод

Компьютер не может представить слишком большие числа с высокой точностью, для этого программисты используют алгоритм длинной арифметики, в котором числа хранятся в массиве, состоящем из цифр этого числа. Операции над такими числами выполняются над каждой цифрой. В случае возникновения переполнения происходит округление.