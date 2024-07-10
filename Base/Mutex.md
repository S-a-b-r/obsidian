11.07.2024 00:19
Tags: #info

---
# Mutex

Антипример использования в структуре
```go
type BestCache struct {
	 sync.Mutex
	 m map[string]int
}

func New() *BestCache{
	return &BestCache{
		m : make(map[string]int),
	}
}

func (c *BestCache) Get(key string) (int, bool) {
	c.Lock()
	defer c.Unlock()

	val, ok := c.m[key]
	return val, ok
}

func main() {
	cache := New()
	cache.Lock()//метод мьютекса торчит наружу
	cache.Get("test")//Ловим панику
}

```

```go
type BestCache struct {
	mu sync.Mutex
	m map[string]int
}
//my - приватный и нельзя залочить кеш извне
```

---
### Zero-links:
[[00 Backend]] [[00 Golang]]

---
### Links: