
# Disentangling posts on OSNs: Notable posts and topics

Complete Python code of the methodology used to identify notable posts and topics on Online Social Networks.
Intermediate steps are stored in CSV files.

## Dataset transformation

* **Input**: a Pandas DataFrame (df_influencers) with columns:
    * **postID**: id of the post;
    * **AccountID**: id of the influencer account that published the post;
    * **name**: name of the influencer account that published the post;
    * **date**: exact publication date of the post;
    * **year**: year of publication of the post;
    * **week**: week of publication of of the post;
    * **description**: text of the post;
    * **reactions**: number of "likes" and "comments" effectively received by the post;
    * **followers**: number of followers of the influencer at the time of the publication of the post.
    
    
* **Output**: the same Pandas DataFrame with the following columns in addition:
    * **list_hashtags**: the list of hashtags extracted from the text of the post;
    * **expected**: number of "expected" reactions of the post;
    * **score**: performance "score" of the post (effective reactions/expected reactions).

### Compute expected reactions for each post per influencer

Find expected reactions for each post for the current influencer.
* **Input**: DataFrame of all the posts of all influencers;
* **Output**: the same DataFrame, but with the "expected" column in addition, computed for each influencer.

### Compute performance score for each post

Find anomalies (notable posts) for the influencers through the Boxplot Rule method.
* **Input**: DataFrame of all the posts of all influencers;
* **Output**: DataFrame of notable posts of all influencers.

## ANOMALY DETECTION

Apply the Boxplot Rule method to extract **notable posts** for each influencer and save them in a CSV file ("anomalies.csv").
* **Output**: the same previous Pandas DataFrame with the following column in addition:
    * **n_anomalies**: number of anomalies for each week.

## CLUSTERING (Graphs creation and Louvain Community Detection algorithm)

* **Output**: the same previous Pandas DataFrame with the following columns in addition:
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
The ouput DataFrame is then saved in a CSV file ("communities.csv").

## WORDCLOUDS per community per week

This is the final output. It is necessary to select a year and a week and the ouput will be a **wordcloud** (a visual representation of a text, with the characteristic of attributing a larger font to the more frequent terms) of the hashtags for each community of the selected week.

## Authors

- Paola Caso [@paola2108](https://github.com/paola2108)
- Martino Trevisan [@marty90](https://github.com/marty90)
- Luca Vassio [@Luca2030](https://github.com/Luca2030)

