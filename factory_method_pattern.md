# 🏭 Factory Method Pattern – Universal Template (Go)

The **Factory Method Pattern** is a **creational design pattern** that defines an interface for creating objects but lets concrete factories decide which class to instantiate.  
It helps to delegate the instantiation logic and keeps client code **decoupled** from the actual implementations.

---

## 📄 Go Template

```
package main

import "fmt"

//
// 1. Product interface
//    - The common interface for all products created by factories
//
type Product interface {
    Use() string
}

//
// 2. Concrete Products
//    - Different implementations of Product
//
type ProductA struct{}
func (p ProductA) Use() string { return "Using Product A" }

type ProductB struct{}
func (p ProductB) Use() string { return "Using Product B" }

//
// 3. Creator Interface (Factory)
//    - Declares the factory method
//
type Creator interface {
    CreateProduct() Product
}

//
// 4. Concrete Creators (Factories)
//    - Implement the factory method to return different product types
//
type CreatorA struct{}
func (c CreatorA) CreateProduct() Product { return ProductA{} }

type CreatorB struct{}
func (c CreatorB) CreateProduct() Product { return ProductB{} }

//
// 5. Client code
//    - Works with Creator interface and Product interface, no need to know the concrete types
//
func clientCode(c Creator) {
    product := c.CreateProduct()
    fmt.Println(product.Use())
}

func main() {
    var creator Creator

    creator = CreatorA{}
    clientCode(creator) // Output: Using Product A

    creator = CreatorB{}
    clientCode(creator) // Output: Using Product B
}
```

---

## 📌 Steps to Adapt

1. **Define a Product interface** → what all products must implement.  
2. **Create Concrete Products** → implement the Product interface differently.  
3. **Define the Creator interface** → declare the factory method (e.g., `CreateProduct`).  
4. **Implement Concrete Creators (factories)** → decide which product to create.  
5. **Client code** uses only the **Creator interface** and **Product interface**, never touching concrete types directly.

---

## ✅ Benefits of Factory Method

- **Encapsulation of object creation** → Clients don’t know concrete classes.  
- **Open/Closed Principle** → Add new product types easily by creating new factories.  
- **Decoupling** → Client only depends on the abstract product interface.  

---

## 🌍 Real-World Examples

- **Database connectors** → `MySQLFactory`, `PostgreSQLFactory`, `MongoDBFactory`.  
- **Payment processors** → `CreditCardFactory`, `PayPalFactory`, `UPIFactory`.  
- **GUI elements** → `WindowsButtonFactory`, `LinuxButtonFactory`, `MacButtonFactory`.  
- **Logger creation** → `FileLoggerFactory`, `ConsoleLoggerFactory`, `CloudLoggerFactory`.  

---

## ⚡ Summary

The **Factory Method Pattern** lets you delegate object creation to dedicated factory classes instead of calling `new` directly.  
This keeps your client code **clean, flexible, and decoupled** from specific implementations.
