---
title: "Potoo Solutions Capstone Project"
date: 2020-05-04
tags: [Machine Learning, Data Pre-processing, Python]
header:
  overlay_image: /assets/200504/scott-graham.jpeg
  caption: "Photo credit: [**Scott Graham**](https://unsplash.com)"
excerpt: "Predict the third-party sellers' resources by building a classifier with Python. (Difficulty: ★★★☆☆)"
toc: true
toc_label: "Content"
toc_sticky: true
---
_This capstone project is guided by Professor Jennifer Eigo and Potoo CEO, Fred Dimyan. In this post, I'll provide the partial code to manifest some important techniques with `Python` for data analysis. For the complete jupyter notebook, you can find it on my GitHub repository [<i class="fab fa-fw fa-github" aria-hidden="true"></i>](https://github.com/chw18019/Potoo-Solutions)._
{: .notice--primary}

# Introduction
> “The era when warehouses and distribution centers stood as large, staid structures designed to simply meet the demand created by sales from America’s retailers, has evolved into a complex technological infrastructure servicing today’s rapidly expanding Ecommerce space” ― [Marketing at Rakuten, 2019](https://www.rakutensl.com/post/how-ecommerce-is-transforming-todays-supply-chain) 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;With the rise of Ecommerce, the landscape of supply chain management has changed drastically. This is in large part due to third-party sellers, who cause items to end up in online marketplaces for lower prices than they would traditionally be sold at brick and mortar retailers. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Here is when [Potoo Solutions](https://potoosolutions.com/) comes in. The company strives to help clients take back control of their brands with data-driven solutions. The purpose of this project is to predict the third-party selles' resources, to better equip companies to operate in this new environment.

{% include figure image_path="/assets/200504/potoo-bio.png" alt="Potoo solutions" %}

# Data
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The data was provided by Potoo Solutions. I believed Web scraping was perfromed from the well known E-Marketplace Amazon. In this project, the challenge is the data is anything but clean. Multiple files were provided, and the team had to figure out the entity relationships before data analysis. 

## Data preparation
In this part, several techniques will be presented to demonstrate how to wrangle data with Python.

### 1. Get File names in a Folder
```python
from os import walk

mypath = '/Users/chi/FOLDER PATH'
zp_name = []
for (dirpath, dirnames, filenames) in walk(mypath):
    zp_name.extend(filenames)
    break
```

### 2. Unzip, Merge, and Re-zip Files  
```python
import pandas as pd
from zipfile import ZipFile
import gzip
import re

# unzip files 
li=[]
for z in range(len(zp_name)):
    zf = ZipFile(zp_name[z]+'.zip')
    df = pd.read_csv(zf.open(zp_name[z]+'.csv'))
    # use regular expression to find the seller ID in the seller link variable
    df['Seller_id'] = df.Seller_link.str.findall('me=(.*)&rh=')
    # drop unwanted variables
    df = df.drop(labels=['url','Brand_link','Marketplace'], axis=1)
    li.append(df)

# merge
frame = pd.concat(li, axis=0, ignore_index = True)

# zip
frame.to_csv('alldata_withlinks1.csv.gz', compression='gzip')
```

### 3. Open Zip Files and Read them into `Pandas` Dataframe
```python
with gzip.open('alldata.csv.gz') as f:
    df = pd.read_csv(f)
```

### 4. Separate the Numbers from Strings

```python
df3['SP'] = df3['Selling primarily'].str.findall('(.*) \(')
df3['SP_%'] =df3['Selling primarily'].str.findall('\((.*) %\)')
df3['SP'] = [','.join(map(str, l)) for l in df3['SP']]
df3['SP_%'] = [','.join(map(str, l)) for l in df3['SP_%']]
```
### 5. Handle Multiple Sources
```python
# Split the source variable by "/" into list data format
df4['nr'] = df4.seller_source.str.split('/')

# Create dummy variables and called it "nrdum"
nrdum = df4['nr'].str.join(sep=',').str.get_dummies(sep=',')

# Remove the original resource variables and concatenate the dummy variables based on index
df4 = pd.concat([df4, nrdum], axis=1)
df4.drop(columns=['seller_source', 'nr'], inplace=True)


# To remove the duplicates in seller_id and seller_name in rows, we aggregated the numbers of sources by ID and seller name
df4_1 = df4.groupby(['seller_id','seller_name']).sum()
```

### 6. Oversampling Technique
We will face imbalance classification problem if we didn't perform any resample methods.
```python
df_majority = r5[r5.target_balance==1]
df_minority = r5[r5.target_balance==0]
df_minority_upsampled = resample(df_minority, 
                                 replace=True,#sample with replacement
                                 n_samples=965,#to match majority class
                                 random_state=123)

df_upsampled = pd.concat([df_majority, df_minority_upsampled])
```

## Modeling
Because of confidentiality agreement, some results of code are removed.
### 1. Confusion Matrix
```python 
# Confusion matrix using testing Dataset(20%)
cm = pd.crosstab(Y_test, test_pred, rownames=['Actual'], colnames=['Predicted'], margins = True)
cm.rename(columns={0:'Walmart',1:'Min-seller',2:'Costco',3:'Sears', 4:'Target'}, 
          index={0:'Walmart',1:'Min-seller',2:'Costco',3:'Sears', 4:'Target'},
          inplace=True)
sns.heatmap(cm, annot=True, fmt="d", center=cm.iloc[1,1],cmap="RdYlGn")
```

### 2. Feature Importance 

```python
num = 20

feature_importance = clf.feature_importances_
# make importances relative to max importance
sorted_idx_fi = np.argsort(feature_importance)[-num:]
pos = np.arange(sorted_idx_fi.shape[0]) + .5
plt.barh(pos[-num:], feature_importance[sorted_idx_fi], align='center')
plt.yticks(pos[-num:], X_train.columns[sorted_idx_fi])
plt.xlabel('Feature Importance')
plt.title('Feature Importances for Random Forest')
plt.show()
```

### 3. Permutation Importance
```python
num = 20
results = permutation_importance(model, X_train, y_train, scoring='neg_mean_absolute_error')
# get importance
importance = results.importances_mean
sorted_idx = np.argsort(importance)[-num:]
pos = np.arange(sorted_idx.shape[0]) + .5
plt.barh(pos[-num:], importance[sorted_idx], align='center')
plt.yticks(pos[-num:], X.columns[sorted_idx])
plt.xlabel('Permutation Feature Importance Scores')
plt.title('Permutation Feature Importance for NN')
plt.show()
```

### 4. Partial Dependence Plots

```python
for i in range(len(reso)):
  fig, ax = plt.subplots(figsize=(10, 10))
  ax.set_title("Partial Dependence in class %s" % reso[i])
  plot_partial_dependence(clf, X_train, brands, target=i,
                          n_jobs=3, grid_resolution=20,ax=ax)
```

# Conclusion
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The model created is a Random Forest model, boasting an overall accuracy of 88%. Our team divided target variables into five separate categories to predict seller source. They are Walmart (Sam’s Club), Min-seller, Costco, Sears and Target. The team then used Partial Dependence Plots to determine the relationship between a specific variable and these particular categories. For example, a seller who sells Kirkland Signature brand will have a lower chance to source from Walmart, but a higher chance from Costco. While there are only 5 target categories here, this could easily be expanded as new data is made available. There is also potential to delve deeper. Looking directly at a brand’s product lines would provide a more granular focus, granting organizations stronger decision making capability. Therefore, any client of Potoo could be provided with this information (albeit to varying accuracy). These insights will not only help Potoo more clearly understand the supply chain, but will also create a marketable product they can provide to brands. Ultimately, the model created helps to mitigate a vital problem and can be applied to a myriad of industries and organizations. 
