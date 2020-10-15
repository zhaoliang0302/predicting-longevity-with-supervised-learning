# predicting-longevity-with-supervised-learning
Supervised learning models to predict telomere length from the CDC's NHANES data

**Introduction and Background:** Telomeres are caps of protective DNA at the ends of chromosomes. These caps prevent the loss of the ends of the chromosomes and spontaneous end joining. Telomeres shorten with each cell division, and the loss of DNA sequence eventually leads to cell senescence and death. Short telomeres have been associated with aging and disease; therefore, telomere length may be considered a biomarker for aging. Is there any way to maintain telomere length and what some strategies for slowing the process of telomere shortening? Physical activity has been associated with delayed aging and may serve as a mechanism for maintaining telomere length. As evidence of this hypothesis, longer telomeres have been found in immune cells of people who engage in regular, moderate exercise. Further, exercise has been found to increase the activity of telomerase, an enzyme that maintains telomere length, in mouse myocardium. 

**Hypothesis and Plan:** For this study, I hypothesized that specific factors associated with physical activity and physical fitness would predict telomere length. For example, can VO2 max, a measure of cardiovascular fitness, predict telomere length? What combination of factors are most predictive of telomere length? Determination of such factors could help scientists direct research to understanding specific mechanisms of aging and longevity. It could also help individuals optimize health by prioritizing their efforts to specific, effective behaviors. To test this hypothesis, my general plan was to use supervised machine learning algorithms to determine factors most predictive of telomere length.

**Data:** For this analysis, I used the National Health and Nutrition Examination Survey (NHANES) dataset from the Centers for Disease Control and Prevention (CDC) from the year 1999-2000. Continuous NHANES refers to the survey after 1999, when the survey moved to continuous data collection in two-year cycles. The sample for each two-year cycle is representative of the non-institutionalized, U.S. population. These surveys were designed to assess the health and nutritional status of adults and children in the United States. The survey is unique in that it combines interviews and physical examinations. The NHANES interview data includes demographic, socioeconomic, dietary, and health-related questions. The examination component consists of clinical and physiological measurements, as well as laboratory tests. All data were numerical. The dataset is interesting to me because, as an exercise scientist and molecular biologist, I am interested in specific physical fitness tests and related factors and their relationships to each other that might influence molecular and cellular mechanisms of human health.

Data from ten separate files from the period of 1999-2000 were downloaded in SAS and converted to CSV. I selected columns from tables of most interest to my research question: Demographics, Balance Testing, Cardiovascular Fitness Testing, Muscle (Quad Extension) Strength Testing, Balance Questions (Q), Cardio Health Qs, Current Health Status Qs, Physical Activity Qs, Physical Activity Individual Activities Qs, and Physical Functioning Qs. The tables were merged using Python left join, resulting in 3570 records. During this process, data were scrutinized for removal of duplicate values, nonsense values, and features not relevant to the analysis, such as temperature of the room during tests, size of blood pressure cuffs, and questions specific to presence of congestive heart failure. 

**Methods:** Nulls were prevalent throughout the dataset due to the join; not all participants were tested for all measures. At first, NaN values were replaced with “0” because the researchers developing the dataset had used 0 for missing values. However, models were not performing well, and this strategy was later abandoned. Imputation was considered, but this strategy was not possible due to the nature of the survey questions and tests. Instead, features with over 77% missing data were dropped. Next, remaining features were carefully examined for relevance, and features least relevant to the hypothesis being tested were dropped. Finally, it was discovered that the cardiovascular and muscular fitness tests for this cohort of individuals were incompatible; i.e., these individuals were tested for one or the other, but not both. The decision was made to drop the muscular measures because this method would preserve the most data.

The data were not normally distributed, and because one telomere value was so high and the assumptions of linearity were not being met, the data were capped using winsorization and transformed using Box-Cox initially. Models were tested with and without these adjustments, and based on model performance, a decision was made to use winsorization alone.
Data were split into training and test sets for the analysis, and then models were systematically down selected by the following process. First, multiple machine learning regression models were used to analyze the data using five-fold cross-validation, including Ordinary Least Squares, Ridge, Lasso, ElasticNet, K Nearest Neighbors (KNN), Decision Tree, Random Forest, Support Vector Machine, and Gradient Boosting. After running these models, a subset of features was selected to attempt to optimize model performance. SelectKBest from sklearn and recursive feature selection were used to select the top ten features and reinterrogate the models. Models were compared using the following evaluation metrics: mean absolute error (mae), mean squared error (mse), root mean squared error (rmse), mean absolute percentage error (mape), and average cross validation scores. Finally, the best models were selected for further optimization.

**Results:** The final cleaned dataset was comprised of 154 features, 1837 records, and the target variable, mean telomere length. Because assumptions of linearity were not met (tests indicated heteroscedasticity), the models requiring these assumptions for performance did not fit well and were later abandoned. Use of the full feature set for analysis resulted in models that performed slightly better than the selected feature set, so the entire dataset was used. The training R2 scores for the KNN, Decision Tree, and Random Forest models were the highest, and of these three models, the Random Forest had the lowest error measures. The Random Forest model was selected for further optimization using a grid search to tune the model hyperparameters. No additional improvements could be made using this method. The features most important for making the predictions included: age of participant in months (top two), daily hours of TV/computer use, moderate activity in the last 30 days, muscle strengthening activities in the last 30 days, how long were moderate activities conducted during each bout, and warm up heart rate.

**Discussion, Recommendation, and Future Work:** Based on these results, I concluded that the most important features for the model to make its telomere length predictions were the ages of the participants. This result makes sense because telomeres shorten with age. The physical fitness features were not as important to the model; however, two features did have some impact: daily hours of TV/computer use and how often the participant engaged in moderate physical activities in the last 30 days. These results are interesting because they suggest that people do not need a high level of physical fitness to increase their health and longevity. Rather, moderate intensity activities performed consistently and avoidance of sedentary behaviors may make the most impact. It should be noted that these results include only the features of particular interest to this study. It is probable that diet and other behaviors as well as chronic illnesses may contribute heavily to telomere length and perhaps explain more of the variance in telomere lengths than physical fitness. Further study with an expanded set of features would be interesting. Another point to note is that this study does not explain causation, although the literature suggests that exercise does directly impact telomere length. Based on this analysis, as well as the advice of national organizations, such as the U.S. Department of Health and Human Services and the American College of Sports Medicine, the recommended actions would be to limit sedentary time and engage in regular, moderate activity to increase health and longevity. 

**Contents of this Repository:** A Jupyter notebook containing the Python code I used to perform the analyses and a slide presentation describing the experiment.

