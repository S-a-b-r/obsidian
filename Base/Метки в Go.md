27.05.2024 16:20
Tags: #info

---
# Метки в Go
В языке Go есть специальные указатели для операторов:
 - `break`
 - `continue`
 - `goto` (Безусловный оператор перехода)
которые позволяют перемещаться к разным частям кода.

Пример для continue:
```go
outerLoopLabel:
    for i := 0; i < 5; i++ {
        for j := 0; j < 5; j++ {
            fmt.Printf("[%d, %d]\n", i, j)
            continue outerLoopLabel
        }
    }
    fmt.Println("End")
```
Результат:
```txt
[0, 0]
[1, 0]
[2, 0]
[3, 0]
[4, 0]
End
```

---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links: