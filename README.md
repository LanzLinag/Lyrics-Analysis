# Most Used Lyrics in the Billboard Hot 100 (1946–2022)

An analysis of lyrical vocabulary trends across 6,032 Billboard Hot 100 songs spanning 76 years, built to answer one question: **what words dominate hit songs, and has that changed over time?**

## Abstract

"Love" appears in **56.4%** of every Billboard Hot 100 song charted between 1946 and 2022 — more than half of all hits ever to make the chart. It was the single most common word in every decade from the 1940s through the 2000s, peaking around **1990 at ~62% of songs**. Since then, its dominance has declined to roughly **48% by the 2020s** — not because love went out of style, but because lyrical vocabulary has diversified: casual, conversational words like *out*, *baby*, and *time* rose steadily across every decade and have now converged with "love" instead of trailing behind it.

## Dataset

- 6,032 Billboard Hot 100 songs, 1946–2022
- 2,928 unique artists
- Fields used: song title, artist, chart year, chart rank, full lyrics

## Methodology

**1. Cleaning & tokenizing.** The raw lyrics field was stored as a stringified Python list (e.g. `"['someone', 'that', 'i', 'belong']"`) rather than plain text — a quirk from however the source dataset was originally exported. This required parsing with `ast.literal_eval` rather than a standard text-cleaning regex, since a naive `[...]`-stripping approach (my first attempt) deleted the entire lyrics string instead of just section labels like `[Chorus]`.

**2. Stopword removal, in three tiers.** A raw word count is dominated by function words (the, a, to, i, you) that carry no thematic meaning. To isolate real content words, I removed:
   - Grammar words (pronouns, articles, prepositions)
   - Vocal filler / ad-libs (oh, yeah, na, la)
   - Conversational filler (like, know, just, got)
   - Contractions that lost their apostrophes during tokenization (e.g. "don't" → `dont`, "I'll" → `ill`) — these looked like real words but were actually grammatical filler in disguise, and one (`ill`) was especially misleading since it also resembles the unrelated word "ill" (sick).

**3. Measuring by song coverage, not raw frequency.** Early versions of this analysis counted total word occurrences, which meant a single song repeating a word 50+ times in its chorus (e.g. a novelty song built around one name or one nonsense syllable) could outrank words that were genuinely common across hundreds of different songs. I switched to measuring **the percentage of songs containing a word at least once**, which is a far more honest signal of a word being a broad trend rather than one song's quirk.

**4. Decade-normalized comparisons.** Decades in this dataset range from 106 songs (1940s) to 931 songs (2010s), so raw counts aren't directly comparable across eras. All decade comparisons use percentages, and sample size (`n`) is displayed alongside every decade in the charts as a reminder not to over-interpret smaller samples (especially the 1940s and the incomplete 2020s, which only covers 2020–2022).

## Key Findings

1. **"Love" is Billboard's most consistent lyrical theme**, appearing in 56.4% of all songs across 76 years and ranking #1 by song coverage in every decade from the 1940s through the 2000s.
2. **"Love" usage peaked in 1990** (~62% of songs) and has declined fairly steadily since, to its lowest point (~48%) in the 2020s.
3. **Other common words — "out," "baby," "time" — were rare in the 1940s** (10–25% of songs) but climbed steadily across every decade, converging with "love" by 2010–2020.
4. **The story isn't "love died out," it's that vocabulary diversified.** Mid-century hits leaned on one dominant romantic theme; modern hits spread more evenly across a broader, more conversational set of words.

## Caveats & Limitations

- The 2020s decade is incomplete (2020–2022, ~290 songs vs. 800–930 in other decades), making its percentages noisier than earlier, fully-sampled decades.
- Stopword filtering involved subjective judgment calls (e.g. whether to filter slang or non-English words). This analysis chose to keep them rather than filter aggressively, treating genuine slang and crossover-language lyrics as real signal rather than noise.
- This is a descriptive analysis of word frequency and trends — it does not include statistical significance testing, so visually notable differences between decades haven't been formally tested against random variation.

## Project Structure

```
├── song_lyrics_analysis.ipynb   # Full analysis notebook
├── billboard_top_100_1946_2022_lyrics-edited.csv   # Source dataset
└── README.md
```

## Tools Used

Python, pandas, matplotlib, seaborn, `collections.Counter`, `ast`

## Possible Next Steps

- Topic modeling (LDA) to surface underlying lyrical themes beyond single-word counts
- Sentiment analysis to track emotional tone across decades
- A decade-classifier model (predict a song's era from its lyrics alone) — a natural extension that would test whether vocabulary diversification has made decades harder to tell apart
