# Assignment 1
> data: [Cardiovascular Diseases Risk Prediction Dataset](https://www.kaggle.com/datasets/alphiree/cardiovascular-diseases-risk-prediction-dataset?resource=download)

## Notes 
### Suggestions from Eric 
- Also consider use clustering with few prominent attributes, to find a cluster that is highly correlated to `heart_disease`.
- Focus on `recall`, even overall `accuracy` drops a bit.
- [weka.classifers.meta.CostSensitiveClassifier](https://www.youtube.com/watch?v=V9PNyx5-kxM): "While it can serve as an additional method, it should not be the sole approach (not covered in course material)"

### Data mining process model (lecture 1.3)
- PDCA (plan, do, check, action)
- CRISP-DM (cross industry standard protocol for data mining)
<center>
    <img src="./image/CRISP-DM_Process_Diagram.png" width="400" />
</center>

### Key Steps
1. What is the business goal(objective)?
2. Collation of data 
    - Data visualization
    - Data collection, preprocessing, reduction and transformation
3. Model selection
    - Classification / regression / ARM / Clustering, etc.
    - Evaluate model -> interesting insights / knowledge?
4. Actionable insights
    - Present insights
    - Return on investments (ROI)
    - Fine-tune model with new data


## Steps
### Data Preprocessing
#### 1. Select Class Attribute
The dataset we have is of decent quality overall. All attributes have 0% missing variables.

However, upon opening the CSV file, we found that the data is not structured in an optimal way for our analysis. Specifically, Weka automatically taking the last attribute as the class attribute, but for better visualization and understanding of this dataset, we wanted to manually set 'sex' as the class attribute instead. 

<center>
    <img src="./image/weka-before-preprocessing.png" width="500" />
</center>

`Edit` $\rightarrow$ right click on `sex` $\rightarrow$ select `Attribute as class`

<center>
    <img src="./image/weka-after-preprocessing.png" width="500" />
</center>

By setting 'sex' as the class attribute, we are able to get more comprehensive visualization and insights from the dataset regarding how Cardiovascular diseases are distributed across genders.

#### 2. Use IQR to remove outliers and extreme values
 `preprocess` $\rightarrow$ `filter` $\rightarrow$ `filters/unsupervised/attribute/interquartileRange` $\rightarrow$ `apply`

Outliers (258141 vs 50713):
<center>
    <img src="./image/iqr-outlier.png" width="500" />
</center>
Extreme Values (306318 vs 2536):
<center>
    <img src="./image/iqr-extreme-values.png" width="500" />
</center>

 `preprocess` $\rightarrow$ `filter` $\rightarrow$ `filters/unsupervised/instance/removeWithValues` $\rightarrow$ `apply`

After removing outlier and extreme value data points, the total number of instances in our dataset decreased from 308854 to 256396. The removal of these distort data points helped optimize the  skewness of attributes such as `weight`.

Weight before adjustment:
<center>
    <img src="./image/weight-before.png" width="500" />
</center>

Weight after adjustment:
<center>
    <img src="./image/weight-after.png" width="500" />
</center>

#### 3. Remove Duplicates
Removed 79 duplicated instances
<center>
    <img src="./image/weka-remove-duplicates.png" width="500" />
</center>

#### 4. Attribute Selection
`Supervised` $\rightarrow$ `attribute` $\rightarrow$ `AttributeSelection`
Narrow down attributes to 4 most closely associated to heart disease.
<center>
    <img src="./image/AttributeSelection.png" width="500" />
</center>

#### 4. Class Balancing
Balance heart_disease class, so the ratio between surveyees who have heart disease and these who don't is 1: 1.
<center>
    <img src="./image/weka-undersample.png" width="500" />
</center>


#### More preprocessing ideas to explore
- [x] numeric transformation: `Discretize`
- [x] resample instance: `SpreadSubsample`
- [ ] ? normalize data

### Key Discovery
- General Health: very good -> good -> excellent -> fair -> poor 
    - most people does not realize their cardiovascular health: the largest percentage group is from 'good'
- The majority heart disease patients check up 'within past year' (they visit hospital more often?)
- Doing exercise does not decrease the risk of heart disease
- Skin or Other cancers, Depression, Diabetes all have weak correlation with cardiovascular health
- People having Arthritis are more likely to also have heart disease
- Male are more likely
- Age 55+ are significantly under greater risk
- Height, Weight, BMI do not impact the chance of having heart disease
- People who smoke are more likely
- Alcohol, Fruit, Green Vege, Fried Potato do not affect



### Clustering
Significant portion of our attributes(have or don't have heart disease) are nominal data, clustering does not apply to them.

Clustering algorithms such as k-means work best with numerical data.


#### K-Means clustering
Using clustering analysis, we observed the following: individuals in the top 50th percentile (>64) for green vegetable intake demonstrated less likelihood of hearth disease compared to those with lower green vegetable consumption.
<center>
    <img src="./image/heart-vs-greenvege.png" width="500" />
</center>

### Classification
- Preprocessing
    - remove unwanted attributes
    - discretize 7 attributes(height, weight, BMI, alcohol/fruit/greenVege/friedPotato consumption) into 8 bins
#### One R
Result:
<center>
    <img src="./image/one-r.png" width="500" />
</center>

#### J48 Decision Tree
Result:
<center>
    <img src="./image/j48.png" width="500" />
</center>
Tree visualization:
<center>
    <img src="./image/j48-tree.png" width="500" />
</center>
