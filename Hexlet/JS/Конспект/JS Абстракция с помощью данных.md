## Введение—JS: Абстракция с помощью данных

Абстракция – основной способ борьбы со сложностью в программировании. Она позволяет уйти от деталей реализации и сосредоточиться на главном.

Хороший пример абстракции – функция сортировки массива. Не важно как она устроена, важно что она приводит к результату, который нас интересует.

Другой пример – функции высшего порядка, такие как `map`, `filter` и `reduce`, которые позволяют обрабатывать коллекции без знания их внутреннего устройства. Причём коллекции не обязательно должны быть плоскими, подобные функции можно написать для сколь угодно сложных структур, например, для деревьев. И благодаря такой абстракции мы можем сосредоточиться на самой обработке, а не на процессе обхода данных.

С другой стороны, сами данные нередко имеют сложную структуру. Представление пользователя в нетривиальной системе может потребовать описания десятков и сотен различных параметров и данных, связанных с ними. В такой ситуации полезно спрятать эту структуру за набором функций, что скроет внутреннюю сложность и упростит поддержку кода. Это называется абстракцией с помощью данных.

В этом курсе мы познакомимся с некоторыми базовыми принципами проектирования программ. С тем, как моделировать и представлять в программе объекты реального (и воображаемого) мира. Примером проектирования послужит создание библиотеки для работы с графическими примитивами, такими как точки, отрезки, фигуры. Эта библиотека, с одной стороны, достаточно понятна для всех (в том числе визуально), с другой — очень просто представляется в коде.

Основные темы курса:

- Предметная область (Domain Model)
- Онтология
- Уровни проектирования (Барьеры абстракции)
- Инварианты

## Онтология—JS: Абстракция с помощью данных

Программы, которые пишут программисты, всегда создаются под определённую предметную область. Например, бухгалтерский софт основывается на правилах ведения бухгалтерского учёта, а сайт для просмотра сериалов — на понятиях из телеиндустрии, таких, как "сезон" и "эпизод". То же самое относится и ко всему остальному: бронирование авиабилетов, отелей, поиск туров, продажа и покупка автомобилей и так далее.

![Domain](https://cdn2.hexlet.io/derivations/image/original/eyJpZCI6IjMyMTRkMjI0M2VmMzUwZGFhNzk3YjNhZTBmMzZiYTAzLnBuZyIsInN0b3JhZ2UiOiJjYWNoZSJ9?signature=16e08156e0f8bba6ac0a155a495215aec2c15118d129eab1713714a72504d52c)

Понимание предметной области, для которой вы пишете программу, так же важно, как и умение программировать. Это не значит, что её нужно знать досконально — иногда область может быть по-настоящему сложной (например, та же бухгалтерия или технологическое производство), но общее понимание всё же требуется.

Рассмотрим в качестве примера Хекслет, так как вы с ним достаточно хорошо знакомы. Вы неплохо знаете его предметную область, хотя вряд ли думали о ней так, как мы сделаем это сейчас. В первую очередь, для её понимания нужно выделить ключевые понятия. У обучающих ресурсов это, как правило, "курс" и "урок", но на самом деле их гораздо больше. В случае Хекслета ещё можно выделить профессию, челендж (практика после курса), code review, квиз (набор вопросов и ответов), участника курса (вы становитесь участником, когда вступаете в курс), проект. Этот список можно продолжать ещё долго. Вероятно, вы удивитесь, но на Хекслете более 200 подобных понятий, которые также называют _сущностями_, и все они описаны в коде.

Сущности находятся в некоторых взаимоотношениях друг с другом. Например, квиз содержит (агрегирует) в себе вопросы. А каждый вопрос, в свою очередь, содержит в себе ответы. Профессия состоит из курсов, а курсы — из уроков, уроки — из теории, квиза и практики. Эти связи имеют конкретные названия. Например:

- Один урок может находиться только в одном курсе, но курс содержит множество уроков. Такая связь называется "один ко многим" (one-to-many или o2m).
- Один курс могут проходить множество пользователей, и один пользователь может проходить много курсов. Такая связь уже называется "многие ко многим" (many-to-many или m2m).
- Реже встречается отношение "один к одному" (one-to-one или o2o). На Хекслете так связаны урок и упражнение.

![ERD](https://cdn2.hexlet.io/derivations/image/original/eyJpZCI6Ijk1YjIyZDdlMjk3MzA1YTNmMzI1NGFlZDViNjNkMDNiLnBuZyIsInN0b3JhZ2UiOiJjYWNoZSJ9?signature=1755715e2eb7be9584a1d7f5963fc3b36ea7555a8006e6373efc249e07dbf973)

В реальности всё ещё чуть сложнее, потому что одна и та же сущность с точки зрения разных систем может выглядеть совершенно по-разному. Пользователь Хекслета с точки зрения бухгалтера (а у нас есть бухгалтер!) и с точки зрения поддержки — две большие разницы.

Описание объектов рассматриваемой области и связей между ними называется **онтологией предметной области**. Эту онтологию хорошо знают эксперты соответствующей области (в бухгалтерии — бухгалтер, в обучении — преподаватель). Но, в отличие от программистов, они часто представляют её на интуитивном уровне, неформально. На практике программисты (или бизнес-аналитики и менеджеры) общаются с заказчиками, которые могут сами выступать в роли экспертов и строят вместе с ними формальную онтологию (это происходит постоянно в процессе развития проекта и не выделяется в отдельный этап проектирования). То есть выделяют конкретные термины, договариваются о том, что они означают и как связаны друг с другом. Затем с помощью [ER-модели](https://ru.wikipedia.org/wiki/ER-модель) программист формирует необходимую модель данных. ER-модель используется при высокоуровневом (концептуальном) проектировании баз данных. На этом этапе уже проявляются зачатки архитектуры будущего приложения.

Далеко не всегда можно однозначно сказать, какая связь существует между двумя сущностями. Иногда программисты думают наперёд и сразу формируют более сложные связи, например, m2m, а не o2m, что сказывается на сложности кода. Чем сложнее связь, тем больше кода и выше стоимость его создания и поддержки. Сложность связей можно описать так (правее — сложнее): o2o, o2m, m2m. Иногда программисты ошибаются при выборе той или иной связи, что обычно говорит о недостаточно хорошем понимании предметной области. Приведу интересный пример. Предположим, что в системе нужно реализовать пользователя и заграничный паспорт. Интуитивно кажется что между этими понятиями связь один к одному: ведь один пользователь может иметь один заграничный паспорт. Так? Не совсем: паспорт может поменяться, если он был утерян или закончился его срок действия. К тому же, в некоторых странах разрешено владение одновременно несколькими заграничными паспортами.

С другой стороны, реальный мир всегда сложнее и полнее, чем любая модель. И задача программиста состоит не в том, чтобы создать универсальную и всеобъемлющую модель некоторой области, а в том, чтобы понять потребности конкретного бизнеса, выделить для них только значимые части рассматриваемой предметной области и перенести их в код.

В зависимости от языка меняется способ представления сущностей в коде. В некоторых определяются типы (используя АТД, интерфейсы или классы), в других — структуры, а третьи вообще не предоставляют никаких вариантов, кроме словарей (ассоциативных массивов).

Объектно-ориентированное программирование (ООП) имеет непосредственное отношение к рассматриваемой теме. Со следующего урока мы начнём изучать различные предметные области и строить подходящие модели данных, попутно изучая новые возможности JavaScript.

![DDD](https://cdn2.hexlet.io/derivations/image/original/eyJpZCI6Ijk5Y2I1YWVmYjNlOGJjMDE5OTI0NWRkNmZjMTE3YmNhLnBuZyIsInN0b3JhZ2UiOiJjYWNoZSJ9?signature=16d6b4b51fa3fa60f9daa1bac8fc3422db396ef06133c8a83f1a278841db1f10)

Наиболее полно рассматриваемая тема раскроется тогда, когда мы начнём изучать ORM. Сейчас достаточно того, что у вас есть общее представление о предметной области, сущностях и связях. Не забудьте почитать информацию по приведённым ссылкам — чем больше вопросов появится на этом этапе, тем лучше.

---

##### Дополнительные материалы

1. [Онтология](<https://ru.wikipedia.org/wiki/Онтология_(информатика)>)
2. [Предметно-ориентированное проектирование](https://ru.wikipedia.org/wiki/Предметно-ориентированное_проектирование)
3. [ER-модель](https://ru.wikipedia.org/wiki/ER-модель)

## Точки на координатной плоскости—JS: Абстракция с помощью данных

Одна из самых удобных тем для тренировки навыков моделирования — геометрия. В отличие от других разделов математики, она визуально представима и интуитивно понятна для всех. Для начала вспомним базовые понятия, с которыми будем иметь дело.

Координатная плоскость (декартова) — плоскость, на которой задана система координат. Координаты задаются на двух пересекающихся под прямым углом прямых (числовых осях) `x` и `y`.

```javascript
                     6 | y
                     5 |
                     4 |
                     3 |     . (2, 3)
                     2 |
                     1 |
                       |
----------------------------------------------------
                       |  1  2  3  4  5  6         x
                       |
                       |
                       |
                       |
                       |
                       |
```

Самый простой примитив, который можно расположить на плоскости — точка. Её положение определяется двумя координатами, и в математике она записывается так: `(2, 3)`, где первое число — координата по оси `x`, а второе — по оси `y`. В коде её можно представить как массив, состоящий из двух элементов.

```javascript
const point = [2, 3];
```

Этого уже достаточно для выполнения полезных геометрических действий. Например, для поиска симметричной точки относительно оси `x`, достаточно инвертировать (поменять знак на противоположный) второе число.

```javascript
                     6 | y
                     5 |
                     4 |
                     3 |     . (2, 3)
                     2 |
                     1 |
                       |
----------------------------------------------------
                       |  1  2  3  4  5  6         x
                    -1 |
                    -2 |
                    -3 |     . (2, -3)
                    -4 |
                    -5 |
                    -6 |
const point = [2, 3];

const [x, y] = point;
// x = 2, y = 3

const symmetricalPoint = [x, -y];
// x = 2, y = -3
```

Иногда бывает нужно найти точку, находящуюся между двумя другими точками ровно посередине (ещё говорят, что нужно найти середину отрезка). Такая точка вычисляется через поиск среднего арифметического каждой из координат. То есть координата `x` "срединной" точки равна `(x1 + x2) / 2`, а координата `y` — `(y1 + y2) / 2`.

```javascript
const getMiddlePoint = (p1, p2) => {
  const x = (p1[0] + p2[0]) / 2;
  const y = (p1[1] + p2[1]) / 2;

  return [x, y];
};

const point1 = [2, 3];
const point2 = [-4, 0];

console.log(getMiddlePoint(point1, point2)); // => [-1, 1.5]
```

Подобных операций в геометрии очень много. С точки зрения организации кода, все функции, связанные с работой точек, логично поместить в модуль `points`.

В свою очередь, точки объединяются в отрезки. Каждый отрезок задаётся парой точек, одна из которых — начало, другая — конец отрезка. Отрезок в коде можно представить аналогично точке — массивом из двух элементов, в котором каждый элемент — точка.

```javascript
const point1 = [3, 4];
const point2 = [-8, 10];
const segment = [point1, point2];
```

## Семантика массивов—JS: Абстракция с помощью данных

В предыдущем уроке сказано, что массив — самый простой способ представить точку. Но правильный ли это способ? Давайте разберёмся.

Когда мы говорим про конструкции языка, то, помимо синтаксиса, нужно всегда помнить о семантике. То есть о том, для решения каких задач используются конструкции языка. На практике же инструменты часто используются не по назначению. Это приводит к созданию кода, который сложнее понять и отлаживать, а значит и поддерживать.

Массив (индексированный), по своей сути, — коллекция, набор некоторых однотипных значений, подразумевающих возможность перебора и одинаковой обработки. Кроме того, эти значения друг с другом жёстко не связаны и могут существовать независимо. В массиве часто (но не всегда) отсутствует позиционность, то есть жёстко зафиксированные места для его значений. Либо позиция зависит от конкретной задачи. Вот некоторые примеры из жизни, где массивы подходят:

- Список стоп-слов
- Список пользователей
- Список уроков курса
- Список ходов шахматной партии (порядок важен)

Применительно к нашей графической библиотеке массив подходит, например, для хранения коллекции точек или набора отрезков.

Сама точка не является коллекцией. Это единое целое, части которого не имеют смысла сами по себе. Между ними нельзя установить никакого порядка, в отличие от, скажем, списка пользователей. А код, который работает с конкретной точкой, представленной массивом, всегда ожидает, что массив состоит из двух элементов, каждый из которых имеет определённую позицию. Другими словами, массив используется как структура для описания составного объекта (то есть такого, который описывается не одним значением, а несколькими, в данном случае — двумя числами-координатами).

В такой ситуации правильнее использовать объект:

```javascript
const point = { x: 2, y: 3 };
const symmetricalPoint = { x: -point.x, y: point.y };
```

Кода стало чуть больше, но семантика важнее. Такой код проще понимать, потому что вместо странных `point[0]` или `point[1]` появляются совершенно понятные с первого взгляда конструкции: `point.x`. Даже при выводе на экран сразу понятно, о чём идёт речь.

Представьте, как будет выглядеть представление отрезка на массивах. Как массив массивов:

```javascript
const point1 = [2, 3];
const point2 = [-8, 10];
const segment = [point1, point2];

// код сложный для понимания
point1[1];
point2[0];
segment[1][0];
```

Понять, что это отрезок, нереально без понимания контекста. Единственное, что частично спасает ситуацию — хорошие имена переменных, но этого мало.

Использование правильной (подходящей под задачу) структуры данных намного важнее:

```javascript
const point1 = { x: 3, y: 4 };
const point2 = { x: -8, y: 10 };
const segment = { beginPoint: point1, endPoint: point2 };

// такой код уже проще для понимания
point1.x;
point2.y;
segment.endPoint.y;
```

Запомните простое правило: код, который заставляет думать — неговорящие имена, плохие абстракции, неправильные структуры данных, сильная зависимость от контекста — плохой код (при этом важно не путать лёгкость и простоту).

Использование объекта сразу даёт ещё одно крайне важное преимущество — расширяемость. Индексированный массив, используемый как структура, хрупок. Поменять местами значение аргументов нельзя — сломается весь код, который рассчитывал на определённый порядок, либо придётся всё переписывать. Расширить тоже просто так не получится: часть кода, конечно, продолжит работать, но часть может сломаться (например, `[x, y] = point`). А использование объекта не полагается на порядок ключей и уж точно не зависит от их количества. В любой момент можно добавить новый ключ, и программа почти наверняка останется работоспособной.

Какие ещё данные нужно представлять в виде объектов? Любую одиночную сущность:

- Пользователь
- Курс
- Урок
- Платёж
- Шахматная партия (помимо даты, имён и места, она содержит набор ходов)

---

##### Дополнительные материалы

1. [Ментальное программирование 1](https://www.youtube.com/watch?v=EEq1wdM2M2w)
2. [Ментальное программирование 2](https://www.youtube.com/watch?v=Hk_uSvADUIo)
3. [Simple Made Easy](https://www.youtube.com/watch?v=SxdOUGdseq4)

## Создание абстракции—JS: Абстракция с помощью данных

Декартова система координат — не единственный способ графического описания. Другой способ — полярная система координат.

> Полярная система координат — двухмерная система координат, в которой каждая точка на плоскости однозначно определяется двумя числами — полярным углом и полярным радиусом. Полярная система координат особенно полезна в случаях, когда отношения между точками проще изобразить в виде радиусов и углов; в более распространённой декартовой, или прямоугольной, системе координат, такие отношения можно установить только путём применения тригонометрических уравнений. (c) Wikipedia

![Polar Coordinate System](https://cdn2.hexlet.io/derivations/image/original/eyJpZCI6ImE1YmY5ODFjMjhjNjc4OTE1YWM4NzY5ZmU1OGRjYmQzLnBuZyIsInN0b3JhZ2UiOiJjYWNoZSJ9?signature=5c925a26ebaa18e1b01ea53574740fd2dbe126246d7e9caae22385227626459f)

Вообразите себе ситуацию. Вы разрабатываете графический редактор (Photoshop!), и ваша библиотека для работы с графическими примитивами построена на базе декартовой системы координат. В какой-то момент вы понимаете, что переход на полярную систему поможет сделать систему проще и быстрее. Какую цену придётся заплатить за такое изменение? Вам придётся переписать практически весь код.

```javascript
const point = { x: 2, y: 3 };
const symmetricalPoint = { x: -point.x, y: point.y };
```

Связано это с тем, что ваша библиотека не скрывает внутреннюю структуру. Любой код, использующий точки или отрезки, знает о том, как они устроены внутри. Это относится как к коду, который создаёт новые примитивы, так и к коду, который извлекает из них составные части. Изменить ситуацию и спрятать реализацию достаточно просто, используя функции:

```
const point = makePoint(3, 4);
const symmetricalPoint = makePoint(-getX(point), getY(point));
```

В примере мы видим три функции `makePoint()`, `getX()` и `getY()`. Функция `makePoint()` называется конструктором, потому что она создаёт новый примитив, функции `getX()` и `getY()` — селекторами (selector), от слова "select", что в переводе означает "извлекать" или "выбирать". Такое небольшое изменение ведёт к далеко идущим последствиям. Главное, что в прикладном коде (том, который использует библиотеку) отсутствует работа со структурой напрямую.

```
// То есть мы работаем не так
const point = [1, 4]; // мы знаем, что это массив
console.log(point[1]); // прямое обращение к массиву

// А так
const point = makePoint(3, 4); // мы не знаем как устроена точка
console.log(getY(point)); // мы получаем доступ к частям только через селекторы
```

Глядя на код, даже нельзя сказать, что из себя представляет точка "изнутри", какими конструкциями языка представлена (для этого можно воспользоваться отладочной печатью). Так мы построили абстракцию данных. Суть этой абстракции заключается в том, что мы скрываем внутреннюю реализацию. Можно сказать, что создание абстракции с помощью данных приводит к сокрытию этих данных от внешнего кода.

А вот один из способов реализовать абстракцию для работы с точкой:

```javascript
const makePoint = (x, y) => ({ x, y });

const getX = (point) => point.x;
const getY = (point) => point.y;
```

Теперь мы можем менять реализацию без необходимости переписывать весь код (однако, переписывание отдельных частей всё же может понадобиться). Несмотря на то, что мы используем функцию `makePoint()`, которая создаёт точку на основе декартовой системы координат (принимает на вход координаты `x` и `y`), внутри она вполне может представляться в полярной системе координат. Другими словами, во время конструирования происходит трансляция из одного формата в другой:

```javascript
const makePoint = (x, y) => {
  // конвертация
  return {
    angle: Math.atan2(y, x),
    radius: Math.sqrt(x ** 2 + y ** 2),
  };
};
```

Начав однажды работать через абстракцию данных, назад пути нет. Придерживайтесь всегда тех функций, которые вы создали сами. Либо тех, которые вам предоставляет используемая библиотека.

## Интерфейсы—JS: Абстракция с помощью данных

В IT широко распространён термин "Интерфейс", который по смыслу похож на то, как мы используем это слово в повседневной жизни. Например, пользовательский интерфейс представляет собой совокупность элементов управления сайтом, банкоматом, телефоном и так далее. Интерфейсом пульта управления от телевизора являются кнопки. Интерфейсом автомобиля можно назвать все рычаги управления и кнопки. Резюмируя, можно сказать, что интерфейс определяет способ взаимодействия с системой.

Создавать хорошие интерфейсы не так уж просто, как может показаться на первый взгляд. Я бы даже сказал, что это крайне сложно. Каждый день мы встречаемся с неудобными интерфейсами, начиная от способов открывания дверей и заканчивая работой лифтов. Чем сложнее система (то есть, чем больше возможных состояний), тем сложнее сделать интерфейс. Даже в примитивном примере с кнопкой включения телевизора (два состояния — вкл/выкл), можно реализовать либо две кнопки, либо одну, которая ведёт себя по-разному в зависимости от текущего состояния.

В программировании всё устроено похожим образом. Интерфейсом называют набор функций (имена и их сигнатуры, то есть количество и типы входящих параметров, а также возвращаемое значение), не зависящих от конкретной реализации. Такое определение один в один совпадает с понятием [абстрактного типа данных](https://ru.wikipedia.org/wiki/Абстрактный_тип_данных). Например, для точек интерфейсными являются все функции, которые мы реализовывали в практике, и которые описывались в теории.

Как соотносятся между собой понятия абстракция и интерфейс? Абстракция — это слово, описывающее в первую очередь те данные, с которыми мы работаем. Например, почти каждое веб-приложение включает в себя абстракцию "пользователь". На Хекслете есть абстракция "курс", "проект" и многое другое. Интерфейсом же называется набор функций, с помощью которых можно взаимодействовать с данными.

Но функции бывают не только интерфейсные, но и вспомогательные, которые не предназначены для вызывающего кода и используются исключительно внутри абстракции:

```javascript
// Функции makeUser, getAge, isAdult — интерфейс абстракции User.
// Они используются внешним (пользовательским, вызывающим) кодом.
const makeUser = (name, birthday) => ({ name, birthday });

const getAge = (user) => calculateAge(user.birthday);

const isAdult = (user) => getAge(user) >= 18;

// Эта функция не является частью интерфейса абстракции User.
// Она является "внутренней".
const calculateAge = (birthday) => {
  const milliSecondsInYear = 31556926 * 1000;

  // Date.now() – возвращает количество миллисекунд,
  // прошедших с 1 января 1970 года 00:00:00 по UTC
  // Date.parse(str) – разбирает строковое представление даты
  // и также возвращает количество миллисекунд
  return Math.trunc((Date.now() - Date.parse(birthday)) / milliSecondsInYear);
};
```

В сложных абстракциях (которые могут быть представлены внешними библиотеками), количество неинтерфейсных функций значительно больше, чем интерфейсных. Вплоть до того, что интерфейсом библиотеки могут являться одна или две функции, но в самой библиотеке их сотни. То, насколько хороша ваша абстракция, определяется, в том числе тем, насколько удобен её интерфейс.

## Уровневое проектирование—JS: Абстракция с помощью данных

Рассмотрим ещё одну простую систему — рациональные числа и операции над ними. Напомню, что рациональным называют число, которое может быть представлено в виде дроби `a/b`, где a — это числитель дроби, b — знаменатель дроби. Причём b не должно быть нулём, поскольку деление на ноль не допускается.

Рациональные числа в JS не поддерживаются, поэтому абстракцию для них создадим самостоятельно. Как обычно, нам понадобятся конструктор и селекторы:

```javascript
const num = makeRational(1, 2); // создали рациональное число "одна вторая"
const numer = getNumer(num); // 1
const denom = getDenom(num); // 2
```

С помощью трёх функций мы определили рациональное число. Одна функция (конструктор) собирает его из частей, другие (селекторы) позволяют каждую часть извлечь. Чем при этом, с точки зрения языка, является `num` — абсолютно не важно. Хоть функцией (и такое возможно), хоть массивом или объектом. Во внутренней реализации можно даже использовать строки:

```javascript
const makeRational = (numer, denom) => `${numer}/${denom}`;

const getNumer = (rational) => rational.split("/")[0];

const getDenom = (rational) => rational.split("/")[1];

console.log(makeRational(10, 3)); // => 10/3
```

Несмотря на то, что мы научились представлять рациональные числа, эта абстракция сама по себе малоприменима. Абстракция становится полезна тогда, когда появляется возможность оперировать ей. Для рациональных чисел базовыми операциями можно считать арифметические, например, сложение, вычитание или умножение. Умножение рациональных чисел — самая простая операция. Для её выполнения нужно перемножить числители и знаменатели:

```
3/4 * 4/5 = (3 * 4)/(4 * 5) = 12/20
```

Самое интересное начинается в процессе реализации. Если предположить, что реальная структура рационального числа выглядит так: `{ numer: 2, denom: 3 }`, то, чисто технически, решение может быть таким:

```javascript
const mul = (rational1, rational2) => {
  return {
    numer: rational1["numer"] * rational2["numer"],
    denom: rational1["denom"] * rational2["denom"],
  };
};
```

С точки зрения вызывающего кода всё нормально, абстракция сохранена. На вход в `mul` подаются рациональные числа, на выходе — рациональное число. А вот внутри никакой абстракции нет, обращение с рациональными числами строится на основе знания их устройства. Любое изменение внутренней реализации рациональных чисел потребует переписывания всех операций, работающих с рациональными числами напрямую — то есть без селекторов или конструктора. Данный код нарушает принцип одного уровня абстракции (single layer abstraction).

При разработке сложных систем используется подход — _уровневое проектирование_. Он заключается в том, что системе придаётся структура при помощи последовательных уровней. Каждый из уровней строится путём комбинации частей, которые на данном уровне рассматриваются как элементарные. Части, которые строятся на каждом уровне, работают как элементарные на следующем уровне.

> Уровневое проектирование пронизывает всю технику построения сложных систем. Например, при проектировании компьютеров резисторы и транзисторы сочетаются (и описываются при помощи языка аналоговых схем), и из них строятся и-, или- элементы и им подобные, служащие основой языка цифровых схем. Из этих элементов строятся процессоры, шины и системы памяти, которые в свою очередь служат элементами в построении компьютеров при помощи языков, подходящих для описания компьютерной архитектуры. Компьютеры, сочетаясь, дают распределённые системы, которые описываются при помощи языков описания сетевых взаимодействий, и так далее. (c) SICP

```javascript
const mul = (rational1, rational2) => {
  return makeRational(
    getNumer(rational1) * getNumer(rational2),
    getDenom(rational1) * getDenom(rational2)
  );
};
```

В нашем примере базовым уровнем являются типы, встроенные в сам язык: числа и объекты. На их основе сформирован уровень для представления рациональных чисел: `makeRational`, `getDenom`, `getNumer`. Затем — уровень, на котором реализованы арифметические операции над рациональными числами: сложение, вычитание, умножение и так далее.

Подчеркну, что речь идёт про реализацию самих уровней. Например, операция сложения полностью опирается на конструктор и селекторы, но ничего не знает и не может знать про внутреннее устройство самих рациональных чисел. С другой стороны, это не значит, что в одном месте не могут появиться функции из разных уровней. Могут и это нормально во многих случаях. Например:

```javascript
const f = (rational1, rational2) => {
  const rational3 = sum(rational1, rational2);
  const denom = getDenom(rational3);
  const numer = getNumer(rational3);
  console.log(`Denom: ${denom}`);
  console.log(`Numer: ${numer}`);
};
```

## Инварианты—JS: Абстракция с помощью данных

Абстракция позволяет нам не думать о деталях реализации и сосредоточиться на её использовании. Более того, при необходимости реализацию абстракции можно всегда переписать, не боясь сломать использующий её код. Но есть ещё одна важная причина, по которой нужно использовать абстракцию — соблюдение инвариантов.

Инвариант в программировании — логическое выражение, определяющее непротиворечивость состояния (набора данных).

Разберёмся на примере. Когда мы описали конструктор и селекторы для рациональных чисел, то неявно подразумевали выполнение следующих инвариантов:

```javascript
const num = makeRational(numer, denom);
numer === getNumer(num); // true
denom === getDenom(num); // true

// Или с учётом нормализации
// numer / denom === getNumer(num) / getDenom(num);
```

Передав в конструктор рационального числа числитель и знаменатель, мы ожидаем, что получим их (те же числа), если применим селекторы к этому рациональному числу. Именно так определяется корректность работы данной абстракции. Этот код практически является тестами!

Инварианты существуют относительно любой операции. И иногда они довольно хитрые. Например, рациональные числа можно сравнивать между собой, но не прямым способом, потому, что одни и те же дроби можно представлять разными способами: _1/2_ и _2/4_. Код, который не учитывает этого факта, работает некорректно:

```javascript
const num1 = makeRational(2, 4);
const num2 = makeRational(8, 16);

console.log(num1 === num2); // false
```

Задача приведения дроби к нормальной форме называется _нормализацией_. В это понятие входит несколько операций, например, сокращение дроби, определение знака, перенос знака в числитель. Реализовать нормализацию можно разными способами. Самый очевидный — выполнять её во время создания дроби, внутри функции `makeRational()`. Другой — выполнять нормализацию уже при обращении через функции `getNumer()` и `getDenom()`. Последний способ обладает недостатком — вычисление нормальной формы происходит на каждый вызов. Избежать этого можно, используя технику [мемоизации](https://ru.wikipedia.org/wiki/Мемоизация).

Учитывая новые вводные, становится понятно, что инвариант, связывающий конструктор и селекторы, нуждается в модификации. Функции `getNumer()` и `getDenom()` должны вернуть не переданные значения, а значения после нормализации (если дробь уже нормализована, то это будут те же самые значения).

```javascript
const num = makeRational(10, 20);
getNumer(num); // 1
getDenom(num); // 2
```

Абстракция не только прячет от нас реализацию, но и отвечает за соблюдение инвариантов. Любая работа в обход абстракции чревата тем, что не будут учтены внутренние преобразования:

```javascript
// Обход конструктора

// Эти данные не нормализованы, потому что не использовался конструктор
const num = { numer: 10, denom: 20 };

// Возвращается не то, что должно (ожидается нормализованный возврат):
getNumer(num); // 10
getDenom(num); // 20

// Прямая модификация

const num = makeRational(10, 20);
// тут не может быть нормализации, так как прямое изменение
num.numer = 40;

getNumer(num); // 40
getDenom(num); // 20
```

То есть работа с данными напрямую, минуя абстракцию, может легко сломать инварианты, которые обеспечивались дополнительной логикой в конструкторе или селекторах. Поэтому важно пользоваться кодом так, как было задумано авторами.

Глядя на примеры выше, возникает закономерный вопрос. А можно ли сделать так, чтобы обойти абстракцию было нельзя? Глобально – да. Такой подход называют сокрытием данных (data hiding). Обычно для обеспечения сокрытия в языках используется [специальный синтаксис](https://github.com/tc39/proposal-class-fields#private-fields). Однако, защиту данных можно организовать и без специальных средств, только за счёт функций высшего порядка. Данный способ основан на создании абстракций с помощью анонимных функций, замыканий и передачи сообщений (подробнее в SICP). Если вы хотите узнать об этом больше, то попробуйте наш курс [JS: Составные данные](https://ru.hexlet.io/courses/compound_data)

- [Компаниям](https://ru.hexlet.io/teams)

## Дополнительные материалы

### [JS: Абстракция с помощью данных](https://ru.hexlet.io/courses/js-data-abstraction)

1. [Введение](https://ru.hexlet.io/courses/js-data-abstraction/lessons/intro/theory_unit)
   - Без материалов
2. [Онтология](https://ru.hexlet.io/courses/js-data-abstraction/lessons/ontology/theory_unit)
   - [Онтология](<https://ru.wikipedia.org/wiki/Онтология_(информатика)>)
   - [Предметно-ориентированное проектирование](https://ru.wikipedia.org/wiki/Предметно-ориентированное_проектирование)
   - [ER-модель](https://ru.wikipedia.org/wiki/ER-модель)
3. [Точки на координатной плоскости](https://ru.hexlet.io/courses/js-data-abstraction/lessons/points/theory_unit)
   - Без материалов
4. [Семантика массивов](https://ru.hexlet.io/courses/js-data-abstraction/lessons/arrays/theory_unit)
   - [Ментальное программирование 1](https://www.youtube.com/watch?v=EEq1wdM2M2w)
   - [Ментальное программирование 2](https://www.youtube.com/watch?v=Hk_uSvADUIo)
   - [Simple Made Easy](https://www.youtube.com/watch?v=SxdOUGdseq4)
5. [Создание абстракции](https://ru.hexlet.io/courses/js-data-abstraction/lessons/abstractions/theory_unit)
   - Без материалов
6. [Интерфейсы](https://ru.hexlet.io/courses/js-data-abstraction/lessons/interface/theory_unit)
   - Без материалов
7. [Уровневое проектирование](https://ru.hexlet.io/courses/js-data-abstraction/lessons/levels/theory_unit)
   - Без материалов
8. [Инварианты](https://ru.hexlet.io/courses/js-data-abstraction/lessons/invariants/theory_unit)
   - Без материалов
