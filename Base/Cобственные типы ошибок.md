01.10.2024 00:32
Tags: #info

---
# Cобственные типы ошибок
Иногда в ошибке, кроме текста, нужно передать дополнительную информацию. Разберём пример, в котором вместе с текстом ошибки будем возвращать время её возникновения:

```go
import (
    "fmt"
    "time"
)
// Создадим собственный тип, который удовлетворяет интерфейсу error
// TimeError — тип для хранения времени и текста ошибки.
type TimeError struct {
    Time time.Time
    Text string
}

// Error добавляет поддержку интерфейса error для типа TimeError.
func (te TimeError) Error() string {
    return fmt.Sprintf("%v: %v", te.Time.Format(`2006/01/02 15:04:05`), te.Text)
}

// NewTimeError возвращает переменную типа TimeError c текущим временем.
func NewTimeError(text string) TimeError {
    return TimeError{
        Time: time.Now(),
        Text: text,
    }
}

func testFunc(i int) error {
    // несмотря на то что NewTimeError возвращает тип TimeError,
    // у testFunc тип возвращаемого значения равен error
    if i == 0 {
        return NewTimeError(`параметр в testFunc равен 0`)
    }
    return nil
}

func main() {
    if err := testFunc(0); err != nil {
        fmt.Println(err)
    }
    // 2021/05/28 11:33:00: Параметр в testFunc равен 0
}
```

Если ошибкой может выступать переменная любого интерфейсного типа `error`, значит, можно использовать операцию **утверждения типа** (**type assertion**) для конвертации ошибки в конкретный базовый тип

```go
if err := testFunc(0); err != nil {
    if v, ok := err.(TimeError); ok {
        fmt.Println(v.Time, v.Text)
    } else {
        fmt.Println(err)
    }
}
```

Если ошибки могут быть разных типов, логично использовать конструкцию выбора типа:
```go
if err := testFunc(0); err != nil {
    switch v := err.(type) {
    case TimeError:
        fmt.Println(v.Time, v.Text)
    case *os.PathError:
        fmt.Println(v.Err)
    default:
        fmt.Println(err)
    }
}
```

Но лучше применить функцию `As` пакета `errors`, так как она, в отличие от `type assertion`, работает с «обёрнутыми» ошибками, которые разберём ниже. `As` находит первую в цепочке ошибку `err`, устанавливает тип, равным этому значению ошибки, и возвращает `true`.

```go
if err := testFunc(0); err != nil {
    var te TimeError
    if errors.As(err, &te) { //  Сравниваем полученную и контрольную ошибки. Сравнение идёт по типу ошибки.
        fmt.Println(te.Time, te.Text)
    } else {
        fmt.Println(err)
    }
}
```

---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links:
[[Ошибки]][[Сравнение и обертывание ошибок (wrapping error)]]