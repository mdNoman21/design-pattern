# Universal Observer Pattern Template (Go)
go
``` package main

import "fmt"

// 1. Observer interface (receives updates)
type Observer interface {
    Update(data string)
}

// 2. Subject interface (registers/removes notifies observers)
type Subject interface {
    Register(o Observer)
    Remove(o Observer)
    NotifyAll(data string)
}

// 3. Concrete Subject (keeps a list of observers)
type ConcreteSubject struct {
    observers []Observer
}

func (s *ConcreteSubject) Register(o Observer) {
    s.observers = append(s.observers, o)
}

func (s *ConcreteSubject) Remove(o Observer) {
    idx := -1
    for i, obs := range s.observers {
        if obs == o {
            idx = i
            break
        }
    }
    if idx != -1 {
        s.observers = append(s.observers[:idx], s.observers[idx+1:]...)
    }
}

func (s *ConcreteSubject) NotifyAll(data string) {
    for _, obs := range s.observers {
        obs.Update(data)
    }
}

// 4. Concrete Observers
type ObserverA struct{}
func (a ObserverA) Update(data string) {
    fmt.Println("ObserverA got update:", data)
}

type ObserverB struct{}
func (b ObserverB) Update(data string) {
    fmt.Println("ObserverB got update:", data)
}

// 5. Example usage
func main() {
    subject := &ConcreteSubject{}

    a := ObserverA{}
    b := ObserverB{}

    subject.Register(a)
    subject.Register(b)

    // Notify all observers
    subject.NotifyAll("Data changed!")

    // Remove one observer
    subject.Remove(a)
    subject.NotifyAll("Another update")
}
```
## How to Use This Template
1. Observer: Interface for objects that should react to changes/events.

2. Subject: Interface/type for things that send out notifications.

3. ConcreteSubject: Tracks observers, implements add/remove/notify.

4. ConcreteObserver: Implements the Observer interface, reacts to updates.

5. Register/Remove/Notify: Manage observers and send updates.

# Real-world usage:

1. User interfaces (UI components observing state changes).

2. Newsletters (subscribers observe new articles).

3. Data bindings/reactive UI (views observe data model).

4. Event Driven systems like in games, GUIs, server events, device notifications.
