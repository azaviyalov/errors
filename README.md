# errors

Package errors provides simple error handling primitives.

See [Fork compatibility](#fork-compatibility)

`go get github.com/azaviyalov/errors`

The traditional error handling idiom in Go is roughly akin to
```go
if err != nil {
        return err
}
```
which applied recursively up the call stack results in error reports without context or debugging information. The errors package allows programmers to add context to the failure path in their code in a way that does not destroy the original value of the error.

## Adding context to an error

The errors.Wrap function returns a new error that adds context to the original error. For example
```go
_, err := ioutil.ReadAll(r)
if err != nil {
        return errors.Wrap(err, "read failed")
}
```
## Retrieving the cause of an error

Using `errors.Wrap` constructs a stack of errors, adding context to the preceding error. Depending on the nature of the error it may be necessary to reverse the operation of errors.Wrap to retrieve the original error for inspection. Any error value which implements this interface can be inspected by `errors.Cause`.
```go
type causer interface {
        Cause() error
}
```
`errors.Cause` will recursively retrieve the topmost error which does not implement `causer`, which is assumed to be the original cause. For example:
```go
switch err := errors.Cause(err).(type) {
case *MyError:
        // handle specifically
default:
        // unknown error
}
```

[Read the package documentation for more information](https://godoc.org/github.com/azaviyalov/errors).

## Supported Go versions

- 1.21
- 1.22
- 1.23

## Fork compatibility

This is a fork of https://github.com/pkg/errors. The original library has been archived and is not supported.

This library has the same features as the original one, but includes updated tests for

- `go.mod` support instead of `$GOPATH`
- Go 1.21 changes in function closure names

To use this library instead of the original one, you should use the `replace` directive:

```
go mod edit -replace github.com/pkg/errors=github.com/azaviyalov/errors@v1.0.0

```


## License

BSD-2-Clause
