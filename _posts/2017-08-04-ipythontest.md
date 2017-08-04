---

layout: post
title: "Including Jupyter Notebooks in Jekyll blog posts"
date: 2017-08-04 
comments: true

---

I've been looking for ways to incorporate Jupyter notebooks into my new blog. This post explains a process that I've come across, although there may be other ways to blog directly from iPython Notebooks. 

This methods allows you to include styled cell input and output (including graphs, Data Frames, etc.) 

When you have completed your iPython Notebook (`jekyll_test.ipynb` in this example), run the following command in your terminal to convert your `jekyll_test.ipynb` file into Markdown:  

`$ ipython nbconvert jekyll_test.ipynb --to markdown`  

You can read more about converting iPython Notebooks [here](https://ipython.org/ipython-doc/3/notebook/nbconvert.html). In your terminal you should see the following: 

```terminal
$ ipython nbconvert jekyll_test.ipynb --to markdown
[NbConvertApp] Converting notebook jekyll_test.ipynb to markdown
[NbConvertApp] Support files will be in jekyll_test_files/
[NbConvertApp] Making directory jekyll_test_files
[NbConvertApp] Writing 286 bytes to jekyll_test.md
```

This puts related image files from graphs into a new folder, `jekyll_test_files/`, so you will have to make sure the path matches up. I have created a top-level folder named `ipynb` to keep things orderly.

Next, copy the contents from the resulting Markdown folder into your blog post. I had to change the image file path so that images display properly.  

`![png](jekyll_test_files/jekyll_test_2_1.png)`  

I added `/ipynb/` to the beginning of the path:

`![png](/ipynb/jekyll_test_files/jekyll_test_2_1.png)`  

# Result 

Here is the result from a simple notebook that I used here: 

```python
import pandas as pd
```


```python
import glob
import matplotlib.pyplot as plt
%matplotlib inline  
import matplotlib
matplotlib.style.use('ggplot')
import numpy as np
```


```python
pathlists = ('C:\\Users\\531323\\Documents\\Monitoring_need4b/*00136536*.csv',
             'C:\\Users\\531323\\Documents\\Monitoring_need4b/*00136543*.csv',
            'C:\\Users\\531323\\Documents\\Monitoring_need4b/*00137916*.csv',
            'C:\\Users\\531323\\Documents\\Monitoring_need4b/*00137919*.csv',
            'C:\\Users\\531323\\Documents\\Monitoring_need4b/*00137922*.csv',
            'C:\\Users\\531323\\Documents\\Monitoring_need4b/*00137925*.csv')
                
```


```python
energy = {}
df_list= []
df2sortedlist = []
```


```python
for val in pathlists:
    allFiles = glob.glob(val)
    for filename in sorted(allFiles):
        df_list.append(pd.read_csv(filename,sep=";"))
        full_df = pd.concat(df_list)
    df2 = full_df.drop_duplicates()
    df2sorted = df2.sort_values('created')
    
    df2sorted['date'] = pd.to_datetime(df2sorted.created)

    #print(pathlists.index(val))
    energy[pathlists.index(val)] = pd.DataFrame(df2sorted[['date',
                                               'energy no-error,Wh,inst-value,0,0,0']])
    energy[pathlists.index(val)].plot(x="date",
                                     y="energy no-error,Wh,inst-value,0,0,0")
    df_list= []
    
    
    
```


![png](/ipynb/jekyll_test_files/output_4_0.png)



![png](output_4_1.png)



![png](output_4_2.png)



![png](output_4_3.png)



![png](output_4_4.png)



![png](output_4_5.png)



```python
# for the seventh meter (hot water meter)
path_HW = 'C:\\Users\\531323\\Documents\\Monitoring_need4b/*61657161*.csv'
```


```python
allFiles = glob.glob(path_HW)
for filename in sorted(allFiles):
    df_list.append(pd.read_csv(filename,sep=";"))
full_df = pd.concat(df_list)
```


```python

full_df['date'] = pd.to_datetime(full_df.created)
full_df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>serial-number</th>
      <th>device-identification</th>
      <th>created</th>
      <th>value-data-count</th>
      <th>fabrication-no,,inst-value,0,0,0</th>
      <th>energy,Wh,inst-value,0,0,0</th>
      <th>volume,m3,inst-value,0,0,0</th>
      <th>on-time,hour(s),inst-value,0,0,0</th>
      <th>flow-temp,�C,inst-value,0,0,0</th>
      <th>return-temp,�C,inst-value,0,0,0</th>
      <th>...</th>
      <th>volume-flow,m3/h,max-value,0,0,1</th>
      <th>energy,Wh,inst-value,1,0,1</th>
      <th>energy,Wh,inst-value,2,0,1</th>
      <th>volume,m3,inst-value,0,1,1</th>
      <th>volume,m3,inst-value,0,2,1</th>
      <th>energy,Wh,inst-value,0,3,1</th>
      <th>date,,inst-value,0,0,1</th>
      <th>manufacturer-specific,,inst-value,0,0,0</th>
      <th>error-flags-dev-spec,,inst-value,0,0,0</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16000368</td>
      <td>61657161</td>
      <td>2015-12-31 00:00:00</td>
      <td>0</td>
      <td>61657161</td>
      <td>302000</td>
      <td>6,70</td>
      <td>4217</td>
      <td>34,34</td>
      <td>18,69</td>
      <td>...</td>
      <td>0,000</td>
      <td>0</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>0</td>
      <td>1999-11-30 00:00:00</td>
      <td>000000005a010000530000000000000000000000000000...</td>
      <td>0</td>
      <td>2015-12-31 00:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16000368</td>
      <td>61657161</td>
      <td>2015-12-31 00:10:00</td>
      <td>0</td>
      <td>61657161</td>
      <td>302000</td>
      <td>6,70</td>
      <td>4218</td>
      <td>34,11</td>
      <td>18,94</td>
      <td>...</td>
      <td>0,000</td>
      <td>0</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>0</td>
      <td>1999-11-30 00:00:00</td>
      <td>000000005a010000530000000000000000000000000000...</td>
      <td>0</td>
      <td>2015-12-31 00:10:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16000368</td>
      <td>61657161</td>
      <td>2015-12-31 00:20:00</td>
      <td>0</td>
      <td>61657161</td>
      <td>302000</td>
      <td>6,70</td>
      <td>4218</td>
      <td>33,97</td>
      <td>19,11</td>
      <td>...</td>
      <td>0,000</td>
      <td>0</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>0</td>
      <td>1999-11-30 00:00:00</td>
      <td>000000005a010000530000000000000000000000000000...</td>
      <td>0</td>
      <td>2015-12-31 00:20:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16000368</td>
      <td>61657161</td>
      <td>2015-12-31 00:30:00</td>
      <td>0</td>
      <td>61657161</td>
      <td>302000</td>
      <td>6,70</td>
      <td>4218</td>
      <td>33,82</td>
      <td>19,23</td>
      <td>...</td>
      <td>0,000</td>
      <td>0</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>0</td>
      <td>1999-11-30 00:00:00</td>
      <td>000000005a010000530000000000000000000000000000...</td>
      <td>0</td>
      <td>2015-12-31 00:30:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16000368</td>
      <td>61657161</td>
      <td>2015-12-31 00:40:00</td>
      <td>0</td>
      <td>61657161</td>
      <td>302000</td>
      <td>6,70</td>
      <td>4218</td>
      <td>33,72</td>
      <td>19,32</td>
      <td>...</td>
      <td>0,000</td>
      <td>0</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>0</td>
      <td>1999-11-30 00:00:00</td>
      <td>000000005a010000530000000000000000000000000000...</td>
      <td>0</td>
      <td>2015-12-31 00:40:00</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 34 columns</p>
</div>




```python
df2 = full_df.drop_duplicates()
df2sorted = df2.sort_values('created')
df2sorted['date'] = pd.to_datetime(df2sorted.created)


```


```python
energy7 = df2sorted[['date','energy,Wh,inst-value,0,0,0']]
energy7.plot(x="date", y="energy,Wh,inst-value,0,0,0")
```




    <matplotlib.axes._subplots.AxesSubplot at 0xaf515f8>




![png](output_9_1.png)



```python
energy[6]=energy7 
#del energy['6']
```


```python
type(energy)
```




    dict




```python
for key,value in energy.iteritems():
    print value.plot(x='date')
```

    Axes(0.125,0.2;0.775x0.7)
    Axes(0.125,0.2;0.775x0.7)
    Axes(0.125,0.2;0.775x0.7)
    Axes(0.125,0.2;0.775x0.7)
    Axes(0.125,0.2;0.775x0.7)
    Axes(0.125,0.2;0.775x0.7)
    Axes(0.125,0.2;0.775x0.7)
    


![png](output_12_1.png)



![png](output_12_2.png)



![png](output_12_3.png)



![png](output_12_4.png)



![png](output_12_5.png)



![png](output_12_6.png)



![png](output_12_7.png)



```python
energy[6]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>energy,Wh,inst-value,0,0,0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-12-31 00:00:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-12-31 00:10:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-12-31 00:20:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-12-31 00:30:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-12-31 00:40:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2015-12-31 00:50:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2015-12-31 01:00:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015-12-31 01:10:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2015-12-31 01:20:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2015-12-31 01:30:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2015-12-31 01:40:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2015-12-31 01:50:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2015-12-31 02:00:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2015-12-31 02:10:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2015-12-31 02:20:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2015-12-31 02:30:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2015-12-31 02:40:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2015-12-31 02:50:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2015-12-31 03:00:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2015-12-31 03:10:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2015-12-31 03:20:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2015-12-31 03:30:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2015-12-31 03:40:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2015-12-31 03:50:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2015-12-31 04:00:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2015-12-31 04:10:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2015-12-31 04:20:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2015-12-31 04:30:01</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2015-12-31 04:40:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2015-12-31 04:50:00</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>330</th>
      <td>2017-07-02 07:00:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>331</th>
      <td>2017-07-02 07:10:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>332</th>
      <td>2017-07-02 07:20:06</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>333</th>
      <td>2017-07-02 07:30:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>334</th>
      <td>2017-07-02 07:40:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>335</th>
      <td>2017-07-02 07:50:04</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>336</th>
      <td>2017-07-02 08:00:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>337</th>
      <td>2017-07-02 08:10:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>338</th>
      <td>2017-07-02 08:20:04</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>339</th>
      <td>2017-07-02 08:30:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>340</th>
      <td>2017-07-02 08:40:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>341</th>
      <td>2017-07-02 08:50:05</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>342</th>
      <td>2017-07-02 09:00:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>343</th>
      <td>2017-07-02 09:10:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>344</th>
      <td>2017-07-02 09:20:05</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>345</th>
      <td>2017-07-02 09:30:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>346</th>
      <td>2017-07-02 09:40:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>347</th>
      <td>2017-07-02 09:50:05</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>348</th>
      <td>2017-07-02 10:00:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>349</th>
      <td>2017-07-02 10:10:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>350</th>
      <td>2017-07-02 10:20:05</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>351</th>
      <td>2017-07-02 10:30:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>352</th>
      <td>2017-07-02 10:40:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>353</th>
      <td>2017-07-02 10:50:06</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>354</th>
      <td>2017-07-02 11:00:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>355</th>
      <td>2017-07-02 11:10:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>356</th>
      <td>2017-07-02 11:20:05</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>357</th>
      <td>2017-07-02 11:30:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>358</th>
      <td>2017-07-02 11:40:03</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>359</th>
      <td>2017-07-02 11:50:06</td>
      <td>2222000</td>
    </tr>
  </tbody>
</table>
<p>79119 rows × 2 columns</p>
</div>




```python
energy[0]= energy[0].rename(index=str,
                 columns={"date":"date",
                       "energy no-error,Wh,inst-value,0,0,0":"energy1"})
energy[1]= energy[1].rename(index=str,
                 columns={"date":"date",
                       "energy no-error,Wh,inst-value,0,0,0":"energy2"})
energy[2]= energy[2].rename(index=str,
                 columns={"date":"date",
                       "energy no-error,Wh,inst-value,0,0,0":"energy3"})
energy[3]= energy[3].rename(index=str,
                 columns={"date":"date",
                       "energy no-error,Wh,inst-value,0,0,0":"energy4"})
energy[4]= energy[4].rename(index=str,
                 columns={"date":"date",
                       "energy no-error,Wh,inst-value,0,0,0":"energy5"})
energy[5]= energy[5].rename(index=str,
                 columns={"date":"date",
                       "energy no-error,Wh,inst-value,0,0,0":"energy6"})
energy[6]= energy[6].rename(index=str,
                 columns={"date":"date",
                       "energy,Wh,inst-value,0,0,0":"energy7"})
```


```python
energy[0]
dfs = [energy[0],energy[1],energy[2],energy[3],energy[4],energy[5],
      energy[6]]
dffinal = reduce(lambda left,right: pd.merge(left,right,on='date'),dfs )
```


```python
energy[0]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>energy1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-12-31 00:00:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-12-31 00:10:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-12-31 00:20:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-12-31 00:30:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-12-31 00:40:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2015-12-31 00:50:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2015-12-31 01:00:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015-12-31 01:10:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2015-12-31 01:20:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2015-12-31 01:30:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2015-12-31 01:40:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2015-12-31 01:50:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2015-12-31 02:00:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2015-12-31 02:10:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2015-12-31 02:20:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2015-12-31 02:30:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2015-12-31 02:40:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2015-12-31 02:50:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2015-12-31 03:00:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2015-12-31 03:10:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2015-12-31 03:20:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2015-12-31 03:30:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2015-12-31 03:40:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2015-12-31 03:50:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2015-12-31 04:00:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2015-12-31 04:10:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2015-12-31 04:20:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2015-12-31 04:30:01</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2015-12-31 04:40:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2015-12-31 04:50:00</td>
      <td>17990</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>331</th>
      <td>2017-07-02 07:10:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>332</th>
      <td>2017-07-02 07:20:06</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>333</th>
      <td>2017-07-02 07:30:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>334</th>
      <td>2017-07-02 07:40:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>335</th>
      <td>2017-07-02 07:50:04</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>336</th>
      <td>2017-07-02 08:00:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>337</th>
      <td>2017-07-02 08:10:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>338</th>
      <td>2017-07-02 08:20:04</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>339</th>
      <td>2017-07-02 08:30:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>340</th>
      <td>2017-07-02 08:40:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>341</th>
      <td>2017-07-02 08:50:05</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>342</th>
      <td>2017-07-02 09:00:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>343</th>
      <td>2017-07-02 09:10:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>344</th>
      <td>2017-07-02 09:20:05</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>345</th>
      <td>2017-07-02 09:30:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>346</th>
      <td>2017-07-02 09:40:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>347</th>
      <td>2017-07-02 09:50:05</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>348</th>
      <td>2017-07-02 10:00:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>349</th>
      <td>2017-07-02 10:10:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>350</th>
      <td>2017-07-02 10:20:05</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>351</th>
      <td>2017-07-02 10:30:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>352</th>
      <td>2017-07-02 10:40:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>353</th>
      <td>2017-07-02 10:50:06</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>354</th>
      <td>2017-07-02 11:00:03</td>
      <td>597840</td>
    </tr>
    <tr>
      <th>355</th>
      <td>2017-07-02 11:10:03</td>
      <td>597850</td>
    </tr>
    <tr>
      <th>356</th>
      <td>2017-07-02 11:20:05</td>
      <td>597850</td>
    </tr>
    <tr>
      <th>357</th>
      <td>2017-07-02 11:30:03</td>
      <td>597850</td>
    </tr>
    <tr>
      <th>358</th>
      <td>2017-07-02 11:40:03</td>
      <td>597850</td>
    </tr>
    <tr>
      <th>359</th>
      <td>2017-07-02 11:50:06</td>
      <td>597850</td>
    </tr>
    <tr>
      <th>360</th>
      <td>2017-07-02 12:00:04</td>
      <td>597850</td>
    </tr>
  </tbody>
</table>
<p>79121 rows × 2 columns</p>
</div>




```python

```


```python
dffinal
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>energy1</th>
      <th>energy2</th>
      <th>energy3</th>
      <th>energy4</th>
      <th>energy5</th>
      <th>energy6</th>
      <th>energy7</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-12-31 00:00:00</td>
      <td>17990</td>
      <td>154930</td>
      <td>603630</td>
      <td>48010</td>
      <td>69900</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-12-31 00:10:00</td>
      <td>17990</td>
      <td>155000</td>
      <td>603680</td>
      <td>48010</td>
      <td>69910</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-12-31 00:20:00</td>
      <td>17990</td>
      <td>155060</td>
      <td>603720</td>
      <td>48010</td>
      <td>69910</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-12-31 00:30:00</td>
      <td>17990</td>
      <td>155130</td>
      <td>603770</td>
      <td>48010</td>
      <td>69920</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-12-31 00:40:00</td>
      <td>17990</td>
      <td>155200</td>
      <td>603810</td>
      <td>48010</td>
      <td>69920</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2015-12-31 00:50:00</td>
      <td>17990</td>
      <td>155270</td>
      <td>603860</td>
      <td>48010</td>
      <td>69930</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2015-12-31 01:00:00</td>
      <td>17990</td>
      <td>155340</td>
      <td>603910</td>
      <td>48010</td>
      <td>69930</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015-12-31 01:10:00</td>
      <td>17990</td>
      <td>155410</td>
      <td>603950</td>
      <td>48010</td>
      <td>69940</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2015-12-31 01:20:00</td>
      <td>17990</td>
      <td>155480</td>
      <td>603990</td>
      <td>48010</td>
      <td>69940</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2015-12-31 01:30:00</td>
      <td>17990</td>
      <td>155550</td>
      <td>604020</td>
      <td>48020</td>
      <td>69950</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2015-12-31 01:40:00</td>
      <td>17990</td>
      <td>155630</td>
      <td>604060</td>
      <td>48020</td>
      <td>69950</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2015-12-31 01:50:00</td>
      <td>17990</td>
      <td>155700</td>
      <td>604100</td>
      <td>48020</td>
      <td>69960</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2015-12-31 02:00:00</td>
      <td>17990</td>
      <td>155780</td>
      <td>604140</td>
      <td>48020</td>
      <td>69960</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2015-12-31 02:10:00</td>
      <td>17990</td>
      <td>155850</td>
      <td>604180</td>
      <td>48020</td>
      <td>69970</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2015-12-31 02:20:00</td>
      <td>17990</td>
      <td>155930</td>
      <td>604230</td>
      <td>48020</td>
      <td>69970</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2015-12-31 02:30:00</td>
      <td>17990</td>
      <td>156010</td>
      <td>604270</td>
      <td>48020</td>
      <td>69980</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2015-12-31 02:40:00</td>
      <td>17990</td>
      <td>156090</td>
      <td>604320</td>
      <td>48020</td>
      <td>69980</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2015-12-31 02:50:00</td>
      <td>17990</td>
      <td>156170</td>
      <td>604360</td>
      <td>48020</td>
      <td>69990</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2015-12-31 03:00:00</td>
      <td>17990</td>
      <td>156250</td>
      <td>604380</td>
      <td>48020</td>
      <td>69990</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2015-12-31 03:10:00</td>
      <td>17990</td>
      <td>156330</td>
      <td>604410</td>
      <td>48020</td>
      <td>70000</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2015-12-31 03:20:00</td>
      <td>17990</td>
      <td>156410</td>
      <td>604430</td>
      <td>48020</td>
      <td>70000</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2015-12-31 03:30:00</td>
      <td>17990</td>
      <td>156500</td>
      <td>604470</td>
      <td>48020</td>
      <td>70010</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2015-12-31 03:40:00</td>
      <td>17990</td>
      <td>156580</td>
      <td>604520</td>
      <td>48020</td>
      <td>70010</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2015-12-31 03:50:00</td>
      <td>17990</td>
      <td>156660</td>
      <td>604560</td>
      <td>48030</td>
      <td>70020</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2015-12-31 04:00:00</td>
      <td>17990</td>
      <td>156750</td>
      <td>604590</td>
      <td>48030</td>
      <td>70020</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2015-12-31 04:10:00</td>
      <td>17990</td>
      <td>157220</td>
      <td>604640</td>
      <td>48030</td>
      <td>70030</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2015-12-31 04:20:00</td>
      <td>17990</td>
      <td>157700</td>
      <td>604670</td>
      <td>48030</td>
      <td>70030</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2015-12-31 04:30:01</td>
      <td>17990</td>
      <td>158180</td>
      <td>604720</td>
      <td>48030</td>
      <td>70040</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2015-12-31 04:40:00</td>
      <td>17990</td>
      <td>158670</td>
      <td>604770</td>
      <td>48030</td>
      <td>70040</td>
      <td>0</td>
      <td>302000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2015-12-31 04:50:00</td>
      <td>17990</td>
      <td>158900</td>
      <td>604810</td>
      <td>48030</td>
      <td>70050</td>
      <td>0</td>
      <td>302000</td>
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
    </tr>
    <tr>
      <th>79089</th>
      <td>2017-07-02 07:00:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292220</td>
      <td>303740</td>
      <td>752130</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79090</th>
      <td>2017-07-02 07:10:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292280</td>
      <td>303740</td>
      <td>752140</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79091</th>
      <td>2017-07-02 07:20:06</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292330</td>
      <td>303740</td>
      <td>752140</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79092</th>
      <td>2017-07-02 07:30:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292380</td>
      <td>303740</td>
      <td>752150</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79093</th>
      <td>2017-07-02 07:40:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292430</td>
      <td>303740</td>
      <td>752150</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79094</th>
      <td>2017-07-02 07:50:04</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292480</td>
      <td>303750</td>
      <td>752160</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79095</th>
      <td>2017-07-02 08:00:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292540</td>
      <td>303750</td>
      <td>752160</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79096</th>
      <td>2017-07-02 08:10:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292580</td>
      <td>303750</td>
      <td>752170</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79097</th>
      <td>2017-07-02 08:20:04</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292630</td>
      <td>303750</td>
      <td>752180</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79098</th>
      <td>2017-07-02 08:30:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292660</td>
      <td>303750</td>
      <td>752180</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79099</th>
      <td>2017-07-02 08:40:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292720</td>
      <td>303750</td>
      <td>752200</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79100</th>
      <td>2017-07-02 08:50:05</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292750</td>
      <td>303750</td>
      <td>752210</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79101</th>
      <td>2017-07-02 09:00:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292780</td>
      <td>303750</td>
      <td>752220</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79102</th>
      <td>2017-07-02 09:10:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292810</td>
      <td>303750</td>
      <td>752230</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79103</th>
      <td>2017-07-02 09:20:05</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8292850</td>
      <td>303750</td>
      <td>752250</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79104</th>
      <td>2017-07-02 09:30:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293110</td>
      <td>303750</td>
      <td>752260</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79105</th>
      <td>2017-07-02 09:40:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293500</td>
      <td>303750</td>
      <td>752270</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79106</th>
      <td>2017-07-02 09:50:05</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293600</td>
      <td>303750</td>
      <td>752280</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79107</th>
      <td>2017-07-02 10:00:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293690</td>
      <td>303750</td>
      <td>752290</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79108</th>
      <td>2017-07-02 10:10:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293750</td>
      <td>303750</td>
      <td>752310</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79109</th>
      <td>2017-07-02 10:20:05</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293830</td>
      <td>303750</td>
      <td>752320</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79110</th>
      <td>2017-07-02 10:30:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8293920</td>
      <td>303750</td>
      <td>752330</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79111</th>
      <td>2017-07-02 10:40:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8294000</td>
      <td>303750</td>
      <td>752340</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79112</th>
      <td>2017-07-02 10:50:06</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8294080</td>
      <td>303750</td>
      <td>752360</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79113</th>
      <td>2017-07-02 11:00:03</td>
      <td>597840</td>
      <td>1347820</td>
      <td>8294180</td>
      <td>303750</td>
      <td>752370</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79114</th>
      <td>2017-07-02 11:10:03</td>
      <td>597850</td>
      <td>1347820</td>
      <td>8294300</td>
      <td>303750</td>
      <td>752380</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79115</th>
      <td>2017-07-02 11:20:05</td>
      <td>597850</td>
      <td>1347820</td>
      <td>8294410</td>
      <td>303750</td>
      <td>752390</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79116</th>
      <td>2017-07-02 11:30:03</td>
      <td>597850</td>
      <td>1347820</td>
      <td>8294670</td>
      <td>303750</td>
      <td>752400</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79117</th>
      <td>2017-07-02 11:40:03</td>
      <td>597850</td>
      <td>1347820</td>
      <td>8295080</td>
      <td>303750</td>
      <td>752410</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
    <tr>
      <th>79118</th>
      <td>2017-07-02 11:50:06</td>
      <td>597850</td>
      <td>1347820</td>
      <td>8295230</td>
      <td>303750</td>
      <td>752430</td>
      <td>0</td>
      <td>2222000</td>
    </tr>
  </tbody>
</table>
<p>79119 rows × 8 columns</p>
</div>




```python
type(dffinal)
```




    pandas.core.frame.DataFrame




```python
dffinal.plot(x="date")
```




    <matplotlib.axes._subplots.AxesSubplot at 0xa76a898>




![png](output_20_1.png)



```python
dffinal.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 79119 entries, 0 to 79118
    Data columns (total 8 columns):
    date       79119 non-null datetime64[ns]
    energy1    79119 non-null int64
    energy2    79119 non-null int64
    energy3    79119 non-null int64
    energy4    79119 non-null int64
    energy5    79119 non-null int64
    energy6    79119 non-null int64
    energy7    79119 non-null int64
    dtypes: datetime64[ns](1), int64(7)
    memory usage: 5.4 MB
    


```python
dffinal.isnull().sum()
```




    date       0
    energy1    0
    energy2    0
    energy3    0
    energy4    0
    energy5    0
    energy6    0
    energy7    0
    dtype: int64




```python
from pandas import Series, DataFrame
import pandas as pd
from datetime import datetime, timedelta
import numpy as np

def rolling_mean(data, window, min_periods=1, center=False):
    ''' Function that computes a rolling mean

    Parameters
    ----------
    data : DataFrame or Series
           If a DataFrame is passed, the rolling_mean is computed for all columns.
    window : int or string
             If int is passed, window is the number of observations used for calculating 
             the statistic, as defined by the function pd.rolling_mean()
             If a string is passed, it must be a frequency string, e.g. '90S'. This is
             internally converted into a DateOffset object, representing the window size.
    min_periods : int
                  Minimum number of observations in window required to have a value.

    Returns
    -------
    Series or DataFrame, if more than one column    
    '''
    def f(x):
        '''Function to apply that actually computes the rolling mean'''
        if center == False:
            dslice = col[x-pd.datetools.to_offset(window).delta+timedelta(0,0,1):x]
                # adding a microsecond because when slicing with labels start and endpoint
                # are inclusive
        else:
            dslice = col[x-pd.datetools.to_offset(window).delta/2+timedelta(0,0,1):
                         x+pd.datetools.to_offset(window).delta/2]
        if dslice.size < min_periods:
            return np.nan
        else:
            return dslice.mean()

    data = DataFrame(data.copy())
    dfout = DataFrame()
    if isinstance(window, int):
        dfout = pd.rolling_mean(data, window, min_periods=min_periods, center=center)
    elif isinstance(window, basestring):
        idx = Series(data.index.to_pydatetime(), index=data.index)
        for colname, col in data.iterkv():
            result = idx.apply(f)
            result.name = colname
            dfout = dfout.join(result, how='outer')
    if dfout.columns.size == 1:
        dfout = dfout.ix[:,0]
    return dfout


```


```python
#s = Series(dffinal['energy1'])
#rm = rolling_mean(dffinal, window='10min')
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-67-78fd7c165bf2> in <module>()
    ----> 1 s = Series(dffinal['energy1'])
          2 rm = rolling_mean(dffinal, window='10min')
    

    NameError: name 'Series' is not defined



```python
dffinal = dffinal.set_index(['date'])
```


```python
dffinal2 =  dffinal.resample("10min").ffill().rolling(window=1).mean()
```


```python
dffinal2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>energy1</th>
      <th>energy2</th>
      <th>energy3</th>
      <th>energy4</th>
      <th>energy5</th>
      <th>energy6</th>
      <th>energy7</th>
    </tr>
    <tr>
      <th>date</th>
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
      <th>2015-12-31 00:00:00</th>
      <td>17990.0</td>
      <td>154930.0</td>
      <td>603630.0</td>
      <td>48010.0</td>
      <td>69900.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:10:00</th>
      <td>17990.0</td>
      <td>155000.0</td>
      <td>603680.0</td>
      <td>48010.0</td>
      <td>69910.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:20:00</th>
      <td>17990.0</td>
      <td>155060.0</td>
      <td>603720.0</td>
      <td>48010.0</td>
      <td>69910.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:30:00</th>
      <td>17990.0</td>
      <td>155130.0</td>
      <td>603770.0</td>
      <td>48010.0</td>
      <td>69920.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:40:00</th>
      <td>17990.0</td>
      <td>155200.0</td>
      <td>603810.0</td>
      <td>48010.0</td>
      <td>69920.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:50:00</th>
      <td>17990.0</td>
      <td>155270.0</td>
      <td>603860.0</td>
      <td>48010.0</td>
      <td>69930.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:00:00</th>
      <td>17990.0</td>
      <td>155340.0</td>
      <td>603910.0</td>
      <td>48010.0</td>
      <td>69930.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:10:00</th>
      <td>17990.0</td>
      <td>155410.0</td>
      <td>603950.0</td>
      <td>48010.0</td>
      <td>69940.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:20:00</th>
      <td>17990.0</td>
      <td>155480.0</td>
      <td>603990.0</td>
      <td>48010.0</td>
      <td>69940.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:30:00</th>
      <td>17990.0</td>
      <td>155550.0</td>
      <td>604020.0</td>
      <td>48020.0</td>
      <td>69950.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:40:00</th>
      <td>17990.0</td>
      <td>155630.0</td>
      <td>604060.0</td>
      <td>48020.0</td>
      <td>69950.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:50:00</th>
      <td>17990.0</td>
      <td>155700.0</td>
      <td>604100.0</td>
      <td>48020.0</td>
      <td>69960.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:00:00</th>
      <td>17990.0</td>
      <td>155780.0</td>
      <td>604140.0</td>
      <td>48020.0</td>
      <td>69960.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:10:00</th>
      <td>17990.0</td>
      <td>155850.0</td>
      <td>604180.0</td>
      <td>48020.0</td>
      <td>69970.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:20:00</th>
      <td>17990.0</td>
      <td>155930.0</td>
      <td>604230.0</td>
      <td>48020.0</td>
      <td>69970.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:30:00</th>
      <td>17990.0</td>
      <td>156010.0</td>
      <td>604270.0</td>
      <td>48020.0</td>
      <td>69980.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:40:00</th>
      <td>17990.0</td>
      <td>156090.0</td>
      <td>604320.0</td>
      <td>48020.0</td>
      <td>69980.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:50:00</th>
      <td>17990.0</td>
      <td>156170.0</td>
      <td>604360.0</td>
      <td>48020.0</td>
      <td>69990.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 03:00:00</th>
      <td>17990.0</td>
      <td>156250.0</td>
      <td>604380.0</td>
      <td>48020.0</td>
      <td>69990.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 03:10:00</th>
      <td>17990.0</td>
      <td>156330.0</td>
      <td>604410.0</td>
      <td>48020.0</td>
      <td>70000.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 03:20:00</th>
      <td>17990.0</td>
      <td>156410.0</td>
      <td>604430.0</td>
      <td>48020.0</td>
      <td>70000.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 03:30:00</th>
      <td>17990.0</td>
      <td>156500.0</td>
      <td>604470.0</td>
      <td>48020.0</td>
      <td>70010.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 03:40:00</th>
      <td>17990.0</td>
      <td>156580.0</td>
      <td>604520.0</td>
      <td>48020.0</td>
      <td>70010.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 03:50:00</th>
      <td>17990.0</td>
      <td>156660.0</td>
      <td>604560.0</td>
      <td>48030.0</td>
      <td>70020.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 04:00:00</th>
      <td>17990.0</td>
      <td>156750.0</td>
      <td>604590.0</td>
      <td>48030.0</td>
      <td>70020.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 04:10:00</th>
      <td>17990.0</td>
      <td>157220.0</td>
      <td>604640.0</td>
      <td>48030.0</td>
      <td>70030.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 04:20:00</th>
      <td>17990.0</td>
      <td>157700.0</td>
      <td>604670.0</td>
      <td>48030.0</td>
      <td>70030.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 04:30:00</th>
      <td>17990.0</td>
      <td>157700.0</td>
      <td>604670.0</td>
      <td>48030.0</td>
      <td>70030.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 04:40:00</th>
      <td>17990.0</td>
      <td>158670.0</td>
      <td>604770.0</td>
      <td>48030.0</td>
      <td>70040.0</td>
      <td>0.0</td>
      <td>302000.0</td>
    </tr>
    <tr>
      <th>2015-12-31 04:50:00</th>
      <td>17990.0</td>
      <td>158900.0</td>
      <td>604810.0</td>
      <td>48030.0</td>
      <td>70050.0</td>
      <td>0.0</td>
      <td>302000.0</td>
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
    </tr>
    <tr>
      <th>2017-07-02 07:00:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292160.0</td>
      <td>303740.0</td>
      <td>752120.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 07:10:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292220.0</td>
      <td>303740.0</td>
      <td>752130.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 07:20:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292280.0</td>
      <td>303740.0</td>
      <td>752140.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 07:30:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292330.0</td>
      <td>303740.0</td>
      <td>752140.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 07:40:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292380.0</td>
      <td>303740.0</td>
      <td>752150.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 07:50:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292430.0</td>
      <td>303740.0</td>
      <td>752150.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 08:00:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292480.0</td>
      <td>303750.0</td>
      <td>752160.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 08:10:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292540.0</td>
      <td>303750.0</td>
      <td>752160.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 08:20:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292580.0</td>
      <td>303750.0</td>
      <td>752170.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 08:30:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292630.0</td>
      <td>303750.0</td>
      <td>752180.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 08:40:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292660.0</td>
      <td>303750.0</td>
      <td>752180.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 08:50:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292720.0</td>
      <td>303750.0</td>
      <td>752200.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 09:00:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292750.0</td>
      <td>303750.0</td>
      <td>752210.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 09:10:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292780.0</td>
      <td>303750.0</td>
      <td>752220.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 09:20:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292810.0</td>
      <td>303750.0</td>
      <td>752230.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 09:30:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8292850.0</td>
      <td>303750.0</td>
      <td>752250.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 09:40:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293110.0</td>
      <td>303750.0</td>
      <td>752260.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 09:50:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293500.0</td>
      <td>303750.0</td>
      <td>752270.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 10:00:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293600.0</td>
      <td>303750.0</td>
      <td>752280.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 10:10:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293690.0</td>
      <td>303750.0</td>
      <td>752290.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 10:20:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293750.0</td>
      <td>303750.0</td>
      <td>752310.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 10:30:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293830.0</td>
      <td>303750.0</td>
      <td>752320.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 10:40:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8293920.0</td>
      <td>303750.0</td>
      <td>752330.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 10:50:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8294000.0</td>
      <td>303750.0</td>
      <td>752340.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 11:00:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8294080.0</td>
      <td>303750.0</td>
      <td>752360.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 11:10:00</th>
      <td>597840.0</td>
      <td>1347820.0</td>
      <td>8294180.0</td>
      <td>303750.0</td>
      <td>752370.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 11:20:00</th>
      <td>597850.0</td>
      <td>1347820.0</td>
      <td>8294300.0</td>
      <td>303750.0</td>
      <td>752380.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 11:30:00</th>
      <td>597850.0</td>
      <td>1347820.0</td>
      <td>8294410.0</td>
      <td>303750.0</td>
      <td>752390.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 11:40:00</th>
      <td>597850.0</td>
      <td>1347820.0</td>
      <td>8294670.0</td>
      <td>303750.0</td>
      <td>752400.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
    <tr>
      <th>2017-07-02 11:50:00</th>
      <td>597850.0</td>
      <td>1347820.0</td>
      <td>8295080.0</td>
      <td>303750.0</td>
      <td>752410.0</td>
      <td>0.0</td>
      <td>2222000.0</td>
    </tr>
  </tbody>
</table>
<p>79128 rows × 7 columns</p>
</div>




```python
dffinal2.diff(periods=1,axis=0).plot(y='energy3')
```




    <matplotlib.axes._subplots.AxesSubplot at 0xa650b70>




![png](output_28_1.png)



```python
dffinal2.diff(periods=1,axis=0).plot(y='energy1')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x4e56cb70>




![png](output_29_1.png)



```python
dffinal2.diff().head(18)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>energy1</th>
      <th>energy2</th>
      <th>energy3</th>
      <th>energy4</th>
      <th>energy5</th>
      <th>energy6</th>
      <th>energy7</th>
    </tr>
    <tr>
      <th>date</th>
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
      <th>2015-12-31 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2015-12-31 00:10:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:20:00</th>
      <td>0.0</td>
      <td>60.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:30:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:40:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:50:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:00:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:10:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:20:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:30:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>30.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:40:00</th>
      <td>0.0</td>
      <td>80.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:50:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:00:00</th>
      <td>0.0</td>
      <td>80.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:10:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:20:00</th>
      <td>0.0</td>
      <td>80.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:30:00</th>
      <td>0.0</td>
      <td>80.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:40:00</th>
      <td>0.0</td>
      <td>80.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 02:50:00</th>
      <td>0.0</td>
      <td>80.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



## Now processing the interior temperatures


```python
In_tempe = {}
df_tempe= []

```


```python
#C:\\Users\\531323\\Documents\\Monitoring_need4b/*00136536*.csv'
#C:\\Users\\531323\\Documents\\Monitoring_need4b/*00136536*.csv
tpth ='C:\\Users\\531323\\Documents\\Monitoring_need4b\\Temperaturedata\\20*'    
allFiles2 = glob.glob(tpth)
```


```python
my_cols = range(1,27)

for filename in sorted(allFiles2):
        
        df_tempe.append(pd.read_csv(filename,sep=",",names=my_cols))
        full_df = pd.concat(df_tempe)
dftemp = full_df

    
```


```python
dftemp.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>...</th>
      <th>17</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>06/01/2016</td>
      <td>00:00:16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>06/01/2016</td>
      <td>00:03:33</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>06/01/2016</td>
      <td>00:03:34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>06/01/2016</td>
      <td>00:06:51</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>06/01/2016</td>
      <td>00:09:35</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>




```python
dftemp.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>...</th>
      <th>17</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3634</th>
      <td>01/07/2017</td>
      <td>23:58:16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3635</th>
      <td>01/07/2017</td>
      <td>23:58:22</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.2</td>
      <td>45.9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3636</th>
      <td>01/07/2017</td>
      <td>23:58:31</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3637</th>
      <td>01/07/2017</td>
      <td>23:58:50</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.29</td>
      <td>44.79</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3638</th>
      <td>01/07/2017</td>
      <td>23:59:02</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>




```python
dftemp['date'] = pd.to_datetime(dftemp[1]+' ' +dftemp[2],format='%d/%m/%Y %H:%M:%S')
```


```python
type(dftemp['date'])
```




    pandas.core.series.Series




```python
dftemp.shape[0]
```




    2285499




```python
import random
mylen = dftemp.shape[0]
```


```python
[random.randint(0,1000) for r in xrange(19172)]

np.asarray([random.randint(0,10000) for r in xrange(mylen)])/100

```




    array([64, 28,  9, ...,  1, 43, 17])




```python
dftemp['extra'] = np.asarray([random.randint(0,30000) for r in xrange(mylen)])
```


```python
dftemp.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>...</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>date</th>
      <th>extra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>06/01/2016</td>
      <td>00:00:16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2016-01-06 00:00:16</td>
      <td>15638</td>
    </tr>
    <tr>
      <th>1</th>
      <td>06/01/2016</td>
      <td>00:03:33</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2016-01-06 00:03:33</td>
      <td>28273</td>
    </tr>
    <tr>
      <th>2</th>
      <td>06/01/2016</td>
      <td>00:03:34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2016-01-06 00:03:34</td>
      <td>16003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>06/01/2016</td>
      <td>00:06:51</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>55.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2016-01-06 00:06:51</td>
      <td>3332</td>
    </tr>
    <tr>
      <th>4</th>
      <td>06/01/2016</td>
      <td>00:09:35</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2016-01-06 00:09:35</td>
      <td>22412</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
dftemp.describe()
```

    C:\Users\531323\AppData\Local\Continuum\Anaconda2\lib\site-packages\numpy\lib\function_base.py:3834: RuntimeWarning: Invalid value encountered in percentile
      RuntimeWarning)
    




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>...</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>extra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>28146.0</td>
      <td>28148.0</td>
      <td>208486.00000</td>
      <td>208486.000000</td>
      <td>225703.000000</td>
      <td>225703.000000</td>
      <td>232219.000000</td>
      <td>232219.000000</td>
      <td>231707.000000</td>
      <td>231707.000000</td>
      <td>...</td>
      <td>295527.000000</td>
      <td>300831.000000</td>
      <td>300831.000000</td>
      <td>205062.000000</td>
      <td>205062.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.285499e+06</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>22.45891</td>
      <td>42.691307</td>
      <td>21.527089</td>
      <td>41.543835</td>
      <td>22.743341</td>
      <td>41.960500</td>
      <td>22.082157</td>
      <td>40.564842</td>
      <td>...</td>
      <td>37.635380</td>
      <td>22.325397</td>
      <td>45.426417</td>
      <td>20.381141</td>
      <td>42.888687</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.500382e+04</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.24264</td>
      <td>6.499668</td>
      <td>2.599933</td>
      <td>6.287669</td>
      <td>2.622477</td>
      <td>4.714986</td>
      <td>2.547873</td>
      <td>6.455259</td>
      <td>...</td>
      <td>6.926898</td>
      <td>2.853055</td>
      <td>6.456952</td>
      <td>2.970974</td>
      <td>5.233943</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.658613e+03</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>16.79000</td>
      <td>26.890000</td>
      <td>16.100000</td>
      <td>18.890000</td>
      <td>13.800000</td>
      <td>28.600000</td>
      <td>-1.010000</td>
      <td>-1.010000</td>
      <td>...</td>
      <td>17.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.890000</td>
      <td>29.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.497000e+03</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.500400e+04</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.251400e+04</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>28.79000</td>
      <td>94.400000</td>
      <td>32.790000</td>
      <td>66.190000</td>
      <td>30.200000</td>
      <td>99.800000</td>
      <td>28.390000</td>
      <td>92.800000</td>
      <td>...</td>
      <td>99.900000</td>
      <td>30.390000</td>
      <td>85.300000</td>
      <td>28.700000</td>
      <td>62.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.000000e+04</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 25 columns</p>
</div>




```python
dftemp['extra'].dtypes
```




    dtype('int32')




```python
#dftemp['extra'].apply(pd.Timedelta)+dftemp['date']
```


```python
dftemp['date2']= dftemp['extra'].apply(pd.Timedelta)+dftemp['date']

```


```python
#dftemp.head()

del dftemp['date']
```


```python
dftemp3= dftemp[dftemp.date2.duplicated(keep=False)]

a = dftemp3.groupby('date2').apply(lambda x: list(x.index))
#print (a)
len(a)
a.shape
```




    (9L,)




```python
#dftemp.date2.duplicated(keep=False)
```


```python

dftemp = dftemp.set_index(['date2'])


```


```python
dftemp4 = dftemp.resample("10min").mean()
```


```python
#dftemp4 =  dftemp.resample("10min").ffill().rolling(window=1).mean()
```


```python
dftemp4.describe
```




    <bound method DataFrame.describe of                        3    4          5          6          7          8  \
    date2                                                                       
    2016-01-06 00:00:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 00:10:00  NaN  NaN        NaN        NaN        NaN        NaN   
    2016-01-06 00:20:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 00:30:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 00:40:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 00:50:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 01:00:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 01:10:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 01:20:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 01:30:00  NaN  NaN        NaN        NaN        NaN        NaN   
    2016-01-06 01:40:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 01:50:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 02:00:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 02:10:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 02:20:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 02:30:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 02:40:00  NaN  NaN        NaN        NaN        NaN        NaN   
    2016-01-06 02:50:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 03:00:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 03:10:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 03:20:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 03:30:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 03:40:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 03:50:00  NaN  NaN        NaN        NaN        NaN        NaN   
    2016-01-06 04:00:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 04:10:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 04:20:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 04:30:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 04:40:00  0.0  0.0        NaN        NaN        NaN        NaN   
    2016-01-06 04:50:00  0.0  0.0        NaN        NaN        NaN        NaN   
    ...                  ...  ...        ...        ...        ...        ...   
    2017-07-01 19:00:00  0.0  0.0  24.000000  48.433333  23.390000  46.363333   
    2017-07-01 19:10:00  0.0  0.0  24.000000  48.223333  23.390000  46.230000   
    2017-07-01 19:20:00  0.0  0.0  24.000000  48.090000  23.390000  46.163333   
    2017-07-01 19:30:00  NaN  NaN  23.926667  48.000000  23.390000  46.090000   
    2017-07-01 19:40:00  0.0  0.0  24.000000  47.933333  23.390000  46.090000   
    2017-07-01 19:50:00  0.0  0.0  24.000000  47.900000  23.390000  46.030000   
    2017-07-01 20:00:00  0.0  0.0  23.926667  47.900000  23.390000  45.966667   
    2017-07-01 20:10:00  0.0  0.0  23.963333  47.790000  23.390000  45.900000   
    2017-07-01 20:20:00  0.0  0.0  23.890000  47.730000  23.390000  45.863333   
    2017-07-01 20:30:00  0.0  0.0  23.890000  47.700000  23.390000  45.790000   
    2017-07-01 20:40:00  NaN  NaN  23.890000  47.626667  23.390000  45.790000   
    2017-07-01 20:50:00  0.0  0.0  23.890000  47.560000  23.390000  45.730000   
    2017-07-01 21:00:00  0.0  0.0  23.890000  47.500000  23.390000  45.700000   
    2017-07-01 21:10:00  0.0  0.0  23.890000  47.500000  23.390000  45.626667   
    2017-07-01 21:20:00  0.0  0.0  23.890000  47.433333  23.390000  45.590000   
    2017-07-01 21:30:00  0.0  0.0  23.890000  47.363333  23.390000  45.590000   
    2017-07-01 21:40:00  0.0  0.0  23.890000  47.290000  23.356667  45.500000   
    2017-07-01 21:50:00  NaN  NaN  23.790000  47.200000  23.290000  45.500000   
    2017-07-01 22:00:00  0.0  0.0  23.790000  47.200000  23.290000  45.466667   
    2017-07-01 22:10:00  0.0  0.0  23.790000  47.126667  23.290000  45.400000   
    2017-07-01 22:20:00  0.0  0.0  23.700000  47.200000  23.245000  45.395000   
    2017-07-01 22:30:00  0.0  0.0  23.700000  47.200000  23.200000  45.400000   
    2017-07-01 22:40:00  0.0  0.0  23.700000  47.290000  23.200000  45.466667   
    2017-07-01 22:50:00  0.0  0.0  23.700000  47.363333  23.200000  45.530000   
    2017-07-01 23:00:00  NaN  NaN  23.700000  47.433333  23.200000  45.590000   
    2017-07-01 23:10:00  0.0  0.0  23.700000  47.500000  23.200000  45.626667   
    2017-07-01 23:20:00  0.0  0.0  23.666667  47.560000  23.200000  45.760000   
    2017-07-01 23:30:00  0.0  0.0  23.600000  47.560000  23.200000  45.790000   
    2017-07-01 23:40:00  0.0  0.0  23.600000  47.730000  23.200000  45.863333   
    2017-07-01 23:50:00  0.0  0.0  23.600000  47.790000  23.200000  45.900000   
    
                                 9         10         11         12      ...       \
    date2                                                                ...        
    2016-01-06 00:00:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 00:10:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 00:20:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 00:30:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 00:40:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 00:50:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 01:00:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 01:10:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 01:20:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 01:30:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 01:40:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 01:50:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 02:00:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 02:10:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 02:20:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 02:30:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 02:40:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 02:50:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 03:00:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 03:10:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 03:20:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 03:30:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 03:40:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 03:50:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 04:00:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 04:10:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 04:20:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 04:30:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 04:40:00        NaN        NaN        NaN        NaN      ...        
    2016-01-06 04:50:00        NaN        NaN        NaN        NaN      ...        
    ...                        ...        ...        ...        ...      ...        
    2017-07-01 19:00:00  25.100000  43.860000  24.290000  45.730000      ...        
    2017-07-01 19:10:00  25.133333  43.633333  24.323333  45.590000      ...        
    2017-07-01 19:20:00  25.200000  43.360000  24.390000  45.530000      ...        
    2017-07-01 19:30:00  25.200000  43.163333  24.356667  45.400000      ...        
    2017-07-01 19:40:00  25.267500  42.997500  24.290000  45.400000      ...        
    2017-07-01 19:50:00  25.290000  42.966667  24.290000  45.400000      ...        
    2017-07-01 20:00:00  25.096667  42.660000  24.290000  45.400000      ...        
    2017-07-01 20:10:00  24.630000  42.133333  24.290000  45.400000      ...        
    2017-07-01 20:20:00  24.193333  41.723333  24.290000  45.400000      ...        
    2017-07-01 20:30:00  23.926667  41.530000  24.290000  45.363333      ...        
    2017-07-01 20:40:00  23.666667  41.363333  24.290000  45.290000      ...        
    2017-07-01 20:50:00  23.533333  41.290000  24.200000  45.200000      ...        
    2017-07-01 21:00:00  23.356667  41.290000  24.200000  45.200000      ...        
    2017-07-01 21:10:00  23.230000  41.290000  24.200000  45.200000      ...        
    2017-07-01 21:20:00  23.066667  41.400000  24.200000  45.200000      ...        
    2017-07-01 21:30:00  22.926667  41.526667  24.200000  45.200000      ...        
    2017-07-01 21:40:00  22.960000  42.066667  24.200000  45.126667      ...        
    2017-07-01 21:50:00  23.226667  42.260000  24.200000  45.163333      ...        
    2017-07-01 22:00:00  23.426667  42.326667  24.200000  45.090000      ...        
    2017-07-01 22:10:00  23.566667  42.526667  24.166667  45.090000      ...        
    2017-07-01 22:20:00  23.600000  42.800000  24.150000  45.067500      ...        
    2017-07-01 22:30:00  23.666667  43.193333  24.100000  45.060000      ...        
    2017-07-01 22:40:00  23.730000  43.530000  24.100000  45.200000      ...        
    2017-07-01 22:50:00  23.865000  43.847500  24.100000  45.200000      ...        
    2017-07-01 23:00:00  23.890000  44.060000  24.100000  45.230000      ...        
    2017-07-01 23:10:00  24.000000  44.290000  24.100000  45.290000      ...        
    2017-07-01 23:20:00  24.066667  44.430000  24.100000  45.433333      ...        
    2017-07-01 23:30:00  24.133333  44.626667  24.100000  45.500000      ...        
    2017-07-01 23:40:00  24.200000  44.700000  24.066667  45.500000      ...        
    2017-07-01 23:50:00  24.230000  44.730000  24.000000  45.500000      ...        
    
                         18         19         20         21         22  23  24  \
    date2                                                                         
    2016-01-06 00:00:00 NaN        NaN        NaN  18.200000  55.790000 NaN NaN   
    2016-01-06 00:10:00 NaN        NaN        NaN  18.166667  55.566667 NaN NaN   
    2016-01-06 00:20:00 NaN        NaN        NaN  18.100000  55.397500 NaN NaN   
    2016-01-06 00:30:00 NaN        NaN        NaN  18.033333  55.433333 NaN NaN   
    2016-01-06 00:40:00 NaN        NaN        NaN  18.000000  55.533333 NaN NaN   
    2016-01-06 00:50:00 NaN        NaN        NaN  17.890000  55.700000 NaN NaN   
    2016-01-06 01:00:00 NaN        NaN        NaN  17.856667  55.663333 NaN NaN   
    2016-01-06 01:10:00 NaN        NaN        NaN  17.790000  55.572000 NaN NaN   
    2016-01-06 01:20:00 NaN        NaN        NaN  17.730000  55.433333 NaN NaN   
    2016-01-06 01:30:00 NaN        NaN        NaN  17.700000  55.396667 NaN NaN   
    2016-01-06 01:40:00 NaN        NaN        NaN  17.700000  55.360000 NaN NaN   
    2016-01-06 01:50:00 NaN        NaN        NaN  17.666667  55.163333 NaN NaN   
    2016-01-06 02:00:00 NaN        NaN        NaN  17.600000  55.000000 NaN NaN   
    2016-01-06 02:10:00 NaN        NaN        NaN  17.600000  54.933333 NaN NaN   
    2016-01-06 02:20:00 NaN        NaN        NaN  17.550000  54.900000 NaN NaN   
    2016-01-06 02:30:00 NaN        NaN        NaN  17.500000  54.900000 NaN NaN   
    2016-01-06 02:40:00 NaN        NaN        NaN  17.500000  54.826667 NaN NaN   
    2016-01-06 02:50:00 NaN        NaN        NaN  17.463333  54.730000 NaN NaN   
    2016-01-06 03:00:00 NaN        NaN        NaN  17.390000  54.626667 NaN NaN   
    2016-01-06 03:10:00 NaN        NaN        NaN  17.390000  54.700000 NaN NaN   
    2016-01-06 03:20:00 NaN        NaN        NaN  17.323333  54.700000 NaN NaN   
    2016-01-06 03:30:00 NaN        NaN        NaN  17.356667  54.590000 NaN NaN   
    2016-01-06 03:40:00 NaN        NaN        NaN  17.290000  54.560000 NaN NaN   
    2016-01-06 03:50:00 NaN        NaN        NaN  17.267500  54.475000 NaN NaN   
    2016-01-06 04:00:00 NaN        NaN        NaN  17.290000  54.466667 NaN NaN   
    2016-01-06 04:10:00 NaN        NaN        NaN  17.222500  54.400000 NaN NaN   
    2016-01-06 04:20:00 NaN        NaN        NaN  17.166667  54.326667 NaN NaN   
    2016-01-06 04:30:00 NaN        NaN        NaN  17.100000  54.290000 NaN NaN   
    2016-01-06 04:40:00 NaN        NaN        NaN  17.100000  54.290000 NaN NaN   
    2016-01-06 04:50:00 NaN        NaN        NaN  17.100000  54.326667 NaN NaN   
    ...                  ..        ...        ...        ...        ...  ..  ..   
    2017-07-01 19:00:00 NaN  24.963333  45.700000  24.100000  45.400000 NaN NaN   
    2017-07-01 19:10:00 NaN  24.963333  45.626667  24.100000  45.400000 NaN NaN   
    2017-07-01 19:20:00 NaN  25.000000  45.590000  24.100000  45.400000 NaN NaN   
    2017-07-01 19:30:00 NaN  24.926667  45.590000  24.100000  45.400000 NaN NaN   
    2017-07-01 19:40:00 NaN  25.000000  45.590000  24.100000  45.400000 NaN NaN   
    2017-07-01 19:50:00 NaN  24.926667  45.590000  24.100000  45.400000 NaN NaN   
    2017-07-01 20:00:00 NaN  25.000000  45.560000  24.033333  45.326667 NaN NaN   
    2017-07-01 20:10:00 NaN  25.000000  45.433333  24.100000  45.400000 NaN NaN   
    2017-07-01 20:20:00 NaN  25.000000  45.400000  24.100000  45.400000 NaN NaN   
    2017-07-01 20:30:00 NaN  25.000000  45.400000  24.066667  45.363333 NaN NaN   
    2017-07-01 20:40:00 NaN  25.000000  45.400000  24.000000  45.290000 NaN NaN   
    2017-07-01 20:50:00 NaN  24.926667  45.400000  24.000000  45.290000 NaN NaN   
    2017-07-01 21:00:00 NaN  24.963333  45.400000  24.000000  45.363333 NaN NaN   
    2017-07-01 21:10:00 NaN  24.963333  45.400000  24.000000  45.400000 NaN NaN   
    2017-07-01 21:20:00 NaN  24.945000  45.345000  24.000000  45.400000 NaN NaN   
    2017-07-01 21:30:00 NaN  25.000000  45.290000  24.000000  45.400000 NaN NaN   
    2017-07-01 21:40:00 NaN  25.000000  45.230000  24.000000  45.400000 NaN NaN   
    2017-07-01 21:50:00 NaN  24.890000  45.200000  24.000000  45.400000 NaN NaN   
    2017-07-01 22:00:00 NaN  24.890000  45.200000  24.000000  45.400000 NaN NaN   
    2017-07-01 22:10:00 NaN  24.890000  45.090000  24.000000  45.400000 NaN NaN   
    2017-07-01 22:20:00 NaN  24.890000  45.090000  23.926667  45.400000 NaN NaN   
    2017-07-01 22:30:00 NaN  24.926667  45.500000  23.926667  45.400000 NaN NaN   
    2017-07-01 22:40:00 NaN  25.000000  45.833333  23.890000  45.400000 NaN NaN   
    2017-07-01 22:50:00 NaN  25.100000  46.193333  23.890000  45.400000 NaN NaN   
    2017-07-01 23:00:00 NaN  25.166667  46.400000  23.890000  45.400000 NaN NaN   
    2017-07-01 23:10:00 NaN  25.200000  46.566667  23.890000  45.400000 NaN NaN   
    2017-07-01 23:20:00 NaN  25.200000  46.833333  23.890000  45.400000 NaN NaN   
    2017-07-01 23:30:00 NaN  25.200000  46.933333  23.890000  45.400000 NaN NaN   
    2017-07-01 23:40:00 NaN  25.260000  47.060000  23.890000  45.400000 NaN NaN   
    2017-07-01 23:50:00 NaN  25.230000  47.090000  23.890000  45.466667 NaN NaN   
    
                         25  26         extra  
    date2                                      
    2016-01-06 00:00:00 NaN NaN  16880.600000  
    2016-01-06 00:10:00 NaN NaN   6869.333333  
    2016-01-06 00:20:00 NaN NaN  18984.200000  
    2016-01-06 00:30:00 NaN NaN  16768.250000  
    2016-01-06 00:40:00 NaN NaN  18128.000000  
    2016-01-06 00:50:00 NaN NaN  12187.500000  
    2016-01-06 01:00:00 NaN NaN  17264.750000  
    2016-01-06 01:10:00 NaN NaN  15719.000000  
    2016-01-06 01:20:00 NaN NaN  12886.750000  
    2016-01-06 01:30:00 NaN NaN  19293.333333  
    2016-01-06 01:40:00 NaN NaN  15502.000000  
    2016-01-06 01:50:00 NaN NaN   6199.000000  
    2016-01-06 02:00:00 NaN NaN  15565.500000  
    2016-01-06 02:10:00 NaN NaN  19233.000000  
    2016-01-06 02:20:00 NaN NaN  14141.200000  
    2016-01-06 02:30:00 NaN NaN  14635.250000  
    2016-01-06 02:40:00 NaN NaN   8995.000000  
    2016-01-06 02:50:00 NaN NaN  16256.250000  
    2016-01-06 03:00:00 NaN NaN  22731.000000  
    2016-01-06 03:10:00 NaN NaN  14685.000000  
    2016-01-06 03:20:00 NaN NaN   9523.750000  
    2016-01-06 03:30:00 NaN NaN  20619.000000  
    2016-01-06 03:40:00 NaN NaN  14215.250000  
    2016-01-06 03:50:00 NaN NaN  11551.500000  
    2016-01-06 04:00:00 NaN NaN  18057.000000  
    2016-01-06 04:10:00 NaN NaN  16678.400000  
    2016-01-06 04:20:00 NaN NaN  16539.250000  
    2016-01-06 04:30:00 NaN NaN   7325.000000  
    2016-01-06 04:40:00 NaN NaN  13957.000000  
    2016-01-06 04:50:00 NaN NaN  19353.250000  
    ...                  ..  ..           ...  
    2017-07-01 19:00:00 NaN NaN  16186.200000  
    2017-07-01 19:10:00 NaN NaN  18291.192308  
    2017-07-01 19:20:00 NaN NaN  13411.400000  
    2017-07-01 19:30:00 NaN NaN  17569.458333  
    2017-07-01 19:40:00 NaN NaN  15606.807692  
    2017-07-01 19:50:00 NaN NaN  15290.884615  
    2017-07-01 20:00:00 NaN NaN  14695.480000  
    2017-07-01 20:10:00 NaN NaN  16160.120000  
    2017-07-01 20:20:00 NaN NaN  14911.880000  
    2017-07-01 20:30:00 NaN NaN  14605.040000  
    2017-07-01 20:40:00 NaN NaN  15388.625000  
    2017-07-01 20:50:00 NaN NaN  13184.080000  
    2017-07-01 21:00:00 NaN NaN  16593.080000  
    2017-07-01 21:10:00 NaN NaN  14278.480000  
    2017-07-01 21:20:00 NaN NaN  14630.703704  
    2017-07-01 21:30:00 NaN NaN  16961.680000  
    2017-07-01 21:40:00 NaN NaN  15102.280000  
    2017-07-01 21:50:00 NaN NaN  18977.440000  
    2017-07-01 22:00:00 NaN NaN  15987.880000  
    2017-07-01 22:10:00 NaN NaN  14273.920000  
    2017-07-01 22:20:00 NaN NaN  17125.642857  
    2017-07-01 22:30:00 NaN NaN  14510.520000  
    2017-07-01 22:40:00 NaN NaN  15315.400000  
    2017-07-01 22:50:00 NaN NaN  12793.615385  
    2017-07-01 23:00:00 NaN NaN  13550.000000  
    2017-07-01 23:10:00 NaN NaN  17759.600000  
    2017-07-01 23:20:00 NaN NaN  14325.800000  
    2017-07-01 23:30:00 NaN NaN  14898.920000  
    2017-07-01 23:40:00 NaN NaN  13960.600000  
    2017-07-01 23:50:00 NaN NaN  15284.720000  
    
    [78192 rows x 25 columns]>




```python
dftemp4.columns.tolist()
```




    [3L,
     4L,
     5L,
     6L,
     7L,
     8L,
     9L,
     10L,
     11L,
     12L,
     13L,
     14L,
     15L,
     16L,
     17L,
     18L,
     19L,
     20L,
     21L,
     22L,
     23L,
     24L,
     25L,
     26L,
     'extra']




```python
#del dftemp4[1]
#del dftemp4[2]
del dftemp4[3]
del dftemp4[4]
del dftemp4[23]
del dftemp4[24]
del dftemp4[25]
del dftemp4[26]
del dftemp4['extra']

```


```python

dftemp4.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
      <th>14</th>
      <th>15</th>
      <th>16</th>
      <th>17</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
    </tr>
    <tr>
      <th>date2</th>
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
      <th>2016-01-06 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.200000</td>
      <td>55.790000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:10:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.166667</td>
      <td>55.566667</td>
    </tr>
    <tr>
      <th>2016-01-06 00:20:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.100000</td>
      <td>55.397500</td>
    </tr>
    <tr>
      <th>2016-01-06 00:30:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.033333</td>
      <td>55.433333</td>
    </tr>
    <tr>
      <th>2016-01-06 00:40:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.000000</td>
      <td>55.533333</td>
    </tr>
  </tbody>
</table>
</div>




```python
dftemp4.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
      <th>14</th>
      <th>15</th>
      <th>16</th>
      <th>17</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
    </tr>
    <tr>
      <th>date2</th>
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
      <th>2017-07-01 23:10:00</th>
      <td>23.700000</td>
      <td>47.50</td>
      <td>23.2</td>
      <td>45.626667</td>
      <td>24.000000</td>
      <td>44.290000</td>
      <td>24.100000</td>
      <td>45.290000</td>
      <td>22.79</td>
      <td>47.930000</td>
      <td>16.033333</td>
      <td>15.433333</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.20</td>
      <td>46.566667</td>
      <td>23.89</td>
      <td>45.400000</td>
    </tr>
    <tr>
      <th>2017-07-01 23:20:00</th>
      <td>23.666667</td>
      <td>47.56</td>
      <td>23.2</td>
      <td>45.760000</td>
      <td>24.066667</td>
      <td>44.430000</td>
      <td>24.100000</td>
      <td>45.433333</td>
      <td>22.79</td>
      <td>48.060000</td>
      <td>15.963333</td>
      <td>15.960000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.20</td>
      <td>46.833333</td>
      <td>23.89</td>
      <td>45.400000</td>
    </tr>
    <tr>
      <th>2017-07-01 23:30:00</th>
      <td>23.600000</td>
      <td>47.56</td>
      <td>23.2</td>
      <td>45.790000</td>
      <td>24.133333</td>
      <td>44.626667</td>
      <td>24.100000</td>
      <td>45.500000</td>
      <td>22.73</td>
      <td>47.933333</td>
      <td>15.963333</td>
      <td>16.226667</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.20</td>
      <td>46.933333</td>
      <td>23.89</td>
      <td>45.400000</td>
    </tr>
    <tr>
      <th>2017-07-01 23:40:00</th>
      <td>23.600000</td>
      <td>47.73</td>
      <td>23.2</td>
      <td>45.863333</td>
      <td>24.200000</td>
      <td>44.700000</td>
      <td>24.066667</td>
      <td>45.500000</td>
      <td>22.70</td>
      <td>47.760000</td>
      <td>16.033333</td>
      <td>16.293333</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.26</td>
      <td>47.060000</td>
      <td>23.89</td>
      <td>45.400000</td>
    </tr>
    <tr>
      <th>2017-07-01 23:50:00</th>
      <td>23.600000</td>
      <td>47.79</td>
      <td>23.2</td>
      <td>45.900000</td>
      <td>24.230000</td>
      <td>44.730000</td>
      <td>24.000000</td>
      <td>45.500000</td>
      <td>22.70</td>
      <td>47.566667</td>
      <td>16.166667</td>
      <td>15.766667</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.23</td>
      <td>47.090000</td>
      <td>23.89</td>
      <td>45.466667</td>
    </tr>
  </tbody>
</table>
</div>




```python




```


```python
dftemp4.plot(y=12)
```




    <matplotlib.axes._subplots.AxesSubplot at 0xb90e208>




![png](output_60_1.png)



```python
dfsubset = dftemp4[[5,7,9,11,13,15,17]]
```


```python
dfsubset.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0xec99668>




![png](output_62_1.png)



```python
dftemp4.columns.tolist()
```




    [5L,
     6L,
     7L,
     8L,
     9L,
     10L,
     11L,
     12L,
     13L,
     14L,
     15L,
     16L,
     17L,
     18L,
     19L,
     20L,
     21L,
     22L]




```python
dftemp4.columns = ['T1','RH_1','T2','RH_2','T3',"RH_3",
                  'T4','RH_4','T5','RH_5','T6',"RH_6",
                  'T7','RH_7','T8','RH_8','T9',"RH_9"]
```


```python
dftemp4.columns.tolist()
```




    ['T1',
     'RH_1',
     'T2',
     'RH_2',
     'T3',
     'RH_3',
     'T4',
     'RH_4',
     'T5',
     'RH_5',
     'T6',
     'RH_6',
     'T7',
     'RH_7',
     'T8',
     'RH_8',
     'T9',
     'RH_9']




```python
dfsubset_temp = dftemp4[['T1','T2','T3','T4','T5','T7','T8','T9']]
```


```python
dfsubset_temp.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0xa2b59b0>




![png](output_67_1.png)



```python
dffinal2.diff().head(8)

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>energy1</th>
      <th>energy2</th>
      <th>energy3</th>
      <th>energy4</th>
      <th>energy5</th>
      <th>energy6</th>
      <th>energy7</th>
    </tr>
    <tr>
      <th>date</th>
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
      <th>2015-12-31 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2015-12-31 00:10:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:20:00</th>
      <td>0.0</td>
      <td>60.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:30:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:40:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:50:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:00:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 01:10:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dffinal2.diff().head()

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>energy1</th>
      <th>energy2</th>
      <th>energy3</th>
      <th>energy4</th>
      <th>energy5</th>
      <th>energy6</th>
      <th>energy7</th>
    </tr>
    <tr>
      <th>date</th>
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
      <th>2015-12-31 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2015-12-31 00:10:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:20:00</th>
      <td>0.0</td>
      <td>60.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:30:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2015-12-31 00:40:00</th>
      <td>0.0</td>
      <td>70.0</td>
      <td>40.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfsubset_temp.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>T1</th>
      <th>T2</th>
      <th>T3</th>
      <th>T4</th>
      <th>T5</th>
      <th>T7</th>
      <th>T8</th>
      <th>T9</th>
    </tr>
    <tr>
      <th>date2</th>
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
      <th>2016-01-06 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.200000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:10:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.166667</td>
    </tr>
    <tr>
      <th>2016-01-06 00:20:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.100000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:30:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.033333</td>
    </tr>
    <tr>
      <th>2016-01-06 00:40:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfsubset_temp.index.names = ['date']
dfsubset_temp.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>T1</th>
      <th>T2</th>
      <th>T3</th>
      <th>T4</th>
      <th>T5</th>
      <th>T7</th>
      <th>T8</th>
      <th>T9</th>
    </tr>
    <tr>
      <th>date</th>
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
      <th>2016-01-06 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.200000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:10:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.166667</td>
    </tr>
    <tr>
      <th>2016-01-06 00:20:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.100000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:30:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.033333</td>
    </tr>
    <tr>
      <th>2016-01-06 00:40:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
alltogether = pd.merge(dffinal2.diff(),dfsubset_temp,left_index=True, right_index=True)
```


```python
alltogether.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1243f6d8>




![png](output_73_1.png)



```python
backup = pd.HDFStore('backup3.h5')
```


```python
backup['alltogether'] = alltogether
```


```python
backup['dfsubset_temp'] = dfsubset_temp
```


```python
backup.close()
```


```python
backup = pd.HDFStore('backup3.h5')
#var1 = backup['var1']
```


```python
print(backup)
```

    <class 'pandas.io.pytables.HDFStore'>
    File path: backup3.h5
    /alltogether              frame        (shape->[78192,15])
    /dfsubset_temp            frame        (shape->[78192,8]) 
    


```python
alltogether.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>energy1</th>
      <th>energy2</th>
      <th>energy3</th>
      <th>energy4</th>
      <th>energy5</th>
      <th>energy6</th>
      <th>energy7</th>
      <th>T1</th>
      <th>T2</th>
      <th>T3</th>
      <th>T4</th>
      <th>T5</th>
      <th>T7</th>
      <th>T8</th>
      <th>T9</th>
    </tr>
    <tr>
      <th>date</th>
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
      <th>2016-01-06 00:00:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.200000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:10:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.166667</td>
    </tr>
    <tr>
      <th>2016-01-06 00:20:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.100000</td>
    </tr>
    <tr>
      <th>2016-01-06 00:30:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.033333</td>
    </tr>
    <tr>
      <th>2016-01-06 00:40:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
