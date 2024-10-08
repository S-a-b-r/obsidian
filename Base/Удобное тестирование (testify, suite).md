07.10.2024 00:45
Tags: #info

---
# Удобное тестирование (testify, suite)

Можно тестировать код, используя только `*testing.T`, но он не предоставляет доступ к функциям вроде проверки на равенство, проверки на возврат ошибки или проверки на наличие паники при вызове переданного колбэка.

Этот пробел заполняют сторонние библиотеки, среди которых самая популярная — [testify](https://github.com/stretchr/testify). Следует отметить, что они не являются заменой стандартного `testing`, а расширяют и дополняют его.

- [testify](https://github.com/stretchr/testify) — это швейцарский нож в мире тестирования Go-кода. Поэтому поговорим про часто используемые пакеты из этого репозитория отдельно.
- [assert](https://pkg.go.dev/github.com/stretchr/testify/assert){target="_blank"} — пакет, в котором собрано множество удобных функций с говорящими названиями: вроде `assert.Equal` или `assert.Nil`. Предназначен для использования внутри обычных Go-тестов вида `func TestXxx(t *testing.T)` (про необычные поговорим, когда доберёмся до [suite](https://pkg.go.dev/github.com/stretchr/testify/suite)).
- [require](https://pkg.go.dev/github.com/stretchr/testify/require) — то же самое, что [assert](https://pkg.go.dev/github.com/stretchr/testify/assert), но в случае, если проверки из этого пакета падают, выполнение теста останавливается. То есть внутри используется `Fatal` вместо `Error`.
- [suite](https://pkg.go.dev/github.com/stretchr/testify/suite) — пакет, который вводит концепцию тест-сьюта (**test suite**). Если вы работали с тестами на Java или Python, то, скорее всего, она вам знакома.

**Сьют** — это объект, содержащий сами тесты в виде методов, а также некоторый набор переменных, доступный всем тестам. Кроме того, сьют даёт возможность определить код, который будет вызван перед и/или после исполнения теста. Эта функция полезна, например, для очистки базы данных между тестами (в случае интеграционных тестов).

Типичный пример работы со [suite](https://pkg.go.dev/github.com/stretchr/testify/suite) из официальной документации выглядит так:
```go
import (
    "testing"
    "github.com/stretchr/testify/suite"
)

// ExampleTestSuite — это тестовый сьют, который создан путём эмбеддинга suite.Suite.
type ExampleTestSuite struct {
    suite.Suite
    VariableThatShouldStartAtFive int
}

// SetupTest заполняет переменную VariableThatShouldStartAtFive перед началом теста.
func (suite *ExampleTestSuite) SetupTest() {
    suite.VariableThatShouldStartAtFive = 5
}

func (suite *ExampleTestSuite) TestExample() { // все тесты должны начинаться со слова Test
    suite.Equal(5, suite.VariableThatShouldStartAtFive)
}

func TestExampleTestSuite(t *testing.T) {
    // чтобы go test смог запустить сьют, нужно создать обычную тестовую функцию
    // и вызвать в ней suite.Run
    suite.Run(t, new(ExampleTestSuite))
}
```

## Паттерны тестирования

Есть несколько рекомендаций, как стоит и не стоит писать тесты. Они не специфичны для Go, поэтому их можно применить для тестирования на любом языке.

### Каждый тест должен тестировать что-то одно

Например, если вы тестируете функцию `func Divide(a, b int) (int, error)`, не стоит писать такой код:

```go
func TestDivision(t *testing.T) {
    result, err := Divide(0, 1)
    require.NoError(t, err)
    assert.Equal(t, 0, result)

    result, err = Divide(4, 2)
    require.NoError(t, err)
    assert.Equal(t, 2, result)

    _, err = Divide(1, 0)
    require.Error(err)
} 
```

Он будет падать при любой проблеме в тестируемой функции. Лучше разбить его на три теста (или три подтеста внутри этого теста), каждый из которых тестирует свой специфический сценарий:

```go
func TestDivision(t *testing.T) {
    t.Run("ZeroNumerator", func(t *testing.T) {
        result, err := Divide(0, 1)
        require.NoError(t, err)
        assert.Equal(t, 0, result)
    })

    t.Run("BothNonZero", func(t *testing.T) {
        result, err = Divide(4, 2)
        require.NoError(t, err)
        assert.Equal(t, 2, result)
    })

    t.Run("ZeroDenominator", func(t *testing.T) {
        _, err = Divide(1, 0)
        require.Error(err)
    })
} 
```

### Тесты не должны зависеть друг от друга

Если один тест опирается на глобальное состояние, устанавливаемое другими тестами, — это проблема. Тестовая архитектура такого типа приводит к неприятным ошибкам, когда локально тесты проходят, а на CI/CD иногда падают, потому что время от времени порядок выполнения тестов меняется.

Эта же рекомендация относится к тестам внутри одного сьюта. При написании каждого теста нужно исходить из того, что ему на вход передаётся состояние после вызова подготовительного метода `SetupTest`.

### Результат работы теста — это не лог

При написании теста стоит считать, что при успешном прохождении теста никто на его логи смотреть не будет. Все контракты, которые фиксирует тест, должны быть прописаны в виде проверок. В этом случае можно безопасно гонять тесты на CI/CD без участия человека.

## Table-driven-тесты

В примерах выше тесты содержат много повторяющегося кода, и это неспроста. Существует паттерн тестирования, который называется table-driven, или, говоря по-русски, «табличное тестирование». Он реализуется не только в Go, но и в других языках программирования.

Суть паттерна в отделении тестовых данных от выполнения самих тестов. Для коротких тестов паттерн может показаться избыточным, однако он позволяет эффективно организовать и расширять тесты, если тестовых наборов требуется несколько.

---
### Zero-links:
[[00 Backend]] [[00 Golang]] [[00 Tests]]

---
### Links: