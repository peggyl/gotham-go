# Fuzzing Beyond Security: Automated Testing with go-fuzz

### Presenter: Filippo Valsorda, Cloudflare (@FiloSottile](https://twitter.com/@FiloSottile))
----

- Function should parse binary input from network (e.g. DNS packet).
- Write unit tests for one string, two strings, empty string... `go test` passes, put in production! DNS server crashes and burns. :(
- Fuzzing: feeding programs with automatically generated inputs, to trigger unexpected behavior.
- Take starting input and mutating. e.g. `\x0cHello World!` (mutates into) `x0c Hello World.`
- Coverage-guided fuzzing: use coverage information to decide if a mutation was useful or not. If coverage increases, keep the new input and iterate, else discard the mutation.
- Coverage doesn't actually do static analysis, just keeps track of which lines are reached.
- Fuzzer mutates input until it finds a mutation that changes the coverage.
- American Fuzzy Lop: afl-fuzz - popular C/C++ fuzzer by Michal Zalweski (lcamtuf). Uses compile-time instrumentation to keep track of code coverage, looking for corner cases triggering memory errors and other bugs. Found security vulnerabilities in dozens of programs (also repro'ed the Heartbleed vulnerability). http://lcamtuf.coredump.cox/afl/technical_details.txt
- Coverage-guided fuzzing is great at automatically finding corner cases. The ones the developer didn't write tests for.
- In Go, more likely to find bugs than security vulnerabilities, but we still love finding them!
- Enter [go-fuzz](https://github.com/dvyukov/go-fuzz) by Dmitry Vyukov. Uses AST rewriting to get coverage info and fuzz Go functions. Found 100s of bugs in Go stdlib.
- Explains how to use go-fuzz. ParseBinary failed when input was [0x30]
- github.com/miekg/dns used in Cloudflare's DNS server, parses DNS packets coming from network. Found deeply nested bug that required lots of offsets to be in certain states.
- The best people to write unit tests are the developers. The best people to write fuzz tests are the developers.
- Commit fuzz functions like you commit unit tests. Benefits: others can improve/refuse, run in CI, keep corpus and use to test big refactors, compare different implementations.
- Since **you** write the Fuzz function, you can check any condition, not just crashes. e.g. dns.Msg.PackBuffer misbehaves if passed buffer is not zeroed, compare outputs of `0x00` and `0xff` buffers and fail if they differ.
