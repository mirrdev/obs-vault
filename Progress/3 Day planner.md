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