07.08.2023 22:02
Tags:

---
# watch in script

`Watch` (метод-наблюдатель) отслеживает изменения указанного свойства и срабатывает при каждом изменении.

Это объект, ключи которого — выражения для наблюдения, а значения — коллбэки, вызываемые при их изменении. Значения также могут быть строками с именами методов, или объектами, содержащими дополнительные опции. Экземпляр Vue вызовет `$watch()`, соответствующий каждому ключу объекта при своём создании.

Ключи (выражения для наблюдения) — названия свойств компонента, за которыми мы ведём наблюдение.

### Варианты записи watch

Часто используемый вариант — запись в виде функции:

```
watch: {
  filters (newFilters, oldFilters) {
    console.log('Filters have been changed!')
  }
}
```

Запись в виде объекта с опциями:

```
watch: {
  filters: {
    immediate: true,
    deep: true,
    handler (newFilters, oldFilters) {
      console.log('Filters have been changed!')
    }
  }
}
```

Сокращённая запись с присваиванием метода (на практике используется редко):

```
watch: {
  filters: 'handleFilters' // указываем название метода-обработчика
}
```

### Параметры функции

Функция принимает два параметра: _новое значение после изменения_ и _старое значение до изменения_. Это очень удобно, когда нам нужно сравнить произведённые изменения.

### Опция deep

Чтобы слежение реагировало на изменения во вложенных объектах, передайте `deep: true` в объекте параметров. Для наблюдения за изменениями массивов этого не требуется.

### Опция immediate

Если передано `immediate: true`, коллбэк будет вызван сразу же после начала наблюдения с текущим значением выражения. Это удобно, когда нужно выполнить определённую логику ещё до первого изменения параметра.

### handler

Это название метода, который мы используем при объектном синтаксисе. Он выступает в роли метода-наблюдателя.

> **Watch, в отличие от computed, может быть асинхронным**. Поэтому вместе с ним можно использовать `async`/`await` и `then`/`catch` конструкции. `Watch` — это отличное место, чтобы вызвать получение данных с сервера, например, при изменении `id` или других параметров.
---
### Zero-links:
[[00 Frontend]]
[[00 Vue]]

---
### Links:
[[Script в компоненте]]