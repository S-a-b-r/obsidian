20.05.2024 23:34
Tags: #info

---
# Константы в Go

```go
const pi = 3.14159
const doublePi = pi * 2
const version = "1.0.0"

// эквивалентно

const (
   pi = 3.14159
   doublePi = pi * 2
   version = "1.0.0"
)

func main() {
    fmt.Println(version, pi, doublePi)
}
```
Пример использования констант. Но есть ньюанс в типизации.

## Не типизированные константы

```go
const intConst = 5 
const floatConst = 5.0
const runeConst = 'A'
const strConst = "Hello, world!"
const boolConst = true
```

Первая мысль - intConst будет иметь тип int32(int64). Но отсутствие явно указанного типа задает для intConst **целочисленную константу с неопределённым типом** (`untyped int`). Конкретный тип значения этой константы ещё не определён и в разных контекстах будет интерпретироваться компилятором по-разному. 

Это позволяет ослабить типизацию для констант, не отказываясь от сильной типизации глобально.

Благодаря этому свойству будет работать следующий код:
```go
package main

import (
    "fmt"
)

const id = 100

func main() {
    var i int64 = id
    var f float64 = id

    fmt.Println("i=", i, "f=", f)
}
```

Если определить `id` как переменную `var id = 100`, то возникнут ошибки компиляции при определении переменных `i` и `f`:
```txt
./prog.go:10:16: cannot use id (variable of type int) as type int64 in variable declaration 
./prog.go:11:18: cannot use id (variable of type int) as type float64 in variable declaration
```


Если в группе у константы не указано значение, то оно равно значению предыдущей константы.
```go
const (
    pi = 3.1415
    e
    name = "John Doe"
    fullName
)

func main() {
    fmt.Println("pi =", pi, "e =", e)
    fmt.Println("name =", name, "fullName =", fullName)
}//pi = 3.1415 e = 3.1415 \n name = John Doe fullName = John Doe
```

## Типизированные константы

Если при объявлении указывать тип константы явным образом, она становится **типизированной** и подчиняется правилам сильной типизации Go. 

```go
const flag uint8 = 128

func main() {
    var i int = flag
    fmt.Println(i)
}
```
При компиляции этого примера возникнет ошибка `cannot use flag (constant 128 of type uint8) as type int in variable declaration`, так как у константы `flag` тип `uint8`, а у переменной `i` тип `int`.

Нельзя объявлять пустую константу в одиночку. 
```go
const Weight int //missing init expr for Weight
```


---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links: