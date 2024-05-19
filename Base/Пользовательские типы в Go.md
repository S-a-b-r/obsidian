19.05.2024 16:58
Tags: #info

---
# Пользовательские типы в Go

```go
type Name string
type Fruit string

var fruit Fruit
var name Name

fruit = "Apple"
name = fruit // ошибка типизации
             // cannot use fruit (variable of type Fruit) as type Name in assignment
```

Для пользовательских типов можно определять методы

```go
// декларация пользовательского типа
type MyType string
// декларация метода для пользовательского типа
func (mt MyType) MethodForMyType() {
    //логика метода
}
```

---
### Zero-links:
[[00 Backend]] [[00 Golang]] 

---
### Links:
[[Типы в Go]]