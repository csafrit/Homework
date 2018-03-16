

```python
#import modules
import pandas as pd
import numpy as np
import json
import os
import sys
import csv
from sklearn import preprocessing
```


```python
#upload data.parse data.
data=json.load(open("../Weel-04-Pandas/Instructions/HeroesOfPymoli/purchase_data.json"))
fantasy_data=pd.DataFrame(data)
fantasy_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
#Total number of players
total_players = len(fantasy_data["SN"].unique())
print(total_players)
```

    573
    


```python
#Number of Unique Items
unique_items_group = fantasy_data.groupby(["Item ID"])
total_items = len(unique_items_group)
print(total_items)
```

    183
    


```python
#Average Purchase Price
average_Price = fantasy_data["Price"].mean()
print(average_Price)
```

    2.931192307692303
    


```python
 #Total Number of Purchases
total_Purchases = fantasy_data["Price"].count()
print(total_Purchases)
```

    780
    


```python
#Total Revenue
total_Revenue = fantasy_data["Price"].sum()
print(total_Revenue)
```

    2286.3299999999963
    


```python
#Create df with unique SN to work with to remove duplicate players.
#sys.setrecursionlimit(10000) <--- this...crashed the program
fantasy_data1 = fantasy_data.drop_duplicates(["SN"])
fantasy_data1.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
#Gender
total_gender = fantasy_data1["Gender"].count()
```


```python
#Percentage and Count of Male Players
male = fantasy_data1["Gender"].value_counts()['Male']
male_percent = (male/total_gender) * 100
print(f"Male: {male}\n% Male: {male_percent}")
```

    Male: 465
    % Male: 81.15183246073299
    


```python
#Percentage and Count of Female Players
female = fantasy_data1["Gender"].value_counts()['Female']
female_percent = (female/total_gender) * 100
print(f"Female: {female}\n % Female: {female_percent}")
```

    Female: 100
     % Female: 17.452006980802793
    


```python
#Percentage and Count of Other / Non-Disclosed
non_gender_specific = total_gender - male - female
non_gender_specific_percent = (non_gender_specific/total_gender) * 100
print(f" non_specfic: {non_gender_specific} \n % non_specifc: {non_gender_specific}")
```

     non_specfic: 8 
     % non_specifc: 8
    


```python
#By Gender
gender_group = fantasy_data1.groupby(["Gender"])
gender_group.mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
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
      <td>22.260000</td>
      <td>86.130000</td>
      <td>2.887900</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>22.621505</td>
      <td>91.144086</td>
      <td>2.988645</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>25.875000</td>
      <td>121.125000</td>
      <td>3.447500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Purchase Count
female_purchases = fantasy_data.loc[fantasy_data["Gender"] == "Female"]
female_pc = female_purchases["Item ID"].count()
female_pc
```




    136




```python
male_purchases = fantasy_data.loc[fantasy_data["Gender"] == "Male"]
male_pc = male_purchases["Item ID"].count()
male_pc
```




    633




```python
other_purchases = fantasy_data.loc[fantasy_data["Gender"] == "Other / Non-Disclosed"]
other_pc = other_purchases["Item ID"].count()
other_pc
```




    11




```python
#Gender Average Purchase Price
average_purchase_price = gender_group["Price"].mean()
average_purchase_price
```




    Gender
    Female                   2.887900
    Male                     2.988645
    Other / Non-Disclosed    3.447500
    Name: Price, dtype: float64




```python
male_avg_price = average_purchase_price["Male"]
female_avg_price = average_purchase_price["Female"]
other_avg_price = average_purchase_price["Other / Non-Disclosed"]
```


```python
#Total Purchase Value
total_purchase_value = gender_group["Price"].sum()
total_purchase_value
```




    Gender
    Female                    288.79
    Male                     1389.72
    Other / Non-Disclosed      27.58
    Name: Price, dtype: float64




```python

Purchase_gender_df = pd.DataFrame({
"male_tpv": [total_purchase_value["Male"]],
"female_tpv": [total_purchase_value["Female"]],
"other_tpv": [total_purchase_value["Other / Non-Disclosed"]],
})
Purchase_gender_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>female_tpv</th>
      <th>male_tpv</th>
      <th>other_tpv</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>288.79</td>
      <td>1389.72</td>
      <td>27.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Prepping gender norm data
g_p = Purchase_gender_df
min_max_scaler = preprocessing.MinMaxScaler()
g_p_scaled = min_max_scaler.fit_transform(g_p)

gender_norm_p_df = pd.DataFrame({"Gender": ["Male", "Female", "Other"],
                                            "Purchase Count": [female_pc, male_pc, other_pc],
                                            "Average Purchase Price": [female_avg_price, male_avg_price, other_avg_price],
                                            "Total Purchase Value": [female_tpv, male_tpv, other_tpv],
                                         "Normalized Totals": [g_p_scaled, g_p_scaled, g_p_scaled],
                                           #"Normalized Totals": [g_p_scaled[:,0], g_p_scaled[:,1], g_p_scaled[:,2]],
                                            })
gender_norm_p_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Gender</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.887900</td>
      <td>Male</td>
      <td>[[0.0, 0.0, 0.0]]</td>
      <td>136</td>
      <td>288.79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.988645</td>
      <td>Female</td>
      <td>[[0.0, 0.0, 0.0]]</td>
      <td>633</td>
      <td>1389.72</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.447500</td>
      <td>Other</td>
      <td>[[0.0, 0.0, 0.0]]</td>
      <td>11</td>
      <td>27.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
g_p = Purchase_gender_df.values.astype(float)
min_max_scaler = preprocessing.MinMaxScaler()
g_p_scaled = min_max_scaler.fit_transform(g_p)
print(g_p_scaled)
```

    [[ 0.  0.  0.]]
    


```python
#Normalized Totals
#df_norm = (df - df.mean()) / (df.max() - df.min())

#Purchase sum
#avg purchase price
#purchase value
x = df.values #returns a numpy array
min_max_scaler = preprocessing.MinMaxScaler()
x_scaled = min_max_scaler.fit_transform(x)
df = pandas.DataFrame(x_scaled)

# Create x, where x the 'scores' column's values as floats
gender_sum = gender_group["Item ID"].count()

# Create a minimum and maximum processor object
min_max_scaler = preprocessing.MinMaxScaler()

# Create an object to transform the data to fit minmax processor
gender_sum_scaled = min_max_scaler.fit_transform(gender_sum)

# Run the normalizer on the dataframe
gender_sum_norm = pd.DataFrame(gender_sum_scaled)
# View the dataframe
gender_sum_norm



#df_norm
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-248-439f8b6d5163> in <module>()
          8 min_max_scaler = preprocessing.MinMaxScaler()
          9 x_scaled = min_max_scaler.fit_transform(x)
    ---> 10 df = pandas.DataFrame(x_scaled)
         11 
         12 # Create x, where x the 'scores' column's values as floats
    

    NameError: name 'pandas' is not defined



```python
#Again by bins of 4 years
#Identify range
age_low = fantasy_data["Age"].min()
age_high = fantasy_data["Age"].max()
print(age_low, age_high)
```

    7 45
    


```python
#create bins

age_bins = [4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48]
age_ranges = ["4-8", "8-12", "12-16", "16-20", "20-24", "24-28", "28-32", "32-36", "36-40", "40-44", "44-48"]
```


```python
#place ages from fantasy data into bins

fantasy_data["Age Range"] = pd.cut(fantasy_data["Age"], age_bins, labels=age_ranges)
fantasy_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Age Range</th>
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
      <td>36-40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>32-36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Group by Age Range, (optional) find oldest in each age group
age_groups = fantasy_data.groupby("Age Range")
age_groups.mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4-8</th>
      <td>7.136364</td>
      <td>85.909091</td>
      <td>2.788182</td>
    </tr>
    <tr>
      <th>8-12</th>
      <td>10.541667</td>
      <td>87.125000</td>
      <td>3.385417</td>
    </tr>
    <tr>
      <th>12-16</th>
      <td>14.942529</td>
      <td>92.816092</td>
      <td>2.745862</td>
    </tr>
    <tr>
      <th>16-20</th>
      <td>19.248447</td>
      <td>84.248447</td>
      <td>2.907019</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>22.647059</td>
      <td>89.605042</td>
      <td>2.924748</td>
    </tr>
    <tr>
      <th>24-28</th>
      <td>25.634615</td>
      <td>93.855769</td>
      <td>2.974712</td>
    </tr>
    <tr>
      <th>28-32</th>
      <td>30.257576</td>
      <td>91.166667</td>
      <td>3.061970</td>
    </tr>
    <tr>
      <th>32-36</th>
      <td>34.394737</td>
      <td>105.631579</td>
      <td>2.981053</td>
    </tr>
    <tr>
      <th>36-40</th>
      <td>38.648649</td>
      <td>112.972973</td>
      <td>2.901351</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>42.500000</td>
      <td>83.500000</td>
      <td>2.960000</td>
    </tr>
    <tr>
      <th>44-48</th>
      <td>45.000000</td>
      <td>124.000000</td>
      <td>2.720000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Purchase Count
age_groups["Item ID"].count()
```




    Age Range
    4-8       22
    8-12      24
    12-16     87
    16-20    161
    20-24    238
    24-28    104
    28-32     66
    32-36     38
    36-40     37
    40-44      2
    44-48      1
    Name: Item ID, dtype: int64




```python
#Age Average Purchase Price
age_groups["Price"].mean()
```




    Age Range
    4-8      2.788182
    8-12     3.385417
    12-16    2.745862
    16-20    2.907019
    20-24    2.924748
    24-28    2.974712
    28-32    3.061970
    32-36    2.981053
    36-40    2.901351
    40-44    2.960000
    44-48    2.720000
    Name: Price, dtype: float64




```python
#Age Total Purchase Value
age_groups["Price"].sum()
```




    Age Range
    4-8       61.34
    8-12      81.25
    12-16    238.89
    16-20    468.03
    20-24    696.09
    24-28    309.37
    28-32    202.09
    32-36    113.28
    36-40    107.35
    40-44      5.92
    44-48      2.72
    Name: Price, dtype: float64




```python
#Age Normalized totals
```


```python
#Adds column of how many times a SN appears to fantasy_data
fantasy_data["SN_count"] = fantasy_data.groupby(['SN'])['SN'].transform('count')
```


```python
fantasy_sn_groups = fantasy_data.groupby("SN")
spenders_price_count_df = fantasy_sn_groups["Price"].count()
spenders_price_average_df = fantasy_sn_groups["Price"].mean()
spenders_price_total_df = fantasy_sn_groups["Price"].sum()

spenders_df = pd.DataFrame({"Purchase count": spenders_price_count_df,
                           "Average purchase price": spenders_price_average_df,
                           "Total purchase value": spenders_price_total_df},
                           columns = ["Purchase count", "Average purchase price", "Total purchase value"])

top_five_spenders_df = spenders_df.sort_values("Total purchase value", ascending = False)
top_five_spenders_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase count</th>
      <th>Average purchase price</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>3.412000</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.390000</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.185000</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.243333</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.860000</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Identify the 5 most popular items by purchase count, then list (in a table):
#Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value
item_popularity = fantasy_data["Item Name"].value_counts()
print(item_popularity[:5])
#The top 5 are the most popular items- Final Critic:Retribution Axe
```

    Final Critic                            14
    Arcane Gem                              11
    Betrayal, Whisper of Grieving Widows    11
    Stormcaller                             10
    Retribution Axe                          9
    Name: Item Name, dtype: int64
    


```python
Items_grouped_df = fantasy_data.groupby(["Item ID","Item Name"])
Items_pc_df = Items_grouped_df["Price"].count()
Items_Purchase_Value_df = Items_grouped_df["Price"].sum()
Items_price_df = Items_grouped_df["Price"].unique()



Items_df = pd.DataFrame({"Purchase Count": Items_pc_df,
                         "Item Price": Items_price_df,
                         "Total Purchase Value": Items_Purchase_Value_df},
                        columns =["Purchase Count","Item Price","Total Purchase Value"])


Top_Items_df = Items_df.sort_values("Purchase Count", ascending = False)
Top_Items_df["Total Purchase Value"] = Top_Items_df["Total Purchase Value"].map("${:.2f}".format)
Top_Items_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>[2.35]</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>[2.23]</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>[2.07]</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>[1.24]</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>[1.49]</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python

#Identify the 5 most profitable items by total purchase value, then list (in a table):
#Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value
#Group by purchase value, sort descending. Filter by most popular. 

fantasy_item_df = fantasy_data.groupby("Item Name")

item_price_count_df = fantasy_item_df["Price"].count()
item_price_average_df = fantasy_item_df["Price"].mean()
item_price_total_df = fantasy_item_df["Price"].sum()

item_df = pd.DataFrame({"Purchase count": item_price_count_df,
                           "Average purchase price": item_price_average_df,
                           "Total purchase value": item_price_total_df},
                           columns = ["Purchase count", "Average purchase price", "Total purchase value"])

top_five_item_df = item_df.sort_values("Total purchase value", ascending = False)
top_five_item_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase count</th>
      <th>Average purchase price</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>2.757143</td>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>4.140000</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>3.465000</td>
      <td>34.65</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>4.250000</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>6</td>
      <td>4.950000</td>
      <td>29.70</td>
    </tr>
  </tbody>
</table>
</div>


