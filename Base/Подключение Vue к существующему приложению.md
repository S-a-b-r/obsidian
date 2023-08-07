17.07.2023 22:25
Tags:

---
# Подключение Vue к существующему приложению

Vue можно встроить в существующее приложение следующим образом:
```
<div id="filter"> Разметка <app>... </app> </div>
```

```
import Vue from 'vue'; 
import App from 'anywhere/app'; const filter = document.getElementById('filter'); const app = new Vue({ el: filter, component: { 'app': App }, ... });
```

---
### Zero-links:
[[00 Vue]]

---
### Links:

