<p align="center">
  
  ___
</p>
<h3 align="center">
  
  [Sentimental Recommendation System, _Unsupervised ML_](https://github.com/arif9799/Sentimental-Recommendation-System)
</h3>
<br>

<p align="center">
  <img src="https://github.com/arif9799/arif9799/blob/main/gifs/RecommendationSystem.gif" width="250" alt="Description">
</p>
<br>

_OpinionCraft: Unleashing Sentimental Insights through Unsupervised ML_

An unsupervised approach to mine opinions, thoughts and emotions based on the mathematical notion of the words that determines the sentiment of the reviews that are being processed to achieve results for recommendation. The principal focus is to retrieve user’s search query (Product & Category), based on which the user will be recommended top-n products from that category alone. The Underlying mechanism in simplest terms is to figure out the sentiments of the reviews either as positive or negative, followed by clustering unique items to decide top-k products based on higher average of connotation scores. In the following sections, you'll get to know the craft and intricacies of the System built.
<br>
<br>


This is an unsupervised approach to mine opinions, thoughts and emotions based on the mathematical notion of the words that determines the sentiment of the reviews that are being processed to achieve results for recommendation.

The principal focus here is to retrieve user’s search query (Product & Category), based on which the user will be recommended top-n products from that category alone. The Underlying mechanism in simplest terms is to figure out the sentiments of the reviews either as positive or negative, followed by clustering unique items to decide top-k products based on higher average of connotation scores.
 
The workflow is well portrayed in the picture below:

<p align="center">
  <img src="https://user-images.githubusercontent.com/93501171/167966496-19fed6b7-cea9-4854-8580-ff7382b25c9e.png" width="600" alt="Description">
</p>
  
The approach taken is as follows:

 - The first and the most common step is to pre-process the Data-sets. This involved feature selection that was hand-curated, handling missing values, which in this case was not that much of a hassle as the proportion of missing values was infinitesimal, so just dropping the missing values was enough. Then, selecting observations of unique products that are adequate enough for the model to learn unbiased parameters and dropping those products for which there are not enough number of observations (the threshold set here is 100). Moreover, Feature Engineering of variables such as 'ratio_votes' based on helpful & total_votes, 'True_value' based on 'star_rating' and 'predicted_value' based on 'score' (connotation score).
 - The second stage of the approach comprises of tokenizing the review_body where all of the reviews had to go through a function that cleans the text, converts them to standard ASCII values using 'unidecode()' function, turns all the text into lower-case, gets rid of numbers, removes stop words using gensim.utils package's simple pre-process function and eliminates words that have no semantic meaning based on the corpus available in "nltk library".
 - Third and the most important step, that is the model training, where the models were trained on individual data-sets of the three chosen for-trial-purposes, which can then be used for generic implementation. To train each model, first a basic model object from gensim.models is created using the "Word2Vec()" function, where certain parameters are to be specified such as learning rates, window size, number of neurons in hidden layers, minimum frequency of the words, thread-workers and many more. Then, the tokenized reviews are passed to the "build_vocab()" function that is callable by model object that was initialized in the first step. This returns a corpus of unique words, set of words across all the reviews in that data-set. Further, the model is trained by passing the processed reviews (tokenized reviews) to the train() function, that is again callable by the model object that was initialized in the first step, which now also contains the vocabulary of the reviews of the data-set. Post training, the model-object consists of the word-embedding, 300 component vectors (1x300) for each word in model's vocabulary, which is numerical representation of each word.
 - In the fourth step, the vector-representation of words or the word-embedding are passed to the 'KMeans' model from sklearn.cluster, which segregates them into two clusters and determines the centroid of positive and negative cluster. 
 - In the fifth step, class of each word is determined in the user-built function 'class_determination', passing the centroids produced by the 'KMeans' and 'Word2Vec' model itself. Distance of each word from both the centroids is calculated using the in-built function 'wv.similar_by_vector', wherein cluster point (the positive and negative centroids) and topn integer are passed as an argument, which returns values in increasing order of number of words and their distances to cluster center specified. Based on the distance of each word to both the cluster centroids, the word is assigned to a class that it is closest to.
 - Once the connotation (class) of the words are determined, that is whether it emits a negative or positive sentiment (represented by -1 or 1), the score of each review is a total summation of negative and positive words, where positive score indicates a good review, bad review otherwise. The results are contrasted with ratings given by the user to that product, with the notion that low ratings will never go with a good review, meaning if the products were rated less indicates the users didn't like a certain product, hence the generalization is that, that product will be given a negative review.
 - In the Final step, a query from the user is taken as input, and based on the category queried by the user, the data-frame containing the pre-calculated scores is picked. The chosen data-frame is scanned for the most recent and top-n (n being the number of reviews for a unique product) scores grouped by unique products, which is averaged. The final output is the top-k (k being the number of unique products) products based on the averages.

