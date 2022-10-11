---
title: Удобный поиск файлов и ссылки
date: 2022-07-01
author: Сергей Ключковский <kaineer@gmail.com>
---

## Fuzzy find

```
  __    ____
 /  `\ /\  _`\
/\_/\_\\ \ \L\ \
\//\//  \ \ ,__/
         \ \ \/
          \ \_\
           \/_/
```

 * 2007 -- Sublime Text
 * 2014 -- Atom [R.I.P.]
 * 2015 -- VSCode
 * 2015 -- FZF.vim (отдельный плагин для VIM)
 * ...

## Зачем: пример demo.js

```shell
$ ls -la1

.rw-rw-r--  199 username 20 Dec  2021 .babelrc
.rw-rw-r--  782 username 20 Dec  2021 .editorconfig
.rw-rw-r--   34 username  5 May 22:47 .eslintignore
.rw-rw-r-- 2,2k username  4 May 15:46 .eslintrc
drwxrwxr-x    - username 30 Jun 13:59 .git
drwxrwxr-x    - username 10 Feb 13:41 .github
.rw-rw-r--   52 username 20 Dec  2021 .gitignore
.rw-rw-r--   60 username 29 Jun 17:37 .mocharc.yaml
.rw-rw-r--   61 username 20 Dec  2021 .npmrc
.rw-rw-r--   17 username 25 Dec  2021 .tmux.json
drwxrwxr-x    - username 20 Dec  2021 bin
drwxrwxr-x    - username 29 Jun 15:28 config
drwxrwxr-x    - username 29 Jun 16:41 dist
.rw-rw-r-- 1,6k username 20 Dec  2021 Jenkinsfile
drwxrwxr-x    - username 29 Jun 15:32 node_modules
.rw-rw-r-- 2,2M username 29 Jun 15:32 package-lock.json
.rw-rw-r-- 5,0k username 30 Jun 11:23 package.json
.rw-rw-r--  349 username 20 Dec  2021 postcss.config.js
.rw-rw-r--  114 username 20 Dec  2021 prettier.config.js
drwxrwxr-x    - username 28 Jun 21:26 public
.rw-rw-r-- 4,9k username 28 Jun 21:26 Readme.md
drwxrwxr-x    - username 20 Dec  2021 server
drwxrwxr-x    - username 29 Jun 11:29 src
drwxrwxr-x    - username 20 Dec  2021 test
.rw-rw-r-- 1,3k username 28 Jun 21:26 todo.md
.rw-rw-r--  410 username  4 May 15:46 tsconfig.json
.rw-rw-r-- 7,4k username 24 Jun 15:47 webpack.config.js
```
 * 26 файлов и каталогов (и это только внутри корня)
 * 11 файлов конфигурации

## Зачем: ищем файлы

```shell
.rw-rw-r--  199 username 20 Dec  2021 .babelrc
.rw-rw-r--  782 username 20 Dec  2021 .editorconfig
.rw-rw-r--   34 username  5 May 22:47 .eslintignore
.rw-rw-r-- 2,2k username  4 May 15:46 .eslintrc
drwxrwxr-x    - username 30 Jun 13:59 .git
drwxrwxr-x    - username 10 Feb 13:41 .github
drwxrwxr-x    - user…,-----------------------------------------.
.rw-rw-r--   52 user…:» test <--- строка поиска                :
.rw-rw-r--   60 user…:-----------------------------------------:
.rw-rw-r--   61 user…: •  public/test-demo/db.json            :
.rw-rw-r--   17 user…:    public/test-demo/js.json            :
drwxrwxr-x    - user…:    public/test-demo/css.json           :
drwxrwxr-x    - user…:    public/test-demo/old.json           :
drwxrwxr-x    - user…:    public/test-demo/php.json           :
.rw-rw-r-- 1,6k user…:    public/test-demo/svg.json           :
drwxrwxr-x    - user…:    public/test-demo/demo.json          :
.rw-rw-r-- 2,2M user…:    public/test-demo/less.json          :
.rw-rw-r-- 5,0k user…:    public/test-demo/node.json          :
.rw-rw-r--  349 user…:    public/test-demo/test.json          :
.rw-rw-r--  114 user…:    .github/workflows/test.yml          :
drwxrwxr-x    - user…:    config/testing/mocharc.yaml         :
.rw-rw-r-- 4,9k user…:    public/test-demo/empty.json         :
drwxrwxr-x    - user…:    public/test-demo/hello.json         :
drwxrwxr-x    - user…:    public/test-demo/react.json         :
drwxrwxr-x    - user…:    public/test-demo/barber.json        :
.rw-rw-r-- 1,3k user…`-----------------------------------------'
.rw-rw-r--  410 username  4 May 15:46 tsconfig.json
.rw-rw-r-- 7,4k username 24 Jun 15:47 webpack.config.js
```

## Зачем: а хотелось бы так

```shell
.rw-rw-r--  199 username 20 Dec  2021 .babelrc
.rw-rw-r--  782 username 20 Dec  2021 .editorconfig
.rw-rw-r--   34 username  5 May 22:47 .eslintignore
.rw-rw-r-- 2,2k username  4 May 15:46 .eslintrc
drwxrwxr-x    - username 30 Jun 13:59 .git
drwxrwxr-x    - username 10 Feb 13:41 .github
drwxrwxr-x    - user…,-----------------------------------------.
.rw-rw-r--   52 user…:» cfgtest                                :
.rw-rw-r--   60 user…:-----------------------------------------:
.rw-rw-r--   61 user…: •  config/testing/eslintrc             :
.rw-rw-r--   17 user…:    config/testing/eslintignore         :
drwxrwxr-x    - user…:    config/testing/mocharc.yaml         :
drwxrwxr-x    - user…:                                         :
drwxrwxr-x    - user…:                                         :
.rw-rw-r-- 1,6k user…:                                         :
drwxrwxr-x    - user…:                                         :
.rw-rw-r-- 2,2M user…:                                         :
.rw-rw-r-- 5,0k user…:                                         :
.rw-rw-r--  349 user…:                                         :
.rw-rw-r--  114 user…:                                         :
drwxrwxr-x    - user…:                                         :
.rw-rw-r-- 4,9k user…:                                         :
drwxrwxr-x    - user…:                                         :
drwxrwxr-x    - user…:                                         :
drwxrwxr-x    - user…:                                         :
.rw-rw-r-- 1,3k user…`-----------------------------------------'
.rw-rw-r--  410 username  4 May 15:46 tsconfig.json
.rw-rw-r-- 7,4k username 24 Jun 15:47 webpack.config.js
```

## Как этого добиться

 * Создаём каталог `config/` в корне репозитория
 * В нём создаём каталоги `testing/`, `building/` и т.д., на что хватит фантазии
 * Переходим в каталоги и создаём символьные ссылки на файлы в корне
 * Пример команды:
```shell
config/building $  ln -s ../../.babelrc babelrc
```
 * Каталог `config/` помещаем в `.npmignore`, во избежание

## Как это выглядит в результате

```shell
$ tree config/

config/
├── building/
│   ├── postcss.config.js -> ../../postcss.config.js
│   ├── tsconfig.json -> ../../tsconfig.json
│   └── webpack.config.js -> ../../webpack.config.js
├── editing/
│   └── prettier.config.js -> ../../prettier.config.js
└── testing/
    ├── eslintignore -> ../../.eslintignore
    ├── eslintrc -> ../../.eslintrc
    └── mocharc.yaml -> ../../.mocharc.yaml
```

## Что хорошего

 * Проще находить файлы конфигурации, сваленные в кучу в корне проекта (нет необходимости помнить все имена файлов по отдельной теме)
 * Если что-то из файлов попадает в две категории (редактирование и проверка кода), делаем для этих файлов ссылки в двух каталогах
 * При этом файлы конфигурации, сами по себе, остаются на месте
 * Если имена для линков задавать без точки, редактор будет показывать их в списке файлов в любом случае (даже если он не показывает скрытые файлы)
 * Ссылки отлично хранятся в репозитории и будут доступны после `git pull`

## Минусы

 * Если в проекте есть папка `config/`, которую необходимо отправлять в npm, настраивать `.npmignore` становится заметно сложнее
 * Если вы программируете под windows, решение, скорее всего, не взлетит.

## Вопросы

```
█████████████████████████████████████████
████ ▄▄▄▄▄ █▀█ █▄ █▄ ▀ ▄▄█▀▄██ ▄▄▄▄▄ ████
████ █   █ █▀▀▀█ ▀▄▄██▀█▄▄▀▀ █ █   █ ████
████ █▄▄▄█ █▀ █▀▀█▄▄ █▀▄▄ ▀▄██ █▄▄▄█ ████
████▄▄▄▄▄▄▄█▄▀ ▀▄▀ ▀▄█ █ ▀ █ █▄▄▄▄▄▄▄████
████ ▄  ▄▀▄ ▄▄▀▄▀ █▀█▄ ▀▀▀▄▀▄ ▀▄█▄▀ ▀████
████▄█ ▄▄▀▄▀ █▄█▀▀▀▄▄  ▀█  ▀▀▄█▀▀▄▀▄█████
████▀▄█▄█▀▄▀▄ ▄█▄ ▀▀█▀█▀▄▀  ▄▄▀▄▄▄█▀▀████
████▄▄▄ ▀ ▄▄▄ ▄ ▄██▄▄██▀▀▀█▀██▄█▄█▀▄█████
█████ ▄██ ▄▄  █▄▀▀█▀▄▀▀▀▀██   ▀▄▄▀█ ▀████
████ ▄█▀█▄▄▄▀▀██▀ ▄▄▄▄▀▀▄▀ ██ █▄ ▄▀▄█████
████ ▄▀▀▄ ▄▀█▄██▄█▄ ██▀▀▄▀▄ ▄▄▀ ▄▀█ ▀████
████ █▀▄█ ▄▄█ █ ▄█▀▀▄ ▄█▄  ▀██▀▄█ ▀▄█████
████▄██▄█▄▄█▀▄▄▄▀▀▀ ▄▄ ▀▄▀▀▄ ▄▄▄ ▀▀█ ████
████ ▄▄▄▄▄ █▄ ▀█▀ ▄▀▄█ ▀█▀▄  █▄█  ▀▄█████
████ █   █ █ ▀ █▄█▄▀▄▄▀▀▀█▀▀▄▄▄▄ ██▄ ████
████ █▄▄▄█ █  ▄ ▄█▀▄▄▄▄▀█ ▀█▄▀ █▀  ██████
████▄▄▄▄▄▄▄█▄██▄█████▄▄█▄██▄▄█▄▄█▄█▄█████
█████████████████████████████████████████
█████████████████████████████████████████
```

 * FAQ
   * lookatme: https://pypi.org/project/lookatme/
   * qrencode: https://fukuchi.org/works/qrencode/

