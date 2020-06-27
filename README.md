# parallel [![Go Report Card](https://goreportcard.com/badge/github.com/JigneshSatam/parallel)](https://goreportcard.com/report/github.com/JigneshSatam/parallel) [![Build Status](https://travis-ci.org/JigneshSatam/parallel.svg?branch=master)](https://travis-ci.org/JigneshSatam/parallel) [![Test Coverage](https://api.codeclimate.com/v1/badges/20812caef2634511a1e6/test_coverage)](https://codeclimate.com/github/JigneshSatam/parallel/test_coverage) [![Issue Count](https://codeclimate.com/github/JigneshSatam/parallel/badges/issue_count.svg)](https://codeclimate.com/github/JigneshSatam/parallel) [![Code Climate](https://codeclimate.com/github/JigneshSatam/parallel/badges/gpa.svg)](https://codeclimate.com/github/JigneshSatam/parallel) [![GoDoc](https://godoc.org/github.com/JigneshSatam/parallel?status.svg)](https://godoc.org/github.com/JigneshSatam/parallel) [![Sourcegraph](https://sourcegraph.com/github.com/JigneshSatam/parallel/-/badge.svg)](https://sourcegraph.com/github.com/JigneshSatam/parallel?badge)

parallel executes any tasks in parallel by using Golang fanout fanin concurrency pattern

## Installation

```go
go get -u github.com/JigneshSatam/parallel
```

## Usage

### Step 1: Import the package
```go

import (
  "github.com/JigneshSatam/parallel"
)
```

### Step 2: Override  `Execute() interface{}` method for `user defined type`
```go
// Let `user` be a user defined type
// Suppose fetching user's `birth date` needs to be in parallel then
// Override `Execute() interface{}` method to make `user` as an `Executor`
// and write the code to fetch the birth date in this Execute() method.

// user -> Let `user` be a user defined type
type user struct {
  name string
}

// Execute() -> Overriden `Execute() interface{}` method
func (u user) Execute() interface{} {
  return getBirthDate(u)
}

func getBirthDate(u user) string {
  return u.name + " 1991-03-13"
}

func Example() {
  users := []user{user{"foo"}, user{"bar"}, user{"baz"}}

  for op := range parallel.Run(users) {
    // Cast `interface{}` to desired output type
    output := op.(string)
    fmt.Println(output)
  }

  // Unordered output:
  // "bar 1991-03-13"
  // "foo 1991-03-13"
  // "baz 1991-03-13"
}
```

## License

MIT.
