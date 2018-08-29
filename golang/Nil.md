# Nil meaning

## For Interface type

checking nil for an interface type equal to the question: Does the interface contains nil object?

> interface{nil}
> interface{} -> nil

```go
func main() {
	var v interface{} = nil
	v == nil // true
}
```

For example, if interface wraps a pointer that doesn't point to anything,
it still means that the interface wraps the pointer itself!
The "== nil" question ask the interface contains something or not, and not it's value contains something or not.

> interface{*ptr<nil>}
> interface{} -> *ptr -> nil

```go
type Obj struct {}

func main() {
	var ptr *Obj
	var v interface{} = ptr
	v == nil // false
}
```

## For Pointer Type

checking nil for a pointer means does it point to a value or currently not.
If it doesn't points to anything, than it's equal to nil.

> *string<nil>
> *string -> nil

When it's pointing to a value than it's not equal to nil.

> *string<"Hello, World!">
> *string -> "Hello, World!"
