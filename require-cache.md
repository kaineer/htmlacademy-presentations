---
title: Кэширование в node
date: 2022-10-11
author: Сергей Ключковский <kaineer@gmail.com>
---

# Когда это возникает

 * Сценарий
   * file.js: `module.exports = { f: () => 42 };`
   * `const a = require("./path/to/file").f`
   * ... меняем file.js ... `module.exports = { f: () => 38 };`
   * `const b = require("./path/to/file").f`

   * Внезапно, a() === b()

 * А надо, чтобы менялось

# Почему это происходит

 * Каждый модуль, после require сохраняется в `require.cache`
 * Ключом становится `resolve('path/to/file')`
 * Значением — загруженный модуль
 * Когда `require()` в следующий раз подтягивает тот же файл, файл не грузится, а берется из кэша

```
  require(..relPath..) --> resolve(relPath) --> fullPath

  ,-require.cache-----------.
  |                         |
  |  [fullPath] ------------+->,-module---------------.
  |                         |  | * filename           |
  |                         |  | * children[]         |
  `-------------------------'  `----------------------'
```

# Как это обходят, 1

 * `uncachedRequire`

```js
    function requireUncached(module) {
      delete require.cache[require.resolve(module)];
      return require(module);
    }
```

 * Если есть циклические зависимости, стреляет в ногу

# Как это обходят, 2

 * Самописное очищение зависимостей

```js
    const { resolve } = require("path");

    const clearCache = (path) => {
      const fullPath = resolve(path);
      const mod = require.cache[fullPath];

      for (const submod of mod.children) {
        const subpath = submod.filepath; // Не помню
        clearCache(subpath);
      }
      delete require.cache[fullPath];
    }
```

 * Может и хорошо, но не факт, что не сломает загрузку, если есть модули, которые зависят от каких-то наших зависимостей

# Как это обходят, 3

 * Например, https://npm.io/import-fresh
 * Если вдруг занадобится, надо будет посмотреть

# Вывод к которому мы пришли

 * Работает - не трогай
 * Давайте попробуем с другой стороны
