# Design document and dataset description.

For this final task I choose sentiment analysis task for movie review.

I will IMDB [dataset](https://www.kaggle.com/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews) for this task.

## Model
* As I'm using quite simple model (LSTM + Linear) and I have quite easy classification task(binary classification), I trained from scratch, using ML flow
* I created account on databricks to track experiments here
* I'm using binary-cross entropy for this task
* I trained embedings from scratch for this task
* As this task is quite simple, with 5 epochs I get 85% accuracy score

## Structure and client 
* I decided to use synchronous project with Flask
* I decided to create simple HTML Frontend page for movie review, where you write review and get it's sentiment

My project consist of two folders:
* MLFlow - here I have code which creates runs for MLflow on databricks(I have one experiment for these runs)
  * cashe folder - for cashing preprocessed reviews (tokenization, remove punctuation, etc) 
  * data - initial reviews from IMDB dataset
  * train.py - main script: it creates an MLflow run with parameters from sys.argv and saves parameters, metric(accuracy), model and vocab_dict to databricks. Here the parameters.
    * {max_epochs} 
    * {batch_size} 
    * {learning_rate} 
    * {embeding_dim} 
    * {hidden_dim} 
    * {vocab_size}
  * prepare_data.py - helper script for text preprocessing
* Website - folder with flask application and html page for client
  * server.py - application script, with flask functions
  * utils.py - helper for server.py, contains model loading and predict function

Also I have docker-compose file which creates two containers:
 * Model
   1. When container starts, MLflow project train model from scratch, send the results to databricks(for history), then creates model, which will answer the requests from Application
 * Application - Flask application, which gets reviews from users and ask Model which sentiment it is.
