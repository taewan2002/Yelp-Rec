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

2. Got dummies from categories columns: Remove nested properties
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
