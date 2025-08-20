# 🏗️ Builder Pattern – Universal Template & Real-World Examples (Go)

The **Builder Pattern** is a creational design pattern for step-by-step construction of complex objects.  
It provides a fluent, flexible API to incrementally build up an object—perfect when many fields are optional or configuration is detailed.

---

## 📄 Universal Builder Pattern Template (Go)

```
package main

import "fmt"

// 1. Product to build
type Product struct {
    FieldA string
    FieldB int
    FieldC bool
}

// 2. Builder type
type ProductBuilder struct {
    product Product
}

// 3. NewBuilder constructor
func NewProductBuilder() *ProductBuilder {
    return &ProductBuilder{}
}

// 4. "With" methods for each field (chainable)
func (b *ProductBuilder) WithFieldA(a string) *ProductBuilder {
    b.product.FieldA = a
    return b
}
func (b *ProductBuilder) WithFieldB(v int) *ProductBuilder {
    b.product.FieldB = v
    return b
}
func (b *ProductBuilder) WithFieldC(c bool) *ProductBuilder {
    b.product.FieldC = c
    return b
}

// 5. Build() method returns the final product
func (b *ProductBuilder) Build() Product {
    return b.product
}

// 6. Example usage
func main() {
    prod := NewProductBuilder().
        WithFieldA("hello").
        WithFieldB(42).
        WithFieldC(true).
        Build()
    fmt.Println(prod)
}
```

---

## 🌐 Practical Example 1: HTTP Request Builder

```
package main

import (
    "fmt"
    "strings"
)

type HttpRequest struct {
    Method  string
    URL     string
    Headers map[string]string
    Body    string
    Query   map[string]string
}

type HttpRequestBuilder struct {
    r HttpRequest
}

func NewHttpRequestBuilder() *HttpRequestBuilder {
    return &HttpRequestBuilder{
        r: HttpRequest{
            Method:  "GET",
            Headers: make(map[string]string),
            Query:   make(map[string]string),
        },
    }
}

func (b *HttpRequestBuilder) WithMethod(method string) *HttpRequestBuilder {
    b.r.Method = strings.ToUpper(method)
    return b
}
func (b *HttpRequestBuilder) WithURL(url string) *HttpRequestBuilder {
    b.r.URL = url
    return b
}
func (b *HttpRequestBuilder) AddHeader(key, value string) *HttpRequestBuilder {
    b.r.Headers[key] = value
    return b
}
func (b *HttpRequestBuilder) AddQueryParam(key, value string) *HttpRequestBuilder {
    b.r.Query[key] = value
    return b
}
func (b *HttpRequestBuilder) WithBody(body string) *HttpRequestBuilder {
    b.r.Body = body
    return b
}
func (b *HttpRequestBuilder) Build() HttpRequest {
    return b.r
}

func main() {
    request := NewHttpRequestBuilder().
        WithMethod("post").
        WithURL("https://api.example.com/resource").
        AddHeader("Authorization", "Bearer X").
        AddHeader("User-Agent", "MyApp/1.0").
        AddQueryParam("q", "builder pattern").
        WithBody("{\"data\":123}").
        Build()

    fmt.Printf("METHOD: %s\nURL: %s\nHEADERS: %v\nQUERY: %v\nBODY: %s\n",
        request.Method, request.URL, request.Headers, request.Query, request.Body)
}
```

**Output**
```
METHOD: POST
URL: https://api.example.com/resource
HEADERS: map[Authorization:Bearer X User-Agent:MyApp/1.0]
QUERY: map[q:builder pattern]
BODY: {"data":123}
```

---

## 🗄️ Practical Example 2: SQL Database Query Builder

```
package main

import (
    "fmt"
    "strings"
)

type Query struct {
    Fields    []string
    Table     string
    Condition string
    OrderBy   string
    Limit     int
}

type QueryBuilder struct {
    query Query
}

func NewQueryBuilder() *QueryBuilder {
    return &QueryBuilder{
        query: Query{Limit: -1},
    }
}
func (b *QueryBuilder) Select(fields ...string) *QueryBuilder {
    b.query.Fields = fields
    return b
}
func (b *QueryBuilder) From(table string) *QueryBuilder {
    b.query.Table = table
    return b
}
func (b *QueryBuilder) Where(cond string) *QueryBuilder {
    b.query.Condition = cond
    return b
}
func (b *QueryBuilder) OrderBy(order string) *QueryBuilder {
    b.query.OrderBy = order
    return b
}
func (b *QueryBuilder) LimitRows(limit int) *QueryBuilder {
    b.query.Limit = limit
    return b
}
func (b *QueryBuilder) Build() string {
    var sb strings.Builder
    sb.WriteString("SELECT ")
    if len(b.query.Fields) == 0 {
        sb.WriteString("*")
    } else {
        sb.WriteString(strings.Join(b.query.Fields, ", "))
    }
    sb.WriteString(" FROM " + b.query.Table)
    if b.query.Condition != "" {
        sb.WriteString(" WHERE " + b.query.Condition)
    }
    if b.query.OrderBy != "" {
        sb.WriteString(" ORDER BY " + b.query.OrderBy)
    }
    if b.query.Limit >= 0 {
        sb.WriteString(fmt.Sprintf(" LIMIT %d", b.query.Limit))
    }
    return sb.String()
}

func main() {
    sql := NewQueryBuilder().
        Select("name", "age").
        From("users").
        Where("age > 18").
        OrderBy("name ASC").
        LimitRows(10).
        Build()
    fmt.Println(sql)
}
```

**Output**
```
SELECT name, age FROM users WHERE age > 18 ORDER BY name ASC LIMIT 10
```

---

## ✅ Benefits of the Builder Pattern

- Avoids complex or telescoping constructors.
- Makes object creation clean, readable, and maintainable.
- Supports validation and stepwise assembly.
- Ideal for configurations, requests, queries, forms, GUI elements, etc.


