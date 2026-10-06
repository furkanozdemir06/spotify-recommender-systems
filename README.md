# Spotify Track Ranking and Genre Recommendation

A lightweight, interpretable ranking system for Spotify tracks. It combines a track's popularity with a few core audio features into a single weighted score, ranks the whole dataset by that score, and recommends top tracks within a chosen genre.

## Overview

Instead of training a black-box model, this project uses a transparent scoring formula so it is easy to see why a track ranks where it does. The workflow covers data exploration, feature selection, score calculation, a top-10 ranking, and a genre-based recommendation function.

## Dataset

The dataset (`dataset.csv`) contains 114,000 Spotify tracks across 114 genres (1,000 tracks per genre) with 21 columns, including track metadata (`track_name`, `artists`, `album_name`, `track_genre`), `popularity` (0 to 100), and audio features such as `danceability`, `energy`, `valence`, `tempo`, `loudness`, `acousticness`, and `speechiness`.

Only one row had missing values (in `artists`, `album_name`, and `track_name`), and it was dropped, leaving 113,999 tracks.

## Workflow

1. **Exploratory data analysis**
   - Checked shape, data types, missing values, and summary statistics.
   - Verified the genre distribution and plotted the popularity distribution.
2. **Feature selection and cleaning**
   - Kept `track_name`, `artists`, `track_genre`, `popularity`, `danceability`, `energy`, `valence`, and `tempo`.
   - Dropped rows with missing values and plotted a correlation heatmap of the numeric features.
3. **Ranking model**
   - Computed a weighted score for every track (formula below) and sorted tracks from highest to lowest.
4. **Top tracks**
   - Listed and plotted the top 10 ranked tracks.
5. **Genre-based recommendation**
   - Built a `recommend_by_genre(genre, top_n)` function that returns the highest-scoring tracks within a given genre.

## Scoring Formula

All audio features are scaled to 0 to 100 so they are comparable with popularity.

```
score = 0.40 x popularity
      + 0.20 x (danceability x 100)
      + 0.20 x (energy x 100)
      + 0.20 x (valence x 100)
```

Popularity carries the most weight (40%), while danceability, energy, and valence each contribute 20%. The weights are a design choice and can be adjusted to favor different musical qualities.

## Results

Top 5 tracks by score:

| Track | Artist | Genre | Popularity | Score |
|-------|--------|-------|------------|-------|
| Super Freaky Girl | Nicki Minaj | hip-hop | 92 | 91.86 |
| Super Freaky Girl | Nicki Minaj | dance | 92 | 91.86 |
| Super Freaky Girl | Nicki Minaj | dance | 83 | 88.24 |
| That That (prod. & feat. SUGA of BTS) | PSY; SUGA | hip-hop | 81 | 87.86 |
| There's Nothing Holdin' Me Back | Shawn Mendes | dance | 86 | 87.36 |

Example genre recommendations (`recommend_by_genre`):

- **pop:** "There's Nothing Holdin' Me Back" (Shawn Mendes), "Bad Decisions" (benny blanco, BTS, Snoop Dogg), "Calm Down" (Rema, Selena Gomez)
- **rock:** "I Ain't Worried" (OneRepublic), "Cold Heart - PNAU Remix" (Elton John, Dua Lipa, PNAU), "Sultans Of Swing" (Dire Straits)
- **hip-hop:** "Super Freaky Girl" (Nicki Minaj), "That That" (PSY, SUGA), "The Real Slim Shady" (Eminem)

Because the same track can appear under several genres (and sometimes several times in the dataset), the overall top 10 contains repeated songs. Filtering by genre avoids most of this.

## Tech Stack

- Python
- pandas, NumPy
- matplotlib, seaborn
