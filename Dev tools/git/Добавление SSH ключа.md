## Добавление SSH ключа

После создания аккаунта нужно выполнить ещё одну важную операцию — добавления ssh-ключей на github.com. Если по-простому, то ключи позволяют работать репозиториям с Github без необходимости постоянно вводить логин и пароль при синхронизации локального и удаленного репозитория (находящегося на Github).

Эта задача выполняется в два этапа. Сначала нужно сгенерировать ssh-ключи, а затем один из них (публичный) добавить в настройки Github. Подробная инструкция по созданию ssh-ключей доступна на [сайте](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). В двух словах:

```
# Создание ssh-ключей
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Дальше будет несколько вопросов. На все вопросы нужно нажимать Enter.

# Запуск агента ssh, который следит за ключами
eval "$(ssh-agent -s)"

# Добавления нового ssh-ключа в агент
ssh-add ~/.ssh/id_rsa
```

Когда ssh-ключи созданы и добавлены в систему, можно приступать к интеграции с Github. Подробно эта процедура описана в [документации](https://docs.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account). В двух словах:

1.  Выведите содержимое файла _~/.ssh/id_rsa.pub_ и скопируйте его:
    
    ```
    cat ~/.ssh/id_rsa.pub
    ```
    
2.  [Добавьте](https://github.com/settings/keys) ssh-ключ в аккаунт Github. При добавлении вас попросят назвать ключ. Напишите что-нибудь в стиле _home_.

## Основные команды
git init
git status
git restore - востановить файлы из зафиксированных
`Выведет все изменения сделанные в рабочей директории которые были добавлены в индекс`
git diff --staged
git log -p
Тут все коммиты с полным дифом
Мотать вперед f, мотать назад b
Выйти из режима просмотра — q

git show 5120bea3e5528c29f8d1da43731cbe895892eb6d
git blame INFO.md - то последним менял конкретную строку в файле?

```
# Флаг i позволяет искать без учета регистра
git grep -i hexlet

README.md:Hello, Hexlet! How are you?

# Поиск в конкретном коммите
git grep Hexlet 5120bea3

# Поиск по всей истории
# rev-list возвращает список хешей коммитов
git grep hexlet $(git rev-list --all)
```


Показывает сокращенный вывод
git log --oneline
git checkout <хеш коммита> - не меняет ветки только содержимое рабочей директории
git checkout main
git branch
-   У `git rm` есть опция `--cached`. Если её использовать, файл будет удалён из индекса, но не из рабочей директории
![git stash](https://cdn2.hexlet.io/derivations/image/original/eyJpZCI6ImY4ZjU4MmFjY2FlNTQ1NzY4NTM2OTUwOTVkYmUwODJhLmpwZyIsInN0b3JhZ2UiOiJjYWNoZSJ9?signature=c13cc2d6408a8f67a8eda11bb07ba0e8fcde26053081bc6ec1cf64638aa7977e)
git stash 
git stash pop
