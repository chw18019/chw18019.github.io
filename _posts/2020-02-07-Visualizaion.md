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
_For the complete jupyter notebook, you can find it on my GitHub repository [<i class="fab fa-fw fa-github" aria-hidden="true"></i>](https://github.com/chw18019/Potoo-Solutions)._
{: .notice--primary}


# Introduction
While I was the teaching assistant for Data Science using Python at UConn, I made this materials for students to help them get familiar with Python. I think it can be beneficial to others as well.

This post is for general practice of exploratory data analysis(EDA) and data visualization using the classic Boston Housing Dataset. The following shows the packages includes in this file:
*   matplotlib
*   seaborn
*   Pandas Profiling
*   AutoViz
*   geographic data with Basemap



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




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="8" halign="left">CRIM</th>
      <th colspan="8" halign="left">ZN</th>
      <th colspan="8" halign="left">INDUS</th>
      <th colspan="8" halign="left">CHAS</th>
      <th colspan="8" halign="left">NOX</th>
      <th>...</th>
      <th colspan="8" halign="left">TAX</th>
      <th colspan="8" halign="left">PTRATIO</th>
      <th colspan="8" halign="left">B</th>
      <th colspan="8" halign="left">LSTAT</th>
      <th colspan="8" halign="left">MEDV</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>...</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>FlagMEDV</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>297.0</td>
      <td>5.563153</td>
      <td>10.668584</td>
      <td>0.01096</td>
      <td>0.1396</td>
      <td>0.84054</td>
      <td>7.67202</td>
      <td>88.9762</td>
      <td>297.0</td>
      <td>4.587542</td>
      <td>14.935642</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>80.0</td>
      <td>297.0</td>
      <td>13.813266</td>
      <td>6.360921</td>
      <td>1.69</td>
      <td>8.14</td>
      <td>18.10</td>
      <td>18.1</td>
      <td>27.74</td>
      <td>297.0</td>
      <td>0.050505</td>
      <td>0.219354</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>297.0</td>
      <td>0.594430</td>
      <td>0.117883</td>
      <td>0.385</td>
      <td>0.515</td>
      <td>0.583</td>
      <td>0.693</td>
      <td>0.871</td>
      <td>...</td>
      <td>297.0</td>
      <td>465.181818</td>
      <td>175.180025</td>
      <td>188.0</td>
      <td>307.0</td>
      <td>403.0</td>
      <td>666.0</td>
      <td>711.0</td>
      <td>297.0</td>
      <td>19.273401</td>
      <td>1.809845</td>
      <td>14.7</td>
      <td>18.5</td>
      <td>20.2</td>
      <td>20.2</td>
      <td>22.0</td>
      <td>297.0</td>
      <td>337.292727</td>
      <td>112.562557</td>
      <td>0.32</td>
      <td>350.65</td>
      <td>390.11</td>
      <td>396.90</td>
      <td>396.9</td>
      <td>297.0</td>
      <td>16.477138</td>
      <td>6.492101</td>
      <td>4.97</td>
      <td>11.98</td>
      <td>15.39</td>
      <td>19.88</td>
      <td>37.97</td>
      <td>297.0</td>
      <td>16.870370</td>
      <td>4.233162</td>
      <td>5.0</td>
      <td>13.9</td>
      <td>18.0</td>
      <td>20.3</td>
      <td>22.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>209.0</td>
      <td>0.842998</td>
      <td>2.125546</td>
      <td>0.00632</td>
      <td>0.0536</td>
      <td>0.10469</td>
      <td>0.46296</td>
      <td>14.4383</td>
      <td>209.0</td>
      <td>20.992823</td>
      <td>29.059185</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>33.0</td>
      <td>100.0</td>
      <td>209.0</td>
      <td>7.333349</td>
      <td>5.650050</td>
      <td>0.46</td>
      <td>3.33</td>
      <td>5.86</td>
      <td>9.9</td>
      <td>21.89</td>
      <td>209.0</td>
      <td>0.095694</td>
      <td>0.294877</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>209.0</td>
      <td>0.498229</td>
      <td>0.085830</td>
      <td>0.394</td>
      <td>0.433</td>
      <td>0.488</td>
      <td>0.532</td>
      <td>0.871</td>
      <td>...</td>
      <td>209.0</td>
      <td>327.315789</td>
      <td>118.667991</td>
      <td>187.0</td>
      <td>264.0</td>
      <td>296.0</td>
      <td>348.0</td>
      <td>666.0</td>
      <td>209.0</td>
      <td>17.293301</td>
      <td>2.098982</td>
      <td>12.6</td>
      <td>15.6</td>
      <td>17.6</td>
      <td>18.7</td>
      <td>21.2</td>
      <td>209.0</td>
      <td>384.215885</td>
      <td>30.106473</td>
      <td>131.42</td>
      <td>384.30</td>
      <td>392.78</td>
      <td>395.63</td>
      <td>396.9</td>
      <td>209.0</td>
      <td>7.218852</td>
      <td>3.643771</td>
      <td>1.73</td>
      <td>4.70</td>
      <td>6.58</td>
      <td>9.16</td>
      <td>29.55</td>
      <td>209.0</td>
      <td>30.579426</td>
      <td>8.308060</td>
      <td>22.6</td>
      <td>24.0</td>
      <td>27.9</td>
      <td>34.6</td>
      <td>50.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 112 columns</p>
</div>




```python
df.groupby('FlagMEDV').agg([np.mean, np.std, np.median])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">CRIM</th>
      <th colspan="3" halign="left">ZN</th>
      <th colspan="3" halign="left">INDUS</th>
      <th colspan="3" halign="left">CHAS</th>
      <th colspan="3" halign="left">NOX</th>
      <th colspan="3" halign="left">RM</th>
      <th colspan="3" halign="left">AGE</th>
      <th colspan="3" halign="left">DIS</th>
      <th colspan="3" halign="left">RAD</th>
      <th colspan="3" halign="left">TAX</th>
      <th colspan="3" halign="left">PTRATIO</th>
      <th colspan="3" halign="left">B</th>
      <th colspan="3" halign="left">LSTAT</th>
      <th colspan="3" halign="left">MEDV</th>
    </tr>
    <tr>
      <th></th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
      <th>mean</th>
      <th>std</th>
      <th>median</th>
    </tr>
    <tr>
      <th>FlagMEDV</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.563153</td>
      <td>10.668584</td>
      <td>0.84054</td>
      <td>4.587542</td>
      <td>14.935642</td>
      <td>0.0</td>
      <td>13.813266</td>
      <td>6.360921</td>
      <td>18.10</td>
      <td>0.050505</td>
      <td>0.219354</td>
      <td>0.0</td>
      <td>0.594430</td>
      <td>0.117883</td>
      <td>0.583</td>
      <td>5.971384</td>
      <td>0.493702</td>
      <td>5.983</td>
      <td>79.009764</td>
      <td>22.561782</td>
      <td>88.6</td>
      <td>3.314998</td>
      <td>1.976581</td>
      <td>2.4775</td>
      <td>11.861953</td>
      <td>9.662434</td>
      <td>5.0</td>
      <td>465.181818</td>
      <td>175.180025</td>
      <td>403.0</td>
      <td>19.273401</td>
      <td>1.809845</td>
      <td>20.2</td>
      <td>337.292727</td>
      <td>112.562557</td>
      <td>390.11</td>
      <td>16.477138</td>
      <td>6.492101</td>
      <td>15.39</td>
      <td>16.870370</td>
      <td>4.233162</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.842998</td>
      <td>2.125546</td>
      <td>0.10469</td>
      <td>20.992823</td>
      <td>29.059185</td>
      <td>0.0</td>
      <td>7.333349</td>
      <td>5.650050</td>
      <td>5.86</td>
      <td>0.095694</td>
      <td>0.294877</td>
      <td>0.0</td>
      <td>0.498229</td>
      <td>0.085830</td>
      <td>0.488</td>
      <td>6.729780</td>
      <td>0.715886</td>
      <td>6.635</td>
      <td>53.746411</td>
      <td>28.686963</td>
      <td>52.6</td>
      <td>4.477212</td>
      <td>2.099729</td>
      <td>4.0220</td>
      <td>6.263158</td>
      <td>5.720462</td>
      <td>5.0</td>
      <td>327.315789</td>
      <td>118.667991</td>
      <td>296.0</td>
      <td>17.293301</td>
      <td>2.098982</td>
      <td>17.6</td>
      <td>384.215885</td>
      <td>30.106473</td>
      <td>392.78</td>
      <td>7.218852</td>
      <td>3.643771</td>
      <td>6.58</td>
      <td>30.579426</td>
      <td>8.308060</td>
      <td>27.9</td>
    </tr>
  </tbody>
</table>
</div>

### Standardization

#### MinMax Scaler

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
```


```python
x = df.values #returns a numpy array
min_max_scaler = preprocessing.MinMaxScaler()
x_scaled = min_max_scaler.fit_transform(x)
df_mmstd = pd.DataFrame(x_scaled, columns=df.columns)
df_mmstd.drop('FlagMEDV', axis=1,inplace=True)
df_mmstd.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.040544</td>
      <td>0.113636</td>
      <td>0.391378</td>
      <td>0.069170</td>
      <td>0.349167</td>
      <td>0.521869</td>
      <td>0.676364</td>
      <td>0.242381</td>
      <td>0.371713</td>
      <td>0.422208</td>
      <td>0.622929</td>
      <td>0.898568</td>
      <td>0.301409</td>
      <td>0.389618</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.096679</td>
      <td>0.233225</td>
      <td>0.251479</td>
      <td>0.253994</td>
      <td>0.238431</td>
      <td>0.134627</td>
      <td>0.289896</td>
      <td>0.191482</td>
      <td>0.378576</td>
      <td>0.321636</td>
      <td>0.230313</td>
      <td>0.230205</td>
      <td>0.197049</td>
      <td>0.204380</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000851</td>
      <td>0.000000</td>
      <td>0.173387</td>
      <td>0.000000</td>
      <td>0.131687</td>
      <td>0.445392</td>
      <td>0.433831</td>
      <td>0.088259</td>
      <td>0.130435</td>
      <td>0.175573</td>
      <td>0.510638</td>
      <td>0.945730</td>
      <td>0.144040</td>
      <td>0.267222</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.002812</td>
      <td>0.000000</td>
      <td>0.338343</td>
      <td>0.000000</td>
      <td>0.314815</td>
      <td>0.507281</td>
      <td>0.768280</td>
      <td>0.188949</td>
      <td>0.173913</td>
      <td>0.272901</td>
      <td>0.686170</td>
      <td>0.986232</td>
      <td>0.265728</td>
      <td>0.360000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.041258</td>
      <td>0.125000</td>
      <td>0.646628</td>
      <td>0.000000</td>
      <td>0.491770</td>
      <td>0.586798</td>
      <td>0.938980</td>
      <td>0.369088</td>
      <td>1.000000</td>
      <td>0.914122</td>
      <td>0.808511</td>
      <td>0.998298</td>
      <td>0.420116</td>
      <td>0.444444</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### Z-score Methods



```python
from scipy.stats import zscore
numeric_cols = df.select_dtypes(include=[np.number]).columns
df[numeric_cols].apply(zscore)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.419782</td>
      <td>0.284830</td>
      <td>-1.287909</td>
      <td>-0.272599</td>
      <td>-0.144217</td>
      <td>0.413672</td>
      <td>-0.120013</td>
      <td>0.140214</td>
      <td>-0.982843</td>
      <td>-0.666608</td>
      <td>-1.459000</td>
      <td>0.441052</td>
      <td>-1.075562</td>
      <td>0.159686</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.417339</td>
      <td>-0.487722</td>
      <td>-0.593381</td>
      <td>-0.272599</td>
      <td>-0.740262</td>
      <td>0.194274</td>
      <td>0.367166</td>
      <td>0.557160</td>
      <td>-0.867883</td>
      <td>-0.987329</td>
      <td>-0.303094</td>
      <td>0.441052</td>
      <td>-0.492439</td>
      <td>-0.101524</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.417342</td>
      <td>-0.487722</td>
      <td>-0.593381</td>
      <td>-0.272599</td>
      <td>-0.740262</td>
      <td>1.282714</td>
      <td>-0.265812</td>
      <td>0.557160</td>
      <td>-0.867883</td>
      <td>-0.987329</td>
      <td>-0.303094</td>
      <td>0.396427</td>
      <td>-1.208727</td>
      <td>1.324247</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.416750</td>
      <td>-0.487722</td>
      <td>-1.306878</td>
      <td>-0.272599</td>
      <td>-0.835284</td>
      <td>1.016303</td>
      <td>-0.809889</td>
      <td>1.077737</td>
      <td>-0.752922</td>
      <td>-1.106115</td>
      <td>0.113032</td>
      <td>0.416163</td>
      <td>-1.361517</td>
      <td>1.182758</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.412482</td>
      <td>-0.487722</td>
      <td>-1.306878</td>
      <td>-0.272599</td>
      <td>-0.835284</td>
      <td>1.228577</td>
      <td>-0.511180</td>
      <td>1.077737</td>
      <td>-0.752922</td>
      <td>-1.106115</td>
      <td>0.113032</td>
      <td>0.441052</td>
      <td>-1.026501</td>
      <td>1.487503</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>501</th>
      <td>-0.413229</td>
      <td>-0.487722</td>
      <td>0.115738</td>
      <td>-0.272599</td>
      <td>0.158124</td>
      <td>0.439316</td>
      <td>0.018673</td>
      <td>-0.625796</td>
      <td>-0.982843</td>
      <td>-0.803212</td>
      <td>1.176466</td>
      <td>0.387217</td>
      <td>-0.418147</td>
      <td>-0.014454</td>
    </tr>
    <tr>
      <th>502</th>
      <td>-0.415249</td>
      <td>-0.487722</td>
      <td>0.115738</td>
      <td>-0.272599</td>
      <td>0.158124</td>
      <td>-0.234548</td>
      <td>0.288933</td>
      <td>-0.716639</td>
      <td>-0.982843</td>
      <td>-0.803212</td>
      <td>1.176466</td>
      <td>0.441052</td>
      <td>-0.500850</td>
      <td>-0.210362</td>
    </tr>
    <tr>
      <th>503</th>
      <td>-0.413447</td>
      <td>-0.487722</td>
      <td>0.115738</td>
      <td>-0.272599</td>
      <td>0.158124</td>
      <td>0.984960</td>
      <td>0.797449</td>
      <td>-0.773684</td>
      <td>-0.982843</td>
      <td>-0.803212</td>
      <td>1.176466</td>
      <td>0.441052</td>
      <td>-0.983048</td>
      <td>0.148802</td>
    </tr>
    <tr>
      <th>504</th>
      <td>-0.407764</td>
      <td>-0.487722</td>
      <td>0.115738</td>
      <td>-0.272599</td>
      <td>0.158124</td>
      <td>0.725672</td>
      <td>0.736996</td>
      <td>-0.668437</td>
      <td>-0.982843</td>
      <td>-0.803212</td>
      <td>1.176466</td>
      <td>0.403225</td>
      <td>-0.865302</td>
      <td>-0.057989</td>
    </tr>
    <tr>
      <th>505</th>
      <td>-0.415000</td>
      <td>-0.487722</td>
      <td>0.115738</td>
      <td>-0.272599</td>
      <td>0.158124</td>
      <td>-0.362767</td>
      <td>0.434732</td>
      <td>-0.613246</td>
      <td>-0.982843</td>
      <td>-0.803212</td>
      <td>1.176466</td>
      <td>0.441052</td>
      <td>-0.669058</td>
      <td>-1.157248</td>
    </tr>
  </tbody>
</table>
<p>506 rows × 14 columns</p>
</div>

## **Matplotlib**




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




    [<matplotlib.lines.Line2D at 0x7f027d607d68>]




![png](/assets/200217/output_27_1.png)



```python
plt.plot(df.index, df.MEDV, '-o', color="red")
```




    [<matplotlib.lines.Line2D at 0x7f027d5dcc50>]




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




    <matplotlib.collections.PathCollection at 0x7f029103dd68>




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
size = 1000* rng.rand(100)
plt.scatter(df.CRIM, df.MEDV, c=colors, s =size, alpha=0.3, cmap='viridis')
plt.colorbar()
```




    <matplotlib.colorbar.Colorbar at 0x7f028e68fda0>




![png](/assets/200217/output_36_1.png)


### Basic Errorbars


```python
x = np.linspace(0, 10, 50) 
dy=0.8
y = np.sin(x) + dy * np.random.randn(50)
plt.errorbar(x, y, yerr=dy, fmt='.k')
```




    <ErrorbarContainer object of 3 artists>




![png](/assets/200217/output_38_1.png)


### Save plots


```python
# Save the plots, then you can access the file on your right hand side
# Another way to do it, just right click the plots and select save images...
plt.savefig('fancy.png')
```


    <Figure size 432x288 with 0 Axes>


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




    <seaborn.axisgrid.PairGrid at 0x7f028d789128>




![png](/assets/200217/output_48_1.png)


### Correlation Matrix


```python
sns.set(rc={'figure.figsize':(11.7,8.27)})
correlation_matrix = df.corr().round(2)
sns.heatmap(data=correlation_matrix, annot=True)
# annot = True to print the values inside the square
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f028d47b0b8>




![png](/assets/200217/output_50_1.png)


### Boxplot 


```python
sns.boxplot("FlagMEDV","RM", data=df, palette="Set3")
#sns.swarmplot("FlagMEDV","RM", data=df, color=".25")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f028d473860>




![png](/assets/200217/output_52_1.png)



```python
sns.violinplot('FlagMEDV','AGE', data=df, palette=["lightblue","lightpink"])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f028d473d68>




![png](/assets/200217/output_53_1.png)