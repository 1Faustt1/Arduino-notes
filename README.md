# Справочный материал по *Arduino C* языку
Материал написан *1Faustt1*. Мое личное мнение, что нужно людям в своих начинаниях разработки с платами Arduino.

<br>
<br>

## КОММЕНТАРИИ
- однострочный комментарий
```c
// комментарий
```

<br>

- многострочный комментарий
```c
/*
* комментарий
*/
```

<br>

> **ВСЕ КОММЕНАТРИИ НЕ ЗАНИМАЮТ ПАМЯТЬ**

<br>
<br>

## БИБЛИОТЕКИ

<br>

- подключить файл (библиотеку)
```c
#include библиотека
```

<br>
<br>

## ФУНКЦИИ

<br>

- функция которая не возвращает значения
```elm
void имя
```

<hr>

```elm
void setup() {
  - все находящееся внутри {} будет выполнено 1 раз при загрузке Arduino

  - прописываем настройки и режим работы ардуино
  - инициализируем все, что к ней подключено
  - и т.п
}
```

<hr>

```elm
void loop() {
  - все находящееся внутри {} бесконечно повторяется сверху-вниз

  - идет чтение значений приборов, их обработка
  - выводы на дисплей, вращение моторчиков и т.д
  - произведение расчетов
}
```

<hr>

***Функция*** — фрагмент программного кода (подпрограмма), к которому можно обратиться из другого места программы
> **Функция объявляется вне другой функции**

<br>

**Есть два типа функций:** 
```elm
Не возвращает результат

void <имяФункции>() {

}
```

```elm
Возвращает результат

<типДанных> <имяФункции>() {

}
```

<hr>

```elm
void myFunction() { — не возвращающая функция
  - void — слово, показывающее, что функция ничего не возвращает
  - myFunction — название функции
  - return — оператор, возвращающий результат
  - myFunction(); — обращение к функции в коде
}
```

<br>
<br>

## ВРЕМЕННЫЕ ФУНКЦИИ

<br>

`delay();` — задержка, в скобках указывается число миллисекунд (в 1 сек — 1.000 миллисекунд)
- Максимальное значение 4.294.967.295 мс -> ~1200 часов -> ~50 суток

<br>

`delayMicroseconds();` — задержка, в скобках указывается число микросекунд (в 1 сек — 1.000.000 микросекунд)
- Максимальное значение 16.383 мкс -> 16 миллисекунд

<br>

> **ИСПОЛЬЗОВАТЬ НЕ РЕКОМЕНДУЕТСЯ, ПОТОМУ ЧТО ОСТАНАВЛИВАЕТСЯ ВЕСЬ КОД**
> <br>
> **ниже покажу пример, который рекомендуется использовать**

<br>
<br>

`millis();` — возвращает количество миллисекунд, прошедших с момента включения микроконтроллера
- *Макс.значение:* 4.294.967.295 мс -> ~50 суток
- *Разрешение:* 1 миллисекунда 

`micros();` — возвращает количество микросекунд, прошедших с момента включения микроконтроллера
- *Макс.значение:* 4.294.967.295 мкс -> ~70 минут
- *Разрешение:* 4 микросекунды

<br>
<hr>
<br>

***Пример таймера:***
```elm
unsigned long last_time;
void setup() {

}

void loop() {
  if (millis() - last_time > 5000) {
    last_time = millis();
    <код> // выполнятеся параллельно с другим кодом и не тормозит его!
  }
}
```

<br>
<br>

## ТИПЫ ДАННЫХ

***Объявление:***
- изначально без значения
```elm
<тип данных> <имя>;
```
- изначально с значением
```elm
<тип данных> <имя> = <значение>;
```

***Примеры:***
```elm
int hello1; // по умолчанию 0.
```
```elm
int hello2 = 5; // равна 5.
```

<br>

|**Название**|**Вес**|**Диапазон**|**Особенность**|
|------------|-------|------------|---------------|
|boolean|1 байт|0 или 1|Логическая пременная, может принимать значения true (1) и false (0). <br> Знак инверсии — !|
|char|1 байт|-128 ... 127|Хранит номер символа из таблицы символов ASCII|
|byte|1 байт|0 ... 255|Используется для хранения небольших значений чисел|
|int|2 байта|-32.768 ... 32.767|Используется для хранения чисел|
|unsigned int|2 байта|0 ... 65.535|Используется для хранения только чётных чисел|
|word|2 байта|0 ... 65.535|Тоже самое, что и unsigned int|
|long|4 байта|-2.147.483.648 ... 2.147.483.647|Используется для хранения больших чисел -2 млрд ... 2 млрд|
|unsigned long|4 байта|0 ... 4.294.967.295|Используется для хранения только чётных чисел|
|float|4 байта|-3.4028235Е+38 ... 3.4028235Е+35|Хранит числа с плавающей точной (десятичные дроби)<br>**Точность: 6-7 знаков**|
|double|4 байта||Тоже самое, что и float (не используется)|

<br>

***Особенности float***
1. Просваивать только значения с точкой, *даже если оно целое (10.0).*
2. Делить тоже только на числа с точкой, *даже если они целые (переменная / 2.0).*
3. При делении целочисленного типа с целью получить число с плавающей точкой, *писать (float) перед вычислением!*
- ***Пример:***
```elm
float <имя> = (float) <int переменная> / 2.314
```
> **P.S Операции с числами типа float занимают гораздо больше времени, чем с целыми! Если нужна высокая скорость вычислений, лучше применять всякие хитрости.**

<br>
<br>

## ПЕРЕМЕННЫЕ

<br>

***Действия с переменными***
- `+` `-` `*` `/` — сложить, вычесть, умножить, поделить
- `pow(x, a);` — возвести **x** в степень **a** *(x^a)*.<br>**pow может возводить в дробную степень**
- `sqr(x);` — возвести число **x** в квадрат *(x^2)*
- `sqrt(x);` — взять квадратный корень числа **x**
- `abs(x);` — найти модуль числа **|x|**
- `sin(x);` `cos(x);` `tan(x);` — синус, косинус, тангенс
- `round(x);` — математическое округление
- `ceil(x);` — округлить в бóльшую сторону
- `floor(x);` — округлить в меньшую сторону

<br>

- `x += a;` — прибавить **a** к **x**
- `x -= a;` — вычесть **a** из **x**
- `x *= a;` — домножить **x** на **a**
- `x /= a;` — разделить **x** на **a**
- `x++;` — увеличить **x** на **1**
- `x--;` — уменьшить **x** на **1**

<br>
<br>

## КОНСТАНТЫ

<br>

Объявить константу
```elm
const <тип> <имя> = <значение>;
```
Объявить константу через ***define** *(занимает меньше памяти, и присвоенные значения через define являются именем переменных, т.е #define light1 7 — в коде обращаемся к 7*

> **КОНСТАНТА ЗНАЧЕНИЙ НЕ МЕНЯЕТ**

<br>
<br>

## ОБЛАСТЬ ВИДИМОСТИ **ПЕРЕМЕННЫХ**

<br>

Все переменные объявленные **в коде вне функций — *глобальные***

<br>

Переменные объявленные **в коде в функции — *локальные***

<br>
<br>

## ПОРТЫ

<br>

***COM порт***
- `Serial` — объект библиотеки Serial для работы с последовательным портом (COM портом)
- `Serial.begin(<скорость>);` — открыть порт
- `Serial.print();` — вывод в порт. Переменные и цифры напрямую, в текст в ""
- `Serial.println();` — вывод с переводом строки (в языках Python, С#, С++ и т.д — \n)
- `Serial.println(val, n);` — вывод переменной *val* с *n* числом знаков после запятой
- `Serial.println(val, <базис>);` — вывод с указанным базисом:
  - `DEC` — десятичный (человеческие числа)
  - `HEX` — 16-ричная система
  - `OCT` — 8-ричная система
  - `BIN` — двоичная система
<br>
Данные с компьютера попадают в буфер с объемом 64 байта, и ждут обработки

- `Serial.available();` — проверить буфер на наличие входящих данных
- `Serial.read();` — прочитать входящие данные в символьном формате. Согласно ASCII
- `Serial.read() - '0';` — прочитать данные в целочисленном формате. По одной цифре
- `Serial.parseint();` — прочитать данные в целочисленном формате. Число целиком
<br>

![таблица ASCII](https://ucarecdn.com/53caf7fc-f23c-414c-9847-fd43973928ec/-/crop/1024x514/0,75/-/preview/)

<br>

***Порты ввода/вывода***
- аналоговые и цифровые порты могут работать как *ВХОДЫ* и как *ВЫХОДЫ*
- по умолчанию все работают как *ВЫХОДЫ*

<br>

- `pinMode(pin, mode);` — настройка порта
  - `pin` — номер порта:
    - цифорвые: 0-13
    - аналоговые: 14-19 (A0-A5)
  - `mode` — режим работы порта:
    - INPUT - вход, принимает сигнал
    - OUTPUT - выходы, выдает 0 или 5 Вольт
    - INPUT_PULLUP - вход с подтяжкой к 5 Вольт
- `digitalWrite(pin, signal);` — подать цифровой сигнал
  - `pin` — пин, куда подаем сигнал (см.выше)
  - `signal` — какой сигнал подаем:
    - LOW - 0 (false) 0 Вольт
    - HIGH - 1 (true) 5 Вольт
- `digitalRead(pin);` — прочитать цифровой сигнал
  - `pin` — номер пина, с которого считываем

<br>

***Аналоговые порты ввода/вывода***
`analogRead(pin);` — возвращает значение 0 ... 1023 в зависимости от напряжения на пине от 0 до опорного напряжения (грубо 5 Вольт)
`map(val, min, max, new_min, new_max);` — возвращает величину в новом диапазоне
  - `val` — входная величина
  - `min, max` — минимальное и максимальное значение на входе в **map**
  - `new_min, new_max` — соответственно **min** и **max** значения на выходе
`constrain(val, min, max);` — ограничить диапазон переменной **val** и **min** и **max** (map и constrain обычно используются вместе, дополняя друг друга)

<br>
<br>

## ОПЕРАТОРЫ

<br>

***Операторы сравнения***
- "==" — равно
- "!=" — неравно
- ">" — больше
- "<" — меньше
- ">=" — больше или равно
- "<=" — меньше или равно

> **знак = является знаком присвоить**

<br>

***Логические операторы***
- && — логическое **и**
- || — логическое **или**
- ! — логическое **отрицание**

<br>

***Условные операторы***
```elm
if () {
  - условный оператор, проверяет условие внутри () и выполняет код в {} если оно верное
}
```
```elm
if () {
  - условный оператор, проверяет условие внутри () и выполняет код в {} если оно верное
} else {
 - выполнить кусок кода, если не сработал if
}
```
```elm
if () {
  - условный оператор, проверяет условие внутри () и выполняет код в {} если оно верное
} else if () {
  - выполнить кусок кода, если не сработало условие выше
} else if () {
  - выполнить кусок кода, если не сработало условие выше
} else {
  - выполнить кусок кода, если не сработали условия выше
}
```

<br>

```elm
switch (val) { — рассматриваем переменную val
  case 1: <код> — если она равна 1, выполнить код здесь
  break;

  case 2: <код> — если она равна 2, выполнить код здесь
  break;

  ...........
}
```

<br>
<br>

## ШИМ

<br>

***Стандартный Arduino ШИМ генератор***
*Разрядность:* 8 бит
*Вход:* цифровое значение 0 ... 255
*Выход:* ШИМ сигнал со скважностью 0 ... 100%
*Функция:* analogWrite(pin, duty);
  - `pin` — пин, на который пойдет ШИМ
  - `duty` — значание 0 ... 255

<br>
<br>

## ЦИКЛЫ

<br>

```elm
for (counter; condition; change) {
 - сounter — переменная счётчика, обычно создают новую <локальную>, в стиле — int i = 0;
 - сondition — условие, при котором выполняется цикл, например <счётчик меньше 5> — i < 5;
 - сhange — изменение, т.е увеличение или уменьшение счётчика, например — i++, i--, i += 10;
}
```

<br>

`break;` - выйти из цикла
`continue;` - пропустить итерацию

<br>

```elm
while (condition) {
  - condition — условие, при котором выполняется блок кода заключённый в {}
}
```

```elm
while (1) {
  - бесконечный цикл
}
```

```elm
do {} while (condition); - цикл с постусловием
  - condition - условие, при котором выполняется блок кода, заключённый в {}. В отличие от предыдущего цикла, выполнится хотя бы один раз, даже если условие изначально неверно
```

<br>
<br>

## РАНДОМ

<br>

- `random(min, max);` — функция, возвращающая случайное число в диапазоне от min до max (-1)
- `random(max);` — то же самое, но возвращает от 0 до max (-1)
- `randomSeed(value);` — функция, задающая начало отсчёта генератору псевдослучайных чисел
  - `value` - любое число типа **long**

<br>
<br>

## МАССИВЫ

<br>

**Массив** - структура данных в виде набора Компонентов (элементов массива), расположенных в памяти непосредственно друг за другом, и имеющих адреса, по которым их можно читать или перезаписывать.

***Объявление массива:***
- `<тип> <имя> [<число элементов>];`
- `<тип> <имя> [] = {элемент1, элемент2...};`
  Если не указываются элементы, то обязательно нужно указать размер массива, чтобы под него выделилось место в памяти.
  Размер можно не указывать в том случае, если сразу указываются сразу все элементы.
  Примеры:
- `int myInts[6];`
- `int myPins[] = {2, 4, 8, 3, 6};`
- `int mySensVals[6] = {2, 4, -8, 3, 2};`
- `char message[6] = "hello";`

<br>

> **Нумерация элементов начинается с 0!**

***Обращение к массиву:***
- `<имяМассива> [<номерЯчейки>] = <значение>;`
- `<типДанных> <имяПеременной> = <имяМассива> [<номерЯчейки>];`

***Двумерные массивы:***
```elm
int my2array[2][3] = { - 2 строки, 3 столбца
 {10, 300, 1000},
 {666, 20, 1300}
};
```
- `my2array[0][0]; - это 10`
- `my2array[0][2]; - это 1000`
- `my2array[1][1]; - это 20`
- `my2array[1][0]; - это 300`

<br>
<br>

## АППАРАТНЫЕ ПРЕРЫВАНИЯ

<br>

- `attachInterrupt(pin, function, mode);` — подключить прерывание
- `detachInterrupt(pin);` — отключить прерывание
  - `pin` — пин, на который настроена обработкa
  - `function` — вызываемая функция
  - `mode` — режим работы. Их несколько:
    - `LOW` — срабатывает, когда на пине **LOW**
    - `RISING` — срабатывает, когда сигнал меняется с **LOW** на **HIGH**
    - `FALLING` — срабатывает, когда сигнал меняется с **HIGH** на **LOW**
    - `CHANGE` — срабатывает, когда сигнал меняется с **LOW** на **HIGH** и наоборот
   
- `nolnterrupts();` — приостановить обработку прерывания
- `interrupts();` — продолжить обработку пpерывания

<br>
<br>

## Пока-что на этом всё. В дальнейшем буду добавлять ещё.

<br>
