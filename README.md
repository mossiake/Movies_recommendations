# Movies_recommendations
Movies Data Ingestion and Recommendations using BigQuery

This project was done using the MovieLens dataset by Grouplens which can be found on: https://grouplens.org/datasets/movielens/

##Get MovieLensdata

The first step was to get the data into BigQuery. To do so, I used the Google Cloud Shell command line to create a BigQuery dataset to store the MovieLens data. The data was then loaded from a Cloud Storage bucket into the BigQuery dataset.

###Command to create a BigQuery dataset named movies:

Bq -- location=EU mk --dataset movies

bq load --source_format=CSV \
--location=EU \
--autodetect movies.movielens_ratings \
gs://dataeng-movielens/ratings.csv

bq load --source_format=CSV \
--location=EU \
--autodetect movies.movielens_movies_raw \
gs://dataeng-movielens/movies.csv


##The remaining steps of the project can be found on BigQuery: https://console.cloud.google.com/bigquery?sq=774834503256:1dbb4e948969405f9b77c8c23887fce4

Once the dataset was created and the data ingested into BigQuery, I did a bit of exploration of the data and then proceeded to train a model for movies recommendation using BigQuery ML. The model used here is created using collaborative filtering and is applied by identifying which columns play which role in the collaborative filtering setup. In this case, the user factors represent a user_id and the item factors reprefent an item_id.

*It is important to note that my project was done using a Google Cloud free trial, which provides a limited use of BigQuery. In order to still be able to run the model, I used the Google Cloud training model.

The model in a full capacity Google Cloud account is:

CREATE OR REPLACE MODEL movies.movie_recommender
OPTIONS(model_type='matrix_factorization', user_col='userId', item_col='movieId',
rating_col='rating', l2_reg=0.2, num_factors=16) AS
SELECT userId, movieId, rating
FROM movies.movielens_ratings

I used the model to determine the top 5 best drama movies to recommend to a specific user. I also used to model to test customer targeting. In this case,the purpose was to obtain more reviews for a movie so I identified 100 users who may rate it the highest. They could receive an incentive in return for their reviews.

Lastly, I used to the model on a higher scale for batch predictions for all users and movies and showed 100000 of the total results.

As shown above, this model is a great tool to provide recommendations for customers and do customer targeting. 
