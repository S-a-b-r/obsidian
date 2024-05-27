23.05.2024 00:20
Tags: #info

---
# Условный оператор if в go

В Go применяется ленивая проверка условий, она идет слева направо до первого false(В случае &&) и до первого true(В случае ||) и прекращается.
Пример:
```go
a, b := 1, 0

incB := func() bool {
    b = b + 1
    return true
}

if a == 1 || incB() {
    fmt.Println("Hello")
}

fmt.Println(a, b)
//Результат:
//Hello 
//1 0
```

---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links: