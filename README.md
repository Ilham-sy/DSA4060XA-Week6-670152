# DSA 4060XA – Week 6: Evaluating Recommender Systems

**Name:** Ilham
**Student ID:** [670152]

## Lab objective
Compare three recommenders on a small movie-ratings dataset (12 users, 20 movies, 120 ratings), using the right metrics for rating prediction and for Top-3 ranking, then recommend a model for a stated business goal.

## Models compared
1. Popularity Baseline (movie average rating)
2. Content-Based Filtering (genre profile + cosine similarity)
3. Item-Based Collaborative Filtering (item–item cosine similarity)

Split: each user's most recent rating is the test item (108 train, 12 test). A test item is "relevant" if rating ≥ 4.

## Results (K = 3)

| Model | MAE | RMSE | Precision@3 | Recall@3 | NDCG@3 | Coverage | Novelty | Diversity |
|---|---|---|---|---|---|---|---|---|
| Popularity Baseline | 1.086 | 1.201 | 0.067 | 0.20 | 0.10 | 0.35 | 1.689 | 0.736 |
| Content-Based | 1.094 | 1.381 | 0.067 | 0.20 | 0.20 | 0.70 | 1.344 | 0.542 |
| Item-Based CF | **0.924** | **1.070** | 0.000 | 0.00 | 0.00 | **0.85** | **1.695** | **0.780** |

## Best model for rating prediction
**Item-Based CF** (lowest MAE and RMSE), though only modestly better than the baseline.

## Best model for ranking
**Content-Based** (highest NDCG@3; it found 1 of 5 relevant held-out movies and ranked it first). Popularity found 1 but ranked it third. CF found none.

## Coverage, novelty and diversity findings
- **Coverage:** CF (0.85) > Content (0.70) > Popularity (0.35). Popularity shows the same few movies to everyone.
- **Novelty:** CF (1.69) ≈ Popularity (1.69) > Content (1.34).
- **Diversity:** CF (0.78) > Popularity (0.74) > Content (0.54).

## Final recommended model
**Item-Based Collaborative Filtering.** The business wants relevant discovery without the same popular titles dominating. CF has the widest coverage, the highest novelty and diversity, and the best rating accuracy. The trade-off is that it found no relevant movie in the Top-3 in this test.

## Extension (Option A – change K)
Precision@K mostly falls as K grows (best possible is 1/K with one relevant item per user), while Recall@K rises or stays the same. Content-Based recall rose from 0.2 to 0.4 at K=5.

## Limitations
1. Very small data: 12 users and 12 test ratings. Only 5 users have a relevant test item, so one extra hit changes Recall by 0.2.
2. Only one held-out movie per user. Other movies the user might like, but never rated, are counted as misses.
3. Genre is the only content feature, so Content-Based cannot tell apart movies in the same genre.
4. Item-Based CF fills missing ratings with 0 before computing similarity, which can distort similarities in sparse data.
5. Offline metrics do not show how real users would respond (clicks, watch time).

## How to run
Restart the kernel and run all cells top to bottom in `DSA4060XA_Week6_Guided_Evaluation_Lab.ipynb`. Requires Python, pandas, NumPy, scikit-learn, matplotlib.
