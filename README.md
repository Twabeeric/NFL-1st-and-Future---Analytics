NFL 1st and Future – Analytics
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
DATA SET AND FEATURE ENGINEERING
Characteristics of the dataset included InjuryRecord missing 28 PlayKeys, PlayerTrackData missing 74,526,875 event values and PlayList missing 16910 StadiumType, 18691 Weather and 367 PlayType values. This is consequential because tree models have a selection bias against columns with missing values and imputed values might skew the analysis. We note the columns as they are likely not to depended on for analysis.
First step to handling the data was to free up memory by downgrading the datatypes for all data to make it easier to fit into memory.
All the datasets were merged by common primary keys and sliced for the row representing the termination of the play (where time on the play is maximum).
Fig 1: PlayList
    Feature
  Definition
     IsWet
   Is it raining or snowing? [0,1]
     IsSunny
   Is it sunny or clear? [0,1]
     IsCloudy
   Is it cloudy or overcast? [0,1]
     IsSnow
   Is it snowing? [0,1]
     IsControlled
   Is temperature controlled? [0,1]
     IsDomeOpen
   Is dome/bowl with roof open? [0,1]
     IsDomeClosed
   Is dome/bowl with roof closed? [0,1]
     IsIndoor
   Is played indoor? [0,1]
     IsOutdoor
   Is played outdoor? [0,1]
     IsStadiumUnkown
   Conditions unknown
     PlayerDay0
   Sequences player day participation in a 116-day season
     Preseason
   Signify [0,1] games played prior to the regular season
     Temperature0
   Binned temperature feature in 10-degree bins from zero
     Weather0
   Bucketing weather into snow, rain, clear, cloudy, unknown
 StaduimType0
   Bucketing stadiumtype into indoor, outdoor, dome open, dome closed
 Fig 2: InjuryRecord
    Feature
  Definition
 BodyPart
   String for which bodypart was injured
 
     injuryKnee
  Binary [0,1] for knee injury
     injuryAnkle
   Binary [0,1] for ankle injury
     injuryHeel
   Binary [0,1] for heel injury
     injuryToes
   Binary [0,1] for toe injury
     injuryFoot
   Binary [0,1] for foot injury
 RecoveryTime
   Converts the one-hot encoded days for recovery into a numerical feature.
 Fig 3: PlayerTrackData
    Feature Definition
      v_horizontal Calculates angular speed on x-axis
      v_vertical Calculates angular speed on y-axis
      x_vdiff_f1delta Change in x over change in time
      x_vdiff_adiff_f2delta Change in change in x over change in time
      y_vdiff_f1delta Change in y over change in time
      y_vdiff_adiff_f2delta Change in change in y over change in time
      dis_vdiff_f1delta Change in dis over change in time
      dis_vdiff_adiff_f2delta Change in change in dis over change in time
      dir_vdiff_f1delta Change in dir over change in time
      dir_vdiff_adiff_f2delta Change in change in dir over change in time
      o_vdiff_f1delta Change in o over change in time
      o_vdiff_adiff_f2delta Change in change in o over change in time
      s_vdiff_f1delta Change in s over change in time
      s_vdiff_adiff_f2delta Change in change in s over change in time
      x_vdiff_f1delta_max Quantify extremes above group max
      x_vdiff_adiff_f2delta_max Quantify extremes above group max
      y_vdiff_f1delta_max Quantify extremes above group max
      y_vdiff_adiff_f2delta_max Quantify extremes above group max
      dis_vdiff_f1delta_max Quantify extremes above group max
      dis_vdiff_adiff_f2delta_max Quantify extremes above group max
      dir_vdiff_f1delta_max Quantify extremes above group max
      dir_vdiff_adiff_f2delta_max Quantify extremes above group max
      o_vdiff_f1delta_max Quantify extremes above group max
      o_vdiff_adiff_f2delta_max Quantify extremes above group max
      s_vdiff_f1delta_max Quantify extremes above group max
      s_vdiff_adiff_f2delta_max Quantify extremes above group max
      x_vdiff_f1delta_std Quantify extremes beyond group std
      x_vdiff_adiff_f2delta_std Quantify extremes beyond group std
      y_vdiff_f1delta_std Quantify extremes beyond group std
      y_vdiff_adiff_f2delta_std Quantify extremes beyond group std
      dis_vdiff_f1delta_std Quantify extremes beyond group std
      dis_vdiff_adiff_f2delta_std Quantify extremes beyond group std
     dir_vdiff_f1delta_std Quantify extremes beyond group std
  
     dir_vdiff_adiff_f2delta_std
  Quantify extremes beyond group std
     o_vdiff_f1delta_std
   Quantify extremes beyond group std
     o_vdiff_adiff_f2delta_std
   Quantify extremes beyond group std
     s_vdiff_f1delta_std
   Quantify extremes beyond group std
 s_vdiff_adiff_f2delta_std
   Quantify extremes beyond group std
 * Mean and standard deviation for speed, distance, acceleration features are contained in the function called createstat() but having poor predictive quality, they are commented out in the notebook.
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
ANAL YSIS
1. INJURIES IN GENERAL
Factor Analysis:
Factor Analysis can be used as a form of exploratory feature analysis by dimension reduction.
Factor analysis was done to discover hidden relationships between features by extracting maximum common variance. After calculating eigen values, we create 13 factors which explain 0.6867 of the total variances. Even after calculating for 46 factors, the total explained variance was approximately 0.6521.
Fig 5:
       Playe rKey
   GameID
 PlayKey
     Bod yPar t
 Surface
  D M_ M1
     D M_ M7
   DM _M 28
   DM_ M42
  Recovery Time
     86
    47307
    47307-10
 47307-10-18
      Knee
 Syntheti c
   1
      1
    0
    0
   7
 87
    47307
     47307-10
   47307-10-18
   Ankl e
  Syntheti c
   1
  1
    0
     0
    7
 
 We then calculate the features that had the highest loadings on each of the factors and assume that these factors are linearly related to a smaller number of unobservable factors. The three top features for each loading is shown below:
Factor 0: dis_vdiff_adiff_f2delta, s_vdiff_adiff_f2delta , s_vdiff_f1delta (Cum. Var 0.1608) Factor 1: v_horizontal , v_vertical , o_vdiff_adiff_f2delta (Cum. Var 0.2414)
Factor 2: IsIndoor , FieldType , IsDomeOpen (Cum. Var 0.3058)
Factor 3: Position, PositionGroup , RosterPosition (Cum. Var 0.3556)
Factor 4: x_vdiff_adiff_f2delta, x_vdiff_f1delta , y_vdiff_f1delta (Cum. Var 0.4002) Factor 5: IsSunny , Weather , Temperature0 (Cum. Var 0.4419)
Factor 6: StadiumType , IsOutdoor , IsIndoor (Cum. Var 0.4802)
Factor 7: PlayerDay0, PlayerGame , IsSnow (Cum. Var 0.5137)
Factor 8: PlayType , PlayerGamePlay , PlayerGame (Cum. Var 0.5459) Factor 9: IsWet , IsSnow , Weather (Cum. Var 0.5741)
Factor 10: s, dis, s_vdiff_adiff_f2delta (Cum. Var 0.5986)
Factor 11: dir , IsDomeOpen , StadiumType (Cum. Var 0.6200)
Factor 12: IsDomeOpen , StadiumType , Temperature0 (Cum. Var 0.6397) Factor 13: y, IsDomeOpen , IsStadiumUnknown (Cum. Var 0.6521)
