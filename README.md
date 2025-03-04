
# Predicting Pulsars with Decision Trees and XG Boost

![GitHub repo size](https://img.shields.io/github/repo-size/hakkeray/predicting-pulsars-with-decision-trees-and-xgboost)
![GitHub license](https://img.shields.io/github/license/hakkeray/predicting-pulsars-with-decision-trees-and-xgboost?color=black)

Predicting Pulsars with Decision Trees and XG Boost is a `supervised machine learning feature classification project` that uses `Decision Trees and XGBoost` to `predict and classify signals as either a pulsar or noise`.

![](./_93_1.png)

HTRU2 is a data set which describes **a sample of pulsar candidates collected during the High Time Resolution Universe Survey.**

Pulsars are a rare type of Neutron star that produce radio emission detectable here on Earth. They are of considerable scientific interest as probes of space-time, the inter-stellar medium, and states of matter.

As pulsars rotate, their emission beams sweep across the sky which produces a detectable pattern of broadband radio emission when crossing our line of sight. As pulsars rotate rapidly, this pattern repeats periodically. Thus pulsar search involves looking for periodic radio signals with large radio telescopes.

Each pulsar produces a slightly different emission pattern, which varies slightly with each rotation. Detection of a potential signal is known as a 'candidate', which is averaged over many rotations of the pulsar, as determined by the length of an observation. In the absence of additional info, each candidate could potentially describe a real pulsar. **However in practice almost all detections are caused by radio frequency interference (RFI) and noise, making legitimate signals hard to find.** Thus, legitimate pulsar examples are a minority positive class, and spurious examples the majority negative class.

![](./output/output_20_2.png)

The data set shared here contains **16,259 spurious examples caused by RFI/noise**, and **1,639 real pulsar examples**. Each row lists the variables first, and the class label is the final entry. The class labels used are 0 (negative) and 1 (positive).

Each candidate is described by 8 continuous variables, and a single class variable. The first four are simple statistics obtained from the integrated pulse profile (folded profile). This is an array of continuous variables that describe a longitude-resolved version of the signal that has been averaged in both time and frequency. The remaining four variables are similarly obtained from the DM-SNR curve.

    * Mean of the integrated profile.
    * Standard deviation of the integrated profile.
    * Excess kurtosis of the integrated profile.
    * Skewness of the integrated profile.
    * Mean of the DM-SNR curve.
    * Standard deviation of the DM-SNR curve.
    * Excess kurtosis of the DM-SNR curve.
    * Skewness of the DM-SNR curve.
    * Class

HTRU 2 Summary:

    * 17,898 total examples
            * 1,639 positive examples
            * 16,259 negative examples

## Prerequisites

Before you begin, ensure you have met the following requirements:

* You have installed the latest version of `Jupyter Notebook`
* You have a `<Windows/Linux/Mac>` machine.

## Running the Time Series Forecasting with SARIMAX and Gridsearch Project

To run this project locally, follow these steps:

In the command line/terminal:

```bash
git clone https://github.com/hakkeray/predicting-pulsars-with-decision-trees-and-xgboost
cd predicting-pulsars-with-decision-trees-and-xgboost
jupyter notebook
```

# Summary

## Pipeline

To begin the analysis, I used a pipeline to determine the most accurate models for predicting a pulsar. After performing Standard Scaling on the dataset, I split the dataset into train-test prediction models for Logistic Regression, Support Vector Machines, Decision Trees and XG Boost. All were fairly accurate, with Decision Trees and XG Boost topping the list for accuracy scores.

## Decision Trees

Following this, I proceeded with a Decision Tree classifier with balanced class weights, which did fairly well, scoring 96% accuracy. However, because of the imbalanced classes, the F1 score is the most important validator for model accuracy, and the Decision Tree classifier scored 82%.

## XGBoost

Moving on to XGBoost, I scored 98% accuracy with an 89% F1 score. I was able to successfully identify 466 pulsars, missing only 78 that we our model mistakenly identified as noise.

# RECOMMENDATIONS

     * Focus on Kurtosis Integrated Profile
 
     * Focus on Standard Deviation DM-NSR Curve
 
     * Validate model predictions with analysis of other celestial objects 
     producing cosmic rays to see if they show the same attributes.

# FUTURE WORK

1. Improving the model, trying other ways of scaling, balancing class weights.

2. Looking at stars right before they die - predicting whether or not it will become a pulsar or not (could be slightly impossible considering stars live for billions  of years…)

## Contact

If you want to contact me you can reach me at <rukeine@gmail.com>.

## License

This project uses the following license: [MIT License](./LICENSE.md).

```python
#         _ __ _   _
#  /\_/\ | '__| | | |
#  [===] | |  | |_| |
#   \./  |_|   \__,_|
#
```

---

## Process Outline

    * IMPORT PACKAGES + LIBRARIES
    
    * OBTAIN DATA
    
    * PRE-PROCESSING
    
    * EDA + VISUALIZATIONS
    
    * MODELING:
        * MODEL 1: DECISION TREES
        * MODEL 2: RANDOM FOREST
        * MODEL 3: XGBOOST
        * MODEL 4: GRIDSEARCH CV (ALL)
        
    * INTERPRET RESULTS
    
    * CONCLUSION + SUMMARY
    
    * FUTURE WORK


## Libraries

```python
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.pipeline import Pipeline
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression
from sklearn import svm
from sklearn import tree

from sklearn.tree import DecisionTreeClassifier 
from sklearn.metrics import accuracy_score,roc_curve,auc,average_precision_score,recall_score,precision_score,f1_score,classification_report
from sklearn.tree import export_graphviz
from IPython.display import Image  
from pydotplus import graph_from_dot_data
from xgboost import XGBClassifier
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import GridSearchCV
```

# Dataset

<html>
<body>
<div>
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mean of the integrated profile</th>
      <th>Standard deviation of the integrated profile</th>
      <th>Excess kurtosis of the integrated profile</th>
      <th>Skewness of the integrated profile</th>
      <th>Mean of the DM-SNR curve</th>
      <th>Standard deviation of the DM-SNR curve</th>
      <th>Excess kurtosis of the DM-SNR curve</th>
      <th>Skewness of the DM-SNR curve</th>
      <th>target_class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>140.562500</td>
      <td>55.683782</td>
      <td>-0.234571</td>
      <td>-0.699648</td>
      <td>3.199833</td>
      <td>19.110426</td>
      <td>7.975532</td>
      <td>74.242225</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>102.507812</td>
      <td>58.882430</td>
      <td>0.465318</td>
      <td>-0.515088</td>
      <td>1.677258</td>
      <td>14.860146</td>
      <td>10.576487</td>
      <td>127.393580</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>103.015625</td>
      <td>39.341649</td>
      <td>0.323328</td>
      <td>1.051164</td>
      <td>3.121237</td>
      <td>21.744669</td>
      <td>7.735822</td>
      <td>63.171909</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>136.750000</td>
      <td>57.178449</td>
      <td>-0.068415</td>
      <td>-0.636238</td>
      <td>3.642977</td>
      <td>20.959280</td>
      <td>6.896499</td>
      <td>53.593661</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>88.726562</td>
      <td>40.672225</td>
      <td>0.600866</td>
      <td>1.123492</td>
      <td>1.178930</td>
      <td>11.468720</td>
      <td>14.269573</td>
      <td>252.567306</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
</body>
</html>

# Descriptive Statistics

<html>
<body>
<div>
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MEAN_IP</th>
      <th>STD_IP</th>
      <th>KURTOSIS_IP</th>
      <th>SKEWNESS_IP</th>
      <th>MEAN_CURVE</th>
      <th>STD_CURVE</th>
      <th>KURTOSIS_CURVE</th>
      <th>SKEWNESS_CURVE</th>
      <th>TARGET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
      <td>17898.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>111.079968</td>
      <td>46.549532</td>
      <td>0.477857</td>
      <td>1.770279</td>
      <td>12.614400</td>
      <td>26.326515</td>
      <td>8.303556</td>
      <td>104.857709</td>
      <td>0.091574</td>
    </tr>
    <tr>
      <td>std</td>
      <td>25.652935</td>
      <td>6.843189</td>
      <td>1.064040</td>
      <td>6.167913</td>
      <td>29.472897</td>
      <td>19.470572</td>
      <td>4.506092</td>
      <td>106.514540</td>
      <td>0.288432</td>
    </tr>
    <tr>
      <td>min</td>
      <td>5.812500</td>
      <td>24.772042</td>
      <td>-1.876011</td>
      <td>-1.791886</td>
      <td>0.213211</td>
      <td>7.370432</td>
      <td>-3.139270</td>
      <td>-1.976976</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>100.929688</td>
      <td>42.376018</td>
      <td>0.027098</td>
      <td>-0.188572</td>
      <td>1.923077</td>
      <td>14.437332</td>
      <td>5.781506</td>
      <td>34.960504</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>115.078125</td>
      <td>46.947479</td>
      <td>0.223240</td>
      <td>0.198710</td>
      <td>2.801839</td>
      <td>18.461316</td>
      <td>8.433515</td>
      <td>83.064556</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>127.085938</td>
      <td>51.023202</td>
      <td>0.473325</td>
      <td>0.927783</td>
      <td>5.464256</td>
      <td>28.428104</td>
      <td>10.702959</td>
      <td>139.309331</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>192.617188</td>
      <td>98.778911</td>
      <td>8.069522</td>
      <td>68.101622</td>
      <td>223.392140</td>
      <td>110.642211</td>
      <td>34.539844</td>
      <td>1191.000837</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
</body>
</html>

```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 17898 entries, 0 to 17897
    Data columns (total 9 columns):
    MEAN_IP           17898 non-null float64
    STD_IP            17898 non-null float64
    KURTOSIS_IP       17898 non-null float64
    SKEWNESS_IP       17898 non-null float64
    MEAN_CURVE        17898 non-null float64
    STD_CURVE         17898 non-null float64
    KURTOSIS_CURVE    17898 non-null float64
    SKEWNESS_CURVE    17898 non-null float64
    TARGET            17898 non-null int64
    dtypes: float64(8), int64(1)
    memory usage: 1.2 MB

# EXPLORE

Exploratory Data Analysis (EDA)

## Comparing Attributes

### `Hotmap( )`

```python
def hotmap(df, figsize=(10,8)):
    ##### correlation heatmap
    corr = df.corr()
    fig, ax = plt.subplots(figsize=figsize)
    mask = np.zeros_like(corr, dtype=np.bool)
    idx = np.triu_indices_from(mask)
    mask[idx] = True
    
    sns.heatmap(np.abs(corr),square=True, mask=mask, annot=True,
            cmap=sns.color_palette("magma"),ax=ax,linewidth=2,edgecolor="k")
    ax.set_ylim(len(corr), -.5,.5)
    
    plt.title("CORRELATION BETWEEN VARIABLES")
    plt.show();
    
    ##### descriptive statistics heatmap
    fig, ax = plt.subplots(figsize=figsize)
    
    sns.heatmap(df.describe()[1:].transpose(),annot=True, ax=ax, 
                linecolor="w", linewidth=2,cmap=sns.color_palette("Set2")) #"Set2"
    ax.set_ylim(len(corr), -.5,.5)
    plt.title("Data summary")
    plt.show()
    
    plt.figure(figsize=(13,8))
    
    ### compare proportion of target classes
    plt.subplot(121)
    ax = sns.countplot(y = df["TARGET"],
                       palette=["b","lime"],
                       linewidth=1,
                       edgecolor="k"*2)
    for i,j in enumerate(df["TARGET"].value_counts().values):
        ax.text(.7,i,j,weight = "bold",fontsize = 27)
    plt.title("Count for target variable in datset")


    plt.subplot(122)
    plt.pie(df["TARGET"].value_counts().values,
            labels=["not pulsars","pulsars"],
            autopct="%1.0f%%",wedgeprops={"linewidth":2,"edgecolor":"white"})
    circ = plt.Circle((0,0),.7,color = "white")
    plt.gca().add_artist(circ)
    plt.subplots_adjust(wspace = .2)
    plt.title("Proportion of target variable in dataset")
    plt.show()

hotmap(df, figsize=(10,8))
```

![png](./output/output_20_0.png)

![png](./output/output_20_1.png)

![png](./output/output_20_2.png)

Target Class Values are highly differentiated for the following features:
    * Kurtosis Integrated Profile
    * Skewness Integrated Profile

Other candidates include:
  
    * Mean Curve
    * Standard Deviation Cruve
    * Kurtosis Curve
    * Skewness Curve

Least likely to be important in distinguishing pulsars and RFI include:

    * Mean Integrated Profile
    * Standard Deviation IP

## Comparing Pulsar vs NonPulsar : Mean and Standard Deviation

[GREEN == PULSAR , BLUE == NON-PULSAR]

![png](./output/output_22_1.png)

The mean and standard deviation of the Skewness Curve if also a good candidate predictor for our target class.

# Distribution of Target Class Variables

![png](./output/output_24_1.png)

# Pairplot

![png](./output/output_25_0.png)

# Scatterplot

![png](./output/output_27_0.png)

# Boxplot

![png](./output/output_28_1.png)

# Stackplot

![png](./output/output_29_0.png)

# 3D Plot

![png](./output/output_30_0.png)

# Jointplot

![png](./output/output_31_0.png)

![png](./output/output_32_0.png)

# Barplots

![png](./output/output_33_0.png)

![png](./output/output_34_1.png)

![png](./output/output_36_1.png)

![png](./output/output_37_1.png)

![png](./output/output_38_1.png)

## MODEL

## Split Data

```python
# create our feature set X and labels y:

y = df['TARGET'].copy()
X = df.drop(columns=['TARGET']).copy()
```


```python
display(y.shape, X.shape)
```


    (17898,)



    (17898, 8)

```python
# We'll do a 75/25 split on the dataset for training/test.
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.33)
```

## Construct Pipelines

1. Scale data using StandardScaler()
2. Construct Pipelines

```python
# logistic regression
pipe_lr = Pipeline([('scl', StandardScaler()),
                   ('pca', PCA(n_components=2)),
                   ('clf', LogisticRegression(class_weight='balanced'))])

# support vector
pipe_svm = Pipeline([('scl', StandardScaler()),
                    ('pca', PCA(n_components=2)),
                    ('clf', svm.SVC(class_weight='balanced'))])


# decision tree
pipe_dt = Pipeline([('scl', StandardScaler()),
                   ('pca', PCA(n_components=2)),
                    ('clf', tree.DecisionTreeClassifier(class_weight='balanced'))])

# xgboost
pipe_xgb = Pipeline([('xgb', StandardScaler()),
                    ('pca', PCA(n_components=2)),
                     ('clf', XGBClassifier(class_weight='balanced'))])
```


```python
# List of pipelines for ease of iteration
pipelines = [pipe_lr, pipe_svm, pipe_dt, pipe_xgb]
```


```python
# Dictionary of pipelines and classifier types for ease of reference
pipe_dict = {0: 'Logistic Regression', 
             1: 'Support Vector Machine', 
             2: 'Decision Tree', 
             3: 'XG Boost'}
```


```python
# Fit the pipelines
for pipe in pipelines:
    pipe.fit(X_train, y_train)
```


```python
# Compare accuracies
for idx, val in enumerate(pipelines):
    print('%s pipeline training accuracy: %.3f' % (pipe_dict[idx], val.score(X_train, y_train)))
    print('%s pipeline test accuracy: %.3f' % (pipe_dict[idx], val.score(X_test, y_test)))
```

    Logistic Regression pipeline training accuracy: 0.935
    Logistic Regression pipeline test accuracy: 0.937
    Support Vector Machine pipeline training accuracy: 0.955
    Support Vector Machine pipeline test accuracy: 0.955
    Decision Tree pipeline training accuracy: 1.000
    Decision Tree pipeline test accuracy: 0.959
    XG Boost pipeline training accuracy: 0.976
    XG Boost pipeline test accuracy: 0.973



```python
# Identify the most accurate model on test data
best_acc = 0.0
best_clf = 0
best_pipe = ''


for idx, val in enumerate(pipelines):
    if val.score(X_test, y_test) > best_acc:
        best_acc = val.score(X_test, y_test)
        best_pipe = val
        best_clf = idx
        
print('Classifier with best accuracy: %s' % pipe_dict[best_clf])

# Save pipeline to file
joblib.dump(best_pipe, 'best_pipeline.pkl', compress=1)
print('Saved %s pipeline to file' % pipe_dict[best_clf])
```

    Classifier with best accuracy: XG Boost
    Saved XG Boost pipeline to file


# `Decision Tree`

Decision Tree

## Standardize


```python
# Standardize the data
std = StandardScaler()
X_train_transformed = std.fit_transform(X_train)
X_test_transformed = std.transform(X_test)
```

## Create Instance


```python
## Create an instance of decision tree classifier
dt_clf = DecisionTreeClassifier(criterion='entropy', class_weight='balanced')
```

## Fit


```python
# Fit the training data to the model
dt_clf.fit(X_train_transformed, y_train)
```




    DecisionTreeClassifier(class_weight='balanced', criterion='entropy',
                           max_depth=None, max_features=None, max_leaf_nodes=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=1, min_samples_split=2,
                           min_weight_fraction_leaf=0.0, presort=False,
                           random_state=None, splitter='best')



## DOT Graph


```python
# Create DOT data
dot_data = export_graphviz(dt_clf, out_file=None, 
                           feature_names=X.columns,  
                           class_names=np.unique(y).astype('str'), 
                           filled=True, rounded=True, special_characters=True)

# Draw graph
graph = graph_from_dot_data(dot_data)  

# Show graph
Image(graph.create_png())
```




![png](./output/output_59_0.png)



## Make Predictions


```python
# Make predictions for test data
y_pred = dt_clf.predict(X_test_transformed)
```

## Evaluate

### Accuracy


```python
# Check the accuracy, AUC, and create a confusion matrix

acc = accuracy_score(y_test,y_pred) * 100
print('Accuracy is :{0}'.format(acc))
```

    Accuracy is :96.56339935669544


### AUC


```python
# Check the AUC for predictions
false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, y_pred)
roc_auc = auc(false_positive_rate, true_positive_rate)
print('\nAUC is :{0}'.format(round(roc_auc, 2)))

```

    
    AUC is :0.91


### Confusion Matrix


```python
# Create and print a confusion matrix 
print('\nConfusion Matrix')
print('----------------')
pd.crosstab(y_test, y_pred, rownames=['True'], colnames=['Predicted'], margins=True)
```

    
    Confusion Matrix
    ----------------

<html>
<body>
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Predicted</th>
      <th>0</th>
      <th>1</th>
      <th>All</th>
    </tr>
    <tr>
      <th>True</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>5242</td>
      <td>121</td>
      <td>5363</td>
    </tr>
    <tr>
      <td>1</td>
      <td>82</td>
      <td>462</td>
      <td>544</td>
    </tr>
    <tr>
      <td>All</td>
      <td>5324</td>
      <td>583</td>
      <td>5907</td>
    </tr>
  </tbody>
</table>
</div>
</body>
</html>

```python
# Print confusion matrix
cnf_matrix = confusion_matrix(y_test, y_pred)
print('Confusion Matrix:\n', cnf_matrix)
```

    Confusion Matrix:
     [[5242  121]
     [  82  462]]


    TRUE POSITIVES: 462 Pulsars correctly identified,
    TRUE NEGATIVES: 5242 correctly classified as noise
    FALSE POSITIVES: 121 RFI/noise misclassified as pulsars
    FALSE NEGATIVES: 82 Pulsars misclassifed as noise


```python
def plot_confusion_matrix(cm, classes,
                          normalize=False,
                          title='Confusion matrix',cmap=plt.cm.Blues):
    
    import itertools
    # Check if normalize is set to True
    # If so, normalize the raw confusion matrix before visualizing
    if normalize:
        cm = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]
        print("Normalized confusion matrix")
    else:
        print('Confusion matrix, without normalization')


    
    fig, ax = plt.subplots(figsize=(10,10))
    #mask = np.zeros_like(cm, dtype=np.bool)
    #idx = np.triu_indices_from(mask)
    
    #mask[idx] = True

    plt.imshow(cm, cmap=cmap, aspect='equal')
    
    # Add title and axis labels 
    plt.title('Confusion Matrix') 
    plt.ylabel('True label') 
    plt.xlabel('Predicted label')
    
    # Add appropriate axis scales
    tick_marks = np.arange(len(classes))
    plt.xticks(tick_marks, classes, rotation=45)
    plt.yticks(tick_marks, classes)
    #ax.set_ylim(len(cm), -.5,.5)
    
    # Text formatting
    fmt = '.2f' if normalize else 'd'
    # Add labels to each cell
    thresh = cm.max() / 2.
    # iterate thru matrix and append labels  
    for i, j in itertools.product(range(cm.shape[0]), range(cm.shape[1])):
        plt.text(j, i, format(cm[i, j], fmt),
                 horizontalalignment='center',
                 color='darkgray' if cm[i, j] > thresh else 'black',
                size=14, weight='bold')
    
    # Add a legend
    plt.colorbar()
    plt.show() 
```


```python
# Plot normalized confusion matrix
plot_confusion_matrix(cnf_matrix, classes=['Non-Pulsar', 'Pulsar'], normalize=True,
                      title='Normalized confusion matrix')
```

    Normalized confusion matrix



![png](./output/output_72_1.png)


## Parameter Tuning
    
    * Create an array for max_depth values ranging from 1 - 32
    * In a loop, train the classifier for each depth value (32 runs) 
    * Calculate the training and test AUC for each run
    * Plot a graph to show under/over fitting and optimal value
    * Interpret the results

### Max Depth


```python
# Check for the best depth parameter
max_depths = np.linspace(1, 32, 32, endpoint=True)
train_results = []
test_results = []

# Identify the optimal tree depth for given data
for max_depth in max_depths:
    dt = DecisionTreeClassifier(criterion='entropy', max_depth=max_depth, class_weight='balanced')
    dt.fit(X_train_transformed, y_train)
    train_pred = dt.predict(X_train_transformed)
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_train, train_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
   
    # Add auc score to previous train results
    train_results.append(roc_auc)
    y_pred = dt.predict(X_test_transformed)
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, y_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
   
    # Add auc score to previous test results
    test_results.append(roc_auc)

# PLOT AUC curve
plt.figure(figsize=(11,8))
plt.plot(max_depths, train_results, 'k', label='Train AUC')
plt.plot(max_depths, test_results, 'r', label='Test AUC')
plt.ylabel('AUC score')
plt.xlabel('Tree depth')
plt.title('MAX TREE DEPTH')
plt.legend()
plt.show()
```


![png](./output/output_75_0.png)

    Max tree depth optimal value does not improve beyond 3 for test data.

### Min Sample Split

Now we'll check for the best min_samples_splits parameter for our decision tree.

    * Create an array for min_sample_splits values ranging from 0.1 - 1 with an 
    increment of 0.1
    * In a loop, train the classifier for each min_samples_splits value (10 runs)
    * Calculate the training and test AUC for each run
    * Plot a graph to show under/over fitting and optimal value
    * Interpret the results


```python
# Identify the optimal min-samples-split for given data

min_samples_splits = np.linspace(0.1, 1.0, 10, endpoint=True)
train_results = []
test_results = []

for min_samples_split in min_samples_splits:
    dt = DecisionTreeClassifier(criterion='entropy', min_samples_split=min_samples_split, class_weight='balanced')
    dt.fit(X_train_transformed, y_train)
    train_pred = dt.predict(X_train_transformed)
    
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_train, train_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
    train_results.append(roc_auc)
    
    y_pred = dt.predict(X_test_transformed)
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, y_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
    test_results.append(roc_auc)

plt.figure(figsize=(11,8))
plt.plot(min_samples_splits, train_results, 'k', label='Train AUC')
plt.plot(min_samples_splits, test_results, 'r', label='Test AUC')
plt.xlabel('Min. Sample splits')
plt.ylabel('AUC score')
plt.title('MIN SAMPLES SPLITS')
plt.legend()
plt.show()
```

![png](./output/output_78_0.png)

    AUC does not improve beyond 0.2 for test data.

### Minimum Sample Leafs

Now we'll check for the best min_samples_leafs parameter value for our decision tree.

    * Create an array for min_samples_leafs values ranging from 0.1 - 0.5 
    with an increment of 0.1
    * In a loop, train the classifier for each min_samples_leafs value (5 runs)
    * Calculate the training and test AUC for each run
    * Plot a graph to show under/over fitting and optimal value
    * Interpret the results

```python
# Calculate the optimal value for minimum sample leafs
min_samples_leafs = np.linspace(0.1, 0.5, 5, endpoint=True)
train_results = []
test_results = []

for min_samples_leaf in min_samples_leafs:
    dt = DecisionTreeClassifier(criterion='entropy', min_samples_leaf=min_samples_leaf, class_weight='balanced')
    dt.fit(X_train_transformed, y_train)
    train_pred = dt.predict(X_train_transformed)
    
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_train, train_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
    train_results.append(roc_auc)
    
    y_pred = dt.predict(X_test_transformed)
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, y_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
    test_results.append(roc_auc)

# PLOT
plt.figure(figsize=(11,7))
plt.plot(min_samples_leafs, train_results, 'b', label='Train AUC')
plt.plot(min_samples_leafs, test_results, 'r', label='Test AUC')
plt.ylabel('AUC score')
plt.xlabel('Min. Sample Leafs')
plt.title('MIN SAMPLE LEAFS')
plt.legend()
plt.show()
```

![png](./output/output_81_0.png)

    Highest AUC for both train and test data maximized at 0.10.

### Maximum Features

Now we'll check for the best max_features parameter value for our decision tree.

    * Create an array for max_features values ranging from 1 - 12 (1 features vs all)
    * In a loop, train the classifier for each max_features value (12 runs)
    * Calculate the training and test AUC for each run
    * Plot a graph to show under/over fitting and optimal value
    * Interpret the results

```python
# Find the best value for optimal maximum feature size
max_features = list(range(1, X_train.shape[1]))
train_results = []
test_results = []

for max_feature in max_features:
    dt = DecisionTreeClassifier(criterion='entropy', max_features=max_feature, class_weight='balanced')
    dt.fit(X_train_transformed, y_train)
    train_pred = dt.predict(X_train_transformed)
    
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_train, train_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
    train_results.append(roc_auc)
    
    y_pred = dt.predict(X_test_transformed)
    false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, y_pred)
    roc_auc = auc(false_positive_rate, true_positive_rate)
    test_results.append(roc_auc)

plt.figure(figsize=(13,8))
plt.plot(max_features, train_results, 'b', label='Train AUC')
plt.plot(max_features, test_results, 'r', label='Test AUC')
plt.ylabel('AUC score')
plt.xlabel('max features')
plt.title('MAX FEATURES')
plt.legend()
plt.show()
```

![png](./output/output_84_0.png)

    Increasing parameters has no clear effect on training data (flat AUC). 
    Optimal value for test data is 5.

## Retrain classifer

We'll now use the best values from each training phase above and feed it back to our classifier and see if have any improvement in predictive performance.

    * Train the classifier with optimal values identified
    * Compare the AUC with vanilla DT AUC
    * Interpret the results of comparison

```python
# Re-train DT classifier with optimal values identified above
dt_clf = DecisionTreeClassifier(criterion='entropy', class_weight='balanced',
                                max_features=5,
                                max_depth=3,
                                min_samples_split=0.2,
                                min_samples_leaf=0.1)
# fit model
dt_clf.fit(X_train_transformed, y_train)

# make predictions
y_pred = dt_clf.predict(X_test_transformed)

# roc_auc
false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, y_pred)
roc_auc = auc(false_positive_rate, true_positive_rate)
roc_auc
```

    0.9257396472014128

### DOT Graph

```python
# Create DOT data
dot_data = export_graphviz(dt_clf, out_file=None, 
                           feature_names=X.columns,  
                           class_names=np.unique(y).astype('str'), 
                           filled=True, rounded=True, special_characters=True)

# Draw graph
graph = graph_from_dot_data(dot_data)  

# Show graph
Image(graph.create_png())
```

![png](./output/output_89_0.png)


```python
def modelX(algorithm, X_train, y_train, X_test, y_test, of_type):
    from sklearn.metrics import classification_report
    print ("**********"*7)
    print ("MODEL X")
    print ("**********"*7)
    algorithm.fit(X_train, y_train)
    y_pred = algorithm.predict(X_test)
    
    print (algorithm)
    print ("\n accuracy_score :", accuracy_score(y_test, y_pred))
    
    print ("\nclassification report :\n",(classification_report(y_test, y_pred)))
        
    plt.figure(figsize=(13,10))
    plt.subplot(221)
    sns.heatmap(confusion_matrix(y_test, y_pred),annot=True,fmt = "d",linecolor="k",linewidths=3)
    plt.title("CONFUSION MATRIX",fontsize=20)
    
    pred_probs = algorithm.predict_proba(X_test)[:,1]
    fpr,tpr,thresholds = roc_curve(y_test, pred_probs)
    plt.subplot(222)
    plt.plot(fpr,tpr,label = ("Area_under the curve :",auc(fpr,tpr)),color = "r")
    plt.plot([1,0],[1,0],linestyle = "dashed",color ="k")
    plt.legend(loc = "best")
    plt.title("ROC - CURVE & AREA UNDER CURVE",fontsize=20)
    
    if of_type == "feat":
        
        dataframe = pd.DataFrame(algorithm.feature_importances_, X_train.columns).reset_index()
        dataframe = dataframe.rename(columns={"index":"features",0:"coefficients"})
        dataframe = dataframe.sort_values(by="coefficients",ascending = False)
        plt.subplot(223)
        ax = sns.barplot(x ="coefficients", y="features",data=dataframe, palette="husl")
        plt.title("FEATURE IMPORTANCES",fontsize =20)
        for i,j in enumerate(dataframe["coefficients"]):
            ax.text(.011,i,j,weight = "bold")
    
    elif of_type == "coef":
        try:
            dataframe = pd.DataFrame(algorithm.coef_.ravel(), X_train.columns).reset_index()
            dataframe = dataframe.rename(columns={"index":"features",0:"coefficients"})
            dataframe = dataframe.sort_values(by="coefficients",ascending = False)
            plt.subplot(223)
            ax = sns.barplot(x = "coefficients" ,y ="features",data=dataframe,palette="husl")
            plt.title("FEATURE IMPORTANCES",fontsize =20)
            for i,j in enumerate(dataframe["coefficients"]):
                ax.text(.011,i,j,weight = "bold")
        except:
            print(f"{0} has no coef argument", str(algorithm))
```

```python
# UNSCALED DATA
modelX(dt_clf, X_train, y_train, X_test, y_test, "feat")
```

    **********************************************************************
    MODEL X
    **********************************************************************
    DecisionTreeClassifier(class_weight='balanced', criterion='entropy',
                           max_depth=3, max_features=5, max_leaf_nodes=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=0.1, min_samples_split=0.2,
                           min_weight_fraction_leaf=0.0, presort=False,
                           random_state=None, splitter='best')
    
     accuracy_score : 0.9656339935669545
    
    classification report :
                   precision    recall  f1-score   support
    
               0       0.99      0.97      0.98      5363
               1       0.78      0.88      0.82       544
    
        accuracy                           0.97      5907
       macro avg       0.88      0.93      0.90      5907
    weighted avg       0.97      0.97      0.97      5907
    

![png](./output/output_91_1.png)

Kurtosis Integrated Profile ('KURTOSIS_IP') is by far the most important classifying feature when it comes to identifying Pulsars. Let's double check the other metrics with our scaled/transformed data:

```python
# SCALED DATA
modelX(dt_clf, X_train_transformed, y_train, X_test_transformed, y_test, "coef")
```

    **********************************************************************
    MODEL X
    **********************************************************************
    DecisionTreeClassifier(class_weight='balanced', criterion='entropy',
                           max_depth=3, max_features=5, max_leaf_nodes=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=0.1, min_samples_split=0.2,
                           min_weight_fraction_leaf=0.0, presort=False,
                           random_state=None, splitter='best')
    
     accuracy_score : 0.9481970543423057
    
    classification report :
                   precision    recall  f1-score   support
    
               0       0.98      0.96      0.97      5363
               1       0.69      0.81      0.74       544
    
        accuracy                           0.95      5907
       macro avg       0.83      0.88      0.86      5907
    weighted avg       0.95      0.95      0.95      5907
    
    0 has no coef argument DecisionTreeClassifier(class_weight='balanced', criterion='entropy',
                           max_depth=3, max_features=5, max_leaf_nodes=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=0.1, min_samples_split=0.2,
                           min_weight_fraction_leaf=0.0, presort=False,
                           random_state=None, splitter='best')


![png](./output/output_93_1.png)


`F1 Score`
The F1 score (also F-score or F-measure) is a measure of a test's accuracy. It considers both the precision p and the recall r of the test to compute the score: p is the number of correct positive results divided by the number of all positive results returned by the classifier, and r is the number of correct positive results divided by the number of all relevant samples (all samples that should have been identified as positive). The F1 score is the harmonic mean of the precision and recall, where an F1 score reaches its best value at 1 (perfect precision and recall) and worst at 0.

`Harmonic Mean`
`f1 = 2*(P*R / P+R)`


```python
f1 = f1_score(y_test, y_pred)
f1
```

    0.8245462402765774



Because the data involves imbalanced classes, F1 score is most important metric for us to validate the model's accuracy. Let's compare the Decision Tree classifier performance to XGBoost next.

# `XG Boost`

Moving ahead with XG Boost

## Create Instance and Fit

```python
# Fit XG Boost model  
# Instantiate XGBClassifier with balanced class weights
xgb_clf = XGBClassifier(class_weight='balanced')

# Fit XGBClassifier
xgb_clf.fit(X_train_transformed, y_train)
```




    XGBClassifier(base_score=0.5, booster='gbtree', class_weight='balanced',
                  colsample_bylevel=1, colsample_bynode=1, colsample_bytree=1,
                  gamma=0, learning_rate=0.1, max_delta_step=0, max_depth=3,
                  min_child_weight=1, missing=None, n_estimators=100, n_jobs=1,
                  nthread=None, objective='binary:logistic', random_state=0,
                  reg_alpha=0, reg_lambda=1, scale_pos_weight=1, seed=None,
                  silent=None, subsample=1, verbosity=1)



## Make Predictions

```python
# Predict on training and test sets
training_preds = xgb_clf.predict(X_train_transformed)
test_preds = xgb_clf.predict(X_test_transformed)
```

## Evaluate

### Accuracy

```python
# Accuracy of training and test sets
training_accuracy = accuracy_score(y_train, training_preds)
test_accuracy = accuracy_score(y_test, test_preds)

print('Training Accuracy: {:.4}%'.format(training_accuracy * 100))
print('Validation accuracy: {:.4}%'.format(test_accuracy * 100))

```

    Training Accuracy: 98.38%
    Validation accuracy: 98.02%

### ROC_AUC and Confusion Matrix

```python
# SCALED DATA
modelX(xgb_clf, X_train_transformed, y_train, X_test_transformed, y_test, "coef")
```

    **********************************************************************
    MODEL X
    **********************************************************************
    XGBClassifier(base_score=0.5, booster='gbtree', class_weight='balanced',
                  colsample_bylevel=1, colsample_bynode=1, colsample_bytree=1,
                  gamma=0, learning_rate=0.1, max_delta_step=0, max_depth=3,
                  min_child_weight=1, missing=None, n_estimators=100, n_jobs=1,
                  nthread=None, objective='binary:logistic', random_state=0,
                  reg_alpha=0, reg_lambda=1, scale_pos_weight=1, seed=None,
                  silent=None, subsample=1, verbosity=1)
    
     accuracy_score : 0.9801929913661758
    
    classification report :
                   precision    recall  f1-score   support
    
               0       0.99      0.99      0.99      5363
               1       0.92      0.86      0.89       544
    
        accuracy                           0.98      5907
       macro avg       0.95      0.92      0.94      5907
    weighted avg       0.98      0.98      0.98      5907
    
    0 has no coef argument XGBClassifier(base_score=0.5, booster='gbtree', class_weight='balanced',
                  colsample_bylevel=1, colsample_bynode=1, colsample_bytree=1,
                  gamma=0, learning_rate=0.1, max_delta_step=0, max_depth=3,
                  min_child_weight=1, missing=None, n_estimators=100, n_jobs=1,
                  nthread=None, objective='binary:logistic', random_state=0,
                  reg_alpha=0, reg_lambda=1, scale_pos_weight=1, seed=None,
                  silent=None, subsample=1, verbosity=1)


![png](./output/output_106_1.png)

## GridSearchCV

Tuning XG Boost with a parameter Gridsearch


```python
param_grid = {
    'learning_rate': [0.1, 0.2, 0.3],
    'max_depth': [6],
    'min_child_weight': [1, 2],
    'subsample': [0.3, 0.5, 0.7],
    'n_estimators': [100],
}
```


```python
grid_clf = GridSearchCV(xgb_clf, param_grid, scoring='accuracy', cv=None, n_jobs=1)
grid_clf.fit(X_train_transformed, y_train)

best_parameters = grid_clf.best_params_

print('Grid Search found the following optimal parameters: ')
for param_name in sorted(best_parameters.keys()):
    print('%s: %r' % (param_name, best_parameters[param_name]))

training_preds = grid_clf.predict(X_train_transformed)
test_preds = grid_clf.predict(X_test_transformed)
training_accuracy = accuracy_score(y_train, training_preds)
test_accuracy = accuracy_score(y_test, test_preds)

print('')
print('Training Accuracy: {:.4}%'.format(training_accuracy * 100))
print('Validation accuracy: {:.4}%'.format(test_accuracy * 100))
```

    Grid Search found the following optimal parameters: 
    learning_rate: 0.3
    max_depth: 6
    min_child_weight: 1
    n_estimators: 100
    subsample: 0.7
    
    Training Accuracy: 99.96%
    Validation accuracy: 97.78%


## Evaluate Model

### AUC


```python
# Check the AUC for predictions
false_positive_rate, true_positive_rate, thresholds = roc_curve(y_test, test_preds)
roc_auc = auc(false_positive_rate, true_positive_rate)
print('\nAUC is :{0}'.format(round(roc_auc, 2)))

```

    
    AUC is :0.92



```python
# compare a few different regularization performances on the dataset:
C_param_range = [0.005, 0.1, 0.2, 0.3, 0.5, 0.6, 0.7, 0.8]
names = [0.005, 0.1, 0.2, 0.3, 0.5, 0.6, 0.7, 0.8, 0.9]
colors = sns.color_palette('Set2', n_colors=len(names))

plt.figure(figsize=(10, 8))

for n, c in enumerate(C_param_range):

    # Predict
    #y_pred = tree_clf.predict(X_test)
    y_pred = dt_clf.predict(X_test_transformed)
    y_score = accuracy_score(y_test, y_pred)
    
    
    fpr, tpr, thresholds = roc_curve(y_test, y_pred)
    #roc_auc = auc(fpr, tpr)
    print('----------------------------------------------')
    print('AUC for {}: {}'.format(names[n], auc(fpr, tpr)))
    lw = 2
    plt.plot(fpr, tpr, color=colors[n],
             lw=lw, label='ROC curve Normalization Weight: {}'.format(names[n]))
    
plt.plot([0, 1], [0, 1], color='navy', lw=lw, linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.yticks([i/20.0 for i in range(21)])
plt.xticks([i/20.0 for i in range(21)])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic (ROC) Curve')
plt.legend(loc='lower right')
plt.show()
```

    ----------------------------------------------
    AUC for 0.005: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.1: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.2: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.3: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.5: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.6: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.7: 0.8839272493446382
    ----------------------------------------------
    AUC for 0.8: 0.8839272493446382



![png](./output/output_113_1.png)

### Confusion matrix

```python
# Create and print a confusion matrix 
print('\nConfusion Matrix')
print('----------------')
pd.crosstab(y_test, test_preds, rownames=['True'], colnames=['Predicted'], margins=True)
```

    
    Confusion Matrix
    ----------------
<html>
<body>
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Predicted</th>
      <th>0</th>
      <th>1</th>
      <th>All</th>
    </tr>
    <tr>
      <th>True</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>5324</td>
      <td>39</td>
      <td>5363</td>
    </tr>
    <tr>
      <td>1</td>
      <td>78</td>
      <td>466</td>
      <td>544</td>
    </tr>
    <tr>
      <td>All</td>
      <td>5402</td>
      <td>505</td>
      <td>5907</td>
    </tr>
  </tbody>
</table>
</div>
</body>
</html>


```python

# Import confusion_matrix
from sklearn.metrics import confusion_matrix

# Print confusion matrix
cnf_matrix = confusion_matrix(y_test, test_preds)
print('Confusion Matrix:\n', cnf_matrix)
```

    Confusion Matrix:
     [[5324   39]
     [  78  466]]

```python
# Plot normalized confusion matrix
plot_confusion_matrix(cnf_matrix, classes=['Non-Pulsar', 'Pulsar'], normalize=True,
                      title='Normalized confusion matrix')
```

    Normalized confusion matrix

![png](./output/output_117_1.png)

```python
# Plot normalized confusion matrix
plot_confusion_matrix(cnf_matrix, classes=['Non-Pulsar', 'Pulsar'], normalize=False,
                      title='Normalized confusion matrix')
```

    Confusion matrix, without normalization

![png](./output/output_118_1.png)

## MSE and R2

```python

from sklearn.metrics import mean_squared_error as mse
from sklearn.metrics import r2_score

# Make predictions and evaluate
print('MSE score:', mse(y_test, y_pred))
print('R-sq score:', r2_score(y_test,y_pred))

```

    MSE score: 0.05180294565769426
    R-sq score: 0.38044238299459265

## Feature Importance

```python
# Feature importance
xgb_clf.feature_importances_
```

    array([0.04194515, 0.03668287, 0.68801844, 0.03353313, 0.02772439,
           0.09195759, 0.03966612, 0.04047231], dtype=float32)


```python
importance = pd.Series(data=xgb_clf.feature_importances_, index=X_train.columns)
importance.sort_values(ascending=False)
```

    KURTOSIS_IP       0.688018
    STD_CURVE         0.091958
    MEAN_IP           0.041945
    SKEWNESS_CURVE    0.040472
    KURTOSIS_CURVE    0.039666
    STD_IP            0.036683
    SKEWNESS_IP       0.033533
    MEAN_CURVE        0.027724
    dtype: float32


```python
def plot_feature_importances(model):
    
    importance = pd.Series(data=model.feature_importances_, index=X_train.columns)
    importance = importance.sort_values(ascending=True)
    n_features = X_train.shape[1]
    plt.figure(figsize=(8,8))
    plt.barh(importance.index, importance.values, align='center') 
    #plt.yticks(np.arange(n_features), X_train.columns.values) 
    plt.xlabel('Feature importance')
    plt.ylabel('Feature')

plot_feature_importances(xgb_clf)
```

![png](./output/output_124_0.png)

```python
print("Testing Accuracy for XG Boost Classifier: {:.4}%".format(accuracy_score(y_test, y_pred) * 100))
```

    Testing Accuracy for XG Boost Classifier: 94.82%

```python
print("Testing F1 Score for XG Boost Classifier: {:.4}%".format(f1_score(y_test, y_pred) * 100))
```
    Testing F1 Score for XG Boost Classifier: 74.11%

# INTERPRET RESULTS

```python
# Cross-Validation
from sklearn.model_selection import cross_val_score

xgb_cv_score = cross_val_score(xgb_clf, X_train_transformed, y_train, cv=3)
mean_xgb_cv_score = np.mean(xgb_cv_score)

print(f"Mean Cross Validation Score: {mean_xgb_cv_score :.2%}")
```

    Mean Cross Validation Score: 98.02%

```python
# Feature Importance
from xgboost import plot_importance

plot_importance(booster=xgb_clf)
```

![png](./output/output_129_1.png)