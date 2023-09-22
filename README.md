[![Tests](https://github.com/netascode/go-ise/actions/workflows/test.yml/badge.svg)](https://github.com/netascode/go-ise/actions/workflows/test.yml)

# go-ise

`go-ise` is a Go client library for Cisco ISE. It is based on Nathan's excellent [goaci](https://github.com/brightpuddle/goaci) module and features a simple, extensible API and [advanced JSON manipulation](#result-manipulation).

## Getting Started

### Installing

To start using `go-ise`, install Go and `go get`:

`$ go get -u github.com/netascode/go-ise`

### Basic Usage

```go
package main

import "github.com/netascode/go-ise"

func main() {
    client, _ := ise.NewClient("https://1.1.1.1", "user", "pwd")

    res, _ := client.Get("/ers/config/internaluser")
    println(res.Get("SearchResult.resources.#.name").String())
}
```

This will print something like:

```
["user1","user2","user3"]
```

#### Result manipulation

`ise.Result` uses GJSON to simplify handling JSON results. See the [GJSON](https://github.com/tidwall/gjson) documentation for more detail.

```go
res, _ := client.Get("/ers/config/internaluser")

for _, user := range res.Get("SearchResult.resources").Array() {
    println(user.Get("@pretty").String()) // pretty print internal users
}
```

#### POST data creation

`ise.Body` is a wrapper for [SJSON](https://github.com/tidwall/sjson). SJSON supports a path syntax simplifying JSON creation.

```go
body := ise.Body{}.
    Set("InternalUser.name", "User1").
    Set("InternalUser.password", "Cisco123")
client.Post("/ers/config/internaluser", body.Str)
```

## Documentation

See the [documentation](https://godoc.org/github.com/netascode/go-ise) for more details.
