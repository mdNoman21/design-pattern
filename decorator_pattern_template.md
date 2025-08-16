# 🎨 Decorator Pattern – Universal Template (Go)

The **Decorator Pattern** is a structural design pattern that lets you **dynamically add new behavior** to objects by "wrapping" them inside other objects.  

Think of it like wrapping a gift box (base object) with different layers of wrapping paper (decorators) — each adds something new.  

---

## 📄 Go Template

```
package main

import "fmt"

//
// 1. Component Interface
//    (base contract for objects and decorators)
//
type Component interface {
    Operation() string
}

//
// 2. Concrete Component
//    (the real object we want to decorate)
//
type ConcreteComponent struct{}

func (c ConcreteComponent) Operation() string {
    return "Base Operation"
}

//
// 3. Base Decorator
//    (wraps a Component, delegates calls, can add behavior)
//
type Decorator struct {
    component Component
}

func (d Decorator) Operation() string {
    if d.component != nil {
        return d.component.Operation()
    }
    return ""
}

//
// 4. Concrete Decorators
//    (add different behaviors)
//
type DecoratorA struct {
    Decorator
}

func (d DecoratorA) Operation() string {
    return d.Decorator.Operation() + " + Extra Feature A"
}

type DecoratorB struct {
    Decorator
}

func (d DecoratorB) Operation() string {
    return d.Decorator.Operation() + " + Extra Feature B"
}

//
// 5. Example Usage
//
func main() {
    // Step 1: Create base component
    var component Component = ConcreteComponent{}
    fmt.Println(component.Operation()) // Base Operation

    // Step 2: Wrap with DecoratorA
    component = DecoratorA{Decorator{component}}
    fmt.Println(component.Operation()) // Base Operation + Extra Feature A

    // Step 3: Wrap with DecoratorB on top of DecoratorA
    component = DecoratorB{Decorator{component}}
    fmt.Println(component.Operation()) // Base Operation + Extra Feature A + Extra Feature B
}
```

---

## 📌 Steps to Adapt

1. **Define a Component interface** → the core contract for your object (e.g., `Operation`, `Render`, `Cost`).  
2. **Concrete Component** → a simple object implementing the interface.  
3. **Base Decorator** → stores a reference to a `Component`, delegates calls.  
4. **Concrete Decorators** → extend the base decorator and override `Operation()` (or relevant method) to add new behavior before/after delegating.  
5. **Usage** → wrap your components dynamically at runtime.  

---

## ✅ Benefits of the Decorator Pattern

- Add responsibilities **dynamically** without subclassing.  
- Combine behaviors flexibly by stacking multiple decorators.  
- Follows the **Open/Closed Principle** → new behavior via new decorators, not modifying existing code.  

---

## 🌍 Real-World Examples

- **I/O Streams in Go/Java** → Wrap a `Reader` with a `BufferedReader`, then with a `GzipReader`.  
- **UI Components** → A text box decorated with scrollbars, then with borders.  
- **Web Middleware** → Logging, authentication, compression layers in HTTP servers.  

---

## ⚡ Summary

The **Decorator Pattern** allows **flexible, runtime-composable object behavior**, making it powerful for building extensible and maintainable systems.  
