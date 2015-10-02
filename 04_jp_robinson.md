# go get -news (Using Go and Python NLTK for news analysis)

### Presenter: JP Robinson ([@NYTDevs](https://twitter.com/@NYTDevs)), The New York Times
----

- Newshound: breaking news aggregator, email alerts as data source, subscribed to 18 major news organizations
- Calendar view: plot news alerts, look at clusters and determine if news events have occurred (e.g. Supreme Court hearings)
- CNN: "The U.S. Supreme Court has reached an important ruling" - first but not best alert?
- Event attendance for news alerts: how many alerts has an organization participated in?
- Occurrences of key phrases used by an organization over some time, e.g. "one" and "obama"
- Slack channel with Newshound Alerts bot with real-time updates
- Twitter account [@newshound_bot](https://twitter.com/newshound_bot)
- Why emails? New orgs tend to be more selective, more text than push notification, contain links to articles and related items, easily gathered, JP worked on email platform @nytimes.com and could see publish->inbox time.
- Why go? Initially Python (imaplib, BeautifulSoup, NLTK), UI was jQuery -> Backbone -> AngularJS, API was JS -> PHP -> Go.
- JP already knew Go and had created structs. Wanted Go for email/text processing because simpler deployment, code reuse and sane testing.
- Hurdles: connect to/read email from an inbox, extract text from the body, extract noun phrases, compare results
- [github.com/mxk/go-imap](https://github.com/mxk/go-imap) - great low-level client, implements RFC 3501, still need to deal with context-types, charsets, and character-transfer-encoding, different open-source pkgs for each of these
- Extract text from HTML: golang.org/x/net/html is more low-level and less magic than BeautifulSoup, but don't need that high-level magic.
- Compare the strings and bytes packages in stdlib: lots of helpful utilities needed already in bytes pkgs
- Go + Python NLTK: don't worry about this for now, fetchd (Go) <- HTTP -> np_extractor (Python)
- Text Normalization: how to compare phrases? Normalize with `x/text/unicode/norm` (e.g. café -> cafe)
- Text Comparison: `strings.Fields(string) []string` to split phrase into words, `strings.EqualFold(string, string) bool` for case-insensitive compare
- Email -> text -> noun phrases -> news
- NSQ/barkd and MongoDB MapReduce

Summary
- Pick quality, idiomatic 3rd party packages
- Use "bytes" over "strings" when possible
- If you can't rewrite, abstract to a service
- Don't fear low-level packages
