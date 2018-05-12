# OBSERVED TREND1 - majority of players are male
# OBSERVED TREND2 - majority of players are between 20-24 years of age, the distribution is right skewed
# OBSERVED TREND3 - the most profitable items are not the ones that are bought most frequently.

```python
# heros of pymoli homework
```


```python
# load dependencies
import pandas as pd
import numpy as np

```


```python
# get the dataz (json)
buy_df = pd.read_json("resources/purchase_data.json")
buy_df.head()
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
# player count (total unique sn(screen name) players)
sn_distinct = buy_df['SN'].nunique()
t_plrs = {'Total Players': [sn_distinct]}
t_plrs_df = pd.DataFrame(t_plrs)
t_plrs_df
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




```python
    #count unique item name
item_distinct = buy_df['Item Name'].nunique()
#item_distinct
```


```python
    #count unique item id
item_id_distinct = buy_df['Item ID'].nunique()
#item_id_distinct
```


```python
    #avg purchase price
avg_buy_price = buy_df['Price'].mean()
#avg_buy_price
```


```python
    #count purchases
cnt_buy = buy_df['SN'].count()
#cnt_buy
```


```python
    #sum purchases
sum_price = buy_df['Price'].sum()
#sum_price
```


```python
#make dataframe of purchasing things
pch_summary = pd.DataFrame({'Number of Unique Items':[item_id_distinct], 'Average Price':[avg_buy_price],'Number of Purchases':[cnt_buy],'Total Revenue':[sum_price]})
pch_summary = pch_summary[['Number of Unique Items','Average Price', 'Number of Purchases', 'Total Revenue']]
pch_summary["Average Price"] = pch_summary["Average Price"].map("${:,.2f}".format)
pch_summary["Total Revenue"] = pch_summary["Total Revenue"].map("${:,.2f}".format)

pch_summary
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
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender demographics
```


```python
        # need a table of all unique sn and gender
        # make a df with just gender an sn with unique values
sn_gender_df = buy_df.filter(['SN','Gender']) # just these two columns
sn_gender_df = sn_gender_df.drop_duplicates() # unique combinations
gender_counts = sn_gender_df.groupby("Gender") # group on gender 
gender_counts = gender_counts.count()   # gets count by gender
gender_counts.columns = ['Total Count'] # rename column from sn to total count
gender_counts['Percentage of Players'] = (gender_counts['Total Count']/sn_distinct)*100 # caluculate the % of Total
gender_counts = gender_counts[['Percentage of Players','Total Count']] # move columns to correct position
gender_counts

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
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.452007</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.151832</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.396161</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# purchase analysis by gender
```


```python
gender_price_df = buy_df.filter(['Gender','Price'])  # just need these two things
buy_data = gender_price_df.groupby("Gender").count()
buy_data['Purchase Count'] = gender_price_df.groupby("Gender").count()  # count of buys
buy_data['Average Purchase Price'] = gender_price_df.groupby("Gender").mean() # avg
buy_data['Total Purchase Value'] = gender_price_df.groupby("Gender").sum() # total
buy_data['Normalized Totals'] = buy_data['Average Purchase Price'] * gender_counts['Total Count'] # normalized

buy_data = buy_data[['Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Totals']] # limit columns

buy_data["Average Purchase Price"] = buy_data["Average Purchase Price"].map("${:,.2f}".format) # format
buy_data["Total Purchase Value"] = buy_data["Total Purchase Value"].map("${:,.2f}".format) # format
buy_data["Normalized Totals"] = buy_data["Normalized Totals"].map("${:,.2f}".format)  # format

buy_data
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
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$281.55</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$1,371.99</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$25.99</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age demographics (bins of 5 starting at <10)  min is 7 max is 45
```


```python
# % of unique players and count of unique players in each bin
# bins
bins = [0,9,14,19,24,29,34,39,44,49]   #  10 count
bin_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40-44','45+']  # 9 count

age_group = pd.cut(buy_df['Age'],bins,labels=bin_names) # cut does the match
buy_df['Age Group'] = age_group # add the age group to my primary dataframe

age_group_count = buy_df.groupby('Age Group').nunique('SN')  
age_group_count = age_group_count[['SN']]
age_group_count.columns = ['Total Count']
age_group_count['Percentage of Players'] = round((age_group_count['Total Count']/sn_distinct)*100 ,2)
age_group_count = age_group_count[['Percentage of Players','Total Count']]
age_group_count
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
    <tr>
      <th>Age Group</th>
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
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>1.75</td>
      <td>10</td>
    </tr>
    <tr>
      <th>45+</th>
      <td>0.17</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_buy = buy_df.filter(['Age Group','Price'])
age_buy_group = age_buy.groupby('Age Group').count()
age_buy_group['Purchase Count'] = age_buy.groupby('Age Group').count()
age_buy_group['Average Purchase Price'] = age_buy.groupby('Age Group').mean()
age_buy_group['Total Purchase Value'] = age_buy.groupby('Age Group').sum()
age_buy_group['Normalized Totals'] = age_buy_group['Average Purchase Price'] * age_group_count['Total Count']

age_buy_group["Average Purchase Price"] = age_buy_group["Average Purchase Price"].map("${:,.2f}".format)  # format
age_buy_group["Total Purchase Value"] = age_buy_group["Total Purchase Value"].map("${:,.2f}".format)  # format
age_buy_group["Normalized Totals"] = age_buy_group["Normalized Totals"].map("${:,.2f}".format)  # format
age_buy_group = age_buy_group[['Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Totals']]
age_buy_group
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
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
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
      <td>$56.63</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$63.71</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$290.54</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$754.47</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$257.75</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$144.86</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$76.76</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>16</td>
      <td>$3.19</td>
      <td>$51.03</td>
      <td>$31.89</td>
    </tr>
    <tr>
      <th>45+</th>
      <td>1</td>
      <td>$2.72</td>
      <td>$2.72</td>
      <td>$2.72</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders - id the top 5 spenders in the game by total purchase value 
# then list in a table sn, purchase count, avg pruchase price, total purchase value
```


```python
spenders = buy_df.filter(['SN','Price'])
by_spenders = spenders.groupby('SN').count()
by_spenders['Purchase Count'] = spenders.groupby('SN').count()
by_spenders['Average Purchase Price'] = spenders.groupby('SN').mean()
by_spenders['Total Purchase Value'] = spenders.groupby('SN').sum()

by_spenders = by_spenders[['Purchase Count','Average Purchase Price','Total Purchase Value']] # limit columns
by_spenders["Average Purchase Price"] = by_spenders["Average Purchase Price"].map("${:,.2f}".format)  # format
by_spenders["Total Purchase Value"] = by_spenders["Total Purchase Value"].map("${:,.2f}".format)  # format
by_spenders_sort = by_spenders.sort_values("Total Purchase Value", ascending=False)
by_spenders_sort
by_spenders_sort.head()

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
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>3</td>
      <td>$3.13</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>2</td>
      <td>$4.59</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
#popular items - 5 most popular items by purchase count then show table
```


```python
pop = buy_df.filter(['Item Name','Price'])
pop_cnt = pop.groupby('Item Name').count()
pop_cnt['Purchase Count'] = pop.groupby('Item Name').count()
pop_cnt['Item Price'] = pop.groupby('Item Name').mean()
pop_cnt['Total Purchase Value'] = pop.groupby('Item Name').sum()
pop_cnt = pop_cnt[['Purchase Count','Item Price','Total Purchase Value']]
pop_cnt["Item Price"] = pop_cnt["Item Price"].map("${:,.2f}".format)  # format
pop_cnt["Total Purchase Value"] = pop_cnt["Total Purchase Value"].map("${:,.2f}".format)  # format
pop_cnt = pop_cnt.sort_values("Purchase Count",ascending=False)

pop_cnt.head()

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
      <th>Item Price</th>
      <th>Total Purchase Value</th>
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
      <td>$2.76</td>
      <td>$38.60</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>$3.46</td>
      <td>$34.65</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# most profitable items - 5 most porfitable itmes by total purchase value then show table
```


```python
val = buy_df.filter(['Item Name','Price'])
pop_val = val.groupby('Item Name').count()
pop_val['Purchase Count'] = pop.groupby('Item Name').count()
pop_val['Item Price'] = pop.groupby('Item Name').mean()
pop_val['Total Purchase Value'] = pop.groupby('Item Name').sum()
pop_val = pop_val[['Purchase Count','Item Price','Total Purchase Value']]
pop_val["Item Price"] = pop_val["Item Price"].map("${:,.2f}".format)  # format
pop_val["Total Purchase Value"] = pop_val["Total Purchase Value"].map("${:,.2f}".format)  # format
pop_val = pop_val.sort_values("Total Purchase Value",ascending=False)

pop_val.head()
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
      <th>Item Price</th>
      <th>Total Purchase Value</th>
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
      <th>Shadowsteel</th>
      <td>5</td>
      <td>$1.98</td>
      <td>$9.90</td>
    </tr>
    <tr>
      <th>Souleater</th>
      <td>3</td>
      <td>$3.27</td>
      <td>$9.81</td>
    </tr>
    <tr>
      <th>Shadow Strike, Glory of Ending Hope</th>
      <td>5</td>
      <td>$1.93</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>Heartseeker, Reaver of Souls</th>
      <td>3</td>
      <td>$3.21</td>
      <td>$9.63</td>
    </tr>
    <tr>
      <th>Agatha</th>
      <td>5</td>
      <td>$1.91</td>
      <td>$9.55</td>
    </tr>
  </tbody>
</table>
</div>


