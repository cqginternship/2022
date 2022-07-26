### Лабораторная работа 4
## Modern C++: Move Semantic. Smart Pointers
#### Задание к выполнению лабораторной работы.

Написать «умный» указатель `copy_ptr`, который при копировании, будет копировать хранящиеся в нем данные.

```cpp
template<class T> class copy_ptr
{
   ...
}
```

Указатель должен реализовывать:
* конструктор по умолчанию;
* конструктор, в качестве параметра принимающий обычный указатель;
* copy-конструктор и copy-оператор присваивания, вызов которых должен приводить к копированию хранящихся внутри данных;
* move-конструктор и move-оператор присваивания, которые должны передавать владение другому указателю вместо копирования;
* стандартные операторы и методы: `*`, `->`, `get`, `reset`, преобразование к `bool`.

Данные функции должны выводить на экран информацию о том, что они были вызваны.

Добавить в класс `CTrace`:

* copy-конструктор;
* copy-оператор присваивания;
* move-конструктор;
* move-оператор присваивания.

Данные функции должны выводить на экран информацию о том, что они были вызваны.

Пример использования:

```
copy_ptr<СTrace> sp1(new СTrace(1));
copy_ptr<CTrace> sp2 = sp1;
```

Продемонстрировать работу указателя, написав unit тесты для созданного класса и используя при этом класс `CTrace`.

*Дополнительное задание 1:* сделать так, чтобы `copy_ptr` умел работать с массивами.

*Дополнительное задание 2:* написать функцию `make_copy`, аналогичную `make_unique` и `make_shared`.