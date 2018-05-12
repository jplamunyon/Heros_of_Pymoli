
# Heroes Of Pymoli Data Analysis
* Of the 573 active players, the vast majority are male (81%). There also exists, a smaller, but notable proportion of female players (17%).

* Our peak age demographic falls between 20-24 (41%) with secondary groups falling between 15-19 (24%) and 10-14 (10%).

* Our best selling item was the Final Critic which resulted in a net profit of $38.60


```python
import pandas as pd
import numpy as np


```


```python
data = "purchase_data.json"
tdata = pd.read_json(data)
tdata.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#playert = tdata["SN"].nunique()
#playert.Players.
```

## Player Count


```python
playerc = tdata[["SN"]]
playerrename = playerc.rename(columns={'SN':'Player'})
playertotal = playerrename["Player"].nunique()
playerdata = [{'Total Players': playertotal}]
playerdf = pd.DataFrame(playerdata)
playerdf
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
items = tdata[["Item Name", "Price"]]
#items = items["Price"].map("${:,.2f}".format)
itemnum = items["Item Name"].value_counts()
itemunique = tdata["Item Name"].nunique()
priceavg = items["Price"].mean()
totalrev = items["Price"].sum()
itemtotal = tdata["Item Name"].count()-1
itemdata = [{'Number of Unique Items': itemunique, 'Average Price': priceavg, 'Number of Purchases': itemtotal, 'Total Revenue': totalrev}]
itemdf = pd.DataFrame(itemdata)
itemdf = itemdf[['Number of Unique Items', 'Average Price', 'Number of Purchases', 'Total Revenue']]

itemdf

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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>2.931192</td>
      <td>779</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
dropgen = tdata.drop_duplicates(['SN'])
gendata = dropgen[['Gender','SN']]
gendtotal = gendata["Gender"].count()-1
gendercount = gendata["Gender"].value_counts()
genperc = pd.DataFrame((gendercount/gendtotal)*100)
genperc = genperc.rename(columns={'Gender': 'Percentage of Players'})
genperc = genperc["Percentage of Players"].map("{:,.2f}".format)
#gento = [{'Percentage of Players': (male/gendtotal)*100, 'Total Count': gendercount},{'Percentage of Players': (female/gendtotal)*100, 'Total Count': gendercount}]
#gendf = pd.DataFrame(gento)
totas = pd.DataFrame(gendercount)
totas = totas.rename(columns={'Gender': 'Total Count'})
totalgen = pd.concat([genperc, totas], axis=1)
totalgen
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.29</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.48</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)


```python
agender = tdata[["Gender", "Price"]]
sumgender = agender.groupby(["Gender"])
purchcnt = sumgender.count()
purchcnt = purchcnt.rename(columns={'Price': 'Purchase Count'})
tgenpurch = sumgender.sum()
tgenpurch = tgenpurch.rename(columns={'Price': 'Total Purchase Value'})
tgenpurch = tgenpurch["Total Purchase Value"].map("${:,.2f}".format)
avgpurch = sumgender.mean()
avgpurch = avgpurch.rename(columns={'Price': 'Average Purchase Price'})
avgpurch = avgpurch["Average Purchase Price"].map("${:,.2f}".format)
totalpg = pd.concat([tgenpurch, avgpurch, purchcnt], axis=1)
totalpg
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
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$382.91</td>
      <td>$2.82</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$1,867.68</td>
      <td>$2.95</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$35.74</td>
      <td>$3.25</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
bindata = dropgen["Age"]
bins = [0,9,15,20,25,30,35,40,100]
demoname = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']
agetab = pd.cut(dropgen["Age"], bins, labels=demoname)
dropgen["Age Summary"] = agetab
age_sum = dropgen.groupby("Age Summary")
age_count = age_sum['SN'].count()
age_perc = pd.DataFrame((age_count/gendtotal)*100)
age_perc = age_perc.rename(columns={'SN': 'Percentage of Players'})
age_perc = age_perc["Percentage of Players"].map("{:,.2f}".format)
age_df = pd.DataFrame(age_count)
age_df = age_df.rename(columns={'SN': 'Total Count'})
totalage = pd.concat([age_perc, age_df], axis=1)
totalage
```

    /anaconda3/lib/python3.6/site-packages/ipykernel/__main__.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy





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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>9.97</td>
      <td>57</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.30</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.91</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.09</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.69</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.37</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
tage = pd.cut(tdata["Age"], bins, labels=demoname)
tdata ["Age Sum Total"] = tage
agesu = tdata.groupby("Age Sum Total")
purage = agesu['Price'].count()
purage_df= pd.DataFrame(purage)
purage_df = purage_df.rename(columns={'Price': 'Purchase Count'})
avgprage = agesu['Price'].mean()
avgprage_df = pd.DataFrame(avgprage)
avgprage_df = avgprage_df.rename(columns={'Price': 'Average Purchase Price'})
avgprage_df = avgprage_df["Average Purchase Price"].map("${:,.2f}".format)
totprage = agesu['Price'].sum()
totprage_df = pd.DataFrame(totprage)
totprage_df = totprage_df.rename(columns={'Price': 'Total Purchase Value'})
totprage_df = totprage_df["Total Purchase Value"].map("${:,.2f}".format)
totagep = pd.concat([purage_df, avgprage_df, totprage_df], axis=1)
totagep
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Sum Total</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>82</td>
      <td>$2.89</td>
      <td>$237.31</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
      <td>$2.90</td>
      <td>$127.49</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
spender = tdata[["SN", "Price"]]
grptot = spender.groupby("SN")
grpcnt = grptot.count()
grpsum = grptot.sum()
grpavg = grptot.mean()
grpsumdf = grpsum.nlargest(5,'Price')
grpsumdf

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
      <th>Price</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
popitem = tdata[["Item Name", "Price"]]
itemp = popitem.groupby("Item Name")
itemc = itemp.count()
item5 = itemc.nlargest(5,'Price')
item5
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
      <th>Price</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>11</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
pdata = tdata[["Item Name", "Price"]]
ind
columns = ['Purchase Count', 'Item Price', 'Total Purchase Value']
profit_df = pd.DataFrame(pdatasort, index)
pdatasort = pdata.groupby("Item Name")
pdatasum = pdatasort.sum()
profit_df
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-21-af1406ac74b8> in <module>()
          1 pdata = tdata[["Item Name", "Price"]]
          2 columns = ['Purchase Count', 'Item Price', 'Total Purchase Value']
    ----> 3 profit_df = pd.DataFrame(pdatasort)
          4 pdatasort = pdata.groupby("Item Name")
          5 pdatasum = pdatasort.sum()


    /anaconda3/lib/python3.6/site-packages/pandas/core/frame.py in __init__(self, data, index, columns, dtype, copy)
        402                                          dtype=values.dtype, copy=False)
        403             else:
    --> 404                 raise ValueError('DataFrame constructor not properly called!')
        405 
        406         NDFrame.__init__(self, mgr, fastpath=True)


    ValueError: DataFrame constructor not properly called!



```python
#protot = tdata.groupby("Item Name")
#profto = protot.sum()
#tdata["Total Purchase Value"] = profto
profit = tdata[["Price", "Item Name"]] #"Total Purchase Value"]]
#prfts1 = profit.groupby(["Item Name","Total Purchase Value","Price"]).size().reset_index().nlargest(5,"Total Purchase Value")#.groupby("Price")[[0]].max()
prfts = profit.groupby("Item Name")
#prftitm = profit["Item Name"]
#prftitm_df = pd.DataFrame(prftitm)
#prftcnt = prfts.count()
#prftcnt_df = pd.DataFrame(prftcnt)
prftsum = prfts.sum()
prftsum_df = pd.DataFrame(prftsum)
#prftpr = profit.groupby
#prftpr_df = pd.DataFrame(prftpr)
#prft_df = pd.concat([prftcnt, prftsum, prftpr_df, prftitm_df], axis=1)
sprft = prftsum.nlargest(5,"Price")
#cntlrg = prftcnt.nlargest(5,"Price")
sprft
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
      <th>Price</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>34.65</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>29.75</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>29.70</td>
    </tr>
  </tbody>
</table>
</div>


