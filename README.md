# Документация UniMatch
### Опубликованная документация доступна по [ссылке](https://ordenmeny.github.io/unimatch-doc/)
### или
### Запустить или обновить локально

#### Установите [Node.js](https://nodejs.org/uk)
#### Установите пакет Diplodoc CLI, выполнив команду:
```
npm i @diplodoc/cli -g
```
#### Откройте терминал и введите команды:
```
> git clone https://github.com/ordenmeny/unimatch-doc.git

> cd unimatch-doc

> npx -y @diplodoc/cli -i ./docs -o output-docs --output-format html
> npx http-server output-docs -p 5005

> listening on 127.0.0.1:5005

```
Замените output-docs на свой путь!


Документация будет доступна по адресу http://localhost:5005


### Как редактировать документацию
#### Документация расположена в директории:
```
unimatch-doc/docs/ru/...
```
Для редактирования — откройте соответствующие .md-файлы внутри этой директории.
Полный справочник по синтаксису и возможностям YFM (Yandex Flavored Markdown) доступен [по ссылке](https://diplodoc.com/docs/ru/index-yfm).