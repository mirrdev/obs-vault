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