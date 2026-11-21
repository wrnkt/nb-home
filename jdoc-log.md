# jdoc :: Log


## TODO
- [ ] fix indexing, show documentation loaded from class name


## Notes

Doesn't seem to be retrieving anything at all.
Tried `jdoc StopWatch`, which I know is available in my `~/.m2` with all my other Spring deps.

```bash
wrnk@fabric
[~/r/util/jdoc] $ ./target/release/jdoc --reindex && ./target/release/jdoc --stats
jdoc: scanning ~/.m2 and ~/.gradle caches...
indexed 0 artifact(s) into /home/wrnk/.local/share/jdoc/index.db
index:     /home/wrnk/.local/share/jdoc/index.db
artifacts: 0
classes:   0
members:   0
db size:   76.0 KB
```

