# Chaos Monkey on Your Laptop

### Presenter: Matthew Campbell, DigitalOcean ([@kanwisher](https://twitter.com/@kanwisher))
----

- Chaos Monkey: Netflix tool that killed servers and made sure things still ran in production.
- Background: instant messaging tool at Thomas Reuters, scale to 300k financial traders and every major bank around the time
- Initially 11 QA to 6 developers, awesome because could do all kinds of testing before release
- Recently, banks have gotten smaller, QA team of 1, can't break things at TR like in some small startups
- Dev environment constantly broken
- Integration environment, download and install everything, but challenge with developers using OS X and then someone on FreeBSD...
- Use Docker for continuous integration tests (worked better than Vagrant)
- Start with performance tests because perf is critical in IM. Fire up cluster on Amazon and time new commits.
- Failure testing: database failovers, e.g MySQL master goes down, enter read-only mode, degraded experience but doesn't go down entirely.
- New dev code doesn't take into account what happens if MySQL master goes down. Add tests where master randomly goes down.
- Test suites in Go are currently pretty functional.
- Less mocking
- Microservice problems: 3 auth services in 3 languages with 3 databases with same data. If one falls over, just use the next one! What if you take down all 3?
- Performance, downtime, discovery
- Hard to handle when things go down, even harder when things are just really slow.
- Case: extreme slowness from 2-5 am, turned out two clients would log on at same time from buggy client that would send 10k logins. Messages got rejected by AOL due to slowness, and would get queued in RAM.
- Slowness repo: randomly stop sending packets, sleep for 1 minute, etc. (open-sourced soon)
- Problems: connection pooling, dead TCP connections, hung connections, network splits / congestion
- Integration tests run in production environment, run as certain users and check perf between nodes.
- Chaos Gopher - new Go tool
- Future: better test suites, allow for flappy tests, static analysis of flapping tests
  * CI is either red or green, but some tests should be allowed to be flappy because non-deterministic.
