# Документация UniMatch
### Запустить или обновить локально

#### Установить [Node.js](https://nodejs.org/uk)

```
> git clone https://github.com/ordenmeny/unimatch-doc.git

> cd unimatch-doc

> npx -y @diplodoc/cli -i ./docs -o ~/unimatch-docs-dist --output-format html
> npx http-server ~/unimatch-docs-dist -p 5005

> listening on 127.0.0.1:5005

```
Документация будет доступна по адресу http://localhost:5005


### Как редактировать документацию
#### Документация расположена в директории:
```
unimatch-doc/docs/ru/...
```
Для редактирования — откройте соответствующие .md-файлы внутри этой директории.
Полный справочник по синтаксису и возможностям YFM (Yandex Flavored Markdown) доступен [по ссылке](https://diplodoc.com/docs/ru/index-yfm).

Опубликованная документация доступна по [ссылке](https://ordenmeny.github.io/unimatch-doc/)