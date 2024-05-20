21.05.2024 00:05
Tags: #info

---
# iota
Служит автоматическим инкрементом для списка объявления констант
```go
const (
    Black = iota
    Gray
    White
)

// счётчик обнуляется
const (
    Yellow = iota
    Red
    Green = iota // это присваивание не обнулит iota
    Blue
)

func main() {
    fmt.Println(Black, Gray, White) 
    fmt.Println(Yellow, Red, Green, Blue)
}

//0 1 2 
//0 1 2 3
```

Также можно пропускать некоторое количество констант или изменять iota
```go
const (
    _ = iota*10 
    ten
    twenty
    thirty
)

const (
    hello = "Hello, world!"  // iota равна 0
    one = 1                  // iota равна 1

    black = iota   // iota равна 2
    gray
)

func main() {
    fmt.Println(ten, twenty, thirty)
    fmt.Println(black, gray)
}
//10 20 30 
//2 3
```


Пример реального кода

```go
type Weekday int

const (
    Monday Weekday = iota + 1
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    Sunday
)

func NextDay(day Weekday) Weekday {
    return (day % 7) + 1
}

func main() {
    var today Weekday = Sunday
    tomorrow := NextDay(today)
    fmt.Println("today =", today, "tomorrow =", tomorrow)
}
```

---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links: