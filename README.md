NFL 1st and Future – Analytics - (View pdf)
For dataset: https://www.kaggle.com/c/nfl-playing-surface-analytics

FEATURE IMPORTANCE, FEATURE INTERACTION, FACTOR ANALYSIS AND RECURSIVE FEATURE ELIMINATION
ABSTRACT

This review tries to examine the effects of factors including playing surfaces on player movements and by extension, their concurrence with lower extremity injuries.
The analysis uncovers specific features and feature interactions which combined with player movement present ideal conditions for risk of injury.
The analysis concentrates on game conditions instead of play conditions, as a quarter of the injury data has no identifying play key. The choice to concentrate on game conditions shows better predictive quality across different models than play conditions.
Our analysis discovers that the specific set of factors that influence lower extremity injuries are mutually exclusive to types of injuries.
Our analysis also reinforces the idea that synthetic playing surfaces play a major role in occurrence of these injuries.
The analysis does not discredit player movements with contributing to injuries, however it proposes that the information given is inadequate as we cannot confirm at what time point in the duration of the play that the injury occurred. Therefore, our representation of speed, directional changes, acceleration and distance produced no predictive qualities.
The analysis is bi-directional: where features are found to promote risk, we look for ways to reduce or eliminate those factors. Where features are found to have low correlation to risk, we look for ways to magnify or encourage their occurrence.
INTRODUCTION
Our analysis examined eight crowd-sourced hypotheses:
1. Special teams : That players on special teams were more prone to injuries
2. Injuries during practice/preseason : That players were prone to more injuries during
practice as there are more practice sessions that regular season games
3. Backups more prone to injury:  That substitutes were more prone to injuries
4. Coaching decisions:  That coaching decisions have an influence on injuries
5. Coming back from injury/% healthy:  That players were more prone to injury
immediately after coming back from injury
 6. Injured in cold temperatures/more start/stops:  That ‘slower’ games have a higher prevalence of injury
7. Same injury, more severe recovery time on synthetic:  That synthetic turf not only had a higher prevalence of injuries but a higher prevalence of more severe injuries
8. Less injuries during wet weather:  That the effect of natural versus synthetic turf is eliminated during games with rain or snow.
9. Games with multiple injuries:  That there are games with perfect conditions to cause multiple injuries.

 
The dataset was then split into three sets:
1. Training dataset (80%)
2. Test dataset (20%)
a. Evaluation dataset (10%)
b. Inference and Scoring (10%)

MODELS AND IMPLEMENTATION Our binary classification algorithm:
1. We create two datasets: baseline data and the feature engineered dataset.
2. Select individual injury to be analyzed (Features affecting different injuries are exclusive
– see Data Augmentation section)
3. Split dataset into training, evaluation and prediction set.
4. Weighted parameters are set for each class to address class imbalance in the dataset
5. Where available, L1 and L2 regularization is set for feature selection
6. Run prediction model
7. Use fitted model to predict on prediction dataset
1. Calculate scoring on prediction dataset labels vs predictions (precision, recall, fscore )
8. Calculate confusion matrix
9. Calculate feature importance/feature interaction

Three models were selected, each with their own strengths and weaknesses in binary classification.
Catboost was selected as it does not need one-hot encoded categorical features, which allows for easy feature engineering by mixing numerical features and string features. Catboost allows for the feature importance, feature interaction importance, hyperparameter grid search and object importance. This model was used to rank feature importance as well as calculate what pairwise feature interactions had the best predictive qualities.
LightGBM was selected as a control for the CatBoost model as it implements the same gradient boosting algorithm and is faster. This model was used to countercheck catboost feature importance and used for one step recursive feature elimination.
Random Forest was selected as it implements a different algorithm and therefore can be used as a comparison to CatBoost/LightGBM to triangulate the answer as well as countercheck. It was primarily used for feature importance.
 
 DATA AUGMENTATION
The idea was to augment the small injury dataset by duplicating knee and ankle injuries data and reversing the injury label. This was based off Player with PlayerKey 47307 who managed to injure his knee and ankle on the same play in the same game. Since the conditions that led to the double injury were similar, the assumption was that conditions affecting knee and ankle injuries are common.
However, running a binary classification on the augmented dataset produced abnormally high log-loss scores i.e. 0.35 while non-augmented data achieved log-loss scores of 0.05. This proves that these two injuries had mutually exclusive sets of features/conditions affecting their occurrence.
Therefore, any analysis should consider all the injuries individually.
Fig 4:
* Data augmentation contained in the function called daugment() but having poor predictive quality, they are commented out in the notebook.
