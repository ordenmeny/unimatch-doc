# Обновление документации

{% list tabs %}

- Через github

  ### Обновить документацию через [github](https://github.com/ordenmeny/unimatch-doc).
  Сделайте fork, внесите изменения и сделайте pull request.

- Локально

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
  {% note info %}

  Замените output-docs на свой путь!

  {% endnote %}

  {% note tip %}

  Для удобной работы над документацией рекомендуется использовать alias в терминале для сборки и запуска сервера.

  {% endnote %}


  Документация будет доступна по адресу http://localhost:5005. Чтобы обновить удаленную документацию, сделайте pull request.

{% endlist %}



### Как редактировать документацию
#### Документация расположена в директории:
```
unimatch-doc/docs/ru/...
```
Для редактирования — откройте соответствующие .md-файлы внутри этой директории.
Полный справочник по синтаксису и возможностям YFM (Yandex Flavored Markdown) доступен [по ссылке](https://diplodoc.com/docs/ru/index-yfm).
