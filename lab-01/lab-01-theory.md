# Теоретические сведения

## Цель работы

Приобрести навыки работы с интерпретатором языка [Haskell](https://www.haskell.org/hugs/downloads/2006-05/WinHugs-May2006.exe). Получить представление об основных типах языка Haskell. Научиться определять простейшие функции.

## Основы работы с интерпретатором Hugs

Для выполнения лабораторных работ будет использоваться интерпретатор языка Haskell. Существует несколько реализаций интерпретатора; в настоящем курсе будет использоваться интерпретатор Hugs (это название является аббревиатурой слов Haskell users' Gofer system. Gofer-название языка программирования, который был одним из предшественников Haskell).&#x20;

После запуска интерпретатора Hugs (Hugs (Haskell98 mode)) на экране появляется диалоговое окно среды разработчика, автоматически загружается специальный файл предопределений типов и определений стандартных функций на языке Haskell (Prelude.hs), и выводится стандартное приглашение к работе. Это приглашение имеет вид `Hugs>;` вообще, перед символом `>` выводится имя последнего загруженного модуля.&#x20;

После вывода приглашения можно вводить выражения языка Haskell, либо команды интерпретатора. Команды интерпретатора отличаются от выражений языка Haskell тем, что начинаются с символа двоеточия `:`. Примером команды интерпретатора является команда `:quit`, по которой происходит завершение работы интерпретатора. Команды интерпретатора можно сокращать до одной буквы; таким образом, команды `:quit` и `:q` эквивалентны. Команда `:set` используется для того, чтобы установить различные опции интерпретатора. Команда `:?` выводит список доступных команд интерпретатора. В дальнейшем мы рассмотрим другие команды.

## Типы

Программы на языке Haskell представляют собой выражения, вычисление которых приводит к значениям. Каждое значение имеет тип. Интуитивно тип можно понимать просто как множество допустимых значений выражения. Для того, чтобы узнать тип некоторого выражения, можно использовать команду интерпретатора `:type` (или `:t`). Кроме того, можно выполнить команду `:set +t`, для того, чтобы интерпретатор автоматически печатал тип каждого вычисленного результата.

Основными типами языка Haskell являются:

* Типы _Integer_ и Int используется для представления целых чисел, причем значения типа _Integer_ не ограничены по длине; тип _Int_ представляет целые числа фиксированной длины.&#x20;
* Типы _Float_ и _Double_ используется для представления вещественных чисел.
* Тип _Bool_ содержит два значения: _True_ и _False_, и предназначен для представления результата логических выражений.&#x20;
* Тип _Char_ используется для представления символов.

Имена типов в языке Haskell всегда начинаются с заглавной буквы.

Язык Haskell является сильно типизированным языком программирования. Тем не менее в большинстве случаев программист не обязан объявлять, каким типам принадлежат вводимые им переменные. Интерпретатор сам способен вывести типы употребляемых пользователем переменных. Однако, если все же для каких-либо целей необходимо объявить, что некоторое значение принадлежит некоторому типу, используется конструкция вида: `переменная :: Тип`. Если включена опция интерпретатора `+t`, он печатает значения в таком же формате.

```haskell
Hugs>:set +t
Hugs>1
1 :: Integer
Hugs>1.2
1.2 :: Double
Hugs>'a'
'a' :: Char
Hugs>True
True :: Bool
```

Из данного протокола можно сделать вывод, что значения типа _Integer_, _Double_ и _Char_ задаются по тем же правилам, что и в языке Си.

Развитая система типов и строгая типизация делают программы на языке Haskell безопасными по типам. Гарантируется, что в правильной программе на языке Haskell все типы используются правильно. С практической точки зрения это означает, что программа на языке Haskell при выполнении не может вызвать ошибок доступа к памяти (Access violation). Также гарантируется, что в программе не может произойти использование неинициализированных переменных. Таким образом, многие ошибки в программе отслеживаются на этапе ее компиляции, а не выполнения.

## Арифметика

Интерпретатор Hugs можно использовать для вычисления арифметических выражений. При этом можно использовать операторы `+`, `-`, `*`_, `/`_ (сложение, вычитание, умножение и деление) с обычными правилами приоритета. Кроме того, можно использовать оператор ^ (возведение в степень). Таким образом, сеанс работы может выглядеть следующим образом:

```haskell
Hugs>2*2
4 :: Integer
Hugs>4*5 + 1
21 :: Integer
Hugs>
2^3 8 :: Integer
```

Кроме того, можно использовать стандартные математические функции `sqrt` (квадратный корень), `sin`, `cos`, `exp` и т.д. В отличие от многих других языков программирования, в Haskell при вызове функции не обязательно помещать аргумент в скобки. Таким образом, можно просто писать `sqrt 2`, а не `sqrt(2)`. Пример:&#x20;

```haskell
Hugs>sqrt 2 
1.4142135623731 :: Double
Hugs>1 + sqrt 2
2.4142135623731 :: Double
Hugs>sqrt 2 + 1
2.4142135623731 :: Double
Hugs>sqrt (2 + 1)
1.73205080756888 :: Double
```

&#x20;Из данного примера можно сделать вывод, что вызов функции имеет более высокий приоритет, чем арифметические операции, так что выражение `sqrt 2 + 1` интерпретируется как `(sqrt 2) + 1`, а не `sqrt (2 + 1)`. Для задания точного порядка вычисления следует использовать скобки, как в последнем примере. (В действительности вызов функции имеет более высокий приоритет, чем любой бинарный оператор.)

Также следует заметить, что в отличие от большинства других языков программирования, целочисленные выражения в языке Haskell вычисляются с неограниченным числом разрядов (Попробуйте вычислить выражение 2^5000.) В отличие от языка Си, где максимально возможное значение типа _int_ ограничено разрядностью машины (на современных персональных компьютерах оно равно $2^(31-1) = 2147483647$), тип _Integer_ в языке Haskell может хранить целые числа произвольной длины.

## Кортежи

Помимо перечисленных выше простых типов, в языке Haskell можно определять значения составных типов. Например, для задания точки на плоскости необходимы два числа, соответствующие ее координатам. В языке Haskell пару можно задать, перечислив компоненты через запятую и взяв их в скобки: `(5,3)`. Компоненты пары не обязательно должны принадлежать одному типу: можно составить пару, первым элементом которой будет строка, а вторым- число и т.д.

В общем случае, если _a_ и _b_ – некоторые произвольные типы языка Haskell, тип пары, в которой первый элемент принадлежит типу _a_, а второй - типу _b_, обозначается как `(a,b)`. Например, пара `(5,3)` имеет тип `(Integer, Integer);` пара `(1, 'a')` принадлежит типу `(Integer, Char)`. Можно привести и более сложный пример: пара `((1,'a'),1.2)` принадлежит типу `((Integer,Char),Double)`. Проверьте это с помощью интерпретатора.

Следует обратить внимания, что хотя конструкции вида `(1,2)` и `(Integer,Integer)` выглядят похоже, в языке Haskell они обозначают совершенно разные сущности. Первая является значением, в то время как последняя - типом. Для работы с парами в языке Haskell существуют стандартные функции `fst` и `snd`, возвращающие, соответственно, первый и второй элементы пары (названия этих функций происходят от английских слов first (первый) и second (второй)). Таким образом, их можно использовать следующим образом:

```haskell
Hugs>fst (5, True)
5 :: Integer
Prelude>snd (5, True)
True :: Bool
```

&#x20;Кроме пар, аналогичным образом можно определять тройки, четверки и т.д. Их типы записываются аналогичным образом.&#x20;

```
Hugs>(1,2,3)
(1,2,3) :: (Integer,Integer,Integer)
Hugs>(1,2,3,4)
(1,2,3,4) :: (Integer,Integer,Integer,Integer)
```

&#x20;Такая структура данных называется кортежем. В кортеже может храниться фиксированное количество разнородных данных. Функции `fst` и `snd` определены только для пар и не работают для других кортежей. При попытке использовать их, например, для троек, интерпретатор выдает сообщение об ошибке.

Элементом кортежа может быть значение любого типа, в том числе и другой кортеж. Для доступа к элементам кортежей, составленных из пар, может использоваться комбинация функций `fst` и `snd`. Следующий пример демонстрирует извлечение элемента `'a'` из кортежа `(1, ('a', 23.12))`:

```haskell
Hugs> fst (snd (1, ('a', 23.12)))
'a' :: Char
```

## Списки

В отличие от кортежей, список может хранить произвольное количество элементов. Чтобы задать список в Haskell, необходимо в квадратных скобках перечислить его элементы через запятую. Все эти элементы должны принадлежать одному и тому же типу. Тип списка с элементами, принадлежащими типу _a_, обозначается как `[a]`.

```haskell
Hugs>[1,2]
[1,2] :: [Integer]
Hugs>['1','2','3']
['1','2','3'] :: [Char]
```

В списке может не быть ни одного элемента. Пустой список обозначается как `[]`. Оператор `:` (двоеточие) используется для добавления элемента в начало списка. Его левым аргументом должен быть элемент, а правым - списокк:

```haskell
Hugs>1:[2,3]
[1,2,3] :: [Integer]
Hugs>'5':['1','2','3','4','5']
['5','1','2','3','4','5'] :: [Char]
Hugs>False:[]
[False] :: [Bool] 
```

С помощью оператора `:` и пустого списка можно построить любой список:

```haskell
Hugs>1:(2:(3:[]))
[1,2,3] :: Integer 
```

Оператор `:` ассоциативен вправо, поэтому в приведенном выше выражении можно опустить скобки:

```haskell
Hugs>1:2:3:[]
[1,2,3] :: Integer
```

Элементами списка могут быть любые значения- числа, символы, кортежи, другие списки и т.д.

```haskell
Hugs>[(1,'a'),(2,'b')]
[(1,'a'),(2,'b')] :: [(Integer,Char)]
Hugs>[[1,2],[3,4,5]]
[[1,2],[3,4,5]] :: [[Integer]] 
```

Для работы со списками в языке Haskell существует большое количество функций. В данной лабораторной работе рассмотрим только некоторые из них.

* Функция `head` возвращает первый элемент списка.&#x20;
* Функция `last` возвращает последний элемент списка.&#x20;
* Функция `tail` возвращает список без первого элемента
* Функция `init` возвращает список без последнего элемента
* Функция `null` проверяет список на пустоту. Если в качестве аргумента этой операции будет задан пустой список, то функция выдаст значение `True`, в противном случае - `False`
* Функция `length` возвращает длину списка.&#x20;
* Функция `elem` проверяет наличие элемента в списке.&#x20;
* Функция `take` возвращает список, состоящий из `n` первых элементов исходного списка.&#x20;
* Функция `zip` возвращает список, состоящий из пар объединенных исходных списков.
* Функция `!!` возвращает элемент, номер которого задан (начиная с 0).

```haskell
Hugs>tail [1,2,3]
[2,3] :: [Integer]

Hugs>tail [1]
[] :: Integer

Hugs>length [1,2,3]
3 :: Int

Hugs> elem 2 [1,2,3]
True :: Bool

Hugs> take 2 [1,2,3]
[1,2] :: [Integer]

Hugs> zip ["20","30"] [1,2,3]
[("20",1),("30",2)] :: [([Char],Integer)]

Hugs> [1,2,3,4,5] !! 3
4 :: Integer

```

Заметьте, что результат функции `length` принадлежит типу _Int_, а не типу _Integer_. Для соединения (конкатенации) списков в Haskell определен оператор `++`.

## Строки

Строковые значения в языке Haskell, как и в Си, задаются в двойных кавычках. Они принадлежат типу _String_.

```haskell
Hugs>"hello"
"hello" :: String 
```

В действительности строки являются списками символов; таким образом, выражения `"hello"`, `['h','e','l','l','o']` и `'h':'e':'l':'l':'o':[]` означают одно и то же, а тип _String_ является синонимом для `[Char]`. Все функции для работы со списками можно использовать при работе со строками:

```haskell
Hugs>head "hello"
'h' :: Char
Hugs>tail "hello"
"ello" :: [Char]
Hugs>length "hello"
5 :: Int
Hugs>"hello" ++ ", world"
"hello, world" :: [Char] 
```

&#x20;Для преобразования числовых значений в строки и наоборот существуют функции `read` и `show`:&#x20;

```haskell
Hugs>show 1
"1" :: [Char]
Hugs>"Formula " ++ show 1
"Formula 1" :: [Char]
Hugs>1 + read "12"
13 :: Integer 
```

Если функция show не сможет преобразовать строку в число, она сообщит об ошибке.

## Функции

До сих пор мы использовали встроенные функции языка Haskell. Теперь пришла пора научиться определять собственные функции. Для этого нам необходимо изучить еще несколько команд интерпретатора (напомним, что эти команды могут быть сокращены до одной буквы):

* Команда `:load` позволяет загрузить в интерпретатор программу на языке Haskell, содержащуюся в указанном файле.&#x20;
* Команда `:edit` запускает процесс редактирования последнего загруженного файла.&#x20;
* Команда `:reload` перечитывает последний загруженный файл

Определения пользовательских функций должны находиться в файле, который нужно загрузить в интерпретатор Hugs с помощью команды `:load`. Для редактирования загруженной программы можно использовать команду `:edit`. Она запускает внешний редактор (по умолчанию это Notepad) для редактирования файла. После завершения сеанса редактирования редактор необходимо закрыть; при этом интерпретатор Hugs перечитает содержимое изменившегося файла. Однако файл можно редактировать и непосредственно из оболочки Windows. В этом случае, для того чтобы интерпретатор смог перечитать файл, необходимо явно вызывать команду `:reload`.

Рассмотрим пример. Создайте в каком-либо каталоге файл `lab1.hs`. Пусть полный путь к этому файлу `с:\labs\lab1.hs` (это только пример, ваши файлы могут называться по-другому). В интерпретаторе Hugs выполните следующие команды:

```haskell
Hugs>:load "c:\\labs\\lab1.hs" 
```

Если загрузка проведена успешно, приглашение интерпретатора меняется на `Main>`. Дело в том, что если не указано имя модуля, считается, что оно равно `Main`.

```haskell
Main>:edit
```

Здесь должно открыться окно редактора, в котором можно вводить текст программы. Введите:

```haskell
x = [1,2,3]
```

Сохраните файл и закройте редактор. Интерпретатор Hugs загрузит файл `с:\labs\lab1.hs` и теперь значение переменной `x` будет определено:

```haskell
Main>x
[1,2,3] :: [Integer]
```

Обратите внимание, что при записи имени файла в аргументе команды `:load` символы `\` дублируются. Также, как и в языке Си, в Haskell символ `\` служит индикатором начало служебного символа (`'\n'` и т.п.) Для того, чтобы ввести непосредственно символ , необходимо, как и в Си, экранировать его еще одним символом . Теперь можно перейти к определению функций. Создайте, в соответствие с процессом, описанным выше, какой-либо файл и запишите в него следующий текст:

```haskell
square :: Integer -> Integer
square x = x * x

```

Первая строка (`square :: Integer -> Integer`) объявляет, что мы определяем функцию `square`, принимающую параметр типа _Integer_ и возвращающую результат типа Integer. Вторая строка (`square x = x * x`) является непосредственно определением функции. Функция `square` принимает один аргумент и возвращает его квадрат. Функции в языке Haskell являются значениями "первого класса". Это означает, что они "равноправны" с такими значениями, как целые и вещественные числа, символы, строки, списки и т.д. Функции можно передавать в качестве аргументов в другие функции, возвращать их из функций и т.п. Как и все значения в языке Haskell, функции имеют тип. Тип функции, принимающей значения типа _a_ и возвращающей значения типа _b_ обозначается как `a->b`. Загрузите созданный файл в интерпретатор и выполните следующие команды:&#x20;

```haskell
Main>:type square
square :: Integer -> Integer
Main>square 2
4 :: Integer
```

Заметим, что в принципе объявление типа функции `square` не являлось необходимым: интерпретатор сам мог вывести необходимую информацию о типе функции из ее определения. Однако, во-первых, выведенный тип был бы более общим, чем `Integer -> Integer`, а во-вторых, явное указание типа функции является "хорошим тоном" при программировании на языке Haskell, поскольку объявление типа служит своего рода документацией к функции и помогает выявлять ошибки программирования. Имена определяемых пользователем функций и переменных должны начинаться с латинской буквы в нижнем регистре. Остальные символы в имени могут быть прописными или строчными латинскими буквами, цифрами или символами `_` и `'` (подчеркивание и апостроф). Таким образом, ниже перечислены примеры правильных имен переменных:

* `var`
* `var1`
* `variableName`
* `variable_name var'`

## Условные выражения

В определении функции в языке Haskell можно использовать условные выражения. Запишем функцию `signum`, вычисляющую знак переданного ей аргумента:

```haskell
signum :: Integer -> Integer
signum x = if x > 0 then 1
 else if x < 0 then -1
 else 0 
```

Условное выражение записывается в виде: `if условие then выражение else выражение`. Обратите внимание, что хотя по виду это выражение напоминает соответствующий оператор в языке Си или Паскаль, в условном выражении языка Haskell должны присутствовать и then-часть и else-часть. Выражения в then-части и в else-части условного оператора должны принадлежать одному типу. Условие в определении условного оператора представляет собой любое выражение типа Bool. Примером таких выражений могут служить сравнения. При сравнении можно использовать следующие операторы:

* `<`, `>`, `<=`, `>=` – эти операторы имеют такой же смысл, как и в языке Си (меньше, больше, меньше или равно, больше или равно).&#x20;
* `==` – оператор проверки на равенство.&#x20;
* `/=` – оператор проверки на неравенство.

Выражения типа _Bool_ можно комбинировать с помощью общепринятых логических операторов `&&` и `||` (И и ИЛИ), и функции отрицания `not`. Примеры допустимых условий:

```haskell
x >= 0 && x <= 10
x > 3 && x /= 10
(x > 10 || x < -10) && not (x == y) 
```

Разумеется, можно определять свои функции, возвращающие значения типа _Bool_, и использовать их в качестве условий. Например, можно определить функцию `isPositive`, возвращающую `True`, если ее аргумент неотрицателен и `False` в противном случае:

```haskell
isPositive :: Integer -> Bool
isPositive x = if x > 0 then True else False 
```

Теперь функцию `signum` можно определить следующим образом:

```
signum :: Integer -> Integer
signum x = if isPositive x then 1
 else if x < 0 then -1
  else 0
```

Отметим, что функцию `isPositive` можно определить и проще:

```haskell
isPositive x = x > 0
```

## Функции многих переменных и порядок определения функций

До сих пор мы определяли функции, принимающие один аргумент. Разумеется, в языке Haskell можно определять функции, принимающие произвольное количество аргументов. Определение функции `add`, принимающей два целых числа и возвращающей их сумму, выглядит следующим образом:

```haskell
add :: Integer -> Integer -> Integer
add x y = x + y 
```

Тип функции `add` может выглядеть несколько загадочно. В языке Haskell считается, что операция `->` ассоциативна вправо. Таким образом, тип функции `add` может быть прочитан как `Integer -> (Integer -> Integer)`, т.е. в соответствие с правилом каррирования, результатом применения функции `add` к одному аргументу будет функция, принимающая один параметр типа _Integer_. Вообще, тип функции, принимающей `n` аргументов, принадлежащих типам `t1, t2,…, tn`, и возвращающей результат типа _a_, записывается в виде `t1->t2-...->tn->a`.

Следует сделать еще одно замечание, касающееся порядка определения функций. В предыдущем разделе мы определили две функции, `signum` и `isPositive`, одна из которых использовала для своего определения другую. Возникает вопрос: какая из этих функций должна быть определена раньше? Напрашивается ответ, что определение `isPositive` должно предшествовать определению функции `signum`; однако в действительности в языке Haskell порядок определения функций не имеет значения! Таким образом, функция `isPositive` может быть определена как до, так и после функции `signum`.
