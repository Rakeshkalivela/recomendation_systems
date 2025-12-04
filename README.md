# Item-Based KNN Movie Recommender System

## Overview
This project implements an Item-Based K-Nearest Neighbors (KNN) recommender system using the MovieLens dataset.
The goal is to recommend movies to users based on item–item similarity computed from historical ratings.

This is the first model in a multi-model recommendation pipeline (CF-based → MF-based → hybrid).

## Dataset

MovieLens (100k)

- 9742 movies
- 610 users
- 100,836 ratings
- Ratings from 0.5 to 5.0
- Strong sparsity (98.3% unobserved interactions)

## Preprocessing

- Removed cold-start movies with fewer than 3 ratings
- Final:

    - 610 users
    - 4,980 movies
    - 94,794 ratings

## Train/Test Split

- Per-user time-based split:
- Oldest 80% → train
- Most recent 20% → test
- If a user has <2 ratings,  all goes to train

## Model - Item-Based KNN

- Sparse CSR user–item matrix
- Cosine similarity
- KNN using sklearn NearestNeighbors
- Prediction via weighted sum of neighbor ratings
- Handles unseen items by fallback strategies

## Why item-based?

- Stable item vectors
- Easy to scale
- Good for explainable recommendations

## Evaluation Metrics

All metrics are computed on per-user ranked recommendation lists:

Metric      Description
Hit@10	    Whether any test movie appears in top-10
Recall@10	Fraction of user’s test items retrieved
NDCG@10	    Ranking quality (position-sensitive)

## Model Performance

Hit@10:    0.5311
Recall@10: 0.0638
NDCG@10:   0.1219

## How Recommendations Work

- For each user, identify movies they have rated.
- For each rated movie, find its K nearest neighbors.
- Compute a weighted score using:
- Similarity weight
- User’s rating for the anchor movie
- Aggregate scores across all rated items.
- Filter out movies already seen.
- Return top-K recommendations.