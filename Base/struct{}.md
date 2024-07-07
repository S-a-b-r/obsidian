07.07.2024 16:00
Tags: #info

---
# struct{}
```go
var c struct{}
// или
c := struct{}{}

fmt.Println(unsafe.Sizeof(c)) //0
fmt.Println(unsafe.Pointer(&c)) //0x11d46e8
```
Размер `struct{}` равен `0`, при этом объект `c` имеет адрес. Такую лазейку можно использовать для оптимизации кода по памяти, а в дальнейшем разберём это на практике.


---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links:
[[Структуры]]