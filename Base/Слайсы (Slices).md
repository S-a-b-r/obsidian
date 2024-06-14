13.06.2024 00:52
Tags: #info

---
# Слайсы (Slices)
Аналог массива, но с динамичной размерностью. Тип слайса записывается как тип массива без указания размера. Но, в отличие от массива, переменная без инициализации равна `nil`.
```go
var mySlice []int
```

Слайс — это обёртка над указателем массива, и в Go слайс используется как структура следующего вида:

- указатель на первый элемент базового массива — `ptr`;
- длина слайса — `len`, количество элементов в слайсе;
- ёмкость слайса — `cap`, количество элементов в массиве.

![[Pasted image 20240613091332.png]]

Если просто объявить такую структуру, то она будет равна `nil`.

Для создания слайса используется встроенная функция `make()`:

```go
    mySlice := make([]TypeOfelement, LenOfslice, CapOfSlice)
    mySlice := make([]int, 0) // слайс [], базовый массив []
    mySlice := make([]int, 5) // слайс [0 0 0 0 0], базовый массив [0 0 0 0 0]
    mySlice := make([]int, 5, 10) // слайс [0 0 0 0 0], базовый массив [0 0 0 0 0 0 0 0 0 0]
```

Также новый слайс может быть создан на основе существующего слайса или массива, для этого используется `[i:j]`(операция взятия слайса), где `i` — индекс первого элемента нового слайса, а `j` — индекс следующего элемента, **не входящего** в новый слайс.

```go
    weekTempArr := [7]int{1, 2, 3, 4, 5, 6, 7}
    workDaysSlice := weekTempArr[:5]
    weekendSlice := weekTempArr[5:]
    fromTuesdayToThursDaySlice := weekTempArr[1:4] 
    weekTempSlice := weekTempArr[:]
    
    fmt.Println(workDaysSlice, len(workDaysSlice), cap(workDaysSlice)) // [1 2 3 4 5] 5 7
    fmt.Println(weekendSlice, len(weekendSlice), cap(weekendSlice)) // [6 7] 2 2 
    fmt.Println(fromTuesdayToThursDaySlice, len(fromTuesdayToThursDaySlice), cap(fromTuesdayToThursDaySlice)) // [2 3 4] 3 6 
    fmt.Println(weekTempSlice, len(weekTempSlice), cap(weekTempSlice)) // [1 2 3 4 5 6 7] 7 7
```

### Изменение размеров слайса
Уменьшение размера слайса производится через операцию взятия слайса. Результат взятия можно присвоить этому же слайсу:
```go
    s := []int{1,2,3} // [1 2 3]
    s = s[:len(s)-1] // [1 2]
```

Для добавления элементов к слайсу используется встроенная функция `append`. Она принимает переменную типа «слайс» и одну или несколько переменных типа элемента слайса, затем возвращает новый слайс, который состоит из копии переданного слайса и переданных в него элементов.
```go
    a := []int{1, 2, 3, 4}
    b := a[2:3]   // b = [3]
    b = append(b, 7)
    fmt.Println(a, len(a), cap(a)) // [1 2 3 7] 4 4
    fmt.Println(b, len(b), cap(b)) // [3 7] 2 2
    b = append(b, 8, 9, 10)
    b[0] = 11
    fmt.Println(a, len(a), cap(a)) // [1 2 3 7] 4 4
    fmt.Println(b, len(b), cap(b)) // [11 7 8 9 10] 5 6
```


---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links: