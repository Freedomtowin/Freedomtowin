
```python
import matplotlib.pyplot as plt
import datetime
import numpy as np
import pandas as pd
import Regression
import DSP
import importlib

store = pd.read_csv('Rossman/store.csv')

dateparse = lambda x: pd.datetime.strptime(x, '%Y-%m-%d')

full = pd.read_csv('Rossman/train.csv',parse_dates=['Date'], date_parser=dateparse,dtype=str)

full = full.rename(columns={"Sales":"Value"})

full = full.sort_values(by=['Store','Date'])

full = full.drop(['SchoolHoliday','Customers'],axis=1)

```


```python
full.Value = full.Value.apply(int)
full.Store = full.Store.apply(int)
full.Promo = full.Promo.apply(int)
full.Open = full.Open.apply(int)
full.DayOfWeek = full.DayOfWeek.apply(int)
```


```python
full = full[full.Store<100]
```



```python
index_df = full[['Date']].groupby(['Date']).count().reset_index()
index_df['index'] = np.arange(0,len(index_df))
full=full.merge(index_df, on='Date')
full
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store</th>
      <th>DayOfWeek</th>
      <th>Date</th>
      <th>Value</th>
      <th>Open</th>
      <th>Promo</th>
      <th>StateHoliday</th>
      <th>index</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>15</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>16</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>17</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>18</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>19</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>20</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>21</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>22</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>23</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>24</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>25</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>26</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>27</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>28</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>29</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>3</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>30</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>31</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>32</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>33</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>34</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>35</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>36</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
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
      <th>90468</th>
      <td>72</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>5251</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90469</th>
      <td>73</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>6026</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90470</th>
      <td>74</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7518</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90471</th>
      <td>75</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7444</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90472</th>
      <td>76</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>12887</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90473</th>
      <td>77</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>10335</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90474</th>
      <td>78</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>5712</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90475</th>
      <td>79</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7150</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90476</th>
      <td>8</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>8492</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90477</th>
      <td>80</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>9810</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90478</th>
      <td>81</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>9279</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90479</th>
      <td>82</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>11492</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90480</th>
      <td>83</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>3866</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90481</th>
      <td>84</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>14949</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90482</th>
      <td>85</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7791</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90483</th>
      <td>86</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>5572</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90484</th>
      <td>87</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>6379</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90485</th>
      <td>88</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7863</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90486</th>
      <td>89</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7295</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90487</th>
      <td>9</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>8565</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90488</th>
      <td>90</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>11495</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90489</th>
      <td>91</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>6164</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90490</th>
      <td>92</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>8227</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90491</th>
      <td>93</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>6548</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90492</th>
      <td>94</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>8372</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90493</th>
      <td>95</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>8445</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90494</th>
      <td>96</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>6370</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90495</th>
      <td>97</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>8380</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90496</th>
      <td>98</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>7012</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
    <tr>
      <th>90497</th>
      <td>99</td>
      <td>5</td>
      <td>2015-07-31</td>
      <td>6571</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>941</td>
    </tr>
  </tbody>
</table>
<p>90498 rows × 8 columns</p>
</div>




```python
groupby_value = full[['Value','index']].groupby(['index']).sum().reset_index()
dummies = pd.get_dummies(full.Store, prefix='Store').iloc[:, 1:]
full = pd.concat([full, dummies], axis=1)

train=full[full['index']<850].copy()
valid=full[full['index']>=850].copy()

O_train,O_valid = train[[x for x in train.columns if'Store_' in x or 'Promo' in x ]].values, valid[[x for x in valid.columns if'Store_' in x or 'Promo' in x ]].values

plt.plot(groupby_value['index'],groupby_value['Value'],'-')
plt.xlabel('time')
plt.ylabel('Value')
plt.show()
```


![png](output_59_0.png)



```python

```


```python
O_train=O_train*train.Open.values.reshape((len(train),1))
O_valid=O_valid*valid.Open.values.reshape((len(valid),1))
```


```python

time_diff_signal = DSP.time_diff_variable(groupby_value[['Value']].values.flatten(),1)
plt.plot(groupby_value['index'][1:],time_diff_signal,'-');plt.xlabel('index');plt.ylabel('Value[t] - Value[t-1]');plt.title('Value[t] - Value[t-1]')
plt.show()
N = len(time_diff_signal)
unfiltered  = DSP.get_frequency_domain(time_diff_signal)
f,y  = unfiltered[:,0],unfiltered[:,1]
y_abs=( 2.0/N * np.abs(y[1:]))
plt.plot(f[1:],y_abs);plt.xlabel('frequency');plt.ylabel('absolute magnitude');plt.title("Absolute Magnitude of Complex Frequencies")
plt.show()

```


![png](output_62_0.png)


    /Users/rohankotwani/anaconda/envs/datasci/lib/python3.5/site-packages/numpy/core/numeric.py:531: ComplexWarning: Casting complex values to real discards the imaginary part
      return array(a, dtype, copy=False, order=order)



![png](output_62_2.png)



```python
train.head(1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store</th>
      <th>DayOfWeek</th>
      <th>Date</th>
      <th>Value</th>
      <th>Open</th>
      <th>Promo</th>
      <th>StateHoliday</th>
      <th>index</th>
      <th>Store_2</th>
      <th>Store_3</th>
      <th>...</th>
      <th>Store_90</th>
      <th>Store_91</th>
      <th>Store_92</th>
      <th>Store_93</th>
      <th>Store_94</th>
      <th>Store_95</th>
      <th>Store_96</th>
      <th>Store_97</th>
      <th>Store_98</th>
      <th>Store_99</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>a</td>
      <td>0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 106 columns</p>
</div>




```python
t_i = np.mean(y_abs) + 1*np.std(y_abs)
t_f = np.mean(y_abs) + 3*np.std(y_abs)
print(t_i,t_f)
```

    34747.7512751 78100.2693837



```python
importlib.reload(Regression)
importlib.reload(DSP)

heap = []
for i in range(1,2):
    T_train = Regression.pandas_poly_feature(train[['index']],i).values
    T_valid = Regression.pandas_poly_feature(valid[['index']],i).values

    skip_list=None
    
    for thresh in np.linspace(t_i,t_f,5):
        abs_filtered = np.absolute(DSP.filter_freq_domain(unfiltered, center=0.25,band=0.24999999,threshold=thresh))

        period_list = set([round(1/(ft)) for ft,ht in abs_filtered if round(1/(ft))>2])
        print(period_list)

        
        if skip_list==period_list:
            print("same sequences")
            continue
        skip_list = period_list
        
        
        S_train = np.ones((len(train),1))
        S_valid = np.ones((len(valid),1))
        for period in period_list:


            x = DSP.generate_impluse_waves(train['index'],period)
            S_train=np.column_stack((S_train,x))
            
            x = DSP.generate_impluse_waves(valid['index'],period)
            S_valid=np.column_stack((S_valid,x))
            

#             x = DSP.generate_sawtooth_waves(train['index'],period)
#             S_train=np.column_stack((S_train,x))

#             x = DSP.generate_sawtooth_waves(valid['index'],period)
#             S_valid=np.column_stack((S_valid,x))
            
#             x = DSP.generate_sin_waves(train['index'],period)
#             S_train=np.column_stack((S_train,x))

#             x = DSP.generate_sin_waves(valid['index'],period)
#             S_valid=np.column_stack((S_valid,x))
            
        S_train,S_valid = S_train[:,1:],S_valid[:,1:]

        
        
        P_train_df = pd.DataFrame(np.column_stack((S_train,T_train,O_train)))
        P_valid_df = pd.DataFrame(np.column_stack((S_valid,T_valid,O_valid)))
        
        print("lasso")
        lasso_heap=[]
        for l1_penalty in np.linspace(0.01, 1, 2):
            z = Regression.sklearn_lasso_regression(P_train_df,train[['Value']],l1_penalty)
            SSE = Regression.numpy_SSE(P_valid_df,valid[['Value']],z)
            lasso_heap.append((SSE,l1_penalty))
        penalty = Regression.heapsort(lasso_heap)[0][1]
        mask = Regression.sklearn_lasso_feature_selection(P_train_df,train[['Value']],penalty)
        print(P_train_df[mask].head(n=1))
        
        z = Regression.numpy_simple_regression(P_train_df[mask].values,train[['Value']])
        SSE = Regression.numpy_SSE(P_valid_df[mask].values,valid[['Value']],z)
        heap.append((SSE,i,thresh,penalty))
        print("SST: ",np.sum((valid['Value']-np.mean(valid['Value']))**2))
        print("SSE: ",SSE)
```

    {3.0, 4.0, 7.0}
    lasso
       0    1    3    4    5    6    7    8    9    10  ...   104  105  106  107  \
    0  1.0 -1.0  1.0 -1.0 -1.0 -1.0  1.0 -1.0 -1.0 -1.0 ...   0.0  0.0  0.0  0.0   
    
       108  109  110  111  112  113  
    0  0.0  0.0  0.0  0.0  0.0  0.0  
    
    [1 rows x 112 columns]
    SST:  114747146830.0
    SSE:  14160325893.8
    {3.0, 4.0, 7.0}
    same sequences
    {3.0, 4.0, 7.0}
    same sequences
    {3.0, 7.0}
    lasso
       0    1    3    4    5    6    8    9    10   11  ...   100  101  102  103  \
    0  1.0 -1.0  1.0 -1.0 -1.0 -1.0 -1.0 -1.0  0.0  0.0 ...   0.0  0.0  0.0  0.0   
    
       104  105  106  107  108  109  
    0  0.0  0.0  0.0  0.0  0.0  0.0  
    
    [1 rows x 108 columns]
    SST:  114747146830.0
    SSE:  14150974636.7
    {3.0, 7.0}
    same sequences



```python
len(train)
```




    81390




```python
SSE,i,thresh,penalty = Regression.heapsort(heap)[0]
print("model paramters: ",SSE,i,thresh,penalty)
print("SST: ",np.sum((valid['Value']-np.mean(valid['Value']))**2))
```

    model paramters:  14150974636.7 1 67262.1398566 0.01
    SST:  114747146830.0



```python
T_train = Regression.pandas_poly_feature(train[['index']],i).values
T_valid = Regression.pandas_poly_feature(valid[['index']],i).values
abs_filtered = np.absolute(DSP.filter_freq_domain(unfiltered, center=0.25,band=0.24999,threshold=thresh))

period_list = set([round(1/(ft)) for ft,ht in abs_filtered if round(1/(ft))>2 ])

S_train = np.ones((len(train),1))
S_valid = np.ones((len(valid),1))
for period in period_list:


    x = DSP.generate_impluse_waves(train['index'],period)
    S_train=np.column_stack((S_train,x))

    x = DSP.generate_impluse_waves(valid['index'],period)
    S_valid=np.column_stack((S_valid,x))
    

    
S_train,S_valid = S_train[:,1:],S_valid[:,1:]
P_train_df = pd.DataFrame(np.column_stack((S_train,T_train,O_train)))
P_valid_df = pd.DataFrame(np.column_stack((S_valid,T_valid,O_valid)))      

if penalty == 0:
    mask = P_train_df.columns
else:
    mask = Regression.sklearn_lasso_feature_selection(P_train_df,train[['Value']],penalty)
print(mask)
print(P_train_df[mask].head())
z = Regression.numpy_simple_regression(P_train_df[mask].values,train[['Value']])

SSE = Regression.numpy_SSE(P_valid_df[mask].values,valid[['Value']],z)
SST = np.sum((valid['Value']-np.mean(valid['Value']))**2)
print("R-squared: ",1-SSE/SST)




fig = plt.figure(figsize=(10,12))
fig.suptitle("Rossman Store Sales", fontsize=16)
ax = plt.subplot(2,1,1)
predict = Regression.numpy_predict(P_valid_df[mask].values,z)
ax.plot(valid['index'],valid['Value'],'b.');plt.plot(valid['index'],predict,'r.',linewidth=1.5);ax.set_title("Sales: valid")
ax = plt.subplot(2,1,2)
predict = Regression.numpy_predict(P_train_df[mask].values,z)
ax.plot(train['index'],train['Value'],'b.');plt.plot(train['index'],predict,'r.',linewidth=1.5);ax.set_title("Sales: train")
plt.show()


```

    [  0   1   3   4   5   6   8   9  10  11  12  13  14  15  16  17  18  19
      20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36  37
      38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54  55
      56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72  73
      74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89  90  91
      92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107 108 109]
       0    1    3    4    5    6    8    9    10   11  ...   100  101  102  103  \
    0  1.0 -1.0  1.0 -1.0 -1.0 -1.0 -1.0 -1.0  0.0  0.0 ...   0.0  0.0  0.0  0.0   
    1  1.0 -1.0  1.0 -1.0 -1.0 -1.0 -1.0 -1.0  0.0  0.0 ...   0.0  0.0  0.0  0.0   
    2  1.0 -1.0  1.0 -1.0 -1.0 -1.0 -1.0 -1.0  0.0  0.0 ...   0.0  0.0  0.0  0.0   
    3  1.0 -1.0  1.0 -1.0 -1.0 -1.0 -1.0 -1.0  0.0  0.0 ...   0.0  0.0  0.0  0.0   
    4  1.0 -1.0  1.0 -1.0 -1.0 -1.0 -1.0 -1.0  0.0  0.0 ...   0.0  0.0  0.0  0.0   
    
       104  105  106  107  108  109  
    0  0.0  0.0  0.0  0.0  0.0  0.0  
    1  0.0  0.0  0.0  0.0  0.0  0.0  
    2  0.0  0.0  0.0  0.0  0.0  0.0  
    3  0.0  0.0  0.0  0.0  0.0  0.0  
    4  0.0  0.0  0.0  0.0  0.0  0.0  
    
    [5 rows x 108 columns]
    R-squared:  0.87667689326



![png](output_68_1.png)



