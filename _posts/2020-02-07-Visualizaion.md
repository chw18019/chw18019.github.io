---
title: "Data Visualization using Python"
date: 2020-02-17
tags: [Data visualization, Python]
header:
  overlay_image: /assets/200217/
  caption: "Photo credit: [**Scott Graham**](https://unsplash.com)"
excerpt: "EDA and Visualization with Python. (Difficulty: ★★★☆☆)"
toc: true
toc_label: "Content"
toc_sticky: true
---
_For the complete jupyter notebook, you can find it on my GitHub repository [<i class="fab fa-fw fa-github" aria-hidden="true"></i>](https://github.com/chw18019/Data-Visualization/)._
{: .notice--primary}


# Introduction
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;While I was the teaching assistant for Data Science using Python at UConn, I made this materials for students to help them get familiar with Python. I think it can be beneficial to others as well.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This post is for general practice of exploratory data analysis(EDA) and data visualization using the classic Boston Housing Dataset. The following shows the packages includes in this file:
*   matplotlib
*   seaborn
*   Pandas Profiling
*   AutoViz


## Variable Recoding
```python
# Create a new variable (one means the MEDV is larger than the mean.)
mean_MEDV = round(df.mean(axis=0)[-1],2)
df['FlagMEDV'] = np.where(df['MEDV']> mean_MEDV, '1', '0')
```

## Groupby & Aggregate function


```python
df.groupby('FlagMEDV').describe()
```

![png](/assets/200217/T1.png)

```python
df.groupby('FlagMEDV').agg([np.mean, np.std, np.median])
```

![png](/assets/200217/T2.png)


## Standardization

### MinMax Scaler

This estimator scales and translates each feature individually such that it is in the given range on the training set, e.g. between zero and one.

The transformation is given by:
<pre><code>X_std = (X - X.min(axis=0)) / (X.max(axis=0) - X.min(axis=0))
X_scaled = X_std * (max - min) + min</code></pre>
where min, max = feature_range.
The transformation is calculated as:

<pre><code>X_scaled = scale * X + min - X.min(axis=0) * scale
where scale = (max - min) / (X.max(axis=0) - X.min(axis=0))</code></pre>
This transformation is often used as an alternative to zero mean, unit variance scaling.


```python
from sklearn import preprocessing

x = df.values #returns a numpy array
min_max_scaler = preprocessing.MinMaxScaler()
x_scaled = min_max_scaler.fit_transform(x)
df_mmstd = pd.DataFrame(x_scaled, columns=df.columns)
df_mmstd.drop('FlagMEDV', axis=1,inplace=True)
df_mmstd.describe()
```

![png](/assets/200217/T3.png)

### Z-score Methods

```python
from scipy.stats import zscore
numeric_cols = df.select_dtypes(include=[np.number]).columns
df[numeric_cols].apply(zscore)
```
![png](/assets/200217/T4.png)


## Matplotlib

```python
%matplotlib inline 
import matplotlib.pyplot as plt
```

### Simple Line Plot


```python
plt.plot(df.MEDV)
```

![png](/assets/200217/output_25_1.png)


```python
plt.plot(df.MEDV, color="orange")
```

![png](/assets/200217/output_26_1.png)



```python
plt.plot(df.index, df.MEDV, 'o', color="red")
```








![png](/assets/200217/output_27_1.png)



```python
plt.plot(df.index, df.MEDV, '-o', color="red")
```








![png](/assets/200217/output_28_1.png)



```python
# making plots from the documentation

fig, ax = plt.subplots()
ax.scatter(df.index, df.MEDV, c='m',  alpha=0.7)
# alpha is the transparency, c is the color,

ax.set_xlabel("My X Label", fontsize=15)
ax.set_ylabel("My Y Label", fontsize=15)
ax.set_title('My Title', fontsize=30)

# ax.grid(True)
fig.tight_layout()

plt.show()
```


![png](/assets/200217/output_29_0.png)



```python
# nest two plots
# making plots from the documentation
# link: https://towardsdatascience.com/plot-organization-in-matplotlib-your-one-stop-guide-if-you-are-reading-this-it-is-probably-f79c2dcbc801
# Synthetic Data
time = np.linspace(0, 10, 1000)
height = np.sin(time)
weight = np.cos(time)
# Plotting all the subplots
fig, axes = plt.subplots(2, 3)

# add a main title - make sure you run this first!
fig.suptitle('My Title', fontsize=30)

axes[0, 0].plot(time, height)
axes[0, 1].plot(time, time**2)
axes[0, 2].hist(height)
axes[1, 0].plot(time, weight, color='green')
axes[1, 1].plot(time, 1/(time+1), color='green')
axes[1, 2].hist(weight, color='green')

# tight_layout may move some axes
#plt.tight_layout()

#fig.savefig('figures/figureAxesSubplots.png')
plt.show()
```


![png](/assets/200217/output_30_0.png)



```python
# more plot options for markers
# link: https://jakevdp.github.io/PythonDataScienceHandbook/04.02-simple-scatter-plots.html

"""
Here are some nice options:

'o', '.', ',', 'x', '+', 'v', '^', '<', '>', 's', 'd'
"""
```


```python
# more plot options for colors
# link: https://matplotlib.org/2.0.2/api/colors_api.html

"""
Here are some nice options:

b: blue
g: green
r: red
c: cyan
m: magenta
y: yellow
k: black
w: white
"""
```

### Scatter Plot


```python
plt.scatter(df.CRIM, df.MEDV)
```








![png](/assets/200217/output_34_1.png)



```python
#More plots at once
plt.figure(figsize=(15, 5))

features = ['LSTAT', 'RM','DIS','NOX']
target = df['MEDV']

for i, col in enumerate(features):
    plt.subplot(1, len(features) , i+1)
    x = df[col]
    y = target
    plt.scatter(x, y, marker='o')
    plt.title(col)
    plt.xlabel(col)
    plt.ylabel('MEDV')
```


![png](/assets/200217/output_35_0.png)



```python
#Changing size, color, and transparency in scatter plots
rng =np.random.RandomState(0)
colors = rng.rand(506)
size = 1000* rng.rand(506)
plt.scatter(df.CRIM, df.MEDV, c=colors, s =size, alpha=0.3, cmap='viridis')
plt.colorbar()
```








![png](/assets/200217/output_36_1.png)


### Basic Errorbars


```python
x = np.linspace(0, 10, 50) 
dy=0.8
y = np.sin(x) + dy * np.random.randn(50)
plt.errorbar(x, y, yerr=dy, fmt='.k')
```








![png](/assets/200217/output_38_1.png)


### Save plots


```python
# Save the plots, then you can access the file on your right hand side
# Another way to do it, just right click the plots and select save images...
plt.savefig('fancy.png')
```




## **Seaborn**


```python
import seaborn as sns
```

### Histograms and Kernel density estimates (KDE)


```python
for col in ['MEDV','ZN']:
  plt.hist(df[col], density=True, alpha=0.5)
```


![png](/assets/200217/output_44_0.png)



```python
for col in ['MEDV','ZN']:
  sns.kdeplot(df[col],shade=True)
```


![png](/assets/200217/output_45_0.png)



```python
sns.distplot(df['MEDV'], bins=30)
plt.show()
```


![png](/assets/200217/output_46_0.png)


### Pair Plot


```python
sns.pairplot(df[['INDUS','NOX','LSTAT','MEDV']], height=2.5)
```








![png](/assets/200217/output_48_1.png)


### Correlation Matrix


```python
sns.set(rc={'figure.figsize':(11.7,8.27)})
correlation_matrix = df.corr().round(2)
sns.heatmap(data=correlation_matrix, annot=True)
# annot = True to print the values inside the square
```








![png](/assets/200217/output_50_1.png)


### Boxplot 


```python
sns.boxplot("FlagMEDV","RM", data=df, palette="Set3")
#sns.swarmplot("FlagMEDV","RM", data=df, color=".25")
```







![png](/assets/200217/output_52_1.png)



```python
sns.violinplot('FlagMEDV','AGE', data=df, palette=["lightblue","lightpink"])
```




![png](/assets/200217/output_53_1.png)


## Pandas Profiling

For more details, please navigate to [the officail documentation](https://pandas-profiling.github.io/pandas-profiling/docs/master/index.html#pandas-profiling). `Pandas-profiling` generates profile reports from a pandas `DataFrame`, which includes basic statiscs.

```cmd
!pip install pandas_profiling
```
```python
from pandas_profiling import ProfileReport
profile = ProfileReport(df)
profile
```

## AutoViz
[AutoViz](https://pypi.org/project/autoviz/) performs automatic visualizaion with only one line of code. Read [this Medium blog](https://towardsdatascience.com/autoviz-a-new-tool-for-automated-visualization-ec9c1744a6ad) to get more details.
```python
!pip install autoviz

from autoviz.AutoViz_Class import AutoViz_Class
AV = AutoViz_Class()
dft = AV.AutoViz("/YOUR_PATH/boston.csv")
```

There're several visualization tool in the market, but I personally think most of them cost way too much. Python has these amazing packages for free, and as well as Java! Maybe next time, we can dive into d3.js! Thanks for reading, until next time!
