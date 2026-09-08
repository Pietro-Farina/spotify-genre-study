# Spotify Genre Study

This project was developed for the Data Mining course and investigates whether musical genres can be recovered from the acoustic characteristics of Spotify tracks.

The study asks whether genre labels correspond to natural groups in acoustic feature space, or whether they only partially describe the underlying similarities between songs. To explore this question, the project combines data cleaning and exploratory analysis with clustering and network analysis. Clustering is used to compare acoustic groupings with genre and subgenre labels, while a song-similarity network is used to study graph structure and detect communities independently of the assigned genres.

## Project Structure

- `Report.pdf` - Course report describing the dataset, methodology, results, discussion, and conclusions.
- `Dataset_cleaning_and_preprocessing.ipynb` - Imports and inspects the Spotify datasets, standardizes their columns and values, harmonizes genre information, assesses compatibility, and prepares the data for analysis.
- `Clustering_Methodology.ipynb` - Compares clustering configurations to determine a suitable number of clusters and whether PCA should be used before the later analysis.
- `Cluster_Analysis.ipynb` - Applies the selected clustering setup to the complete dataset and to selected macro-genres to study the relationship between acoustic clusters and genre labels.
- `Network_Analysis_SongGraph.ipynb` - Builds and evaluates song-similarity graphs, detects communities, and studies their genre composition, acoustic coherence, and central songs.
- `Network_Communities_Subgenre.ipynb` - Performs a focused network analysis of selected macro-genres and their more specific subgenre structure.
- `SongGraph_Music_Discovery.ipynb` - Uses the song-similarity graph to find and examine acoustically similar paths between tracks, including cross-genre and within-genre examples.

The notebooks are intended to be read and executed in the order suggested by their filenames: preprocessing first, followed by clustering and network-based analyses.

## Dataset Cleaning and Preprocessing

The `Dataset_cleaning_and_preprocessing.ipynb` notebook prepares the data used by the rest of the project. It initially imports three Spotify-related datasets, performs an initial quality assessment, and compares their shapes, data types, missing values, and duplicated track identifiers.

The preprocessing then standardizes column names and schemas, extracts track IDs from Spotify URIs when necessary, removes fields that are not required for the analysis, and handles incomplete records. Duplicate songs are examined using both platform identifiers and combinations of song metadata so that repeated or inconsistent records can be identified. The final deduplication uses the combination of song name and artist, keeps the first occurrence, and removes the remaining repeated records between the two dataset.

Finally, the notebook puts the genre labels into a common granularity and checks that the audio features are measured consistently across the datasets.
The third "Spotify/Youtube" dataset was initially imported, inspected, and cleaned, but excluded from the final analysis.

## Clustering Methodology

The main purpose of the `Clustering_Methodology.ipynb` notebook is to select the clustering setup for the subsequent analysis. It first standardizes the continuous variables, such as danceability, energy, loudness, acousticness, and tempo. Musical key is treated as a categorical variable and converted into one-hot encoded columns, while a second feature set excludes both key and mode to allow a comparison between the two approaches.

The notebook compares K-Means clustering on the original standardized features with clustering after Principal Component Analysis (PCA). Several values of $k$ are tested, and silhouette scores and inertia are used to assess the quality of each configuration. These comparisons help determine a suitable number of clusters and whether PCA provides a useful representation.

## Cluster Analysis

The `Cluster_Analysis.ipynb` notebook applies the clustering choices established in the methodology stage. It uses the standardized acoustic features while excluding key and mode, and examines K-Means solutions with different numbers of clusters, including $k=2$, $k=8$, and $k=19$ for the complete dataset.

The resulting clusters are compared with the macro-genre labels using silhouette scores, Adjusted Rand Index (ARI), and Normalized Mutual Information (NMI). Cluster profiles and heatmaps are used to compare the average acoustic features of each cluster and the distribution of genres across clusters.

The notebook then performs a more focused analysis of the Electronic, Latin, and Pop macro-genres. Several values of $k$ are tested for each macro-genre, and the selected cluster configurations are examined through acoustic profiles and subgenre distribution heatmaps.

## Network Analysis

The `Network_Analysis_SongGraph.ipynb` notebook represents songs as nodes in a weighted k-nearest-neighbor graph. The graph is built from standardized acoustic features, excluding key and mode. Similar songs are connected with edges whose weights are derived from their feature distances, and the notebook compares both Euclidean and cosine distance metrics across different numbers of neighbors.

For each graph configuration, the notebook measures basic network properties such as the number of edges, average degree, connected components, density, and clustering. It then applies the Louvain method to detect communities and evaluates them using modularity, local community consistency, genre assortativity, genre entropy, ARI, and NMI. These measures show how closely acoustically defined communities correspond to the predefined macro-genre labels.

The analysis also examines the structure of the detected communities in more detail. It produces community-size and community--genre heatmaps, calculates average acoustic profiles and within-community coherence, and compares communities using their distance from acoustic centroids. Finally, PageRank is calculated within each community to identify structurally central songs and to study whether highly central songs are also representative of their community's acoustic profile.

## Network Communities and Subgenres

The `Network_Communities_Subgenre.ipynb` notebook focuses on whether network communities correspond to more specific subgenres within a macro-genre. It keeps only tracks with a genuine granular genre label, removing records where the genre label is only the broad macro-genre. It then selects predefined subgenre groups from Electronic, Rock, Pop, and Latin music.

For each macro-genre, the notebook standardizes the acoustic features and builds a weighted cosine k-nearest-neighbor graph using five neighbors. Louvain community detection is applied separately to each graph. The resulting communities are evaluated using subgenre assortativity, modularity, local community consistency, and the acoustic dispersion of subgenres compared with the dispersion of network communities.

## Music Discovery with the Song Graph

The `SongGraph_Music_Discovery.ipynb` notebook uses the saved cosine k-nearest-neighbor graph and its Louvain community assignments to demonstrate how the network can support music discovery. It prepares a metadata table that connects graph nodes with track names, artists, genres, subgenres, and acoustic features, and converts edge similarities into distances that can be used as shortest-path costs.

The notebook computes minimum-distance paths between selected pairs of songs using Dijkstra's algorithm. It examines both a cross-genre path and a within-genre path, reporting the songs visited, their communities, the similarity between consecutive tracks, and the total path distance. It also plots how acoustic features change along each path, making it possible to interpret a recommendation route as a gradual transition through the feature space.

Finally, the notebook introduces a hop penalty to balance acoustic similarity against path length. This discourages unnecessarily long routes made of many small steps and produces paths that are more practical for music discovery while still remaining acoustically coherent.

