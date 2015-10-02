# Boring is Beautiful

### Presenter: Kyle Erf ([@](https://twitter.com/@Maria_Fibonacci)), MongoDB
----

- Warning from Kyle: this talk is going to be a philosophical rant / postmortem...
- Good code is not interesting or even succinct. Good code is easy to understand and easy to replace. This is the philosophy with which Go was designed, and deviating from it will only cost you.
- Citing "The Dexterous Butcher" by Chuang Tzu:

> What I care about is the Way, which goes beyond skill. When I first began cutting up oxen, all I could see was the ox itself. After three years I no longer saw the whole ox. And now — now I go at it by spirit and don’t look with my eyes. Perception and understanding have come to a stop and spirit moves where it wants. I go along with the natural makeup, strike in the big hollows, guide the knife through the big openings, and following things as they are. So I never touch the smallest ligament or tendon, much less a main join.
>
> A good cook changes his knife once a year — because he cuts. A mediocre cook changes his knife once a month — because he hacks. I’ve had this knife of mine for nineteen years and I’ve cut up thousands of oxen with it, and yet the blade is as good as though it had just come from the grindstone. There are spaces between the joints, and the blade of the knife has really no thickness. If you insert what has no thickness into such spaces, then there’s plenty of room — more than enough for the blade to play about it. That’s why after nineteen years the blade of my knife is still as good as when it first came from the grindstone.
>
> However, whenever I come to a complicated place, I size up the difficulties, tell myself to watch out and be careful, keep my eyes on what I’m doing, work very slowly, and move the knife with the greatest subtlety, until — flop! the whole thing comes apart like a clod of earth crumbling to the ground. I stand there holding the knife and look all around me, completely satisfied and reluctant to move on, and then I wipe off the knife and put it away.

- What this has to do with Go: most programmers just see the code for the task at hand. The best programmers see the implications and how things fit together.
- Strong C++ programmers try to replicate C++ patterns in Go, e.g. replicators.
- Example: `runtime.setFinalizer` - provide function to run when a type is GC'ed, e.g. close/flush buffer
- But... depends on Go's runtime GC implementation, no control of when it's called, impossible to reason about, not idiomatic
- In Go, just use `defer`!


Unnecessary Concurrency
- Go homepage examples feature concurrent algorithms. But you don't need to write Sieve of Erathosthenes in daily work!
- Start with a for loop. Source: https://medium.com/@IndianGuru/best-practices-for-a-new-go-developer-8660384302fc
- Avoid using too much concurrency when starting out.

- [Martini](https://github.com/go-martini/martini) - functions take in arbitrary parameters and figure out what to do. But this was challenging when onboarding people and figuring out where injected variables came from.
- [Angier](https://github.com/shelman/angier) - structs with many fields and want to transfer certain field names. Use reflection to inspect types and copy over matching fields. But then someone renames a field and things break...
  * Solution: be verbose, use embedding, or assignment

- Boring code is easier to understand, easier to change, and easier to forgot about, so we can focus on what matters.
