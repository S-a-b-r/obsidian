07.08.2023 21:53
Tags: #info 

---
# components в script

Свойство `components` необходимо для подключения дочерних компонентов. Для этого необходимо сделать импорт компонента, добавить имя импортированного компонента в опцию компонента `components` и использовать его в шаблоне.

Обратите внимание, что можно использовать длинную нотацию

```
import Component1 from './component/Component1.vue'

export default {
  components: {
    Component1: Component1
  }
}
```

или сокращенную запись

```
import Component1 from './component/Component1.vue'

export default {
  components: {
    Component1
  }
}
```

---
### Zero-links:
[[00 Frontend]][[00 Vue]]

---
### Links:
[[Script в компоненте]]