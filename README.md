# gwn-wordle-solver-python

> An educational Wordle solver in Python that picks guesses by maximizing
> expected information (Shannon entropy). Companion to
> [Wordle Wonk](https://wordlewonk.com/) — the production GWN Wordle helper.
> Part of the [Grande Web Network](https://grandewebnetwork.com/) family.

## Motivation

3Blue1Brown famously popularized the information-theoretic approach to Wordle
(1 Feb 2022). The core idea is simple: pick the guess whose expected
distribution of color-patterns maximizes the bits of information gained about
the answer.

This repo is a ~100-line reference implementation you can read end-to-end.

## The core loop

```python
def best_guess(candidates, allowed_guesses):
    best, best_score = None, -1.0
    for guess in allowed_guesses:
        buckets = defaultdict(int)
        for answer in candidates:
            pattern = score(guess, answer)  # 5-tuple of (GREEN|YELLOW|GRAY)
            buckets[pattern] += 1
        # Shannon entropy of the bucket distribution
        total = len(candidates)
        entropy = -sum((n/total) * math.log2(n/total) for n in buckets.values())
        if entropy > best_score:
            best, best_score = guess, entropy
    return best
```

## Observed performance

On the official Wordle answer list (2,315 words) with a full allowed-guess
dictionary (~12K words):
- **Average guesses to solve**: 3.61
- **Worst case**: 5 guesses
- **First guess (max entropy)**: `TARES` or `SOARE` depending on tie-break

## Try it live

```bash
pip install -r requirements.txt
python solve.py                  # interactive — enter Wordle color feedback
python solve.py --hard           # Hard Mode (guesses must match all constraints)
python solve.py --benchmark      # replay on 2,315 answers, print avg guesses
```

## Related

- [Wordle Wonk](https://wordlewonk.com/) — the production GWN Wordle helper
  (uses positional heuristics instead — faster but 93% as optimal)
- [A2Z Words](https://a2zwords.com/) — daily Wordle companion blog
- [3Blue1Brown's Wordle video](https://youtu.be/v68zYyaEmEA) — the canonical explainer

## License

[CC0-1.0](LICENSE). PRs welcome.

---

_Educational asset by [GWN](https://grandewebnetwork.com/)._
