# Using `go/types` for Code Comprehension and Refactoring Tools

### Presenter: Alan Donovan, Google NYC Go Team (adonovan@google.com)
----

- go/types is new package in Go 1.5. Created in 2011 by Robert Griesemer, but just became part of stdlib.
- 2nd largest stdlib package by lines of code, 4th for pkg-level documentation.
- Type checker sits above parser and knows the type of each expression. Green tools use the type checker: gofmt, gorename, eg, vet, gomvpkg, golint, lggo, oracle, godoc -analysis
- Used by Grok / Kythe and Sourcegraph.
- net.http.File interface - can show full set of methods including those on embedded types, as well as what interfaces the File type satisfies.

Demos:
- godoc -analysis: hover over variable (blue link) to see type info!
- oracle: in editor, `go-oracle` tool shows type info, like methods for a type or what types satisfy an interface
- gorename: change all references to a type. Can intelligently preserve connections between struct and interface method names - safer than sed. Also detects name collision between two embedded types in a struct.

Type Checker
- Input: parsed files of package, dependency packages
- Output: compile errors, expression information, type-checked package
- Steps: name resolution, type deduction, constant evaluation
  * e.g. `T{K: 0}`: T could be struct or map, K could be field name or key.
