# /ruby/go

### Presenters: Brittany Wald [@joyousbanana](https://twitter.com/joyousbanana) & Jen Eisenberg [@jeisenberg0](https://twitter.com/jeisenberg0), Paperless Post
----

- Internal Tools team, fairly new team
- Create new service to run alongside Rails app
- Rollouts: assign features to users in modular way (10% users, 50% logged in), need more granular insights than currently have
- specs: high performance, microservice, data analysis
- Rails app limited in scalability
- First microservice built specifically for data need
- Why Go? Ruby great for concise business logic, Rails is powerful for web apps, but not for backend API. C not realistic given the resources they had.
- Brittany went to Ardan Labs workshop at GothamGo '14. Go provided low latency. Go runtime is fast and compiles to a binary. They both enjoyed learning Go.
- Felt like they were in a maze and could only see a couple of steps ahead.
- Data Typing: map JSON to SQL. Go can't scan null values out of Postgres. CreatedAt is not null, but RemovedAt can be - that's why it's a pointer.

```go
type Strategy struct {
  CreatedAt time.Time
  RemovedAt *time.Time
}
```
- StackOverflow for similar question: custom marshaler - 0 upvotes, sounded scary.
- How about using 2 structs? Asked Bill Kennedy which is more idomatic. Ended up using a pointer to time.Time.
- Intermediate step between JSON and SQL. Senior developer suggested using pointer.

- There isn't a Rails-like framework for DB, middleware, routing, etc. missing - started writing own ORM. Reading _Design Patterns_ at the time and started using Factory pattern.
- Stored Queries - minimize business logic in Go

- Go Tour, How to Write Go Code, Effective Go -> read about concurrency! It felt inescapable. "Escape is only an illusion, and gaps in your knowledge resurface as bugs in your code."

- Ruby vs Go: Ruby creates thread off main thread while Go starts coroutine.
- Awesome live demo of parallel vs concurrent execution! They've been speaking concurrently, and presenter notes act like a scheduler, but when they speak in parallel (at the same time), you can't really process both... because audience is like a single-core CPU.
- Parallelism in Ruby (MRI) is actually concurrency. Illusion that Ruby is thread-safe because only one thread can hold the GIL at a time, but thread can be passed during blocking I/O. Leads to race conditions.

- Start working towards v1. Use MC (MVC without the V) framework - JSON server and client.
- "Old habits don't die. You have to kill them."
- Initially tried to write Rails MVC in Go. But architectural patterns in Rail -> lots of coupling.
- Repository Pattern. Ultimately needed to test DB access and separate domain entities. Function-based abstractions. Collection of entities and implement in layers (pattern is implemented by the caller level).
- Could have used mixins/modules in Ruby, but they enjoyed writing this in Go.
- Great performance benchmarks even without explicitly using Go's perf-enhancing elements.

- Open-sourcing soon at [github.com/paperlesspost/rollouts](https://github.com/paperlesspost/rollouts)
