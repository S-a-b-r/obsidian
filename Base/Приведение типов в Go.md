19.05.2024 17:01
Tags: #info

---
# Приведение типов в Go
Для приведения типов используется такой синтаксис 
```go
type(variable)

```

Пример:
```go
type Name string
type Fruit string

var fruit Fruit
var name Name

fruit = "Apple"
name = Name(fruit)
```

Пример со строками

```go
package main

func main() {
    var str string
    str = "Hello, world!"
    println(string(str[0]))
}//result : "H"
```


---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links:
[[Типы в Go]]