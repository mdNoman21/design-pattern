# 🛠 Design Patterns – Go Templates  

## 1️⃣ Strategy Pattern Template in Go

The **Strategy Pattern** defines a family of interchangeable algorithms, encapsulates each one, and makes them interchangeable within a context at runtime, without altering the context’s code.

---

## 📄 Template Code (Go)

```
package main

import "fmt"

//
// 1. Strategy Interface
//
type Strategy interface {
    Execute(data string) string
}

//
// 2. Concrete Strategies
//
type StrategyA struct{}
func (s StrategyA) Execute(data string) string {
    return "[A processed] " + data
}

type StrategyB struct{}
func (s StrategyB) Execute(data string) string {
    return "[B processed] " + data
}

//
// 3. Context (Holds a reference to a Strategy)
//
type Context struct {
    strategy Strategy
}

// SetStrategy allows changing behavior at runtime
func (c *Context) SetStrategy(s Strategy) {
    c.strategy = s
}

// DoWork delegates execution to the current strategy
func (c *Context) DoWork(data string) {
    if c.strategy == nil {
        fmt.Println("No strategy set")
        return
    }
    result := c.strategy.Execute(data)
    fmt.Println("Result:", result)
}

//
// 4. Example usage
//
func main() {
    ctx := Context{}

    // Assign Strategy A
    ctx.SetStrategy(StrategyA{})
    ctx.DoWork("Hello") // -> [A processed] Hello

    // Swap to Strategy B at runtime
    ctx.SetStrategy(StrategyB{})
    ctx.DoWork("Hello") // -> [B processed] Hello

    // Remove strategy
    ctx.SetStrategy(nil)
    ctx.DoWork("Hello") // -> No strategy set
}
```

---

## 📌 How to Use This Template

When adapting it:
1. **Rename** `Strategy` to your domain concept (e.g., `PaymentMethod`, `SortAlgorithm`, `SpeakBehavior`).
2. **Rename** `Execute()` to match your behavior (e.g., `Pay()`, `Sort()`, `Speak()`).
3. Implement as many **concrete strategies** as you need.
4. The **context** holds a `Strategy` and delegates execution to it.
5. Use `SetStrategy()` to **change runtime behavior**.
6. The method like `DoWork()` will run the currently set strategy.

---

## ✅ Key Benefit

- Add new behaviors just by adding a new struct implementing the interface — **no change in context code**.  
- Change strategy for any given object **at runtime**, without creating new types.

---

## 🌍 Real-World Use Case: Cache Eviction Algorithms

**Scenario**  
An in-memory cache occasionally gets full and needs to evict items using different algorithms:
- **LRU** – Least Recently Used  
- **FIFO** – First In First Out  
- **LFU** – Least Frequently Used  

**Problem**  
You should be able to **swap eviction algorithms** without changing the cache’s internal structure.

**Solution – Strategy Pattern**  
- `EvictionAlgorithm` interface  
- Concrete strategies for `LRUEviction`, `FIFOEviction`, `LFUEviction`  
- `Cache` (context) stores an `EvictionAlgorithm` and delegates eviction to it  
- Can swap eviction strategies at runtime  

**Illustration (Go)**

```
type EvictionAlgorithm interface {
    Evict(c *Cache)
}

type LRU struct{}
func (l LRU) Evict(c *Cache) { /* LRU eviction code */ }

type FIFO struct{}
func (f FIFO) Evict(c *Cache) { /* FIFO eviction code */ }

type Cache struct {
    storage  map[string]interface{}
    eviction EvictionAlgorithm
}

func (c *Cache) SetEvictionAlgorithm(e EvictionAlgorithm) {
    c.eviction = e
}

func (c *Cache) Remove() {
    c.eviction.Evict(c)
}

func main() {
    cache := Cache{storage: make(map[string]interface{})}

    cache.SetEvictionAlgorithm(LRU{})
    cache.Remove() // uses LRU

    cache.SetEvictionAlgorithm(FIFO{})
    cache.Remove() // uses FIFO
}
```

---

## 🎯 Why Use the Strategy Pattern?

1. **Decouples** the algorithm from the object using it.  
2. Allows **runtime swapping** of algorithms.  
3. Makes adding new algorithms **easy** without changing the context.

---

## 📚 Additional Real-World Examples

- **Compression utilities** → Switch compression algorithms at runtime.  
- **Payment processing** → Select between payment gateways dynamically.  
- **Sorting** → Use different sorting depending on dataset characteristics.  
- **Authentication** → Swap between OAuth, LDAP, JWT at runtime.

---

> ⚡ **Summary:**  
> Use the **Strategy Pattern** when you have multiple ways to perform an operation, and you want to choose or change the method **at runtime** without modifying the object that uses it.
