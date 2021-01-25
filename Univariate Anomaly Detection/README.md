[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="888" alt="Visit QuantNet">](http://quantlet.de/)

## [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **Univariate_AD_offline** [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/)

```yaml

Name of Quantlet: 'Univariate_AD_offline'

Description: 
- 'Find out the potential anomalies of variable –Tran Count, and visualize the results.'
- "Here we use a person's bank account transaction record, in the case of Japan, Australia, or USA."
- 'The definition of outliors has 2 ways– (1) values outside of Mean +- a * SD, (2) values outside of Median +- a * MAD, where a = 1.96 (or 2, 3,...)'


Submitted:  '21 Jan 2021'

Keywords: 
- 'Univariate Anomaly Detection'
- 'rolling mean'
- 'rolling standard deviation (STD)'
- 'rolling median'
- 'rolling median-absolute-deviation (MAD)'


Datafile: './Data/Overseas spend(WH).xlsx'

Output: 
- 'Japan - TS plot detecting anomalies with windowsize 7 (center=True).png'
- 'Japan - TS plot detecting anomalies with windowsize 7 (center=False).png'
- 'Japan - TS plot detecting anomalies with windowsize 31 (center=True).png'
- 'Japan - TS plot detecting anomalies with windowsize 31 (center=False).png'
- 'Japan - TS plot detecting robust anomalies with windowsize 7 (center=True).png'
- 'Japan - TS plot detecting robust anomalies with windowsize 7 (center=False).png'
- 'Japan - TS plot detecting robust anomalies with windowsize 31 (center=True).png'
- 'Japan - TS plot detecting robust anomalies with windowsize 31 (center=False).png'

Author: 
- 'Jane Hsieh (Hsing-Chuan Hsieh)'


```

![Picture1](Australia%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center=False).png)

![Picture2](Australia%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center=True).png)

![Picture3](Australia%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%207%20(center=False).png)

![Picture4](Australia%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%207%20(center=True).png)

![Picture5](Japan%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center=False).png)

![Picture6](Japan%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center=True).png)

![Picture7](Japan%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%207%20(center=False).png)

![Picture8](Japan%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%207%20(center=True).png)

![Picture9](Japan%20-%20TS%20plot%20detecting%20robust%20anomalies%20with%20windowsize%2031%20(center=False).png)

![Picture10](Japan%20-%20TS%20plot%20detecting%20robust%20anomalies%20with%20windowsize%2031%20(center=True).png)

![Picture11](Japan%20-%20TS%20plot%20detecting%20robust%20anomalies%20with%20windowsize%207%20(center=False).png)

![Picture12](Japan%20-%20TS%20plot%20detecting%20robust%20anomalies%20with%20windowsize%207%20(center=True).png)

![Picture13](USA%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center=False).png)

![Picture14](USA%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center=True).png)

![Picture15](USA%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%207%20(center=False).png)

![Picture16](USA%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%207%20(center=True).png)

### PYTHON Code
```python

#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Thu Jan 21 13:12:03 2021

@author: jane_hsieh

Goal: Find out the potential anomalies of variable –"Tran Count", and visualize the results
        Here we use a person's bank account transaction record, in the case of 'Japan', 'Australia',or 'USA' 

Resource:
    1. For simple univariate anomaly detection, we can refer to the following for tutorial:
        https://www.ericsson.com/en/blog/2020/4/anomaly-detection-with-machine-learning
"""
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import os

data_dir = './Data'

#change your current working directory
os.chdir('/Users/jane_hsieh/OneDrive - nctu.edu.tw/Data Science Analysis Templates/Anomaly Detection/Univariate Anomaly Detection')
os.getcwd()


# parameters -----------------------------------#-
country = 'Japan' #'Japan', 'Australia', 'USA'          #-
# ----------------------------------------------#-


# ====================================  0. Input data: FRM prices ====================================================
df =  pd.read_excel(data_dir+'/Overseas spend(WH).xlsx', sheet_name=country,  parse_dates=['CPD Date'], index_col = 'CPD Date')

#0.1 Reorganize data
df.sort_index(inplace=True) 

print('The earliest date of data is:\t', df.index.min())
print('The latest date of data is:\t', df.index.max())

### reset Datetime index from earliest to latest date (daily), in case of missing dates
df = df.reindex(pd.date_range(df.index.min(), df.index.max(), freq = 'D'))

### Check if any missing date (missing data)
print('Check if any missing data in each variable:\n',df.isnull().sum())
'''
Fortunately, there's no missing data! Now we have a series of data from 2017-11-01 to 2019-06-30 '
'''

#df['Tran Count'].plot(figsize=(30,10))
#plt.show()









# ====================================  1. Anomaly Detection: Find Anomalies Using Mean +/- 1.96*SD ====================================================
# 1.1 Calculate the past statistics (mean, std), with a window size (i.e., past window days [including current day] of data would be summarized)
# Note: to reduce lag effect, instead of using 'past window', you can use the window centored at current day (center=True)

# parameters -----------------------------------#-
window = 31                                     #-
centered = False  #True # or False              #-
threshold = 1.96                                #-
# ----------------------------------------------#-
 
rolling_stats = df['Tran Count'].rolling(window, center=centered).agg(['mean', 'std'])  #<<<<<<<<<<<<<<<<<<<<<<<
df2 = df.join(rolling_stats)  #combine the column-wise data into df  

df2['Upper_SD'] = df2['mean'] + threshold * df2['std']  #upper bound of 95% confidence interval
df2['Lower_SD'] = df2['mean'] - threshold * df2['std']  #lower bound of 95% confidence interval

## Find possiblee anomalies by Mean +/- 1.96*SD
def Is_Anomaly(x):
    if  pd.isna(x[['Tran Count', 'mean','std']]).sum() >0:
        return np.nan
    z = abs(x['Tran Count'] - x['mean'])/x['std']
    if z > threshold:
        return 1  # outlier
    else:
        return 0  # normal
    
anomalies = df2.apply(Is_Anomaly, axis=1) 
anomalies.name = 'anomalies'
print('The percentage of anomalies in data is {:.2f}%'.format(np.mean(anomalies)*100))

df2 = df2.join(anomalies)   


# 1.2. Visualization of the results -----------------------------------------------------------------
anomalies = df2[df2['anomalies']==1]

fig, ax = plt.subplots(figsize=(30,10))
ax.plot(df2.index, df2['Tran Count'], linestyle = '-', color='b', label='Tran Count')
ax.plot(df2.index, df2['mean'], linestyle = '-', color='r', label='Mean')
ax.plot(df2.index, df2['Upper_SD'], linestyle = '--', color='g', label = r'Mean $\pm$ 1.96*SD')
ax.plot(df2.index, df2['Lower_SD'], linestyle = '--', color='g')
ax.scatter( anomalies.index, anomalies['Tran Count'], color = 'r' )
#legend = ax.legend(loc="upper right", edgecolor="black")
#legend.get_frame().set_alpha(None)
#legend.get_frame().set_facecolor((0, 0, 0, 0))
#ax.set_title(f'{country} - TS plot detecting anomalies with windowsize {window} (center={str(centered)})')
plt.show()
plt.savefig(f'{country} - TS plot detecting anomalies with windowsize {window} (center={str(centered)}).png', transparent=True) #<<<<<<<<<<<<<<<<<<<<<<<
    

'''
Mean and SD can change drastically due to extreme values (possible anomalies)
To reduce the impact of extreme values, we may use Median and Median-absolute-deviation (MAD), 
insted of mean and std, as standards for detecting anomalies
'''








# ====================================  2. Anomaly Detection: Find Anomalies Using Median +/- 1.96*MAD ====================================================
# 1.1 Calculate the past statistics (median, MAD), with a window size (i.e., past window days [including current day] of data would be summarized)
# Note: to reduce lag effect, instead of using 'past window', you can use the window centored at current day (center=True)
# parameters -----------------------------------#-
threshold = 1.96                                #-
# ----------------------------------------------#-

from scipy import stats
#x = df[:31]
#stats.median_abs_deviation(x['Tran Count'])

rolling_mdn = df['Tran Count'].rolling(window, center=centered).median()
rolling_mdn.name = 'median'
rolling_MAD = df['Tran Count'].rolling(window, center=centered).apply(stats.median_abs_deviation)
rolling_MAD.name = 'MAD'
df3 = df.join([rolling_mdn, rolling_MAD])  #combine the column-wise data into df  

df3['Upper_MAD'] = df3['median'] + threshold * df3['MAD']  #upper bound of robust 95% confidence interval
df3['Lower_MAD'] = df3['median'] - threshold * df3['MAD']  #lower bound of robust 95% confidence interval

## Find possiblee anomalies by Mean +/- 1.96*SD
def Is_Anomaly_MAD(x):
    if  pd.isna(x[['Tran Count', 'median','MAD']]).sum() >0:
        return np.nan
    z = abs(x['Tran Count'] - x['median'])/x['MAD']
    if z > threshold:
        return 1  # outlier
    else:
        return 0  # normal


anomalies = df3.apply(Is_Anomaly_MAD, axis=1) 
anomalies.name = 'anomalies'
print('The percentage of anomalies in data is {:.2f}%'.format(np.mean(anomalies)*100))

df3 = df3.join(anomalies)   



# 1.2. Visualization of the results -----------------------------------------------------------------
anomalies = df3[df3['anomalies']==1]

fig, ax = plt.subplots(figsize=(30,10))
ax.plot(df3.index, df3['Tran Count'], linestyle = '-', color='b', label='Tran Count')
ax.plot(df3.index, df3['median'], linestyle = '-', color='r', label='Median')
ax.plot(df3.index, df3['Upper_MAD'], linestyle = '--', color='g', label = r'Median $\pm$ 1.96*MAD')
ax.plot(df3.index, df3['Lower_MAD'], linestyle = '--', color='g')
ax.scatter( anomalies.index, anomalies['Tran Count'], color = 'r' )
#legend = ax.legend(loc="upper right", edgecolor="black")
#legend.get_frame().set_alpha(None)
#legend.get_frame().set_facecolor((0, 0, 0, 0))
#ax.set_title(f'{country} - TS plot detecting anomalies with windowsize {window} (center={str(centered)})')
plt.show()
plt.savefig(f'{country} - TS plot detecting robust anomalies with windowsize {window} (center={str(centered)}).png', transparent=True) #<<<<<<<<<<<<<<<<<<<<<<<
 










```

automatically created on 2021-01-25