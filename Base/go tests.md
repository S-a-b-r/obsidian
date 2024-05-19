07.05.2024 15:11
Tags:

---
# go tests

Запуск тестов. Тесты работают с пакетом "testing"

Пример:
```
package main

import (
	"testing"
)

func TestFuncCheckWord(t *testing.T) {

	mockWord := "aabaa"
	
	expected := false
	
	if result := CheckWord(mockWord); result != expected {
		t.Errorf("Incorrect result. Expect %t, got %t, checked str: %s",
		expected, result, mockWord)
	}
}
```


#### Принимает флаг -v для более подробного отображения информации о тестах

#### Принимает флаг -cover: Показывает покрытие кода тестами

---
### Zero-links:
[[00 Golang]] [[00 Tests]]

---
### Links:

