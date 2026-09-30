# scout

The working tree of `examples/pipelines/default/edgar-scout.yaml`: an agent that reads
SEC EDGAR's latest 8-K filings, keeps a watchlist of investment ideas, and
never finishes. It is a demonstration of a long-running agent, not investment
advice.

Each cycle the scout fetches page one of EDGAR's current-filings feed, reads at
most three filings that look material, updates the two files below, commits,
and sleeps ten minutes. It reaches `www.sec.gov` and its model, and nothing
else.

| File | What it holds |
| --- | --- |
| `ideas.md` | The watchlist. Each idea names a company and ticker, gives a one-paragraph thesis, links the filings that support it, scores conviction from 1 to 5, and says when it last changed. Ideas are added, revised and retired as filings arrive. |
| `log.md` | One dated line per cycle, including cycles where nothing material happened, so the cadence is visible even when the watchlist does not move. |

The scout commits after every cycle, so `git log` is its history and each
checkpoint the engine takes is a tree between cycles. The files are its
memory: a fork, a retry or a compacted context starts by re-reading them, and
loses nothing the next cycle needs.

`metaharness seed` copies this directory to `~/code/scout` and commits
it.
