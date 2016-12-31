---
date: "2016-12-29T11:47:37+01:00"
categories: ["Development", "Go", "Software Design"]
title: "Private structs, constructors and working with interfaces instead of implementations. – Decoupling code in Go"
author: "Sergio Moya"
images: ["img/twitter-card.jpg"]
aliases: ["/2016/decoupling-code-in-go/"]
---
I've been coding in Go for a while now and I would like to add that I'm very happy to have adopted it as one of my daily use programming languages.

I haven't seen much code from other projects except the standard library, but I noticed that most open source projects developed in go don't follow a common pattern when declaring scopes, specially in the structs.

The following code has been extracted from a well known project and it's been used around the world:

```go
type DiscoveryInterface interface {
  RESTClient() restclient.Interface
}

type DiscoveryClient struct {
  restClient restclient.Interface
  LegacyPrefix string
}

func (c *DiscoveryClient) RESTClient() restclient.Interface {
  if c == nil {
    return nil
  }
  return c.restClient
}

func NewDiscoveryClient(c restclient.Interface) *DiscoveryClient {
  return &DiscoveryClient{restClient: c, LegacyPrefix: "/api"}
}
```

If we analyse the previous code, we see:

* An interface called `DiscoveryInterface`. Despite the name of the interface is not right (See [Effective Go](https://golang.org/doc/effective_go.html#interface-names)).
* A public struct called `DiscoveryClient` that implements `DiscoveryInterface`.
* A method called `NewDiscoveryClient` which returns a pointer to a` DiscoveryClient` struct. This is the constructor.

The purpose of creating a constructor is to encapsulate the struct creation  logic in order to create structs with default values as well as enforce business logic.

```go
client1 := &DiscoveryClient{}
client2 := NewDiscoveryClient(c)

// client1 != client2
```

In the previous example we are getting two structs with different states. Most of the time this will not be an expected behaviour.
So what is the purpose of have this constructor if we are also allowing `DiscoveryClient` to be instantiated directly, getting a struct with an unexpected state?

Usually, because **no reason**. And here my explanation:

1. The constructor should return the `DiscoveryInterface` interface, since it is the only type that we should know from outside the package, avoiding any kind of unwanted coupling.
2. `DiscoveryClient`, since we will not deal with the implementation from outside the package anymore, should be **private**
3. Therefore, the `legacyPrefix` field should also be **private**.
In fact, from the right moment we work with the `DiscoveryInterface` interface, making use of from another package of a struct method/field that is not contemplated in the interface  would violate the [Liskov Substitution Principle] (https: // En.wikipedia.org/wiki/Liskov_substitution_principle).
In case we need read access to that field, we should consider declaring a getter in the interface (See [Effective Go] (https://golang.org/doc/effective_go.html#Getters)).

At a glance, the code would look like:

```go
type DiscoveryInterface interface {
  RESTClient() restclient.Interface
}

type discoveryClient struct {
  restClient restclient.Interface
  legacyPrefix string
}

func (c *discoveryClient) RESTClient() restclient.Interface {
  if c == nil {
    return nil
  }
  return c.restClient
}

func NewDiscoveryClient(c restclient.Interface) DiscoveryInterface {
  return &discoveryClient{restClient: c, legacyPrefix: "/api"}
}
```

As examples of this approach, we can find some packages in the standard library like [context](https://github.com/golang/go/blob/master/src/context/context.go#L229) and  [io](https://github.com/golang/go/blob/master/src/io/io.go#L441) as well.
