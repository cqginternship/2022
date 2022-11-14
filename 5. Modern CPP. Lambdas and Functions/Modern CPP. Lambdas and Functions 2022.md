### Modern C++: lambda, bind, function

# Лабораторная работа

## Практическое применение

Существует множество библиотек для взаимодействия C++ с разными скриптовыми языками:
[ChaiScript](http://chaiscript.com),
[AngelScript](https://www.angelcode.com/angelscript),
[Luabind](https://www.rasterbar.com/products/luabind.html).
Они позволяют динамически (без рекомпиляции проекта) добавлять или изменять функционал приложения.

Все они позволяют вызывать C++ функции из скриптового языка.
Это реализуется через регистрацию функций в этих библиотеках:
```cpp
// Function we want to expose to script writers:
void helloWorld() { std::cout << "Hello World!\n"; }

// ChaiScript:
chai.add(chaiscript::fun(&helloWorld), "helloWorld");

// AngelScript:
engine->RegisterGlobalFunction("void helloWorld()", asFUNCTION(helloWorld), asCALL_CDECL);

// Luabind:
module(L)
[
  def("helloWorld", &helloWorld)
];
```

## Задание

В этой лабе мы будем делать упрощенную версию того, что делают все эти библиотеки, но для
[обратной польской записи](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%B0%D1%8F_%D0%BF%D0%BE%D0%BB%D1%8C%D1%81%D0%BA%D0%B0%D1%8F_%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C).
Пользователь сможет сам задавать все операторы, которые могут быть использованы в выражениях.

Оператор может быть свободной функцией, лямбдой, bind-ом или функциональным объектом.
Операндами могут быть только целые числа `int`.

Нужно реализовать следующий класс:
```cpp
class ReversePolishSolver
{
public:
   /// @brief Registers new operator with argsCount operands
   /// @param argsCount - number of operands of given function
   /// @param name - operator name to use in expressions
   /// @param fn - function to call
   template<size_t argsCount, class T>
   void RegisterOperator(const std::string& name, T fn);

   /// @brief Parses and solves given expression using registered earlier operators
   int Solve(const std::string& expression);
};
```

В случае ошибки, ваш класс должен бросать исключение с пояснением что пошло не так (некорректное выражение, некорректное имя функции и т.д.).

Пример работы с этим классом:
```cpp
ReversePolishSolver solver;
solver.RegisterOperator<2>("+", std::plus<>());
solver.RegisterOperator<1>("double", [](int x) { return x * 2; });
std::cout << solver.Solve("2 double 3 4 + +") << '\n'; // 11

solver.RegisterOperator<2>("-", std::minus<>());
std::cout << solver.Solve("2 double 3 4 + -") << '\n'; // - 3

solver.RegisterOperator<1>("2", ...); // exception (function name can't be a number)
solver.Solve("2 +"); // exception (not enough arguments to call '+')
```

1. Добавить поддержку бинарных операторов;
1. Добавить поддержку унарных операторов;
1. Передавать в RegisterOperator lambda, bind, свободные функции, функциональные объекты;

## Тестирование

Напишите тесты в любом удобном для вас виде (разрешается просто написать их в функции main),
важно протестировать не только пример, данный в задании, но и подумать про различные граничные случаи.
