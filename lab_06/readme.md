# Лабораторная работа 6: *Обработка деревьев, работа с хеш-таблицами*

**Цель работы:** получить навыки применения двоичных деревьев, реализовать основные операции над деревьями: обход деревьев, включение, исключение и поиск узлов; построить и обработать хеш-таблицы, сравнить эффективность поиска в сбалансированных деревьях, в двоичных деревьях поиска и в хеш-таблицах.

## Задание (Вариант 4)

В текстовом файле содержатся целые числа. Построить ДДП из чисел файла. Вывести его на экран в виде дерева. Сбалансировать полученное дерево и вывести его на экран. Построить хеш-таблицу из чисел файла. Использовать закрытое хеширование для устранения коллизий. Осуществить удаление введенного целого числа в ДДП, в сбалансированном дерево, в хеш-таблицу и в файл. Сравнить время удаления, объем памяти и количество сравнений при использовании различных (4-х) структур данных. Если количество сравнений в хеш-таблице больше указанного, то произвести реструктуризацию таблицы, выбрав другую функцию.

## Используемые структуры

### Бинарное дерево

```cpp
struct Tree
{
    int data;                                               // Ключ узла
    Tree *left;                                             // Левый элемент
    Tree *right;                                            // Правый элемент

    Tree();                                                 // Конструктор пустого узла
    Tree(int d);                                            // Конструктор узла с ключом
    void input(ifstream &in);                               // Ввод дерева из файла
    void print_recursive(vector<string> probels, bool end); // Вывод дерева в консоль
    void print_recursive_in_file(ofstream &file);           // Вывод дерева в .ps файл
    void print(string name);                                // Вызов функций вывода
    Tree *remove(Tree *root, int data);                     // Удаление элемента
    void free();                                            // Очистка дерева
};
```

### Сбалансированный узел

```cpp
struct BalancedTree
{
    int data;                                               // Ключ узла
    BalancedTree *left;                                     // Левый элемент
    BalancedTree *right;                                    // Правый элемент
    unsigned char height;                                   // Высота узла

    BalancedTree();                                         // Конструктор пустого узла
    BalancedTree(int k);                                    // Конструктор узла с ключом
    void print_recursive(vector<string> probels, bool end); // Вывод дерева в консоль
    void print_recursive_in_file(ofstream &file);           // Вывод дерева в .ps файл
    void print(string name);                                // Вызов функций вывода
    void free();                                            // Очистка узла
};
```
### Сбалансированное дерево

```cpp
struct BalancedTreeHead
{
    BalancedTree *head;                                     // Вехний элемент сбалансированного дерева

    BalancedTreeHead();                                     // Конструктор пустого дерева
    void input(ifstream &in);                               // Ввод дерева из файла
    void print(string name);                                // Вывод дерева в консоль и в файл .ps
    unsigned char height(BalancedTree *root);               // Получение высоты узла
    int bfactor(BalancedTree *root);                        // Вычисляет разность высот дочерних узлов
    void fixheight(BalancedTree *root);                     // Обновление высоты узла
    BalancedTree *rotateright(BalancedTree *p);             // Правый поворот вокруг узла
    BalancedTree *rotateleft(BalancedTree *q);              // Левый поворот вокруг узла
    BalancedTree *balance(BalancedTree *root);              // Балансировка узла
    BalancedTree *insert(BalancedTree *root, int data);     // Вставка ключа в узел
    BalancedTree *findmin(BalancedTree *root);              // Поиск узла с минимальным ключом в узле
    BalancedTree *removemin(BalancedTree *root);            // Удаление узла с минимальным ключом из дерева p
    BalancedTree *remove(BalancedTree *root, int data);     // Удаление элемента
    void free();                                            // Очистка дерева
};
```

### Элемент хеш-таблицы

```cpp
struct ClosedHashElement
{
    int data;                                               // Значение клча
    int attempt;                                            // Номер попытки записи
    bool free;                                              // Свободна ли ячейка

    ClosedHashElement();                                    // Конструктор пустого элемента
};
```

### Хеш-таблица

```cpp
struct ClosedHashTable
{
    vector<ClosedHashElement> table;                        // Таблица данных
    int digit;                                              // Число для хеширования

    ClosedHashTable();                                      // Конструктор пустой таблицы
    int function(int data);                                 // Функция хеширования
    void input(ifstream &in);                               // Ввод таблицы
    void print();                                           // Вывод таблицы
    int insert(int data);                                   // Вставка элемента
    void reorganisation();                                  // Нахождение новой функции хеширования
    void remove(int data);                                  // Удаление элемента
};
```

### Файл

```cpp
struct File
{
    fstream file;                                           // Сам файл
    string name;                                            // Название файла

    File();                                                 // Пустой конструктор
    void input(string name);                                // Открытие файла
    void print();                                           // Вывод содержимого файла
    void remove(int data);                                  // Удаление элемента из файла
    void close();                                           // Закрытие файла
};
```

## Тесты

### Негативные тесты

1. Несуществующий файл

	```
	Выберите один из файлов:
	1.in	2.in	3.in	4.in	5.in	6.in	7.in	8.in	9.in
	>asd
	Некорректный файл!
	```
2. Некорректный ввод
	
	```
	...
	Введите число, которое хотите удалить
	>as
	>sdf
	>sdf
	>sdf
	>sdf
	>sdfsdf
	>2
	...
	```

### Позитивные тесты

1. Существующий файл

	```
	Выберите один из файлов:
	1.in	2.in	3.in	4.in	5.in	6.in	7.in	8.in	9.in
	>1.in
	
	Бинарное дерево:
	4234
	└▶123
	  ├▶7
	  │ └▶34
	  │   ├▶12
	  │   └▶65
	  │     └▶87
	  └▶645
	
	Сбалансированное дерево:
	34
	├▶7
	│ └▶12
	└▶645
	  ├▶87
	  │ ├▶65
	  │ └▶123
	  └▶4234
	
	Хеш-таблица:
	┏━━━━━┳━━━━┳━━━━┓
	┃Key     ┃Data   ┃Attempt┃
	┣━━━━━╋━━━━╋━━━━┫
	┃0       ┃65     ┃1      ┃
	┃1       ┃NULL   ┃NULL   ┃
	┃2       ┃NULL   ┃NULL   ┃
	┃3       ┃NULL   ┃NULL   ┃
	┃4       ┃NULL   ┃NULL   ┃
	┃5       ┃NULL   ┃NULL   ┃
	┃6       ┃123    ┃1      ┃
	┃7       ┃7      ┃1      ┃
	┃8       ┃645    ┃1      ┃
	┃9       ┃4234   ┃1      ┃
	┃10      ┃34     ┃3      ┃
	┃11      ┃87     ┃3      ┃
	┃12      ┃12     ┃1      ┃
	┗━━━━━┻━━━━┻━━━━┛
	
	Файл:
	4234 123 645 7 34 12 65 87
	
	Введите число, которое хотите удалить
	>87
	
	Дерево:
	Время удаления элемента: 8
	Количество сравнений: 5
	
	4234
	└▶123
	  ├▶7
	  │ └▶34
	  │   ├▶12
	  │   └▶65
	  └▶645
	
	Сбалансированное дерево:
	Время удаления элемента: 9
	Количество сравнений: 2
	
	34
	├▶7
	│ └▶12
	└▶645
	  ├▶123
	  │ └▶65
	  └▶4234
	
	Хеш-таблица:
	Время удаления элемента: 2
	Количество сравнений: 1
	
	┏━━━━━┳━━━━┳━━━━┓
	┃Key     ┃Data   ┃Attempt┃
	┣━━━━━╋━━━━╋━━━━┫
	┃0       ┃65     ┃1      ┃
	┃1       ┃NULL   ┃NULL   ┃
	┃2       ┃NULL   ┃NULL   ┃
	┃3       ┃NULL   ┃NULL   ┃
	┃4       ┃NULL   ┃NULL   ┃
	┃5       ┃NULL   ┃NULL   ┃
	┃6       ┃123    ┃1      ┃
	┃7       ┃7      ┃1      ┃
	┃8       ┃645    ┃1      ┃
	┃9       ┃4234   ┃1      ┃
	┃10      ┃34     ┃3      ┃
	┃11      ┃NULL   ┃NULL   ┃
	┃12      ┃12     ┃1      ┃
	┗━━━━━┻━━━━┻━━━━┛
	
	Файл:
	Время удаления элемента: 645
	Количество сравнений: 8
	
	4234 123 645 7 34 12 65
	```
	
## Сравнение реализаций

### Время

|Количество элементов|Бинарное дерево|Сбалансированное дерево|Хеш-таблица|Файл|
|---|---|---|---|---|
|10|11|3|2|491|
|100|2|4|2|48|
|250|14|3|2|730|
|1000|15|5|2|920|

### Количество сравнений

|Количество элементов|Бинарное дерево|Сбалансированное дерево|Хеш-таблица|Файл|
|---|---|---|---|---|
|10|8|3|1|10|
|100|6|6|1|49|
|250|14|3|2|730|
|1000|19|7|1|551|

### Память

|Количество элементов|Бинарное дерево|Сбалансированное дерево|Хеш-таблица|Файл|
|---|---|---|---|---|
|10|200|210|90|40|
|100|2000|2100|900|400|
|250|5000|5250|2250|1000|
|1000|20000|21000|9000|4000|
	
## Контрольные вопросы
	
1. **Что такое дерево?**<br>Это структура данных, представляющая собой древовидную структуру в виде набора связанных узлов.<br><br>
2. **Как выделяется память под представление деревьев?**<br>Память выделяется отдельно для каждого элемента.<br><br>
3. **Какие стандартные операции возможны над деревьями?**<br>Обход дерева, поиск по дереву, включение в дерево, исключение из дерева.<br><br>
4. **Что такое дерево двоичного поиска?**<br>Это дерево, в котором у всех элементов не больше двух потомков и все потомки слева меньше, а справа - больше, чем уел, в котором находимся.<br><br>
5. **Чем отличается идеально сбалансированное дерево от АВЛ дерева?**<br>В идеально сбалансированном дереве число вершин слева и справа отличается не более, чем на единицу, а в АВЛ дереве высота двух поддеревьех азличается не болле, чем на единицу<br><br>
6. **Чем отличается поиск в АВЛ-дереве от поиска в дереве двоичного поиска?**<br>В АВЛ-дереве стабильно эффективный поиск O(log<sub>2</sub>n), а в ДДП может достигать O(n).<br><br>
7. **Что такое хеш-таблица, каков принцип ее построения?**<br>Это массив, заполненный в порядке, определеным хеш-функцией. Берется значение, к нему применяется хеш-функция, и получается ключ, по которому можно найти элементв таблице.<br><br>
8. **Что такое коллизии? Каковы методы их устранения.**<br>Коллизия - это ситуация, когда нескольким значениям соотвествует один ключ. Есть два решения этой проблемы: метод цепочек и открытая адресация. В первом значения с одним ключом хранятся в одной ячейке списком, а во второй ищутся свободные ячейки рядом.<br><br>
9. **В каком случае поиск в хеш-таблицах становится неэффективен?**<br>Когда возникает очень много коллизий.<br><br>
10. **Эффективность поиска в АВЛ деревьях, в дереве двоичного поиска и в хеш-
таблицах**

	|Структура|В среднем|В худшем случай|
	|---|---|---|
	|АВЛ|O(log<sub>2</sub>N)|O(log<sub>2</sub>N)|
	|ДДП|O(log<sub>2</sub>N)|O(N)|
	|Хеш-таблица|O(1)|O(N)|

## Вывод

Самая эффективная структура данных для поиска элементов это хеш-таблица, но ее недостаток в том, что при возникновении большого количества коллизий эффективность теряется. Самой стабильной по эффективности оказалось АВЛ-дерево.