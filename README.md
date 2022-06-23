
# Disentangling the Information Flood on OSNs: Finding Notable Posts and Topics

This work is part of the paper:
P.Caso, M. Trevisan, and L. Vassio, “Disentangling the Information Flood on OSNs: Finding Notable Posts and Topics” under review at 2022 IEEE/ACM International Conference on Advances in Social Networks Analysis and Mining (ASONAM).

Complete Python code of the methodology used to identify notable posts and topics on Online Social Networks.
In this repository there are two files:
* Influencers_list_Instagram.csv: the list of profiles (politicians, athletes, musicians) and Instagram influencers (food bloggers, travel bloggers, etc.);
* METHODOLOGY.ipynb: Jupyter Notebook of the complete code (Python 3).
Intermediate steps results are stored in CSV files.

## Prerequisites

* **Python 3**;
* **Libraries**: matplotlib, pandas, numpy, collections, networkx, community, graphviz, difflib, csv, re, unidecode, wordcloud, ast.

## STEP 0: Dataset transformation

* Remove posts with expected reactions equal to 0;
* Extract hashtags from the text in the description of a post;
* Unidecode words, remove emojis and insignificant (too frequent) hashtags;
* Remove posts of influencers with zero followers at the publication date of the post.


* **Input**: a Pandas DataFrame (df_influencers) with columns:
    * **postID**: id of the post;
    * **AccountID**: id of the influencer account that published the post;
    * **date**: exact publication date of the post;
    * **year**: year of publication of the post;
    * **week**: week of publication of of the post;
    * **description**: text of the post, including the hashtags;
    * **reactions**: number of "reactions" (for Instagram, sum of likes" and "comments") effectively received by the post;
    * **followers**: number of followers of the influencer at the time of the publication of the post.
    
    
* **Output**: the same Pandas DataFrame (df_influencers) with the following column in addition:
    * **list_hashtags**: the list of hashtags extracted from the text of the post (description);
    
The new DataFrame is named df_influencers_purged.

## STEP 1: ENGAGEMENT MODELING

* **Output**: the same Pandas DataFrame (df_influencers_purged) with the following columns in addition:
    * **expected**: number of "expected" reactions of the post;
    * **score**: engagement "score" of the post (effective reactions/expected reactions).
    
The new DataFrame is named df_expected.

### Compute expected reactions for each post per influencer

To identify the number of likes and comments the post is expected to receive, we take the last 100 posts from a given profile and consider the reactions they obtained. In case the profile created less than 100 posts, we consider all of them.

We drop the top and bottom 25% of those 100 posts in terms of reactions and compute the average number of reactions of the middle 50% of the posts.
We consider this quantity as the expected number of reactions a post would get. 
In literature, this is the classical **interquartile mean** (or 25% trimmed mean).

* **Input**: DataFrame of all the posts of all influencers;
* **Output**: the same DataFrame, but with the "expected" column in addition, computed for each influencer.

### Compute engagement score for each post

When a new post of the profile is published, we consider the number of reactions it obtains and compare it to the expected value.
A score greater than 1 indicates that the post is performing better than usual for a post of the given profile.

## STEP 2: ANOMALY DETECTION

We want to detect posts whose score deviates significantly from the scores normally received by the profile that created the post, i.e., the outliers.
Apply the Boxplot Rule method to extract **notable posts** for each influencer and save them in a CSV file ("anomalies.csv").
* **Output**: the same previous Pandas DataFrame (df_expected) with the following column in addition:
    * **n_anomalies**: number of anomalies for each week.

The new DataFrame is named df_outliers.

## STEP 3: GRAPH MODELING

We build a graph for each time step, where each node represents a post, connected to other posts by a weighted edge if it has at least one hashtag in common with those posts. For the computation of the weight of the arcs, we opt to use the metric based on the Jaccard Index similarity measure.
* **Input**:  DataFrame of notable posts (obtained in the Anomaly Detection phase);
* **Output**: Dictionary (key,value) -> key:   string "year_week", value: NetworkX Graph object.

## STEP 4: LOUVAIN COMMUNITY DETECTION ALGORITHM

* **Output**: the same previous Pandas DataFrame (df_outliers) with the following columns in addition:
    * **community**: identification number of the community to which the post belongs;
    * **degree**: degree of the node (post) in the graph;
    * **clustering_coeff**: clustering coefficient of the node (post) in the graph;
    * **modularity**: modularity of the communities in the graph;
    * **n_communities**: total number of communities in the graph;
    * **n_communities2**: number of communities containing at least 2 nodes (posts) in the graph;
    * **mean_degree**: mean of the degree of the nodes in the graph;
    * **median_degree**: median of the degree of the nodes in the graph;
    * **mean_clustering_coeff**: mean of the clustering coefficient of the nodes in the graph;
    * **median_clustering_coeff**: median of the clustering coefficient of the nodes in the graph.
    
Here, the term "graph" refers to the graph of notable posts of each week.
The ouput DataFrame (df_communities) is then saved in a CSV file ("communities.csv").

## STEP 5: WORDCLOUDS per community per week

This is the final output. It is necessary to select a year and a week and the ouput will be a **wordcloud** (a visual representation of a text, with the characteristic of attributing a larger font to the more frequent terms) of the hashtags for each community of the selected week.
## Authors

- Paola Caso [@paola2108](https://github.com/paola2108)
- Martino Trevisan [@marty90](https://github.com/marty90)
- Luca Vassio [@Luca2030](https://github.com/Luca2030)


## Acknowledgements

This work has been supported by the FacciamolaFacile grant funded by Fondazione TIM for the project 'Reading (\&) Machine' and by the SmartData@PoliTO center for Big Data and Data Science.

