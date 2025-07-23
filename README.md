# Spotify Tracks Attributes and Popularity

📁 **Source**:  
[Spotify Tracks - Attributes and Popularity (Kaggle)](https://www.kaggle.com/datasets/melissamonfared/spotify-tracks-attributes-and-popularity/data)

## 📊 Dataset Overview

This dataset contains information about thousands of Spotify tracks, including audio features, popularity metrics, and metadata related to artists, albums, and genres.

## 📄 Column Descriptions (from `dataset.csv`)

| Column Name         | Description |
|---------------------|-------------|
| `track_id`          | Spotify's unique identifier for the track |
| `artists`           | Name of the performing artist(s) |
| `album_name`        | Title of the album the track belongs to |
| `track_name`        | Title of the track |
| `popularity`        | Popularity score on Spotify (0–100 scale) |
| `duration_ms`       | Duration of the track in milliseconds |
| `explicit`          | Indicates whether the track contains explicit content |
| `danceability`      | How suitable the track is for dancing (0.0 to 1.0) |
| `energy`            | Intensity and activity level of the track (0.0 to 1.0) |
| `key`               | Musical key (0 = C, 1 = C♯/D♭, …, 11 = B) |
| `loudness`          | Overall loudness of the track in decibels (dB) |
| `mode`              | Modality (major = 1, minor = 0) |
| `speechiness`       | Presence of spoken words in the track (0.0 to 1.0) |
| `acousticness`      | Confidence measure of whether the track is acoustic (0.0 to 1.0) |
| `instrumentalness`  | Predicts whether the track contains no vocals (0.0 to 1.0) |
| `liveness`          | Presence of an audience in the recording (0.0 to 1.0) |
| `valence`           | Musical positivity conveyed (0.0 = sad, 1.0 = happy) |
| `tempo`             | Estimated tempo in beats per minute (BPM) |
| `time_signature`    | Time signature of the track (e.g., 4 = 4/4) |
| `track_genre`       | Assigned genre label for the track |


## Bronze Layer (Preprocessed CSVs)

The original dataset (`dataset.csv`) was not ingested directly in raw form. Instead, it was processed using the `Kimball.ipynb` notebook to follow a dimensional modeling approach. This script splits the raw dataset into dimension and fact tables as `.csv` files:

- `dim_artists.csv`: Unique artists with a generated `artist_id`
- `dim_albums.csv`: Unique albums with associated `artist_id` and a generated `album_id`
- `dim_genres.csv`: Unique track genres with a generated `genre_id`
- `dim_tracks.csv`: Tracks with `track_id`, `track_name`, and references to `album_id` and `genre_id`
- `fact_tracks.csv`: Numerical and categorical audio features per `track_id`

These files were saved into `dim/` and `fact/` folders and then loaded as Delta tables in the Silver layer.



## Silver Layer

The CSV files created in the Bronze layer were read into Spark and stored in Delta format. These tables serve as the clean, structured foundation for analytics:

- `dim_artists`
- `dim_albums`
- `dim_genres`
- `dim_tracks`
- `fact_tracks`



## Gold Layer 

From the Silver Delta tables, additional aggregated and analytical views were created for analysis purposes, such as:

- `all_info`: All tracks with artist and album context, excluding IDs
- `avg_metrics_by_artist`: Average track metrics per artist
- `avg_metrics_by_genre`: Average track metrics per genre
- `avg_metrics_by_album_artist`: Average metrics per album and artist
- `song_count_by_artist`: Total number of songs per artist

These tables are written in Delta format and can be used for reporting, exploration, or ML pipelines.


## ✅ Tools & Technologies

- Apache Spark
- Delta Lake
- PySpark
- Lakehouse architecture (Bronze → Silver → Gold)
- Python 3.x