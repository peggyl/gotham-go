# gRPC Go

### Presenter: Sameer Ajmani ([@sajma](https://twitter.com/@sajma)), Google Go team
----

- Open-source version of Stubby (Google's internal RPC system)
- RPC isn't just Remote Procedure Call
- In Go, an RPC starts a goroutine running on the server and provides messaging passing between the client and server goroutines.
- Unary RPC: client sends a request to the server, then the server sends a response.
- Streaming RPC: client and server may each send one or more messages.
- An RPC ends when both sides are done sending messages, either side disconnects, the RPC cancels or times out
- Unary RPC: one request, one response
- Example: mobile Maps app requests route from point A to B
- Streaming RPC: bidrectional message passing
- Messages sent on a stream are delivered FIFO
- Many streams can run simultaneously between same client and server
- Transport provides buffering and flow control
- Example: chat service

[grpc.io](https://grpc.io)
- 10 languages: Java, C, Go, C++, Node.js, Python, Ruby, Objective-C, PHP, C#
- IDL: Proto3
- Transport: HTTP2
- golang.org/x/net/context
- golang.org/x/net/trace

Demo and Code: Google Search
- proto3 file for Google service, takes Request and returns Result
- CLI client, frontend, backend #0 and #1
- client sends query to frontend, which runs Search on both backends and returns first result
- same example as Rob Pike's Concurrency Patterns
- use x/net/trace for real-time request tracing
- when backend #1 returns Result, the context for the search is immediately cancelled (e.g. backend #0)
- RPCs block but can be canceled using a context
- gRPC propagates cancelation from client to server
- backend listens on a TCP port, registers implementation of the GoogleServer, listens
- Each call to Search or Watch runs in its own goroutine
- frontend: Search returns as soon as it gets the first result- gRPC cancels the remaining backend. Search RPCs by ctx.
- Streaming RPC: returns a stream of results instead of one result. Add Watch to the Google Service. Compare:

```go
// Search returns a Google search result for the query
Search(ctx context.Context, in *Request, opts ...grpc.CallOption) (*Result, error)

// Watch returns a stream of Google search results for the query
Watch(ctx context.Context, in *Request, opts ...grpc.CallOption) (Google_WatchClient, error)
```

- client listens on channel for new results until an EOF err is sent
- backend sleep for random interval or end context if done
- frontend has 1 goroutine per backend, and 1 goroutine for wait group and close channel
- send will block indefinitely, channel closes as soon as handler returns, use context.Done() to handle
