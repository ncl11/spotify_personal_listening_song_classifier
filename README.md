# spotify-project
### Project Overview
As a musician and music lover I have long felt that genre is an inadequate metric for classifying music/musical preferences.  Using a one dimensional measurement such as genre can be a useful approximation but it lacks descriptive depth and leaves many nuances of a piece of music unaddressed.  Classifying both John Coltrane and Frank Sinatra as Jazz artists or James Brown's 'Cold Sweat' and Earth Wind and Fire's 'Serpentine Fire' as both Funk songs seems to be reductive to the point where it almost loses all meaning.  Additionally many people, myself included, listen to music that spans a wide variety of genres and styles which further makes attempts to classify an individual's musical preferences based solely on genre insufficient and potentially counterproductive.  
With these issues in mind we ask the question "Is it possible to put aside genre and classify a user's musical preferences using underlying sonic characteristics of a song?"  Being able to classify a song as one which will potentially fit a listener's tastes could be very valuable for developing a recommendation algorithm or fine tuning an existing one.  For this project I have combined my personal Spotify listening data for the previous year with a random sample from the 160K+ track Spotify data set which is available on Kaggle.  Each row of the dataset represents a song and the features have all been pulled from Spotify's API.  The goal of the project is to develop a binary classification algorithm - class 0: song from random sample from kaggle dataset, class 1: song from my personal history.  The final model classifies songs with recall of ~ .9(and accuracy of ~ .84).  It is worth noting that just because a song wasn't in my personal history for the past year doesn't mean I wouldn't like it or haven't listened to it at some point more than a year ago.  For these reasons there is likely lots of overlap between the audio features from a random sample from the Kaggle dataset and my personal Spotify history.  Therefore an expectation of anything approaching a perfect classifier would be unreasonable.  We feel that the model's performance is very strong.   

### Language, Packages and Resources Used
**Language:**  Python 3.8.3  
**Packages:** datetime, sklearn, xgboost, numpy, matplotlib, seaborn, requests, pandas and json  
**Resources:** _get_spotify_uri method partially adapted from https://github.com/TheComeUpCode/SpotifyGeneratePlaylist/blob/master/create_playlist.py  
**Descriptions of Audio Features Pulled from Spotify API:** https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-features/

### get_data Notebook  
With the Create_DF class a user can create two different data sets.  The add_audio_features method takes in a user's personal listening json files and creates a dataframe with audio features from the Spotify API and target variable with value 0 for songs listened to once and 1 for songs listened to more than once.  The merge_personal_kaggle method merges the df created using add_audio_features with a random sample from the Kaggle Spotify data set.  The target variable 'y' is changed to 0 for songs from the random sample and 1 for song from personal listening history.  This code could easily be adapted to create a dataset with a different user's personal listening data.  Request your personal listening data from Spotify and download the Kaggle Spotify dataset.  Client_id, client_secret, spotify_token, spotify_user_id can all be obtained from Spotify's website.  Comments have been provided in the notebook to assist in making the necessary changes.  For this project we use the dataset created by the merge_personal_kaggle method.  

### eda_notebook Notebook  
In this notebook we perform exploratory data analysis on the dataset.  There are many interesting and useful findings highlighted in the notebook.  Some of the more interesting plots are shown below.  
<img src="figs/popularityvsyear.png" width="400">
<img src="figs/loudnessvspopularity.png" width="400">
<img src="figs/acousticnessvsdanceability.png" width="400">
<img src="figs/energy-hist.png" width="400">
<img src="figs/acousticness-violin.png" width="400">
<img src="figs/year-vs-avg-pop.png" width="400">
<img src="figs/corr-heatmap.png" width="800">

### model_building Notebook  
In this notebook we train several models to classify songs from my personal listening history vs songs from a random sample from the Kaggle Spotify dataset.  This model could be used as part of a recommendation algorithm to select songs that a user might be interested in.  In order to know which metric to use we would need to know how often our application was making recommendations.  If recommendation opportunities occurred frequently, for example in order to create many playlists for a user, then recall is likely the best metric.  This metric will allow us to catch the highest possible number of condition positive songs, but this comes at the risk of misclassifying some class negative data points.  Accuracy could also be used if we want to be more careful about the overall rate of correctly classified songs.  As such we train models which optimize both metrics and a user could choose the best model based on the specific application.  We train Logistic Regression, Random Forest, SVC, a Quadratic Discriminant Analysis model, XGBoost and KNeighbors.  As can be seen below, regardless of whether we choose to use accuracy or recall XGBoost performs best, however the hyperparameter choice will be different depending on the metric.  Also the confusion matrices and feature importance measurements can be seen in the notebook. 

### Model Performance
* XGBoost - test recall score:  0.8952042628774423, test accuracy score:  0.8383658969804618
* Logistic Regression- test recall score : 0.8632326820603907, test accuracy score : 0.7806394316163411  
* Random Forest-  test recall score : 0.8792184724689165, test accuracy score : 0.8321492007104796 
* QDA - test recall:  0.8792184724689165, test accuracy:  0.7566607460035524
* SVC - test recall score : 0.8152753108348135, test accuracy score : 0.7921847246891652  
* KNeighbors - test recall score : 0.8472468916518651, test accuracy score : 0.7912966252220248
