# IA_651_PierceWarburton_FinalProject

The dataset examined in this project was of squirrel observations, done by volunteers, in Central Park during 2018. It is publicly available on NYCOpenData's website and can be accessed [here](https://data.cityofnewyork.us/Environment/2018-Central-Park-Squirrel-Census-Squirrel-Data/vfnx-vebw/about_data).
The data has 3023 unique observations each one describing a single squirrel's appearance, location, and general behavior. A single observation will have anywhere between 22 and 31 features accompanying it. The variability is due to different volunteers omitting to add various features to their logged observation, a mistake that can be hardly rectified in post. The purpose of this project however was to predict a squirrels location (on the ground or in a tree) through the observations of their behavior. While seemingly niche this analysis could serve to help both amatuer and professional individuals interested in squirrels better understand when they choose to act in certain ways and how the safety of trees vs the dangers of ground influence their activities 

The first step of building this predictive model was to analyze the variables in the dataset. To this end all the distributions were plotted with Location encoded in color on the plot. This was done to examine by eye if any variables had obvious biases or splits between the two locations that could be helpful for the predictions. To plot this data each column was converted to an interger based on the columns unique values and then those values were graphed in a scatter plot. An example of this process is shown below in Figure 1 where each Combination of Primary and Highlight Color in the dataset is convert to a value between 0 and 21 based on the order of appearance for that particular combination (there being 22 combinations in total) and then plotted.

![image](https://github.com/PierceWarburtonDS/IA_651_PierceWarburton_FinalProject/assets/148472871/361ffdc9-9f74-422f-87b8-04ca8afc29fc)


The first step of the analysis was manipulating the data for later ingestion into various algorithms. The variable of interest as stated above was Location which was given as either "Ground Plane" or "Above Ground". This was changed to 0 and 1 respectively. The predictor variables were then choosen and added to a seperate dataset along with the augmented Location variable. The predictor variables were, with some exceptions, all the behavior features. Climbing and Foraging were discarded given both their obvious baised to ground or tree and the analysis of their relative distribution in respect to Location. See Table 1 below for exact values in both columns. 



Variable | Observations on Ground | Observations in Tree
--- | :---: | :---:
Climbing | 136 | 522
Foraging | 1270 | 165



Thus the final predictors used are shown below in Table 2 where their descriptions from the NYCOPenData page are paraphased.



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



The first step of building this predictive model 
