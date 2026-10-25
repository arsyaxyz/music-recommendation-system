# SoundScout — Music Recommendation System

A portfolio project that recommends similar songs using audio-feature similarity and a hybrid genre-aware ranking model. The interactive demo is built with Gradio and can be launched from Google Colab with a temporary public `gradio.live` link.

## Key features

- Search the catalogue by song title or artist.
- Recommend 5–20 songs from a selected seed track.
- Compare an audio-only baseline with a hybrid model.
- Explore audio features, genre counts, and correlations through EDA.
- Evaluate recommendations using genre match, artist diversity, intra-list diversity, popularity, and catalogue coverage.
- Export the evaluation tables and visualisation from the notebook.

## Dataset

This project uses the [Spotify Tracks Dataset on Kaggle](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset). Download the CSV from the source and upload it when prompted by the Colab notebook. The raw dataset is intentionally not included in this repository; review the source licence and terms before redistribution.

The dataset contains track identifiers, artists, genres, popularity, and audio features. It does **not** include lyrics, so this version supports title- and artist-based search only.

## Models

### Audio-only baseline

Nine numeric features are standardised with `StandardScaler`: danceability, energy, loudness, speechiness, acousticness, instrumentalness, liveness, valence, and tempo. `NearestNeighbors` with cosine distance retrieves audio-similar tracks.

### Hybrid recommender

The hybrid score combines cosine-based audio similarity and Jaccard similarity between genre-label sets. The default audio weight is 0.75 and genre weight is 0.25. These are experimental settings rather than proven optimal weights. The current hybrid implementation reranks a candidate pool retrieved by audio similarity, so candidates outside that pool are not considered.

## Preliminary evaluation

An earlier 100-query offline test in the development notebook reported the following Genre Match@10 results:

| Model | Genre Match@10 |
|---|---:|
| Audio-only | 0.158 |
| Hybrid | 0.561 |

Genre Match@10 measures the fraction of top-10 recommendations sharing at least one genre with the seed track. Because genre overlap is explicitly used by the hybrid ranking, this metric naturally favours the hybrid method. It is a proxy metric, not evidence of user satisfaction. Rerun the notebook to generate the current evaluation outputs for your copy of the dataset.

## Run in Google Colab

1. Open `notebooks/music-recommendation-system.ipynb` in GitHub.
2. Click **Open in Colab** (or upload the notebook to Colab).
3. Run cells in order and upload the Spotify tracks CSV when asked.
4. Run the final cell to launch the Gradio application.
5. Open the temporary `gradio.live` link printed by Colab. Keep the runtime active while testing.

The notebook runs locally in the Colab runtime. The public Gradio link is temporary and should not be treated as a permanently hosted production service.

## Repository structure

```text
music-recommendation-gradio/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   └── music-recommendation-system.ipynb
└── outputs/
    └── .gitkeep
```

## Evaluation caveats

- Evaluation is offline and uses a random sample of 100 query tracks; use larger validation sets for stronger claims.
- Genre Match@10 is partially aligned with the hybrid objective and cannot prove that listeners prefer the recommendations.
- No user ratings, clicks, saves, skips, or listening histories are available.
- Popularity is reported as descriptive context, not as an accuracy metric.

## Technology stack

Python · Pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Gradio · Google Colab

## Future improvements

- Tune hybrid weights on a validation set.
- Add independent human relevance judgments or user-feedback data.
- Explore a broader candidate-generation strategy.
- Add artist diversity constraints and popularity-aware ranking.
- Deploy to a persistent hosting environment after verifying deployment and dataset terms.
