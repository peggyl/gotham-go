# The Legacy of Go

### Presenter: Dave Cheney ([@davecheney](https://twitter.com/@davecheney)), Google Go team
----

- How will Go be remembered? What will be the legacy of Go?
- Start by looking at legacies of other languages:
  * C: high-level assembly language, first to take portability seriously, every major OS written in C
  * C++: object-orientation, zero-cost abstraction (if compile time is not a cost)
  * Rails: standardized project layout, compared to previous web frameworks - they differed in "belligerent" ways, now all web frameworks look similar
- Go: a simple programming language (Dave's closing keynote at GopherConIndia 2015)
- Language authors believed complexity must be avoided. Some wanted to add abstractions to handle complexity, others believed abstractions == complexity.
- **Go is fitness for purpose**
- Tooling: deliberate symbiosis, Russ Cox talked about need for mechanical refactoring and need for generated code to be indistinguishable from human-written code, `gofmt` ensures its ubiquity, almost all code is gofmt'ed and the little that isn't is viewed with suspicion
- No language will be viewed as complete without a canonical style, and a tool to enforce it.
- Katherine Cox-Buday's talk at GopherCon - is our profession a pop culture? "Programming is a pop culture" - Alan Kay
- Look at role of the programmer in a wider social context
  - Tailor J.W. Davis in 1870s improved trousers for customer request, eventually became Levi's Jeans
  - Sunglasses designed to aid pilots to add eyestrain -> Ray-Ban aviators
  - 1979, Sony released Walkman, allowed everyone to be music enthusiasts
  - Languages are not immune to the whim of popular fashion.
  - A new language, to be successful, has to be sufficiently differ
  - Trends toward **static typing**: types go out of style but never for long, moving back to static typing, some languages are retrofitting not for perf (type inference) but for programmer productivity
  - **Interfaces**: refinement of C++ virtual base class, evolution of Java's interface type. **Pure behavior.** Like Smalltalk - objects defined not by membership, but by behavior. Refinement of many previous attempts.
  - Robert C. Martin quote: are languages successful because they offer programmers more choice, or are they successful for the opposite reason, because they offer less?
  - Inheritance** - took away something people didn't miss in the first place
  - **Semicolons;** - still there but hidden, (JavaScript made semicolons optional, and sometimes it works!)
  - **Threads** - programmers no longer have to be concerned with thread management
  - Removing theads and needing to care about the stack, and replacing with goroutines and channels, is Go's most powerful mark.
  - gofmt, interfaces, goroutines - how future programmers will look at Go's legacy
  - Fantastic language to teach the theory and practice of programming, and we've barely scratched the surface
  - More books, blog posts, user groups, conferences, diversity
  - Encourages everyone to lean in and shape how Go will be remembered
