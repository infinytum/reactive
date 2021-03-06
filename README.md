# Reactive [![GoDoc](https://godoc.org/github.com/infinytum/reactive?status.svg)](https://godoc.org/github.com/infinytum/reactive) [![Go Report Card](https://goreportcard.com/badge/github.com/infinytum/reactive)](https://goreportcard.com/report/github.com/infinytum/reactive) [![Codacy Badge](https://api.codacy.com/project/badge/Grade/e7de2c3854bd42babfd89f75cb78ab00)](https://www.codacy.com/app/infinytum/reactive?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=infinytum/reactive&amp;utm_campaign=Badge_Grade)
My attempt on creating a simple RxJs clone

## Features
*  Observables
   *  Multi-Type support
*  Subjects
   *  Subject
   *  ReplaySubject
*  Pipes
   *  Take
   *  TakeEvery
   *  Skip
   *  SkipEvery

## Examples

### Simple Subject

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
	subject := reactive.NewSubject()
	subject.Subscribe(subHandler)
	subject.Next(1)
	subject.Next(2)
	subject.Next(3)
	subject.Next(4)
}

func subHandler(a int) {
	fmt.Println(a)
}
```

Output
```
$ go run main.go
1
2
3
4
```

### Replay Subject

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
    subject := reactive.NewReplaySubject()
    subject.Next(1)
    subject.Next(2)
    subject.Next(3)
    subject.Subscribe(subHandler)
    subject.Next(4)
}

func subHandler(a int) {
	fmt.Println(a)
}
```

Output
```
$ go run main.go
3
4
```

### Multi-Type support

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
	subject := reactive.NewSubject()

	subject.Subscribe(intHandler)
	subject.Subscribe(stringHandler)

	subject.Next(2)
	subject.Next("Hello")
	subject.Next("World")
	subject.Next(4)
	subject.Next(nil)
}

func intHandler(a int) {
	fmt.Print("Int Handler: ")
	fmt.Println(a)
}

func stringHandler(a string) {
	fmt.Print("String Handler: ")
	fmt.Println(a)
}
```

Output
```
Int Handler: 2
String Handler: Hello
String Handler: World
Int Handler: 4
Int Handler: 0
String Handler:
```

### Take Pipe

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
    subject := reactive.NewReplaySubject()
    subject.Pipe(reactive.Take(2)).Subscribe(subHandler)
    subject.Next(1)
    subject.Next(2)
    subject.Next(3)
    subject.Next(4)
}

func subHandler(a int) {
	fmt.Println(a)
}
```

Output
```
$ go run main.go
1
2
```

### TakeEvery Pipe

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
    subject := reactive.NewReplaySubject()
    subject.Pipe(reactive.TakeEvery(2)).Subscribe(subHandler)
    subject.Next(1)
    subject.Next(2)
    subject.Next(3)
    subject.Next(4)
}

func subHandler(a int) {
	fmt.Println(a)
}
```

Output
```
$ go run main.go
2
4
```

### Skip Pipe

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
    subject := reactive.NewReplaySubject()
    subject.Pipe(reactive.Skip(2)).Subscribe(subHandler)
    subject.Next(1)
    subject.Next(2)
    subject.Next(3)
    subject.Next(4)
}

func subHandler(a int) {
	fmt.Println(a)
}
```

Output
```
$ go run main.go
3
4
```

### SkipEvery Pipe

```go
package main

import (
    "github.com/infinytum/reactive"
    "fmt"
)

func main() {
    subject := reactive.NewReplaySubject()
    subject.Pipe(reactive.SkipEvery(2)).Subscribe(subHandler)
    subject.Next(1)
    subject.Next(2)
    subject.Next(3)
    subject.Next(4)
}

func subHandler(a int) {
	fmt.Println(a)
}
```

Output
```
$ go run main.go
1
3
```
