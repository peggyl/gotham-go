# The Modern Garbage Collector

### Presenter: Verónica López ([@Maria_Fibonacci](https://twitter.com/@Maria_Fibonacci)), Ardan Labs
----

- Why?
  * Firm/soft real-time systems require fast and efficient GC.
  * Lower latency = soften haters
  * Soften haters = potential Go adopters _(it's all a ploy!)_
- Inspired by Rick Hudson's talk from GopherCon: short term goal to increase Go adoption, long-term to create virtuous cycle
- Stop the World approach: perfect if program runs once, writes some output, then quits. In long running apps, results in frozen UIs and slow response times.
- Social context: diverse communities expect results vs. coolness (Google makes it!!). Useful tools over easy tools (prefer intersection). Useful == fast/light/portable/etc == progress. Progress == money. Money == duh. Choice of programming language can have great social impact.
- "Modern" - not actually a "new" idea, concurrent garbage collector based on Djikstra paper in 1978, on-the-fly GC (https://bit.ly/1KQCfBu), tri-color collector (white/grey/black), LISP and BEAM already do
- Downsides: 2x compile times, pacing the new GC (heap growth vs CPU resources)
- Pacing GC not evident: too soon = too many GCs, too slow = heap grows out of control
- Veronica asked on Twitter what people thought of when they heard GC. Predominant response: Java. 1 person: Lisp.
- Java 8 better at GC with G1 GC. When contiguous blocks are swept away, compact into one block to avoid fragmentation.
