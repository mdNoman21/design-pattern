# 🏭 Abstract Factory Pattern – Universal Template (Go)

The **Abstract Factory Pattern** is a **creational design pattern** that provides an interface for creating **families of related objects** (products) **without specifying their concrete classes**.  

It’s like a **“factory of factories”**, used when your system needs to work with multiple groups of products interchangeably.

---

## 📄 Go Template

```
package main

import "fmt"

//
// 1. Abstract Product Interfaces
//
type Button interface {
    Render() string
}

type Checkbox interface {
    Check() string
}

//
// 2. Concrete Products – Variant A (e.g., Windows)
//
type WindowsButton struct{}
func (WindowsButton) Render() string { return "Render Windows Button" }

type WindowsCheckbox struct{}
func (WindowsCheckbox) Check() string { return "Check Windows Checkbox" }

//
// 3. Concrete Products – Variant B (e.g., Mac)
//
type MacButton struct{}
func (MacButton) Render() string { return "Render Mac Button" }

type MacCheckbox struct{}
func (MacCheckbox) Check() string { return "Check Mac Checkbox" }

//
// 4. Abstract Factory Interface
//
type GUIFactory interface {
    CreateButton() Button
    CreateCheckbox() Checkbox
}

//
// 5. Concrete Factories
//
type WindowsFactory struct{}
func (WindowsFactory) CreateButton() Button     { return WindowsButton{} }
func (WindowsFactory) CreateCheckbox() Checkbox { return WindowsCheckbox{} }

type MacFactory struct{}
func (MacFactory) CreateButton() Button     { return MacButton{} }
func (MacFactory) CreateCheckbox() Checkbox { return MacCheckbox{} }

//
// 6. Client Code
//
func clientCode(factory GUIFactory) {
    button := factory.CreateButton()
    checkbox := factory.CreateCheckbox()

    fmt.Println(button.Render())
    fmt.Println(checkbox.Check())
}

func main() {
    // Windows Family
    var factory GUIFactory = WindowsFactory{}
    clientCode(factory)

    // Mac Family
    factory = MacFactory{}
    clientCode(factory)
}
```

---

## 📌 Steps to Adapt

1. **Define Abstract Product Interfaces** (e.g., `Button`, `Checkbox`).  
2. **Create Concrete Products** for each variant (e.g., `WindowsButton`, `MacButton`).  
3. **Define Abstract Factory** (e.g., `GUIFactory`) with creation methods for each product.  
4. **Implement Concrete Factories** (e.g., `WindowsFactory`, `MacFactory`) that return products of the same family.  
5. **Client Code** → works only with the abstract factory & product interfaces, without knowing concrete classes.  

---

## ✅ Benefits of Abstract Factory

- Ensures that **related products** (Button + Checkbox) are always used together.  
- Client is **decoupled** from concrete product classes.  
- Makes product families **interchangeable at runtime**.  

---

## 🌍 Real-World Examples

- **Cross-platform UI libraries** → `WindowsFactory`, `MacFactory`, `LinuxFactory`  
- **Database drivers** → `MySQLFactory`, `PostgreSQLFactory`, `MongoDBFactory`  
- **Cloud providers SDK** → `AWSFactory`, `AzureFactory`, `GCPFactory`  
- **Game development** → `MedievalThemeFactory` vs `SciFiThemeFactory` (enemies, weapons, environments)  

---

## ⚡ Summary

The **Abstract Factory Pattern** is ideal when:  
- Your system should work with **families of related objects**.  
- You want to guarantee **consistency across products** in the same family.  
- You want to be able to switch product families **easily at runtime**.  
