07.08.2023 22:19
Tags:

---
# Атрибут lang
Тег `style` также может иметь атрибут `lang`. По умолчанию используется CSS, но мы можем использовать препроцессор. Вот так:

```
<template>
  <div class="some-class">...</div>
</template>

<style scoped lang="stylus">
.some-class
  display flex
  justify-content center
...
</style>
```

В этом примере мы используем stylus. Вы можете использовать целый ряд препроцессоров, таких как sass, scss, less, stylus. Для этого должен быть настроен соответствующий CSS-компилятор. При создании проекта с помощью Vue CLI можно выбрать необходимый, и он будет настроен автоматически.

---
### Zero-links:
[[00 Frontend]]
[[00 Vue]]

---
### Links:
[[style в компоненте]]