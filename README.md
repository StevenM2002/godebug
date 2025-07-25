# godebug

A Go package providing structured error handling and debugging utilities with enhanced error formatting, function call context, and argument inspection.

## Features

- **Structured Error Formatting**: Wrap errors with function context and arguments
- **Call Stack Information**: Automatically capture function names using runtime reflection
- **Argument Inspection**: Include function arguments in error output for better debugging
- **JSON Serialization**: Convert complex data structures to readable JSON format
- **Error Chaining**: Support for nested error structures

## Installation

```bash
go get github.com/StevenM2002/godebug
```

## Usage

### Basic Error Wrapping

```go
import "github.com/StevenM2002/godebug/godebug"

func processUser(userID int, name string) error {
    debug := godebug.Debug{A: []any{userID, name}}
    
    // Some operation that might fail
    if userID < 0 {
        return debug.E(fmt.Errorf("invalid user ID"), "failed to process user")
    }
    
    return nil
}
```

### With Context

```go
func handleRequest(ctx context.Context, req *Request) error {
    debug := godebug.Debug{A: []any{ctx, req}}
    
    err := processRequest(req)
    if err != nil {
        return debug.E(err, "request processing failed")
    }
    
    return nil
}
```

### Struct Serialization

```go
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}

user := User{ID: 123, Name: "John"}
jsonStr := godebug.StructString(user)
// Output: {"id":123,"name":"John"}
```

## Error Output Format

The package produces structured JSON error output:

```json
{
  "fn_name": "main.processUser",
  "args": ["123", "{\"id\":123,\"name\":\"John\"}"],
  "msg": "failed to process user",
  "inner": "invalid user ID"
}
```

## API Reference

### Debug

```go
type Debug struct {
    A []any // Arguments to include in debug output
}
```

### Methods

#### E(err error, msg ...string) error

Formats an error with debugging context including function name, arguments, and message. Supports error chaining for nested debugging information.

### Functions

#### StructString(v any) string

Converts any value to a JSON string representation. Falls back to default string formatting if JSON marshaling fails.

## License

MIT License