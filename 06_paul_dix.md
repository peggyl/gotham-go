# Go snippets from the new InfluxDB storage engine

### Presenter: Paul Dix ([@pauldix](https://twitter.com/@pauldix)), InfluxDB
----

- InfluxDB data: measurement,tags fields timestamp
- Each series and field is converted to a unique ID. Stored in a columnar store (vs row-based in SQL).
- Arranging in key/value stores where key = ID,time. Key space is ordered. Many existing storage engines have this model.
- Why new engine?
  * First used LSM trees (log-structured merge tree), used by Cassandra, but deletes are expensive (in TS, common to delete a large range of data) and too many open file handles (hit OS limits).
  * mmap COW B+ Trees - very mature, but write throughput for their use case was poor (optimized for reads) and scaled badly, no compression.
  * Neither met requirements:
    - high write throughput (100,000 writes/second)
    - awesome read performance (queries hit a range of k/v pairs for time aggregated computations)
    - better compression (extra few bytes every second add up)
    - writes can't block reads
    - write multiple ranges simultaneously
    - hot backups
    - need to open many DBs in a single process
- Time Structured Merge Tree (TSM Tree) - similar to LSM trees
- Time series data -> WAL (append-only file - recommended by Kafka) -> in-memory index -> index (periodic flushes)
- Mutexes are your friend. Use sync.RWMutux for in-memory cache. Channels are abused in Go code; use mutexes for goroutine-safe code without dealing with channels.
- Sort methods: not having generics = more readable code, even if sometimes have to copy-paste a small chunk of code. You read more code than you write, so readability matters!
- `sort.search` for binary search through sorted array. Used in indexing?
- Data files are read-only. Periodically compacted by bg process - keep file sizes low, achieve better compression. Wanted to compact while data is streaming in - not just one write lock.
- Solution: write lock for range of time, e.g. `func (w *WriteLock)LockRange (min, max int64)`
- How to test? Use channel with 10ms timeout, lock from 5-15 in a goroutine, check that the lock is _not_ acquired
- Memory mapping lets the OS handle caching for you. Hide from the garbage collector. Use `syscall.mmap`.
- Compression depends on shape of your data. Random floats are the worst; reusing the same float is the best.
- Write throughput dpeneds on batching, CPU, and memory. Currently bound by CPU, not I/O.

- Detailed writeup next week on http://influxdb.com/blog.html
