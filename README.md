# IA_651_PierceWarburton_FinalProject

The dataset examined in this project was of squirrel observations, done by volunteers, in Central Park during 2018. It is publicly available on NYCOpenData's website and can be accessed [here](https://data.cityofnewyork.us/Environment/2018-Central-Park-Squirrel-Census-Squirrel-Data/vfnx-vebw/about_data).
The data has 3023 unique observations each one describing a single squirrel's appearance, location, and general behavior. A single observation will have anywhere between 22 and 31 features accompanying it. The variability is due to different volunteers omitting to add various features to their logged observation, a mistake that can be hardly rectified in post. The purpose of this project however was to predict a squirrels location (on the ground or in a tree) through the observations of their behavior. While seemingly niche this analysis could serve to help both amatuer and professional individuals interested in squirrels better understand when they choose to act in certain ways and how the safety of trees vs the dangers of ground influence their activities 

The first step of building this predictive model was to analyze the variables in the dataset. To this end all the distributions were plotted with Location encoded in color on the plot. This was done to examine by eye if any variables had obvious biases or splits between the two locations that could be helpful for the predictions. To plot this data each column was converted to an interger based on the columns unique values and then those values were graphed in a scatter plot. An example of this process is shown below in Figure 1 where each Combination of Primary and Highlight Color in the dataset is convert to a value between 0 and 21 based on the order of appearance for that particular combination (there being 22 combinations in total) and then plotted.

![image](https://github.com/PierceWarburtonDS/IA_651_PierceWarburton_FinalProject/assets/148472871/361ffdc9-9f74-422f-87b8-04ca8afc29fc)


As it so happens nothing much of interest was learned from this step, most of the variables were visibly well distributed. Such is the journey od data analysis. Once visual inspection was complete the data was manipulated for later ingestion into various algorithms. The variable of interest as stated above was Location which was given as either "Ground Plane" or "Above Ground". This was changed to 0 and 1 respectively. The predictor variables were then choosen and added to a seperate dataset along with the augmented Location variable. The predictor variables were, with some exceptions, all the behavior features. Climbing and Foraging were discarded given both their obvious baised to ground or tree and the analysis of their relative distribution in respect to Location. See Table 1 below for exact values in both columns. 



Variable | Observations on Ground | Observations in Tree
--- | :---: | :---:
Climbing | 136 | 522
Foraging | 1270 | 165



Thus the final predictors used are shown below in Table 2 where their descriptions from the NYCOPenData page are paraphased. Fortunately all these variables came as either 0 (squirrel is not participating in this activity) or 1 (squirrel is participating in this activity). Note as well that all rows with NAs were dropped from this subset resulting in a final dataset of 2959 rows and 11 rows. 



Variable | Description
:---: | ---
Location | Where squirrel was first sighted
Running | Squirrel seen running
Chasing | Squirrel seen chasing another squirrel
Eating | Squirrel seen eating 
Kuks | Chirpy communicative noise
Quaas | Elongated vocal communication which can indicate presence of a ground predator
Moans | High-pitched vocal communication which can indicate presence of an air predator
Tail flags | Tail movements like writing in the air to appear larger
Tail twitches | Tail waving like a wave running through it
Indifferent | Squirrel was indifferent to human presence
Runs from | Squirrel runs from human presence


With this subset of the data built I then used PCA to inspect whether the Ground/Tree split was captured fully by the variance of the dataset. The first two Principal Components explained only 27% of the datasets variability and furthermore did little to illuminate a Ground/Tree split. Figure 2 below shows the data in this subset plotted against its Location and as can be observered there is no apparent trend observed with the color encoding. 


![image](https://github.com/PierceWarburtonDS/IA_651_PierceWarburton_FinalProject/assets/148472871/de6d3765-273e-4668-a158-b3a3187a4873)


With PCA not revealing an easy way to predict Location, I moved on to creating a Decision Tree. For this the data was further split into test and train subsets with a 30/70 split respectfully. I then iterated through Decision Tree minimum number of samples per leaf between 0 and 50 in an attempt to find the most optimal value for this hyperparameter. This was all done with a depth of 10. Interestingly the best value for this hyperparameter was a minimum of 4 samples per leaf. Using this as a constant and then iterating through the depth of the Decision Tree between 0 and 50 I found the best depth to be 3. This was somewhat surprising given that my understanding would be that the deeper a decision tree is the better its prediction can be. However the accuracy of the different Decision Tree Depths are shown below in Figure 3 and while 3 does given the best accuracy the accuracy does not vary considerably throughout the test. The accuracy during the minimum number of samples per leaf test is shown in Figure 4. 

![image](https://github.com/PierceWarburtonDS/IA_651_PierceWarburton_FinalProject/assets/148472871/a12839e5-b5df-4a88-8253-f30d20d187fb)


![image](https://github.com/PierceWarburtonDS/IA_651_PierceWarburton_FinalProject/assets/148472871/03425cec-4924-4232-9746-30689269dc13)

Thus the best Decision Tree model was found to have a depth of 3 levels and a minimum number of samples per leaf of 4. This final model had an accuracy of 72.41% on the test data and an accuracy of 72.67% on the train data. Its confusion matrix is shown below in Table 3.  

| | Predicted on Ground | Predicted in Tree|
--- | --- | --- |
**Observed on Ground** | 631 | 2 |
**Observed in Tree**| 243 | 12|


Interestingly the Ground Predictions are much more prevelant than the Tree predictions. In other words the model almost never predicts that the squirrel is in a tree. Now the data itself is about 30% Tree Observations and 70% Ground Observations but the bias seen in this model is much more pronounced. The model is giving a split of about 2% Tree Observations and 98% Ground Predictions. However when the data is augmented to even out the Tree/Ground split (the Tree observations are simply appended repeated giving a 43% to 57% split) this prediction bias does change. A new model can be found with an optimal depth of 10 and an optimal minimum sample per leaf number of 5 but the accuracy drops to 63%. For a binary classification problem 63% is just barely above random guessing, hardly a convincing value. The confusion matrix at least looks a little better with a split of 73% Ground Predictions and 27% Tree Predictions. 

| | Predicted on Ground | Predicted in Tree|
--- | --- | --- |
**Observed on Ground** | 708 | 141 |
**Observed in Tree**| 413 | 259|


To try and combat this bias towards Ground Predictions I looked at a random forest with a depth size of 10 for each tree and 2000 trees. However that model converged to a very similar solution as the solitary Decision Tree had. An accuracy of 63% and a Ground/Tree split of 72.5% to 27.5%. Hardly an improvement! My only conclusion then is that a squirrels behavior as characterized by these 10 categories can not accurately describe its location. In other words a squirrel will preform these 10 activities equally commonly whether it is on the ground or in a tree. Whether this is due to how common these 10 behaviors are to squirrels or because squirrels simply act randomly with little concern to their present location I can not say. My suggestion in the use of this model then would be to use it as an example of how squirrel behavior is not determined by it being in a tree or not. Furthermore with the original model in mind with the more extreme Ground/Tree data split I would suggest that instead of spending time analyzing squirrel behavior simply guess that it is currently on the ground and you will have a better outcome than if you flipped a coin. 

What could've improved this model would definitely be better data and more of it. 3000 rows may seem like a lot at first glance but clearly even with 10 features it was not enough to discern a difference in this case. More specific behaviors with more observations might aid in the training of a more accurate Decision Tree that doesn't just select the same prediction every time. 

