# GTD - Classification of Terrorist Groups

<img width="1438" alt="Attacks Plotted By Location" src="https://user-images.githubusercontent.com/67343138/149366581-bc5e3855-2c76-410b-bb76-e64e338ad93b.png">

According to statistics from the Global Terrorism Database, more than 200,000 terrorist attacks have been recorded from 1970 to the present day. Attacks typically involve multiple injuries, deaths and directly cause massive property losses. In addition, they bring tremendous psychological fear and anxiety to the population.

The analysis and prediction of terrorist groups would provide valuable information for antiterrorism and terrorism prevention operations, enabling authorities to assign attacks whose organisation are currently marked as ‘unknown’ and make them accountable.

This project was conducted as the final Capstone in the 3-month Data Science Immersive at General Assembly.
_________

## Contents 
- [Hypothesis](#hypothesis) 
- [Data Source](#data_source)
- [Data Cleaning](#data_cleaning)
- [Feature Engineering](#feature_engineering)
- [Exploratory Data Analysis](#EDA)
- [Pre-Processing](#pre-processing)
- [Modelling](#modelling)
- [Challenges](#challenges)
- [Conclusion](#conclusion)
- [Key Learnings](#key_learnings)
- [Limitations](#limitations)
- [Future Work](#future_work)
- [Libraries](#libraries)
- [Contact](#contact)
_______________
<a name="hypothesis"></a>
### Hypothesis

We will be able predict the ‘terrorist group’ who performed an attack, with an accuracy score higher than baseline.

The classification models used will take the characteristics of each attack and predict who was responsible using the data provided.

_______________
<a name="data_source"></a>
### Data Source

The Global Terrorism Database (GTD) is an open-source database including information on terrorist attacks around the world from 1970 through 2017. The GTD includes systematic data on domestic as well as international terrorist incidents that have occurred during this time period and now includes more than 180,000 attacks. 

The database is maintained by researchers at the National Consortium for the Study of Terrorism and Responses to Terrorism (START), headquartered at the University of Maryland.

Geography: Worldwide

Time period: 1970-2017, except 1993

Unit of analysis: Attack

Variables: >130 variables on location, tactics, perpetrators, targets, summaries and outcomes

Sources: Unclassified media articles 

(Note from data source: ‘Please interpret changes over time with caution. Global patterns are driven by diverse trends in particular regions, and data collection is influenced by fluctuations in access to media coverage over both time and place.’)

<img width="780" alt="GTD - Definition" src="https://user-images.githubusercontent.com/67343138/149366026-0685bd43-be22-4e99-b67e-40d634d18d6e.png">
<p align= "right"><a href="https://start.umd.edu/gtd/">Source</a></p>

_______________
<a name="data_cleaning"></a>
### Cleaning The Data

As the data was found in csv format and fairly readable, the cleaning was focused on:

- Applying correct dates to the events
- Finding missing data, collating information from the whole dataset
- Finalising the target variable
- Assigning correct data types
- Creating new features from data such as ‘related’ (detailing number of connected events)

If data could not be found through any of the processes above, either from the written summary column, feature engineering, calculations on other columns or using tools such as Geopandas for location - the cells were removed.

Columns I felt were unneccessary such as ‘dbsource’ (noting the media source of data), ‘addnotes’ (which gave written descriptions of the events) often with duplicated information and ‘motive’ which I felt held a subjective bias were removed, along with columns that had a large quantity of null values.

_______________
<a name="feature_engineering"></a>
### Feature Engineering

The data set started with 135 features, all detailing characteristics of an event. After cleaning and analysing the data set the columns that remained (before pre-processing) are shown in the table below.

<img width="718" alt="Columns Table 1" src="https://user-images.githubusercontent.com/67343138/149366773-1fd77b1b-a5c8-4cb5-8b50-eae642fcfa01.png"><img width="718" alt="Columns Table 2" src="https://user-images.githubusercontent.com/67343138/149366790-dc7cdabd-4647-4261-b34c-4937c410b819.png"><img width="718" alt="Columns Table 3" src="https://user-images.githubusercontent.com/67343138/149366800-739c1c13-aabc-4c56-918a-783207b3c002.png">

_______________
<a name="EDA"></a>
### Exploratory Data Analysis

The EDA was extensive, first evaluating the covariances and correlations in Pandas dataframe, then visually representing this with a heatmap.

From there I used Matplotlib and Seaborn to identify trends in the categorical data - using pairplots, catplots, pointplots and boxplots, such as Graph 1 below, which shows trends in the 32 terrorist organisations ‘attacktype’, ‘targtype’, ‘weaptype’ and ‘country'. With an overview of the plot it is interesting to see that for attack, target and weapon there is an overall trend that the 32 groups follow, but for country it is much more individual - pertaining to a stronger individualised connection.

Graph 1 : Seaborn Pairplot of Features per Group
<img width="760" alt="Seaborn Pairplot" src="https://user-images.githubusercontent.com/67343138/149365924-29415542-fb4f-4a48-97dd-19fa6f509d3b.png">

Once data had been filtered using the ‘groupby’ function I used bar plots and line graphs to show more detailed relationships such as ‘gname’, ‘attacktype’ and ‘success rate’. I also used Geopandas to plot the attacks globally.

Graph 2: Geopandas Global Map of Attacks
<img width="1726" alt="map - attacks" src="https://user-images.githubusercontent.com/67343138/149361516-555d7322-7af3-4ce1-aad7-024e2e0eb098.png">

My final EDA step was using Tableau, firstly creating maps of the events across the globe, by success rate and by number of kills. Then combining these maps with charts, such as counts of which country each group has attacked onto a dashboard. Graph 3 is a great representation of how Tableau can help to visualise data clearer, showing more in depth data in a visually appealing way - what we can see here is that ‘Hijacking’ is a much rarer occurrence, ‘Bombing/Explosion’ and ‘Armed Assault’ are much more likely in an attack and although Iraq shows a higher amount of attacks using ‘Bombing/Explosion’ it is Afganistan and then Peru that appear more frequently across all attack types.

Graph 3: Tableau Plot of Attack Type per Country
<img width="771" alt="Tableau Attack Type Plot" src="https://user-images.githubusercontent.com/67343138/149365840-f8ee9849-57f0-4394-af40-a61cc5344e2c.png">

Once I had reviewed all the different relationships it appeared to me that ‘Country’ may provide the most insight into which group had performed the attack.

_______________
<a name="pre_processing"></a>
### Pre-Processing

After preliminary tests were run and high test and training scores were returned several steps were taken to reduce overfitting and check for data leakage with differing subsets of data. Both PCA and Chi-Squared tests were run to work with the most suitable subsets of data.

<img width="946" alt="Screenshot 2022-01-07 at 01 38 47" src="https://user-images.githubusercontent.com/67343138/149361714-7ccc709f-44b4-479d-be00-ef168264424f.png">

_______________
<a name="modelling"></a>
### Modelling

Data was fed into multiple classification models, firstly without hyperparameter tuning, providing results from 38% to 94% accuracy (against the baseline of 12%).

The models were then tuned using a pipeline, including SMOTE (to address the imbalance in the data), gridsearch and cross-validation methods (to prevent overfitting).

The data was standardised using a StandardScaler and divided into train and test splits (80 / 20).

Once it was established that the Random Forest Classifier would still perform best after tuning models this model was used with the varying subsets of data.

Graph 4: Model Results After Fine-tuning
<img width="777" alt="Model Results" src="https://user-images.githubusercontent.com/67343138/149365611-b1abca21-4dd1-4ce6-87cc-038f574d062d.png">

Graph 5: Feature Importances From Best Performing Model
<img width="777" alt="Feature Importances" src="https://user-images.githubusercontent.com/67343138/149365529-ab037ea4-27b7-4767-89e4-8cd690a60cfd.png">

_______________
<a name="challenges"></a>
### Challenges

I faced an unexpected challenge of high train and test scores in my modelling. In order to gain confidence in my results I performed several ‘sense checkers’.

The models were evaluated using classification reports, confusion matrices, precision-recall curves, auc-roc curves, graphviz trees and the result of dropping multiple / single features (in order of feature importance). 

Graph 6 shows the model struggled to predict with the least represented groups. For example, the precision and recall line for the Abu Sayyaf Group (ASG), is much lower than than the other better predicted groups.

Graph 6: Precision-Recall Curve From Best Performing Model
<img width="793" alt="Precision-Recall Curve" src="https://user-images.githubusercontent.com/67343138/149364309-0e1e6f1f-b32e-4d74-859e-461fc8a19e07.png">

I also used networkx.algorithms to create a bipartite network between country and terrorist group to explain some of the incorrect predictions made by the model.

I also wanted to check the individual columns were not causing an overfit or giving away too much to the model, so I created a function to drop each column and record the test and training scores (dropping in order of how important the model told us the feature was).

Graph 7 shows the decline in accuracy scores after dropping multiple columns. What I was looking to see is if the test and training scores have a high variance, suggesting overfitting. However we only see a change after dropping 15 columns, which is to be expected with much less data for the model to run on.


Graph 7: Change In Training and Test Scores With Each Column Drop
<img width="782" alt="Drop - Train_Test" src="https://user-images.githubusercontent.com/67343138/149364204-d4c6d5d8-6efe-470b-bddd-69e392cd18f9.png">

_______________
<a name="conclusion"></a>
### Conclusion

The hypothesis was correct, I was able to predict the name of the terrorist organisation who performed an attack from the data provided. The baseline score or ‘best guess’ was a 12% accuracy, my model predicted at a 95% accuracy.

Our EDA findings were also reflected in our feature importances as in order to predict who performed the attack the model found that ‘Country’, ‘Year’ and ‘Extended’ (whether or not the duration of an incident extended more than 24 hours) were the most important characteristics, so this again added confidence to my model.

_______________
<a name="key_learnings"></a>
### Key Learnings

#### Data Science Workflow

This project gave me hands-on experience on the workflow needed within data science. I learnt of the importance, and time required, for cleaning and pre-processing the data ready for EDA. I also found that exploring relationships and visually displaying them was really engaging to me, but that time management was also a key part of the workflow and as much of the work should be related back to the hypothesis as possible.

#### Subjectivity of Data

One of the most notable reflections of the project was the presentation itself made to my fellow students. I was conscious of the subject matter I had chosen throughout the project and took as many steps as possible to see the data for what it was, to look for patterns and relationships in the data and have these results guide my findings, avoiding any bias or subjective judgement. Once I had presented these findings, the questioning received by my fellow students gave me a real insight into story-telling and the depth of knowledge needed around the subject matter in order to portray the best story possible.

_______________
<a name="limitations"></a>
### Limitations

#### Global Data Missing

While the core data set being used in this project is very comprehensive, there are gaps in the worldwide data. For example there is no data from Korea.

#### Bias
Details of each event / attack are sourced from various different media sources, each of which will have differing levels of: process standards, political leanings, data quality assessments and oversight. This means that to a certain extent there will always be a bias inherent in the data.

_______________
<a name="future_work"></a>
### Future Work

In order to expand this project further I would like to take the following next steps:
 
#### Time Series Model
 
I would like to apply a time series model to the data for ‘number of kills’, to see if we are able to find trends in the dates as to when larger and more deadly attacks are more likely to happen.

#### Network Expansion

I have created a network between country and group name and the correlations were really clear. I would like to take this one step further and apply to other features in the data, potentially creating a multi-layered network.

#### Neural Networks

I would also like to apply a neural network such as Tensorflow to the data to see what results are produced when predicting the organisation name.

_______________
<a name="libraries"></a>
### Libraries Used

- Numpy
- Pandas
- SciPy
- Seaborn
- Matplotlib
- Geopandas
- Pydot
- SciKit-Learn
- Tableau
- Networkx

_______________
<a name="contact"></a>
### Contact
 
If you would like to reach out please feel free to contact me at <a href="https://www.linkedin.com/in/katie-stamp/">linkedin</a>.
