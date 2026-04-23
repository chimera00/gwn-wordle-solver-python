# gwn-wordle-solver-python

> Documentation of the information-theoretic Wordle solving approach
> used as a benchmark for [Wordle Wonk](https://wordlewonk.com/) — the
> production GWN Wordle helper. Part of the
> [Grande Web Network](https://grandewebnetwork.com/) family.

> **Note:** This is a *docs-only* repo. No source is published here —
> the live solver runs at [wordlewonk.com](https://wordlewonk.com/).
> This README explains the algorithm for educational reference.

## The idea (credit: 3Blue1Brown, 2022)

Pick the guess whose expected distribution of color-patterns
(GREEN / YELLOW / GRAY per position) maximizes the **Shannon entropy**
of the bucket distribution over still-possible answers. In plain words:
choose the guess that, on average, eliminates the most possibilities no
matter what the colors come back as.

## Pseudocode

```
for each candidate guess G in allowed_guesses:
    buckets = {}
    for each remaining possible answer A:
        pattern = color_feedback(G, A)   # 5-tuple of GREEN|YELLOW|GRAY
        buckets[pattern] += 1
    total = len(possible_answers)
    entropy(G) = -sum( (n/total) * log2(n/total) for n in buckets.values() )

best_guess = argmax(entropy)
```

## Benchmarked behavior

Run against the original Wordle answer list (2,315 words) and a full
~12K-word guess dictionary, pure-entropy strategy yields:

| metric | value |
|---|---|
| average guesses | ~3.61 |
| worst case | 5 guesses |
| max-entropy first guess | `TARES` or `SOARE` (tie-break) |

[Wordle Wonk](https://wordlewonk.com/) uses positional letter-frequency
heuristics instead — faster per guess, ~93% as optimal as pure entropy,
and feels more natural to humans following along.

## Related GWN tools

- [Wordle Wonk](https://wordlewonk.com/) — production GWN Wordle helper
- [A2Z Words](https://a2zwords.com/) — daily Wordle companion blog
- [A2Z Word Finder](https://a2zwordfinder.com/) — dictionary API
- [3Blue1Brown's Wordle video](https://youtu.be/v68zYyaEmEA) — canonical explainer

## License

[CC0-1.0](LICENSE) — docs are free to reuse.

---

_Educational docs by [GWN](https://grandewebnetwork.com/)._
