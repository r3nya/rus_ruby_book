# Объекты

## Интроспекция

Интроспекция - это технология, позволяющая определять тип и структуру объектов в процессе выполнения программы.

Интроспекция объектов выполняется с помощью методов экземпляров из классов Object, Class, Module.

В Ruby вся программа может рассматриваться в качестве объекта. Интроспекция программы выполняется с помощью методов классов Class и Module, и методов из модуля Kernel.

### Проверка выражений

Для проверки выражений используется инструкция `defined?`, принимающая выражение.

##### Выражения:

`Выражение` # -> "expression";

`Глобальная переменная` # -> "global-variable";

`Локальная переменная` # -> "local-variable";

`Переменная класса` # -> "class-variable";

`Переменная экземпляра` # -> "instance-variable";

`Константа` # -> "constant";

`true` # -> "true";

`false` # -> "false";

`nil` # -> "nil";

`self` # -> "self";

`yield` # -> "yield";

`super` # -> "super";

`Выражение присваивания` # -> "assignment";

`Вызов метода` # -> "method".

*****

### Тип объекта

`.class # -> class [Object]`

Класс объекта.  
`1.class # -> Fixnum`

`.singleton_class # -> class [Object]`

Собственный класс объекта.

Для nil, true, false возвращаются NilClass, TrueClass, FalseClass соответственно.

Использование для чисел и идентификаторов считается ошибкой.  
`?1.singleton_class # -> #<Class:#<String:0x921114c>>`

`.nil? # -> [Object]`

Проверка отсутствия подходящего объекта. Только для nil возвращается true.

`.===(object) # -> [Module]`

Проверка типа объекта.

~~~~~ ruby
  Fixnum === 1 # -> true
  Integer === 1 # -> true
  Comparable === 1 # -> true
~~~~~

Используется в альтернативном синтаксисе предложения case, позволяя проверять сразу несколько объектов.

`.is_a?(module) # -> [Object]`  
Синонимы: `kind_of?`

Проверка типа объекта.

~~~~~ ruby
  1.is_a? Fixnum # -> true
  1.is_a? Integer # -> true
  1.is_a? Comparable # -> true
~~~~~

`.instance_of?(module) # -> [Object]`

Проверка типа объекта.

~~~~~ ruby
  1.instance_of? Fixnum # -> true
  1.instance_of? Integer # -> false
  1.instance_of? Comparable # -> false
~~~~~

`.respond_to?( method, include_private = false ) # -> [Object]`

Проверка реакции объекта на вызов метода с переданным идентификатором. Логическая величина влияет на необходимость поиска среди частных методов.

Если метод для объекта не определен, то вместо него вызывается метод `.respond_to_missing?`.

### Иерархия наследования

`.superclass # -> class [Class]`

Базовый класс. Для BasicObject возвращается nil.  
`Fixnum.superclass # -> Integer`

`.name # -> string [Module]`

Название модуля. Для анонимных модулей возвращается nil.  
`Fixnum.name # -> "Fixnum"`

`.to_s # -> string [Module]`

Название модуля (по умолчанию). Для анонимных модулей возвращается nil.  
`Fixnum.to_s # -> "Fixnum"`

`.ancestors # -> array [Module]`

Основная иерархия наследования, включая добавленные модули.

~~~~~ ruby
  Fixnum.ancestors
  # -> [Fixnum, Integer, Numeric, Comparable, Object, Kernel, BasicObject]
~~~~~

`.included_modules # -> array [Module]`

Список всех добавленных модулей из основной иерархии наследования.  
`Fixnum.included_modules # -> [ Comparable, Kernel ]`

`.include?(module) # -> [Module]`

Проверка наличия переданного модуля в иерархии наследования.  
`Fixnum.include? Comparable # -> true`

`.<=>(module) # -> -1. 0, 1 или nil [Module]`

Сравнение положения в иерархии наследования.

+ _-1_ - если первый модуль добавлен ко второму;  
  _0_ - если два модуля ссылаются на один объект;  
  _1_ - если второй модуль добавлен к первому;  
  _nil_ - если два модуля не относятся к одной иерархии наследования.

`.<(module) # -> [Module]`

Используется для вычисления отношений в иерархии наследования. Если два модуля не относятся к одной иерархии наследования, то возвращается nil.

~~~~~ ruby
  Integer < Numeric # -> true
  Numeric < Comparable # -> true
  Integer < Comparable # -> true
~~~~~

`.>(module) # -> [Module]`

Используется для вычисления отношений в иерархии наследования. Если два модуля не относятся к одной иерархии наследования, то возвращается nil.

~~~~~ ruby
  Integer > Numeric # -> false
  Numeric > Comparable # -> false
  Integer > Comparable # -> false
~~~~~

`.<=(module) # -> [Module]`

Используется для вычисления отношений в иерархии наследования. Если два модуля не относятся к одной иерархии наследования, то возвращается nil.

~~~~~ ruby
  Integer <= Integer # -> true
  Numeric <= Comparable # -> true
  Integer <= Comparable # -> true
~~~~~

`.>=(module) # -> [Module]`

Используется для вычисления отношений в иерархии наследования. Если два модуля не относятся к одной иерархии наследования, то возвращается nil.

~~~~~ ruby
  Integer >= Integer # -> true
  Numeric => Comparable # -> false
  Integer => Comparable # -> false
~~~~~

### Состояние объекта

`.to_s # -> string [Object]`

Класс и цифровой идентификатор объекта.

`.inspect # -> string [Object]`

Класс и цифровой идентификатор объекта. Используется при выводе объекта.

`.object_id # -> integer [Object]`

Цифровой идентификатор объекта.  
`1.object_id # -> 3`

`.__id__ # -> integer [Object]`

Цифровой идентификатор объекта.  
`1.__id__ # -> 3`

#### Состояние программы

Так как вся программа по сути является объектом, то интроспекция также позволяет узнать о состоянии выполнения программы.

`.global_variables # -> array [PRIVATE: Kernel]`

Идентификаторы глобальных переменных.

`.local_variables # -> array [PRIVATE: Kernel]`

Идентификаторы локальных переменных.

`::constants # -> array [Module]`

Идентификаторы всех констант в теле программы (в теле Object).

`::nesting # -> array [Module]`

Очередь вызовов метода.

##### Переменные и константы:

`RUBY_PATCHLEVEL` - версия интерпретатора;

`RUBY_PLATFORM` - название используемой системы;

`RUBY_RELEASE_DATE` - дата выпуска интерпретатора;

`RUBY_VERSION` - версия языка;

`$PROGRAM_NAME ($0)`  - имя выполняемой программы (по умолчанию - имя файла с расширением);

`__FILE__` - имя выполняемого файла;

`__LINE__` - номер выполняемой строки кода;

`__Encoding__` - кодировка программы.

*****

`.caller( offset = 1 ) # -> array [Kernel]`

Текущая позиция выполнения в виде массива, содержащего:  
`"файл:строка_кода"` или `"файл: строка_кода in метод"`.

Число игнорируемых строк кода может быть ограничено. Если оно больше, чем количество выполненных строк кода, то возвращается nil.

`.__method__ # -> symbol [Kernel]`  
Синонимы: `__callee__`

Идентификатор текущего метода. Вне тела метода возвращается nil.

#### Константы

`.constants( inherited = true ) # -> array [Module]`

Идентификаторы всех констант в теле модуля. Логическая величина влияет на наличие унаследованных констант.

`.const_defined?( name, inherited = true ) # -> [Module]`

Проверка существования константы. Логическая величина влияет на наличие унаследованных констант.  
`Comparable.const_defined? :Fixnum # -> true`

`.const_get( name, inherited = true ) # -> object [Module]`

Значение константы. Отсутствие константы считается исключением `NameError`. Логическая величина влияет на наличие унаследованных констант.

#### Переменные класса

`.class_variables # -> array`

Список идентификаторов всех существующих переменных класса.

`.class_variable_defined?(name)`

Проверка существования переменной класса.

`.class_variable_get(name) # -> object`

Значение переменной класса. Отсутствие переменной считается исключением `NameError`.

#### Переменные экземпляра

`.instance_variables # -> array`

Идентификаторы всех существующих (инициализированных) переменных экземпляра.

`.instance_variable_defined?(name)`

Проверка существования переменной экземпляра.

`.instance_variable_get(name) # -> object`

Значение переменной экземпляра. Отсутствие переменной считается исключением `NameError`.

### Поведение объекта

`.singleton_methods( inherited = true ) # -> array [Object]`

Идентификаторы всех существующих собственных методов.

`.methods( inherited = false ) # -> array [Object]`

Идентификаторы всех существующих общих и защищенных методов экземпляров.

`.public_methods( inherited = true ) # -> array [Object]`

Идентификаторы всех существующих общих методов экземпляров.

`.protected_methods( inherited = true ) # -> array [Object]`

Идентификаторы всех существующих защищенных методов экземпляров.

`.private_methods( inherited = true ) # -> array [Object]`

Идентификаторы всех существующих частных методов экземпляров.

`.instance_methods( inherited = false ) # -> array [Module]`

Идентификаторы всех существующих общих и защищенных методов экземпляров.

`.public_instance_methods( inherited = true ) # -> array [Module]`

Идентификаторы всех существующих общих методов экземпляров.

`.protected_instance_methods( inherited = true) # -> array [Module]`

Идентификаторы всех существующих защищенных методов экземпляров.

`.private_instance_methods( inherited = true ) # -> array [Module]`

Идентификаторы всех существующих частных методов экземпляров.

`.method_defined?(name) # -> [Module]`

Проверка существования общего или защищенного метода экземпляров.  
`Fixnum.method_defined? :next -> true`

`.public_method_defined?(name) # -> [Module]`

Проверка существования общего метода экземпляров.  
`Fixnum.public_method_defined? :next -> true`

`.protected_method_defined?(name) # -> [Module]`

Проверка существования защищенного метода экземпляров.  
`Fixnum.protected_method_defined? :next -> false`

`.private_method_defined?(name) # -> [Module]`

Проверка существования частного метода экземпляров.  
`Fixnum.private_method_defined? :next -> false`

`.block_given? # -> bool [Kernel]`  
Синонимы: `iterator?`

Проверка передан ли методу блок.

## Метапрограммирование

Метапрограммирование - это технология, позволяющая создавать программы, в результате работы которых создаются новые программы или программы, изменяющие себя в процессе выполнения.

### Выполнение произвольного кода

`.binding # -> binding [PRIVATE: Kernel]`

Используется для сохранения текущего состояния выполнения программы. Этот объект может быть передан методу `kernel.eval`.

`.eval( code, file = nil, line = nil ) # -> object [Binding]`

Используется для выполнения произвольного кода в контексте, существовавшим при создании объекта. Дополнительные аргументы обрабатываются при получении исключения.

`.eval( code, binding = nil, file = nil, line = nil ) # -> object [PRIVATE: Kernel]`

`{ } # -> object`

Используется для выполнения произвольного кода. Дополнительные аргументы обрабатываются при получении исключения.

`.module_exec(*arg) { |*arg| } # -> self [Module]`  
Синонимы: `class_exec`

Используется для выполнения блока кода в теле модуля.

`.module_eval( code, file = nil, line = nil ) # -> self [Module]`

`{ } # -> self`  
Синонимы: `class_eval`

Используется для выполнения произвольного кода или блока в теле модуля.  Дополнительные аргументы обрабатываются при получении исключения.

`.instance_exec(*arg) { |*arg| } # -> self [Object]`

Используется для выполнения блока в области видимости объекта (псевдопеременная self ссылается на объект).

`.instance_eval( code, file = nil, line = nil ) # -> object [Object]`

`{ } # -> self`

Используется для выполнения произвольного кода или блока в области видимости объекта (псевдопеременная self ссылается на объект). Дополнительные аргументы обрабатываются при получении исключения.

`.tap { |object| } # -> object [Object]`

Используется для выполнения произвольного блока, принимающего текущий объект и возвращающего его после выполнения.

### Вызов метода

`.send( name, *arg ) # -> [Object]`  
Синонимы: `__send__`

Используется для вызова произвольного метода. Вызов не существующего метода считается исключением.

`.public_send( name, *arg ) # -> [Object]`

Используется для вызова произвольного общего метода. Вызов не существующего метода считается исключением. Можно вызвать методы, идентификаторы которых содержат произвольные символы.

### Перехват выполнения

Перехват выполнения - это технология, позволяющая изменить стандартное поведение интерпретатора при выполнение тех или иных действий.

Перехват выполнения осуществляется после объявления специальных методов, вызываемых автоматически.

`.method_missing(name, *arg)`

Выполняется при отсутствии вызываемого метода.

`.respond_to_missing?( name, include_private = nil )`

Выполняется если при вызове `object.respond_to?` необходимый метод не будет найден.

`.const_missing(name)`

Выполняется при использовании не существующей константы.

`.singleton_method_added(name)`

Выполняется при объявлении собственного метода объекта.

`.singleton_method_removed(name)`

Выполняется при удалении собственного метода объекта.

`.singleton_method_undefined(name)`

Выполняется при запрете вызова собственного метода объекта.

`.method_added(name)`

Выполняется при объявлении метода.

`.method_removed(name)`

Выполняется при удалении метода.

`.method_undefined(name)`

Выполняется при запрете вызова метода.

`.inherited(class)`

Выполняется при создании производного класса.

`.append_features(module)`

Выполняется при использовании модуля.

`.extended(module)`

Выполняется при использовании модуля для расширения класса.

`.included(module)`

Выполняется при испольовании модуля для добавления методов экземпляров.

### Изменение состояния

`.const_set( name, object ) # -> object [Module]`

Используется для определения констант.

`.remove_const(name) # -> object [PRIVATE: Module]`

Используется для удаления констант. Встроенные классы и модули не могут быть удалены.

`.class_variable_set( sym, obj ) # -> object [Module]`

Используется для определения переменной класса.

`.remove_class_variable(name) # -> object [Module]`

Используется для удаления переменной класса.

`.instance_variable_set( name, object ) # -> object [Object]`

Используется для определения переменной экземпляра.

`.remove_instance_variable(name) # -> object [PRIVATE: Object]`

Используется для удаления переменной экземпляра.

### Изменение поведения

`.define_singleton_method( name, block ) # -> block [Object]`

Используется для определения собственного метода объекта.

`.define_method( name, block ) # -> block [PRIVATE: Object]`

Используется для определения метода экземпляров. Можно создавать методы, идентификаторы которых содержат произвольные символы.

`.alias_method( new_name, old_name ) # -> self [PRIVATE: Module]`

Используется для создания синонимов.

`.remove_method(name) # -> self [PRIVATE: Module]`

Используется для удаления метода. Унаследованные методы также не могут быть вызваны.

`.undef_method(name) # -> self [PRIVATE: Module]`

Используется для запрета вызова метода.

## Остальное

### Приведение типов

Приведение типа - это создание на основе переданного объекта экземпляра другого класса.

###### Неявное приведение:

В Ruby существуют соглашения, определяющие процесс приведения типов.

+ Для получения объекта в виде, удобном для интерпретатора, используются методы `object.to_i`, `object.to_s`, `object.to_a`, `object.to_f`, `object.to_c`, `object.to_r` и т.д.

+ Для плучения объекта в виде, удобном для восприятия человеком, используются методы `object.to_int`, `object.to_str`, `object.to_ary`, `object.to_sym`, `object.to_regexp`, `object.to_proc`, `object.to_hash` и т.д.

Для явного приведения типов используются частные методы экземпляров из модуля Kernel. Когда приведение типов невозможно возвращается nil.

`.Array(object) # -> array`

1. `object.to_ary`
2. `object.to_a`

`.Float(object) # -> float`

1. `object.to_f`

`.Integer( object, numeral_system = 10 ) # -> integer`

1. `object.to_int`
2. `object.to_i`

Когда первым аргументом передается текст, второй аргумент считается системой счисления (от 2 до 36). Для двоичной и шестнадцатеричной систем допускаются приставки 0b (0B) и 0x (0X). В другом случае передаваемый текст должен содержать только десятичные цифры.

`.String(object) # -> string`

1. `object.to_s`

`.format( format, *object ) # -> string`  
Синонимы: `sprintf`

Используется для получения форматированного текста `format % [*object]`.

### Сравнение объектов

#### Проверка равенства

`.==(object) # -> [Object]`  
Синонимы: `===`

Проверка равенства с приведением типов.  
`1 == 1.0 -> true`

`.eql?(object) # -> [Object]`

Проверка равенства без приведения типов.  
`1.eql? 1.0 -> false`

`.equal?(object) # -> [Object]`

Проверка идентичности двух ссылок.

~~~~~ ruby
  1.equal? 1.0 -> false
  "1".equal? "1" -> false
  1.equal? 1 -> true
  :a.equal? :a -> true
~~~~~

#### Comparable

В модуле определены операторы для проверки отношения и равенства объектов (`<, <=, >, >=, ==`), основанные на работе оператора `.<=>`.

`.between?( first, last )`

Проверка входит ли объект между двумя заданными границами.

### Копирование объектов

#### Созание копий

Так как все аргументы передаются по ссылке, то изменение их в теле метода может привести к непредсказуемым последствиям. Для решения этой проблемы перед использованием аргумента обычно создают его копию.

Свойства копии будут ссылаться на те же объекты, что и свойства оригинала.

~~~~~ ruby
  a = { t1: 'sa ma ra', t2: 'john' }
  b = a.clone

  b[:t2] = 'jack'
  b[:t1].sub!('ma', 'ha')

  a[:t1] == 'sa ha ra' # -> true
~~~~~

`.clone # -> object [Object]`

Используется для создания копии объекта, сохраняющей все его модификаторы.

`.dup # -> object [Object]`

Используется для создания копии объекта, разрешенной к изменению.

#### Маршализация (Marshal)

Маршализация позволяет сохранять произвольные объекты, извлекать их из выполняемой программы и восстанавливать в другой программе (программы должны использовать одну версию интерпретатора).

##### Константы:

`Marshal::MAJOR_VERSION` - основная версяи интерпретатора;

`Marshal::MINOR_VERSION` - дополнительная версия интерпретатора.

*****

`::dup( object, io = nil, deep = -1 ) # -> string`

Используется для маршализации объекта. Принимается глубина вложенности маршализуемых свойств (по умолчанию значения свойств не сохраняются). Если передается поток, то результат будет записан в поток.

###### Невозможна маршализация:

+ анонимных модулей или классов;
+ объектов, связанных с ОС (файлы, каталоги, потоки и т.д);
+ экземпляров MatchData, Data, Method, UnboundMethod, Proc, Thread, ThreadGroup, Continuation;
+ объектов, определяющих собственные методы.

`Marshal.dump ?$ # -> "\x04\bI\"\x06$\x06:\x06ET"`

`::load(marshal_data) # -> object`

`(marshal_data) { |result| } # -> object`  
Синонимы: `restore`

Используется для восстановления маршализованного объекта.  
`Marshal.load Marshal.dump(?$) # -> "$"`
