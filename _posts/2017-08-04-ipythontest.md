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


![png](ipynb/jekyll_test_files/output_4_0.png)



![png](/ipynb/jekyll_test_files/output_4_1.png)



![png](/ipynb/jekyll_test_files/output_4_2.png)



![png](/ipynb/jekyll_test_files/output_4_3.png)



![png](/ipynb/jekyll_test_files/output_4_4.png)



![png](/ipynb/jekyll_test_files/output_4_5.png)



```python

```
