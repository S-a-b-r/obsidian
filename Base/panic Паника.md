11.07.2024 11:25
Tags: #info

---
# panic Паника
Во время выполнения программы могут возникнуть различные непредвиденные обстоятельства, из-за которых дальнейшая работа функции будет невозможна. В таком случае выполнение функции немедленно прекращается, паника передаётся вызывающей функции и затем вверх по стеку, пока выполнение программы не завершится.

Однако этот процесс меняется, если паникующая функция имеет отложенные вызовы. Они будут исполнены после выхода из функции и могут понять, что произошла паника.

Это похоже на механизм исключений в PHP, однако крайне не рекомендуется использовать `panic` для обычной работы и отлавливать ее. Вызывать панику следует лишь тогда, когда выполнение программы действительно не может продолжаться и она должна быть завершена. (Принцип don't panic!)
```go
func PanicingFunc () {
    defer func(){
        if r := recover(); r != nil {  // встроенная функция recover останавливает панику и возвращает описание произошедшего
            fmt.Println("Panic is caught", r)    
        } 
    }()
    
    panic("Мне здесь совсем ничего не нравится!") 
    // встроенная функция panic () вызывает панику у функции. 
    // в качестве аргумента ей принято передавать причину паники. Именно она будет возвращена функцией recover
}
```

Без применения оператора `defer` остановить панику было бы невозможно. Он позволяет вклиниться в стек вызовов функций и остановить её. Заметьте, что не всякую панику можно восстановить. Иногда возникают особые ситуации, когда `recover` не срабатывает.

---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links:
[[defer Отложенная функция]]