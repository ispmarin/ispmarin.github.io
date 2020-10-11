---
title: Can I replace NaN with zeros?
categories: data-science
tags:
    - Python
    - Jupyter
date: 2018-07-09
---


This is a common question when working with data. There are lot of situations where we get a dataset and there are NaNs (Not a Number) values on it. We want to plot the results or analyse the values, so we ask: can we replace the NaN values with zeros? The answer is *it depends*. Let's demonstrate the effects of replacing them to understand the impact of doing this.

Let's assume that we are measuring the height of the buildings in downtown São Paulo. 


```python
%matplotlib inline

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```


```python
buildings = {
    'building': ['copan', 'italia', 'bb', 'martinelli', 'altino_arantes', 'mirante_do_vale'],
    'height':[140, 168,143, 130, 161, 170]
}
```


```python
df = pd.DataFrame(buildings)
```

The average height of the buildings in SP is calculated below:


```python
df.height.mean()
```




    152.0



This considers that we know all values on the dataset. What happens if a few of the values are missing?


```python
df.loc[df.building == 'bb', 'height'] = np.nan
```


```python
df
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
      <th>building</th>
      <th>height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>copan</td>
      <td>140.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>italia</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>bb</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>martinelli</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>altino_arantes</td>
      <td>161.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>mirante_do_vale</td>
      <td>170.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.height.mean()
```




    153.8



Removing one data point from this list changed the average, but that's expected. Let's see now what happens if we fill this with zero:


```python
df.fillna(0).height.mean()
```




    128.16666666666666



That changed the average height by a lot, so maybe filling with zeros is not a good idea. Let's see what happens if we have more missing data:


```python
df.loc[df.building == 'martinelli', 'height'] = np.nan
df.height.mean()
```




    159.75




```python
df.fillna(0).height.mean()
```




    106.5



That's even worse. And remember, we usually **don't** have the source dataset to know what the values that we're filling with zero are! So what should we do? How should we work with this missing data? There are a few approaches. We will one of them: filling with average.

## Filling NaN with averages

The idea is to calculate the average values of our dataset and fill the missing value with this average. Let's see what happens:


```python
df = pd.DataFrame(buildings)
df.loc[df.building == 'copan', 'height'] = np.nan
df = df.fillna(df.height.mean())
df
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
      <th>building</th>
      <th>height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>copan</td>
      <td>154.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>italia</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>bb</td>
      <td>143.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>martinelli</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>altino_arantes</td>
      <td>161.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>mirante_do_vale</td>
      <td>170.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.height.mean()
```




    154.4



Well, not too bad, but this can be highly dependent on the data, so be careful with replacing with the average value.


```python
df = pd.DataFrame(buildings)
df.loc[df.building.isin(['martinelli', 'copan']), 'height'] = np.nan
df = df.fillna(df.height.mean())
df
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
      <th>building</th>
      <th>height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>copan</td>
      <td>160.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>italia</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>bb</td>
      <td>143.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>martinelli</td>
      <td>160.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>altino_arantes</td>
      <td>161.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>mirante_do_vale</td>
      <td>170.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.height.mean()
```




    160.5



See how the average went way up? We have a very small dataset here and with a small value distribution, so the changes are not that great, but if your dataset has a larger standard deviation things can get complicated pretty quickly.

## Conclusion

So which value should you input in your data? It depends on your data. If the average of your data is close to zero and the distribution of your data is close to zero, then replacing NaN with zero should not change the dataset distribution too much. But if the data is not centered around zero, usually is a bad idea to replace missing values with zero.

In the next post, we will show how changing values on a dataset changes its distribution and how to control that.
