# Design-pattern
Go templates for design patterns

## 1. Strategy Pattern Template in Go

```package main

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
### How to Use This Template
When adapting it:

1. Rename Strategy to your domain concept (e.g., PaymentMethod, SortAlgorithm, SpeakBehavior).

2. Rename Execute() to match the behavior (e.g., Pay(), Sort(), Speak()).

3. Implement as many concrete strategies as needed.

4. The Context holds the strategy and delegates to it.

5. Use SetStrategy() to change runtime behavior.

6. Call your public method (DoWork() here) to run the strategy.

### ✅ Key Benefit
This setup lets you add new behaviors just by adding a new struct that implements the interface — without touching the Context code.
And you can change that behavior for any given object at runtime without creating new types.
