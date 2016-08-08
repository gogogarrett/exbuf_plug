ExbufPlug
========

A small plug to handle decoding protocol buffers.

ExbufPlug is a wrapper around [exprotobuf](https://github.com/bitwalker/exprotobuf) to handle dealing with
protobufs over http.

The strategy here is to decode a protocol buffer into a base64 string and send it over http - which this plug will
decode into an elixir struct to deal with in your application.

## Useful articles

* [What are protocol buffers](https://developers.google.com/protocol-buffers/)
* [Why use protocol buffers over json](http://blog.codeclimate.com/blog/2014/06/05/choose-protocol-buffers/)

## Installation

Add ExbufPlug to your application

mix.deps

```elixir
defp deps do
  [
    # ...
    {:exbuf_plug, "~> 0.0.1"}
    # ...
  ]
end
```

config.exs

```elixir
config :exbuf_plug, ExbufPlug, %{
  list: [
    "TestEvent",
    "BiggerTestEvent"
  ],
  namespace: "ExbufPlug",
  module_name: "Protobufs",
  header_name: "x-protobuf"
}
```

The items in the configuration allow you to tailor how the decoding behaves.

* `list` - The list of protobufs currently supported
* `namespace` - The namespace around the protobuf module.
* `module_name` - The main module that implements `exprotobuf` to be used for encoding/decoding protobufs.
* `header_name` - The header name to look for to know which protobuf to use for decoding

### Phoenix Controllers

ExbufPlug hooks easily into Phoenix controllers.

It allows you to specify which key in the params should be used to decode the protobuf.

```elixir
defmodule MyApp.MyController do
  use MyApp.Web, :controller

  plug ExbufPlug, [param_key: "event"]

  def index(conn, params, user, claims) do
    # do stuff in here
  end
end
```