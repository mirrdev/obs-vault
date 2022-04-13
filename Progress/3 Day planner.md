# Дневник
```
# Если мы хотим в точности те же версии всех пакетов,
# какие были у остальных разработчиков этого проекта
npm ci
```
4.02.2020
При слиянии можно указать флаг разрешения конфликтов
git merge dev -Xtheirs Все изменения возмутся из dev 
`merge` одну из двух опций `-Xours` или `-Xtheirs`.

Пример эталонного проекта node с jest **[nodejs-package](https://github.com/hexlet-boilerplates/nodejs-package)**

```
# Вот теперь зависимости из devDependencies устанавливаться не будут
npm install --production

# Продакшен режим можно задать и с помощью переменной окружения
NODE_ENV=production npm install
```

Когда же проект собирается для деплоя на сервер (например, через Github Actions), то флаг нужно применять с `npm ci`:


```
npm ci --production
```

### 10.02.22
Чтобы в функуциях высшего порядка (forEach, map, reduce) досрочно выйти из цикла, по аналогии c 
```javascript
for (const key of keys) {
	if (key === null) {
		return false
	}
}
```
Можно использовать every some 
```javascript
const mapDNA = new Map([['G', 'C'], ['C', 'G'], ['T', 'A'], ['A', 'U']]);
const dnaToRna = (str) => {
const res = [];
return [...str].every((v) => mapDNA.has(v) && res.push(mapDNA.get(v))) ? res.join('') : null;
};
```
Они возвращают true или false и не доходят до конца

Реализуйте и экспортируйте функцию `flatten()`. Эта функция принимает на вход массив и выпрямляет его: если элементами массива являются массивы, то _flatten_ сводит всё к одному массиву, раскрывая один уровень вложенности.
```javascript
// BEGIN (write your solution here)
export const flatten = (arr) =>
  arr.reduce((acc, v) => (Array.isArray(v) ? [...acc, ...v] : [...acc, v]), []);
// END
```
Реализуйте и экспортируйте функцию `getMax()`, которая ищет в массиве максимальное значение и возвращает его.

## objects.js

Реализуйте и экспортируйте по умолчанию функцию, которая выполняет глубокое копирование объектов.
```javascript
const cloneDeep = (obj) => Object.entries(obj).reduce((acc, [k, v]) => ({ ...acc, [k]: v.constructor === Object ? cloneDeep(v) : v }), {});
// END
export { cloneDeep as default };
```
**Внимание стандартная функции Object.is() выдает ошибку на типе null**

Узнал о функции сортировки по алфовиту с учетом локали 
```javascript
function SortArray(x, y){
    return x.LastName.localeCompare(y.LastName);
}
```

**Объекты сравниваются через JSON**
```javascript
// @ts-check
/* eslint no-restricted-syntax: ["off", "ForOfStatement"] */
// BEGIN (write your solution here)
export default (arr, param) =>
  arr.find(
    (obj) => JSON.stringify({ ...obj, ...param }) === JSON.stringify(obj)
  ) ?? null;
// END
```

**Использование флага 
```javascript
// @ts-check
/* eslint no-restricted-syntax: ["off", "ForOfStatement"] */
// BEGIN (write your solution here)
export default (arr, param) => {
  const entries = Object.entries(param);
  for (const obj of arr) {
    let find = true;
    for (const [k, v] of entries) {
      if (obj[k] !== v) {
        find = false;
      }
    }
    if (find) {
      return obj;
    }
  }
  return null;
};
// END
```

Разобрать решение учителя с объектами 
https://ru.hexlet.io/code_reviews/508995
_Инкремент и декремент — единственные базовые арифметические операции в JS, которые обладают побочными эффектами (изменяют само значение в переменной)._

Command-query Separation (CQS) — принцип программирования, изобретённый Бертраном Мейером,

объекты первого рода (или класса)". Им обозначают элементы, которые могут быть переданы в функции, возвращены из функций и присвоены переменным (или константам).

Функции высшего порядка — это функции, которые либо принимают, либо возвращают другие функции, либо делают всё сразу.
**CallBack**
У функции, которая передается внутрь метода `sort()` есть свое название. Подобные функции называют колбеками (callback, обратный вызов). Колбеком становится любая функция, которая вызывается не напрямую программистом, а ее вызывает какая-то функция, в которую мы передаем наш колбек.

```javascript
const myMap = (collection, callback) => {
  const result = [];
  for (const item of collection) {
    // Вызов переданного колбека на каждом элементе коллекции
    const newItem = callback(item);
    // Возврат из колбека добавляется в результирующий массив
    result.push(newItem);
  }

  return result;
};

const numbers = [5, 2, 3];
const newNumbers = myMap(numbers, (number) => number ** 2);
console.log(newNumbers); // => [25, 4, 9]
```

предикатом. То есть её задача вернуть либо `true`, либо `false` для каждого элемента коллекции.

агрегацией понимается операция, вычисляющая значение, зависящее от всего набора данных.

Подчеркну, что возвращать аккумулятор надо всегда, даже если он не изменился.
**Андрей Петраков**, здравствуйте! Вы можете обернуть одну функцию в другую и передать в неё все необходимые параметры, включая те значения, которые определены в окружении:

```javascript
Code (possible spoiler)students.reduce((prev, student) => fun(prev, student, mark), {});

// @ts-check

  

// BEGIN (write your solution here)

const cb = (data, obj, param) => {
const groupName = obj[param];
const group = data[groupName] ?? [];
return { ...data, [groupName]: [...group, obj] };
};

const groupBy = (users, key) => users.reduce((acc, user) => cb(acc, user, key), {});
// END
export default groupBy;
```

### 11.02.22

Паради́гма программи́рования — это совокупность идей и понятий, определяющих стиль написания компьютерных программ (подход к программированию). Это способ концептуализации, определяющий организацию вычислений и структурирование работы, выполняемой компьютером. (Wikipedia)

Императивная парадигма — стиль написания кода в виде набора последовательных инструкций (команд) с активным использованием переменных.

В императивном стиле широко используется присваивание (а значит, и переменные) и циклы.

Ключевое отличие функционального стиля от императивного в том, что при таком стиле программа выглядит как спецификация (которая может быть очень сложной), а не как набор инструкций. То есть программа отвечает на вопрос _ЧТО_ («что мы хотим получить»).

Главное отличие декларативной парадигмы от императивной на практике — отсутствие присваивания.

Отсутствие присваивания автоматически означает то, что в функциональной парадигме невозможно использование циклов. Вместо них используется рекурсия. 

Другой важной особенностью функционального стиля считается активное использование функций как объектов первого рода.

Главная причина создания функций — повышение уровня абстракции, а не сокращение дублирования кода.
1.  [Ментальное программирование 2](https://www.youtube.com/watch?v=vkUTX1hruF8)

### 14.02.22
-   Воркфлоу / Workflows
    
    Каждый репозиторий на GitHub может содержать один или несколько воркфлоу. Каждый воркфлоу определяется в отдельном файле конфигурации в каталоге репозитория _.github/workflows_. Несколько воркфлоу могут выполняться параллельно.
    
-   События / Events
    
    Воркфлоу может запускаться одним или несколькими событиями. Это могут быть внутренние события GitHub (например, пуш, релиз или пул-реквест), запланированные события (запускаются в определенное время — например, cron), или произвольными внешними событиями (запускаются вызовом Webhook API GitHub).
    
-   Задания / Jobs
    
    Воркфлоу состоит из одного или нескольких заданий. Задание содержит набор команд, которые запускаются вместе с рабочим процессом. По умолчанию при запуске воркфлоу все его задания выполняются параллельно, однако между ними можно определить зависимость, чтобы они выполнялись последовательно.
    
-   Раннеры / Runners
    
    Каждое задание выполняется на определённом раннере, — временном сервере на GitHub с выбранной операционной системой (Linux, macOS или Windows). Также существуют [автономные раннеры](https://docs.github.com/en/actions/hosting-your-own-runners), которые позволяют создать своё окружение для выполнения экшена.
    
-   Шаги / Steps
    
    Задания состоят из последовательности шагов. Шаг — это либо команда оболочки (shell command), либо экшен (action). Все шаги задания выполняются последовательно на раннере, связанном с заданием. По умолчанию в случае сбоя шага все следующие шаги задания пропускаются.
    
-   Экшен / Actions
    
    Экшен — многократно используемый блок кода, который может служить шагом задания. Каждый экшен может принимать на вход параметры и создавать любые значения, которые затем можно использовать в других экшенах. Разработчики могут создавать собственные экшены или использовать опубликованные сообществом GitHub. Общих экшенов около тысячи, все они доступны на [GitHub Marketplace](https://github.com/marketplace?type=actions).

предметную область

### 15.02.22
Какой подход был бы подходящим для фрагментации (разделения) массива на множество меньших массивов, скажем, максимум из 10 элементов?
```javascript
const chunk1 = (data, ch) =>
	data.reduce(
		(acc, _, i) => (i % ch ? acc : [...acc, data.slice(i, i + ch)]),
	[]
);
```

```javascript
const chunk = (data, ch) => {
const result = [];
	for (let i = 0; i < data.length; i += ch) {
		result.push(data.slice(i, i + ch));
	}
return result;
};
```


**solution.js**
Реализуйте и экспортируйте по умолчанию функцию, которая находит в массиве непрерывные возрастающие последовательности чисел и возвращает массив с их перечислением.
**Примеры**
```javascript
summaryRanges([]);
// []

summaryRanges([1]);
// []

summaryRanges([1, 2, 3]);
// ['1->3']

summaryRanges([0, 1, 2, 4, 5, 7]);
// ['0->2', '4->5']

summaryRanges([110, 111, 112, 111, -5, -4, -2, -3, -4, -5]);
// ['110->112', '-5->-4']
```
```javascript
const writeSegment = (segment) => `${segment.at(0)}->${segment.at(-1)}`;

const summaryRanges = (arr) => {
	const result = [];
	let segment = [];
	for (let i = 0; i < arr.length; i += 1) {
		const current = arr[i];
		const next = arr[i + 1];
		segment.push(arr[i]);
		if (next !== current + 1) {
			if (segment.length > 1) {
			result.push(writeSegment(segment));
			}
		segment = [];
		}
	}
	return result;
};
```
Пересечение интервалов лучше всего решать через Set
```javascript
const sumIntervals = (internals) => {
  const result = new Set();
  internals.forEach(([start, end]) => {
    for (let i = start; i < end; i += 1) {
	  result.add(i);
	}
  });
  return result.size;
};```

### 31.03.22
---
#### План
- [x] Хеклет Деревья
- [x] Udemi Начать курс Петреченко
- [ ] Прорешать coderslang
- [ ] Повторить теорию по абстракциям

##### Сделано


##### Выводы

---

#### Важное
Вершины, находящиеся на одной глубине и имеющие общего родителя, называют братскими или сестринскими.
Нужно быть осторожнее при передаче по ссылке

### 01.04.22

#### План
- [x] Хеклет Деревья
- [x] Udemi Начать курс Петреченко
- [ ] Прорешать coderslang
- [ ] Повторить теорию по абстракциям

##### Сделано


##### Выводы

---

#### Важное
Вершины, находящиеся на одной глубине и имеющие общего родителя, называют братскими или сестринскими.
Нужно быть осторожнее при передаче по ссылке

### 08.04.22
#### <span style="color:red">План (внимание ответственность)</span>
- [ ] Хеклет ООП
- [ ] Udemi Начать курс Петреченко
- [ ] Прорешать coderslang
- [ ] Повторить теорию по абстракциям
- [ ] Pomidoro

##### Сделано


##### Выводы

---

#### Важное
Подход, в котором код представляет из себя набор ***объектов, взаимодействующих друг с другом***, называется объектно-ориентированным программированием (ООП). Объекты, в таком подходе, это не просто тип данных "объект", это сущности, которые **имеют поведение**, то есть методы для работы с ними.

Выше, когда давалось определение `this`, говорилось, что `this` ссылается на текущий объект, к которому привязан метод. И здесь кроется ключевое отличие `this` в JavaScript от `this` в других языках. В JavaScript `this` у метода может измениться:
```javascript
const company1 = { name: 'Hexlet', getName: function getName() { return this.name } };
const company2 = { name: 'Hexlet Plus' };

company1.getName(); // "Hexlet"

company2.getName = company1.getName;

// В обоих случаях одна и та же функция
company2.getName(); // "Hexlet Plus"
company1.getName(); // "Hexlet"
```

Внутри методов this ссылается на текущий объект, к которому привязан метод.

Что здесь произошло? Вызов той же самой функции из другого объекта привел к смене объекта, на который ссылается `this`. Эта особенность называется **поздним связыванием**. 

Значение `this` ссылается на тот объект, из которого происходит вызов метода.


Функции, записанные внутрь свойств объектов, называют методами:

**Реализация функции bind**
```javascript
// @ts-check

// BEGIN (write your solution here)
// Вариант 1
export default (contex, fn, ...bindArgs) => {
  const res = { ...contex };
  res.fn = fn;
  return (...args) => res.fn(...args, ...bindArgs);
};
// Вариант 2
const bind = (context, fn, ...boundArgs) => (...args) =>
  fn.apply(context, [...boundArgs, ...args]);

// END
```


Стрелочные функции отличаются от обычных не только способом записи. Главное их отличие проявляется в том, как они работают с контекстом. Вкратце: 

**Контекст обычных функций зависит от места вызова, а контекст стрелочных функций — от того места, где они были определены.**

 Стрелочная функция не имеет своего контекста, она связывается с лексическим окружением, то есть функцией, внутри которой определена стрелочная функция. Это очень важный момент. Именно функция верхнего уровня задаёт контекст стрелочной функции, а не что-то другое. И это поведение нельзя изменить с помощью функций `call` или `bind`.

`new` (как и многое в js, он работает не так как `new` в других языках). Фактически он создает объект, устанавливает его как контекст во время вызова конструктора (в данном случае `Company`) и возвращает созданный объект. Именно поэтому сам конструктор ничего не возвращает (хотя может, но это другой разговор), а внутри константы `company` оказывается нужный нам объект.


```javascript
// Упрощенная иллюстрация работы new внутри интерпретатора при таком вызове:
// new Company();

const obj = {};
Company.bind(obj)(name, website); // этот вызов просто наполнил this (равный obj) нужными данными
return obj;
```

```javascript
// Специальный синтаксис создания массивов
// Массивы это объекты, вспомните свойство length
const numbers = [10, 3, -3, 0]; // литерал

// Объектный способ создания через конструктор
// Результат ниже эквивалентен тому что происходит выше
const numbers = new Array(10, 3, -3, 0);

// У дат нет литералов, они создаются как объекты
const date = new Date('December 17, 1995 03:24:00');
// У дат очень много методов
date.getMonth(); // 11, в JS месяцы нумеруются с нуля

// Так можно создавать даже функции
// Последний аргумент это тело, все предыдущие – аргументы
const sum = new Function('a', 'b', 'return a + b');
sum(2, 6); // 8
```

Но не все функции могут быть конструкторами. Отсутствие своего контекста делает невозможным использование оператора `new` вместе со стрелочными функциями:

```javascript
const f = () => {};
// TypeError: function is not a constructor
const obj = new f();
```

Прототип – это обычный объект, хранящийся в специальном служебном поле [[prototype]] (к этому полю невозможно обратиться напрямую).

JavaScript автоматически упаковывает примитивные типы в соответствующие объекты, когда встречает вызовы методов на них (и затем автоматически распаковывает).

Все методы которые мы вызываем на строках, хранятся в прототипе конструктора `String`.

Интересно то, как происходит распаковка. Для этого JavaScript автоматически вызывает метод `valueOf()` у объекта:

toString() - вызывается автоматически в тех ситуациях, когда объект используют как строку.


Прототипы, с одной стороны, мощный механизм, с другой — он низкоуровневый.


В первом случае определятся статическое свойство. Оно принадлежит классу в целом, и не принадлежит каждому конкретному объекту этого класса.

Во втором случае это обычное свойство и при создании новых объектов класса Money, каждый из них будет получать свою собственную копию rates.

```javascript
Code (possible spoiler)
class Example {
  static prop = "static";
  prop2 = "not static";
};
console.log(Example.prop); // static
console.log(Example.prop2); // undefined 
const obj = new Example();
console.log(obj.prop); // undefined
console.log(obj.prop2); // not static


class Money {
	constructor(value) {
	this value = value;
	}
}

```
С исключениями нужно запомнить две вещи: код, в котором произошла ошибка, выбрасывает исключение, а код, в котором ошибка обрабатывается – его ловит.

`throw` прерывает дальнейшее выполнение кода. В этом смысле оно подобно `return`, но в отличие от него, прерывает выполнение не только текущей функции, но и всего кода, вплоть до ближайшего в стеке вызовов блока `catch`.

ООП для меня это сообщения, локальное удержание и защита, скрытие состояния и позднее связывание всего. Это можно сделать в Smalltalk и в LISP.
Аланом Кеем
Как говорят математики, найти базис (минимальный набор элементов, из которых можно вывести всё остальное).

***JS: Объектно-ориентированный дизайн***
Шаблон - это типовое решение типовой задачи
Антипатерн - типовое решение типовой задачи, которое приносит больше вреда чем пользы
Абстракция подразумевает наличие некоторого понятия (типа), значения которого обладают _временем жизни_. Это значит, что она создается и затем многократно и по-разному используется. Например, невозможно представить работу с пользователем в виде одной функции.
Существует формальное правило, позволяющее это определить. Если создание объекта и вызов метода можно заменить на обычную функцию, то ни о какой абстракции речи не идёт, и правильный подход, в данной ситуации, сводится к переносу данных из конструктора в сам метод.

Абстракция данных подразумевает то, что объект представляет из себя не способ выполнить операцию, а конкретный набор данных, который, как правило, может меняться с течением времени. Изменение такого объекта должно отражаться на всех остальных.


Привет! Интересный вопрос. Мое мнение - причина в том, что Object.assign мутирует исходный целевой объект, а слияния через деструктуризацию - возвращает новый объект. И получается, что используя Object.assign мы мутируем static свойства инстанса Truncatter. Изменения сохраняются и влияют на следующий тест.

```
Code (possible spoiler)const obj1 = {a: 1, b: 2};const obj2 = {a: 'changed', b: 2, c: 0};Object.assign(obj1, obj2);console.log(obj1); // { a: 'changed', b: 2, c: 0 };
```

Наверное, Obect.assign - это что-то устаревшее)) с сюрпризами

Отмечено как решение

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[1](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Мария Некрасова](https://ru.hexlet.io/u/mary_an)ТС

[02 июня 2021](https://ru.hexlet.io/topics/55069)

Добрый день! подскажите пожалуйста, почему не все тесты проходят [https://ru.hexlet.io/code_reviews/412237](https://ru.hexlet.io/code_reviews/412237)

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Скрыть комментарии](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Максим Литвинов](https://ru.hexlet.io/u/malcom)Поддержка

[03 июня 2021](https://ru.hexlet.io/topics/55069#116894)

Добрый день, Мария! Опции, переданные в метод, должны дополнять, но не полностью заменять дефолтные опции и опции, переданные в конструктор. Посмотрите пример работы в задании:

```
Code (possible spoiler)const truncater = new Truncater({ 'length': 6 });truncater.truncate('one two', { 'separator': '.' }); // 'one tw.'
```

Опции, переданные в метод `truncate` дополнили опции, переданные в конструктор при создании объекта

Отмечено как решение

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[1](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Мария Некрасова](https://ru.hexlet.io/u/mary_an)ТС

[03 июня 2021](https://ru.hexlet.io/topics/55069#116932)

спасибо

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[alex_de_suzo](https://ru.hexlet.io/u/alex_de_suzo)ТС

[11 мая 2021](https://ru.hexlet.io/topics/54592)

[https://ru.hexlet.io/code_reviews/405398](https://ru.hexlet.io/code_reviews/405398) День добрый,подскажите пс-та почему в конструкторе не определяются никакие опции?

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Максим Литвинов](https://ru.hexlet.io/u/malcom)Поддержка

[12 мая 2021](https://ru.hexlet.io/topics/54592#116003)

Добрый день! Судя по выводу тестов, метод `truncate()` не является функцией:

```
Code (possible spoiler)
```

Уточните в задании, как должен называться метод:

> Реализуйте в классе Truncater конструктор и метод truncate()

Посмотрите в [уроке про статические свойства](https://ru.hexlet.io/courses/js-introduction-to-oop/lessons/static/theory_unit), как обратиться к статическому свойству изнутри объекта.

Также обратите внимание на пример вызова метода:

```
Code (possible spoiler)
```

Метод вызывается у созданного объекта, а значит он не статический.

Отмечено как решение

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Nick Petrenko](https://ru.hexlet.io/u/extendsnull)ТС

[19 апреля 2021](https://ru.hexlet.io/topics/54096)

Линтер ругается на файл с тестами:

```
Code (possible spoiler)
```

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Максим Литвинов](https://ru.hexlet.io/u/malcom)Поддержка

[19 апреля 2021](https://ru.hexlet.io/topics/54096#115021)

Спасибо, поправил! Нажмите Сброс, чтобы получить новую версию упражнения

Отмечено как решение

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Тимофей Ч.](https://ru.hexlet.io/u/tmm)ТС

[11 апреля 2021](https://ru.hexlet.io/topics/53895)

Совсем не понял этот абзац. Думал минут 15)

> Главный недостаток связан с невозможностью подмены реализации (тот самый полиморфизм, о котором мы будем говорить в будущем), так как объект создаётся не на этапе конфигурирования системы, а в том месте, где происходит вызов. Это, в свою очередь, ведёт к тому, что придётся дублировать общие опции, а тестирование станет затруднительным (если не невозможным).

Что означает "подмена реализации" и "этап конфигурирования системы"? И почему тестирование такого варианта затруднительно?

В чем всё-таки преимущество способа использовать всегда один и тот же объект, но с доп. опциями? Ведь по сути это ничем не отличается от первого варианта. Ну, кроме излишнего дублирования объектов и опций. Или в этом и есть недостаток?

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[1](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (3)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[P y t h o n](https://ru.hexlet.io/u/python)Cтудент

[21 сентября 2021](https://ru.hexlet.io/topics/53895#122044)

**Максим Литвинов**, Лучше чем в теории объяснил.

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Владислав](https://ru.hexlet.io/u/user-b32a220ac3974acd)ТС

[09 марта 2021](https://ru.hexlet.io/topics/52870)

Здравствуйте! Похоже, у меня проблема с тем, что я неправильно меняю дефолтные options. [https://ru.hexlet.io/code_reviews/383125](https://ru.hexlet.io/code_reviews/383125) Подскажите, как правильно это делать?

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (6)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Максим Литвинов](https://ru.hexlet.io/u/malcom)Поддержка

[10 марта 2021](https://ru.hexlet.io/topics/52870#112751)

Владислав, я так понимаю, вам удалось справиться с этим заданием? Если вам помогли ответы Андрея, можете отметить их как решение. Так вы сможете поблагодарить его за помощь и поможете другим студентам найти ответ на аналогичный вопрос

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Виталина Данилова](https://ru.hexlet.io/u/vitalinadanilova)ТС

[26 февраля 2021](https://ru.hexlet.io/topics/52575)

Добрый день! Не могу пройти один тест. При отсутствии сепаратора я присваиваю ему дефолтное значение и сливаю с другими параметрами. Получается: `{ separator: '...', length: 7 }`. И это логично, иначе зачем нам вообще этот класс с его опциями по умолчанию?? Однако тесты требуют, чтобы вернулась исходная строка. Может быть, я выбрала неправильный способ решения? [Ревью.](https://ru.hexlet.io/code_reviews/379165)

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (3)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Александр Мандриков](https://ru.hexlet.io/u/nunsez)Cтудент

[05 мая 2021](https://ru.hexlet.io/topics/52575#115762)

**Антон Егоров**, исходная строка - это то, что мы передаём изначально. Если эта строка короче, чем наш порог `length`, то логично, что мы не будем ничего отсекать.

Конкретные числовые параметры длины мы уже задаем вторым аргументом метода `truncate()`. При тестировании этого упражнения используются разные значения. Чтобы лучше понять, как должен работать данный метод, вы можете посмотреть тесты.

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Алексей Шлапаков](https://ru.hexlet.io/u/laqi)ТС

[12 января 2021](https://ru.hexlet.io/topics/51141)

День добрый. Не могу врубиться почему при обращении к defaultOptions, мы делаем это через constructor?

```
Code (possible spoiler)
```

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[1](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (1)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Сергей Мелодин](https://ru.hexlet.io/u/melodyn)Поддержка

[13 января 2021](https://ru.hexlet.io/topics/51141#109391)

**Алексей Шлапаков**, приветствую.

Потому что идёт обращение к функции-конструктору. Это рассматривалось немного раньше в уроке по статическим свойствам: [https://ru.hexlet.io/courses/js-introduction-to-oop/lessons/static/theory_unit](https://ru.hexlet.io/courses/js-introduction-to-oop/lessons/static/theory_unit)

Отмечено как решение

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[1](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Vlad Sergy](https://ru.hexlet.io/u/nfirea)ТС

[10 января 2021](https://ru.hexlet.io/topics/51098)

Доброго времени суток Ув. Менторы.

Запуская код локально, линтер ругается на синтаксис.

```
Code (possible spoiler)
```

Значит ли это, что для это фичи нужен babel-eslint ? Или есть другие опции это обойти, заранее Спасибо.

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (3)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Vlad Sergy](https://ru.hexlet.io/u/nfirea)ТС

[25 января 2021](https://ru.hexlet.io/topics/51098#110168)

И вправду дело было в конфиге, спасибо за помощь.

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Влад Мельник](https://ru.hexlet.io/u/vla2d)ТС

[28 декабря 2020](https://ru.hexlet.io/topics/50754)

Здравствуйте! На данный момент зашел в небольшой тупик, вот моя [попытка решения](https://ru.hexlet.io/code_reviews/356677), но все же я не понимаю, почему на некоторых тестах возникают ошибки. В каком моменте я что то упустил?

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (1)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Владимир Лоскутов](https://ru.hexlet.io/u/inline)Cтудент

[25 января 2021](https://ru.hexlet.io/topics/50754#110203)

**Влад Мельник**, если вдруг еще актуально, то значения дефолтных опций в статическом свойстве у вас отличается от исходных (у вас length: 2, исходное значение должно быть 200)

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Андрей Б](https://ru.hexlet.io/u/bleav)ТС

[21 декабря 2020](https://ru.hexlet.io/topics/50593)

Здравствуйте! Помогите пожалуйста продвинуться, [вот недорешение](https://ru.hexlet.io/code_reviews/354706), я так понял, что опции предаются при вызове нового класса, при вызове метода `truncate`, а еще по умолчанию содержатся в статическом свойстве класса. Как заставить метод извлекать опции из переданных параметров а не из вышеобъявленных свойств?

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (1)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Андрей Б](https://ru.hexlet.io/u/bleav)ТС

[22 декабря 2020](https://ru.hexlet.io/topics/50593#108324)

В общем да. Но не додумался ввести в методе значение по умолчанию для опций, поэтому ввел мутируемый объект в тело метода.

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Denis](https://ru.hexlet.io/u/draught)ТС

[17 декабря 2020](https://ru.hexlet.io/topics/50488)

Добрый день!

Перед выполнением упражнения загуглил, что доступ к статическим свойствам осуществляется через название самого класса - сработало. Увидел решение учителя. Вопрос: доступы абсолютно идентичны и всего лишь по-разному пишутся или отличия внутри тоже есть?

[https://ru.hexlet.io/code_reviews/353431](https://ru.hexlet.io/code_reviews/353431)

Спасибо!

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Сергей Мелодин](https://ru.hexlet.io/u/melodyn)Поддержка

[18 декабря 2020](https://ru.hexlet.io/topics/50488#108129)

**Denis**, приветствую.

Класс может не иметь имени (`export default class {`), в таком случае обращение по имени внутри него не сработает, поэтому вариант через this предпочтительнее.

Отмечено как решение

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Anton Minin](https://ru.hexlet.io/u/aminin)ТС

[09 декабря 2020](https://ru.hexlet.io/topics/50277)

Добрый день!

Слово `троеточие` в упражнении режет глаз.

> функция truncate(), которая обрезает слишком длинный текст и ставит в конце троеточие:

По смыслу, там должно быть **многоточие** (ellipsis).

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (5)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Anton Minin](https://ru.hexlet.io/u/aminin)ТС

[10 декабря 2020](https://ru.hexlet.io/topics/50277#107755)

**Сергей Мелодин**, сорян за холивар ;)

Не сообразил ткнуться в задание, прежде чем жать `Ctrl + Enter`.

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Курс «[JS: Объектно-ориентированный дизайн](https://ru.hexlet.io/courses/js-object-oriented-design)»  
↳ Урок «[Изменяемая конфигурация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)»

[Ekaterina Fedoseeva](https://ru.hexlet.io/u/katherina_fed)ТС

[03 декабря 2020](https://ru.hexlet.io/topics/50064)

Добрый день! Взываю о помощи, запуталась в трех соснах, а именно в условиях метода. Вот [ревью](https://ru.hexlet.io/code_reviews/348137).

Я понимаю, что застряла в элементарном решении, но у меня просто ступор(

Решено

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

[Показать ещё комментарии (2)](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[Сергей Мелодин](https://ru.hexlet.io/u/melodyn)Поддержка

[04 декабря 2020](https://ru.hexlet.io/topics/50064#107328)

**katherina_fedoseeva20**, отлично!

[Ответить](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#)

[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit# "Нравится")

Проверить[](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/next?unit=exercise "Далее")

[](https://ru.hexlet.io/my "Мой Хекслет")[Навигация](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#coursenav-modal)

пройдено 9 уроков из 9

-   [Шаг:упражнение](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#lesson)
-   [Обсуждение](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit#community)
-   [Теория](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/theory_unit)

[Задание](https://ru.hexlet.io/courses/js-object-oriented-design/lessons/configuration-setters/exercise_unit#readme-modal)[Сброс](https://ru.hexlet.io/instances/3156736)Подсказка

Редактор



