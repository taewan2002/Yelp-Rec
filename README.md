# Yelp-Rec

## Business Objective
<img width="944" alt="Screenshot 2024-01-15 at 9 03 52 PM" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/af7a0544-c498-4946-910c-808dd1f6ff12">

It provides reviews of restaurant. User evaluation and recommendations using custom evaluation, user evaluation, user evaluation and recommendations of restaurants.

https://www.yelp.com/dataset

## Data Exploration

### Business.json
Restaurant data, such as location, category, and rating of a restaurant

<img width="930" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/ef4d9c61-b1fb-471b-8f66-f6e53f54b134">

<img width="695" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/9072114e-fc59-4a82-a97d-6bdb1da01240">

<img width="452" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/080aa532-7234-443e-a210-eb886f233b90">


### Review.json
Review data for user-generated evaluations

<img width="978" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/5b7c0795-f8b6-4462-94bb-36562f3563f2">


### User.json
User information data, such as user name, number of reviews, etc

<img width="1003" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/2ba98c44-2073-4f40-b9aa-71f616a387c7">

<img width="787" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/f90092ab-ae26-46e6-a809-749ef83d357d">



### Checkin.json
Business hours and number of people coming in at that time data

<img width="486" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/daffe68b-6967-49c4-b75c-f7ffe1016feb">


### Tip.json
Short review data

<img width="872" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/8478f357-efb1-40ee-a899-cb0acc66502d">


### Photos.json
Photo, company ID, caption of photo, and corresponding category data.

### Hypothesis
Ratings, reviews and locations largely influence a business.
So, we think these 3 the main factors one have to look into when we have this kind of dataset.
Larger business with many locations, more difficult to maintain good rating. Have more the number of reviews, stars get down.

1. Grouping the dataset with avg rating and count of reviews and number of locations and sorting it city wise.
   <img width="686" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/daeaffe0-be1a-4760-a92d-36cc2a8aaec9">
   <img width="685" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/94f0b620-edf6-40f0-bd9b-c5769ee38d9c">

2. Find a company that is performing well with more than 4 stars and more than 1000 reviews.
   <img width="487" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/c451409d-47b2-423a-aa03-dd8eed02bcd4">
   <img width="684" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/7c949731-9304-400a-a115-6623f0797083">
   <img width="689" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/30cd9838-0ea6-47d3-b5aa-54ec9dd4e73b">

We decided to proceed with collaborative filtering using only business data and review data.

## Data Preprocessing
1. Select the data for our model
   Ratings, reviews and locations largely influence a business.
   So, I think these 3 the main factors one have to look into when we have this kind of dataset.
   We think it will serve the purpose of the recommendation system to conduct collaborative filtering within one area. We chose 'Philadelphia' because it has the highest number of reviews.

   <img width="397" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/3e26be72-fb4c-4aaa-9039-379e74097fd5">
   
   <img width="579" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/facd45de-5735-4b38-b741-aa7fc5b73651">

3. Got dummies from categories columns: Remove nested properties
   In column has nested attributes. In order to create a feature table, we need to separate those nested attributes into their own columns. Therefore, the following functions will be used to achieve this goal.
   - Function that extract keys from the nested dictionary.
   - Convert string to dictionary.
   - Get dummies from categories.
   - Concat all tables and drop restaurant column
   - Map floating point stars to an integer.
   
   <img width="949" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/8eba4d09-24d8-42f1-88ff-fbcdabd6713f">

## Content based filtering
A method of recommending another item with content similar to the item when the user prefers a specific item.

We have a review and business(restaurant) data. Through the review data, we will understand what characteristics the user likes and proceed with the process of recommending the restaurant with the characteristics.
1. Gets the id value of the highly rated restaurant.
   - Gets the data reviewed by a particular user, and gets the id value of the restaurant rated at a high rate.
   - 'user_id_recommend': ID value of the target user. Proceed with the process of which restaurants to recommend to this user.
   - First, bring in the review data record left by the user, and then bring in business_id with high preference.

   <img width="486" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/05e70d00-4397-44bf-bae1-92b3308503aa">
   
   : The id value of the restaurant identified as the user's preference.

2. Examine what characteristics the restaurants have.
   - Restore information to the business dataset and select and output only important information.
   - Since there is a lot of data, delete columns of the same meaning and identify overall preference characteristics.
  
   <img width="348" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/9331f67c-32cf-43a5-9609-3f7f0d9cc136">

   This is Characteristics of restaurants that are thought to be user-preferred.

3. Use KNN for prediction model.
   - Verify that the data modified by preprocessing, data deleted due to data capacity issues, and selected columns are appropriate.
   - The similarity between items (restaurant characteristics) is measured using KNN. The target value was taken as a store's rating.

   <img width="486" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/bb89d510-7729-4a91-897c-1467c9099864">

4. Using Cosine similarity.
   - Look for a restaurant similar to 'reviewed_rest_feature' (characteristics of user preferred restaurant).
   - A similarity is assigned to each restaurant data as a result of the calculation.
   - The top five results of similarity are output.(indicators are similar)

   <img width="759" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/929c7f41-363d-4a76-8019-9fc6c20c435c">

5. Result
   - Print out the characteristics of these recommended restaurants in detail.
   - Save only features that correspond to at least 3 times out of 5 recommended restaurants.
   - Out of more than 1300 features in total, about 25 can be selected as major features.

   <img width="395" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/fe53ef0d-8951-42ac-b53c-694f0841f491">

### Compared to previous features

<img width="600" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/efb172a1-554a-436d-9e2d-3281558ef92a">

## Collaborative Filtering
A method based on the idea that similar users may have similar restaurant preferences.

This section makes recommendations to restaurant users using collaborative filtering techniques. Implement machine learning techniques such as SVD and PCA to build a recommendation system.

## Data Preprocessing for content based filtering
- Pull out needed columns from subset_review table
- Pull out names and addresses of the restaurants from rest table
- Rest table: A table extracted from restaurants in Philadelphia
- Combine df_review and restaurant table.
- Complete combined_business_data.

<img width="569" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/fc1a70c9-460e-4e1b-8be7-e6454cf6a9a8">

Sorting based on star, then only head (5) of the business id of the most famous restaurants (most rated restaurants) is output.

<img width="329" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/a6bac870-5ae4-4362-a1c0-c7ba97f90255">

## Collaborative Filtering

### Building a utility Matrix (User-Restaurant Matrix)
- This matrix contains each user, each restaurant, and the rating each user gave to each restaurant.
- Notice this matrix will be sparse because every user doesn't review every restaurant.
- After finishing the configuring, the matrix is transposed to represent the user as a column and the restaurant as a row.
<img width="1050" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/cfc58862-3679-408e-984a-2adea1014fb9">

### Decomposing the Matrix
- Now we use Sklearn's truncatedSVD to compress the transpose matrix into the number of rows of 12 matrices.
- It compresses users arbitrarily with 12 components for their tastes.
- Select n_components from truncatedSVD -> Dimension reduction in data and trade-offs in performance should be considered.

<img width="425" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/9d6f17b7-5bb4-4569-846d-2a73002bdd5a">

- Create and apply PCA models to find the optimal values for n_components. -> n_components = 12

### Generate a correlation matrix
- We calculated Pearson R coefficient for every restaurant pair in the result_matrix.
- The correlation-based on similarities between users' tastes.

### Isolating the most popular restaurant from the Correlation Matrix
- Extract the correlation values between the top five restaurants with all other restaurants from corr_matrix.

### Recommend highly correlated restaurants
- Village Whiskey - Bars, Nightlife, Whiskey Bars, Burgers, Restau...
  : ['Chan's Halal Restaurant', 'Moy Fong Restaurant', 'Sister Muhammed's Kitchen']
- Honey's Sit-N-Eat - Southern, Restaurants, American (Traditional)
  : ['16th Street Seafood', 'George's Sandwich Shop', 'Quality Pizza']
- HipCityVeg - Burgers, Vegetarian, Restaurants, Vegan, Sandwiches, Breakfast & Brunch, ...
  : ['Fresh Gulf Seafood', 'Ricky Cafe']
- Penang - Malaysian, Asian Fusion, Cambodian, Chinese, R...
  : ['Applebee's Grill + Bar', 'Mayfair Pizza', 'New England Pizza', 'Potito's Italian American Pastries', 'Wawa Food Markets']
- El Camino Real - Mexican, Tex-Mex, Restaurants, Barbeque
  : ['Bonks Bar', 'Ed's Pizza House', 'El Camino Real', 'Little Kitchen', 'Los Potrillos', 'Ron's Caribbean Cuisine', 'Sorrento's Pizza', 'The Freshworks of Port Richmond', 'Wendy's', 'Wilson's Market']

## Evaluation and Analysis

### Content-based Filtering
- Performance of the KNN

  <img width="487" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/0b571d37-92bc-49d0-9a99-a66b1fdbfb64">

- Actual Selected Features

  <img width="446" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/f02b543a-1d69-4715-8113-758e8b8055ad">

### Collaborative Filtering
- Method 1. Hit Rate
  1. Gets all of the items that you've preferred or clicked on for a particular user.
  2. Percentage of N items recommended to the user → Leave-One-Out Cross-Validation that indicates whether the user actually evaluated any items
  3. After learning the recommendation model with the remaining items, extract the Top K recommendation list
  4. If there is an item on the K recommendation list that I removed earlier, it's a hit, or it's not a hit.

     <img width="487" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/c892b12d-ed18-4e43-8c36-be5bb82c24c8">

  - However, due to the nature of the dataset, the Hit Rate is judged to be low depending on the text nature.
  - Recommend restaurants to each user from the entire dataset.

    <img width="487" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/65731d62-3176-44f4-a730-fc4f87158809">
    
    <img width="495" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/f4d79fd1-aaa2-4905-a005-780a2710fc1b">

- Method 2. Find user_id with the most rows
  1. Find user_id with the most rows

     <img width="487" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/beacdc52-b8d0-476f-86fb-bcb43a65d6de">
     
  2. Check the applicable table

     <img width="443" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/7b9e321b-266a-4522-8d4d-8c2c8cadd5bb">
    
  3. Import restaurants evaluated by the user
  4. Find other restaurants except those already evaluated by the user
  5. Finding restaurants that other users like
  6. Output the list of recommended restaurants

     <img width="486" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/f3ede023-3208-44d4-bcf8-a896366d017f">

  - Just recommend restaurants which have high stars from other users to a specific user.
 
## Comparison to previous projects

### With the same data in the past
- Encode and prepare data: Encode business and user IDs using Sklearn's Label Encoder and store unique users, restaurants, minimum ratings, and maximum ratings.
- Data segmentation: Split data into training and test sets.
- Factor definition for a model: Create a variable to store the number of factors per user/restaurant for the collaborative filtering model.
- Save users and restaurants: Make separate arrangements for users and restaurants input to train and test sets.
- Embedded representation: uses embedding to represent users and restaurants in the data.
- Model Enhanced: Adds bias to each embedding, runs the output of dot products through the sigmoid layer and scales the results using the minimum and maximum ratings of the data to improve model performance.
- Prediction from Test Dataset: Use the generated model to predict the test dataset and compare the actual stars to predict the model performance.
- Calculate cosine similarity: Use cosine similarity to measure cosine similarity between two restaurants.
- Embedding Normalization: Normalizes the embedding so that the dot product between two embeddings reflects cosine similarity.
- Recommendation function: Create a function to calculate the cosine similarity of the target restaurant and other restaurants and return the ticket with the result.

### How different is it compared to ours
- Comparing the actual stars and predictions.

  <img width="486" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/6358fad4-ede1-4d8c-8d8c-0183aa4e5ed9">

- Calculates the cosine similarity between the target and the rest of the other restaurants.

  <img width="531" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/c3797fcc-a608-47d4-99ad-4b614d6485cf">
  <img width="344" alt="image" src="https://github.com/taewan2002/Yelp-Rec/assets/89565530/6085a12c-7189-42b7-a4cd-4ad0175ee721">

## Learning Experience

### Difficulties encountered and how you have solved them
- The datasize is big, but it took too long to upload and download.
- It is a dataset that needs to be converted from json to csv, but many errors occurred in the process. There was a reference that it is convenient to use python 2 version, but sagemaker does not support it.

  -> It was solved through modifications such as replacing the function, changing the mode, and changing the processing to fit the python 3 version.
- photo.json file does not open

  -> We think it's a file that I don't need much, so we exclude it.
- review.json file also encountered memory error during conversion.

  -> Use larger instance, error again. We reduced size.

### If you had more time or if you have to do the project all over again, what would you do or do differently or spend more time on.
- When the recommendation system was turned around, only the data set related to the Philadelphia city, which occupies the largest part of the data set, but it is regrettable that the recommendation system could have been constructed based on more cities if it had been given a little more time.
- It seems that I prepared in a hurry because the 12-week class and the schedule overlap or deviate. If you have more time, it would be nice to implement a recommendation system while feeling the strengths of SageMaker more.

### What you have learned doing the project (individually)
- Dahyeon(Configuration Content-based Filtering): I was able to apply the content of the recommendation system learned in class to the project, enabling a deeper understanding beyond a simple grasp of the concepts. Moreover, as utilizing recommendation systems involves incorporating other models, it provided an opportunity to study additional concepts.
- Doyeon(Production of PPT and presentation): It was a great experience to learn the recommendation system used in the content we always encounter in the major class and implement it with the dataset we wanted. At the same time, it was nice to review various concepts to incorporate the machine learning model.
- Hyewon(Configuration Collaborative Filtering): It was interesting to directly implement a recommendation system model that we can easily access in our daily lives and use a restaurant dataset called yelp, an actual dataset.
- Taewan(SageMaker setting and data preprocessing): I've always been deeply interested in recommendation systems using generative AI and GNNs. Implementing such a vast recommendation system has been an exhilarating experience, and I believe I can swiftly apply this to the service I am currently developing.

### References
- https://www.yelp.com/dataset
- https://github.com/Yelp/dataset-examples
- https://github.com/Yelp/yelp-dataset-examples
- https://brunch.co.kr/@mapthecity/32
- https://www.kaggle.com/code/jagangupta/what-s-in-a-review-yelp-ratings-eda
- https://velog.io/@dlskawns/%EC%86%94%EC%B2%9C%EC%8B%9C%EC%8A%A4%EB%9C%A8%EC%9D%B4%EC%9E%90%EC%A1%B0-%EC%BB%A8%ED%85%90%EC%B8%A0%EB%B9%84%EC%8A%A4-%EC%9D%98-%EB%B9%84%EA%B2%B0%EC%A0%90-%EC%A0%90%EA%B2%80%EC%9D%B4-%EB%B0%94%EB%9E%8C%EC%B9%98%EB%A9%B4%EC%84%9C-%EC%84%A0%ED%83%9D%EC%9D%98-%EC%8B%9C%EA%B0%81%ED%99%94
- https://www.kaggle.com/code/zolboo/recommender-systems-knn-svd-nn-keras
- https://github.com/adarsh18raj/Yelp-Restaurant-Recommendation-System/tree/master
- https://github.com/karentyagi/Restaurant-Recommendations-with-Yelp
