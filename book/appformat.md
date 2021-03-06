# Форматные строки
[](appformat)

Форматная строка - это текст, в котором обычные символы перемешаны с специальными синтаксическими конструкциями. Обычные символы переносятся в результат без изменений, а специальные влияют на форматирование объектов.

Спецсимволы классифицируются по их влиянию на форматирование. Из них только устанавливающие тип форматирования являются обязательными.

Синтаксис спецсимволов (пробелы между ними не используются, а добавлены только для наглядности):  
`%[модификатор][размер][.точность]<тип форматирования>`

Размер влияет на количество символов в результате. По умолчанию в начало текста добавляются дополнительные пробелы.  
`"%5d"%2 # -> "    2"`

Точность влияет на количество цифр после десятичной точки (по умолчанию - 6).

##### Типы форматирования:

###### Для целых чисел:

`b` - преобразует число в десятичную систему счисления. Для отрицательных чисел будет использоваться необходимое дополнение до 2 с символами .. в качестве приставки.

~~~~~ ruby
  "%b" % 2 # -> "10"
  "%b" % -2 # -> "..10"
~~~~~

`B` - аналогично предыдущему, но с приставкой 0B в альтернативной нотации.

`d (или i или u)` - преобразует число из двоичной системы счисления в десятичную.  
`"%d" % 0x01 # -> "1"`

`o` - преобразует число в восьмеричную систему счисления. Для отрицательных чисел будет использоваться необходимое дополнение до 2 с символами .. в качестве приставки.

~~~~~ ruby
  "%o" % 2 # -> "2"
  "%o" % -2 # -> "..76"
~~~~~

`x` - преобразует число в шестнадцатеричную систему счисления. Для отрицательных чисел будет использоваться необходимое дополнение до 2 с символами .. в качестве приставки.

~~~~~ ruby
  "%x" % 2 # -> "2"
  "%x" % -2 # -> "..fe"
~~~~~

`X` - аналогично предыдущему, но с приставкой 0X,  в альтернативной нотации.

###### Для десятичных дробей:

`e` - преобразует число в экспоненциальную нотацию.  
`"%e" % 1.2 # -> "1.200000e+00"`

`E` - аналогично предыдущему, но с использованием символа экспоненты E.

`f` - округление числа.  
`"%.3f" % 1.2 # -> "1.200"`

`g` - преобразует число в экспоненциальную нотацию, если показатель степени будет меньше -4 или больше либо равен точности. В других случаях точность определяет количество значащих цифр.

~~~~~ ruby
  "%.1g" % 1.2 # -> "1"
  "%g" % 123.4 # -> "1e+02"
~~~~~

`G` - аналогично предыдущему, но с использованием символа экспоненты E.

`a` - преобразование числа в формат: знак числа, число в шестнадцатеричной системе счисления с приставкой 0x, символ p, знак показателя степени и показатель степени в десятичной системе счисления.  
`"%.3a" % 1.2 # -> "0x1.333p+0"`

`A` - аналогично предыдущему, но с использованием приставки 0X и символа P.

###### Для других объектов:

`c` - результат будет содержать один символ.  
`"%с" % ?h # -> "h"`

`p` - вызов метода `object.inspect` для объекта.  
`"%p" % ?h # -> "\"h\""`

`s` - преобразование текста. Точность определяет количество символов.  
`"%.3s" % "Ruby" # -> "Rub"`

*****

##### Модификаторы:

`-` - пробелы будут добавляться не в начало, а в конец.  
`4b" % 2 # -> "10  "`

`0 (для чисел)` - вместо пробелов добавляются нули.  
`"%04b" % 2 # -> "0010"`

`+ (для чисел)` - результат будет содержать знак плюса для положительных чисел. Для oxXbB используется обычная запись отрицательных чисел.  
`"%+b" % -2 # -> "-10"`

`# (для эмблем bBoxXaAeEfgG)` - использование альтернативной нотации.

+ Для o повышается точность результата до тех пор пока первая цифра не будет нулем, если не используется дополнительное форматирование.  
`"%#o" % 2 # -> "02"`

+ Для xXbB используются соответствующие приставки.  
`"%#X" % 2 # -> "0X2"`

+ Для aAeEfgG используется десятичная точка даже если в этом нет необходимости.  
`"%#.0E" % 2 # -> "2.E+00"`

+ Для gG используются конечные нули.  
`"%#G" % 1.2 # -> "1.20000"`

`*` - соответствующий спецсимволу объект используется для определения размера. Для отрицательных чисел пробелы добавляются в конце результата.  
`"%*d" % [ -2, 1 ] # -> "1 "`

*****

Если форматная строка содержит несколько спецсимволов, то они будут последовательно использоваться для форматируемых объектов, которые должны храниться в индексном или ассоциативном массивах.

Для индексных массивов модификатор `цифра$` изменяет форматирование элемента с соответствующей позицией (начиная с 1). При этом модификатор должен быть указан для каждого спецсимвола.  
`"%3$d, %1$d, %1$d" % [ 1, 2, 3 ] # -> "3, 1, 1"`

Для ассоциативных массивов модификатор `<идентификатор>` после приставки применяет форматирование для элемента с соответствующим ключом.  
`"%<two>d, %<one>d" % { one: 1, two: 2, three: 3 } # -> "2, 1"`