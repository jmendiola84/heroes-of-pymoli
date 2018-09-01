# Heroes of Pymoli
![Earthquakes Map](images/earthquake.png)
## Data Analysis on a fictitious video game using Pandas.
#### *Project created as part of GWU Data Analytics course.*
> Pandas, Python


## Data Insights
* People do not spend more than $20.00 in the game.
* Most purchases are made by people from 18 to 25 years old.
* Most popular items prices are below the average item price.



```python
import pandas as pd
import numpy as np
```


```python
json_path = ('purchase_data.json')
heroes_df = pd.read_json(json_path, orient='records')
heroes_df.head()
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
#Total Players
total_players = heroes_df["SN"].nunique()
tot_players_dict={"Total Players":[total_players]}
tot_plyr_df = pd.DataFrame.from_dict(tot_players_dict, orient='columns')
tot_plyr_df
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
#Purchase Analysis
#Number of Unique Items
nbr_unique_items = heroes_df["Item ID"].nunique()
#Average Purchase Price
avg_purch_price = heroes_df["Price"].mean()
#Total Number of Purchases
total_nbr_purch = len(heroes_df.index)
#Total Revenue
total_revenue = heroes_df["Price"].sum()

purch_tot_df = pd.DataFrame.from_dict({"Number of unique items":[nbr_unique_items],"Average Price":[avg_purch_price],"Number of Purchases":[total_nbr_purch],"Total Revenue":[total_revenue]}, orient='columns')
purch_tot_df["Total Revenue"] = purch_tot_df["Total Revenue"].map("$ {:,.2f}".format)
purch_tot_df["Average Price"] = purch_tot_df["Average Price"].map("$ {:,.2f}".format)
purch_tot_df
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of unique items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$ 2.93</td>
      <td>780</td>
      <td>183</td>
      <td>$ 2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
#Percentage and Count of Male Players
heroes_gender_df = pd.DataFrame(heroes_df.drop_duplicates(["SN"]))
gender_demo_grp = heroes_gender_df.groupby(['Gender'])
#gender_demo_df[""].mean()
gender_df = pd.DataFrame({"Total Count":gender_demo_grp["Gender"].count(),"Percentage of players":gender_demo_grp["Gender"].count()/total_players*100})
gender_df["Percentage of players"] = gender_df["Percentage of players"].map("{:.2f}%".format)
#gender_df = gender_df.set_index('Gender')
gender_df.head()
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
      <th>Percentage of players</th>
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
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)
#The below each broken by gender
#Purchase Count
#Average Purchase Price
#Total Purchase Value
#Normalized Totals
purch_df = pd.DataFrame(heroes_df)
gender_purch = purch_df.groupby(['Gender'])
gen_purch_df = pd.DataFrame({"Purchase Count":gender_purch["Price"].count(), "Average Purchase Price":gender_purch["Price"].mean(),"Total Purchase Value":gender_purch["Price"].sum(), "Normalized Totals":gender_purch["Price"].sum()/gender_df["Total Count"]})
gen_purch_df["Average Purchase Price"] = gen_purch_df["Average Purchase Price"].map("${:.2f}".format)
gen_purch_df["Total Purchase Value"] = gen_purch_df["Total Purchase Value"].map("${:,.2f}".format)
gen_purch_df["Normalized Totals"] = gen_purch_df["Normalized Totals"].map("${:,.2f}".format)
gen_purch_df.head()
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
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
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
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_df = pd.DataFrame(heroes_df)
group_names = ['<10', '10-13', '14-17', '18-21', '22-25', '26-29', '30-33', '34-37', '38-40', '>40']
bins = [0, 9, 13, 17, 21, 25, 29, 33, 37, 40, 80]

age_demo_grp = age_df.groupby(pd.cut(age_df["Age"], bins, labels=group_names))

# Create the names for the four bins
age_demo_df = pd.DataFrame({"Purchase Count":age_demo_grp["Price"].count(), "Average Purchase Price":age_demo_grp["Price"].mean(),"Total Purchase Value":age_demo_grp["Price"].sum(),"Normalized Totals":age_demo_grp["Price"].sum()/age_demo_grp["SN"].nunique()})
age_demo_df["Average Purchase Price"] = age_demo_df["Average Purchase Price"].map("${:.2f}".format)
age_demo_df["Total Purchase Value"] = age_demo_df["Total Purchase Value"].map("${:,.2f}".format)
age_demo_df["Normalized Totals"] = age_demo_df["Normalized Totals"].map("${:,.2f}".format)
age_demo_df
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
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>$2.98</td>
      <td>$4.39</td>
      <td>28</td>
      <td>$83.46</td>
    </tr>
    <tr>
      <th>10-13</th>
      <td>$2.85</td>
      <td>$4.13</td>
      <td>29</td>
      <td>$82.65</td>
    </tr>
    <tr>
      <th>14-17</th>
      <td>$2.87</td>
      <td>$3.81</td>
      <td>93</td>
      <td>$266.48</td>
    </tr>
    <tr>
      <th>18-21</th>
      <td>$2.87</td>
      <td>$3.84</td>
      <td>187</td>
      <td>$537.29</td>
    </tr>
    <tr>
      <th>22-25</th>
      <td>$2.99</td>
      <td>$3.91</td>
      <td>262</td>
      <td>$782.24</td>
    </tr>
    <tr>
      <th>26-29</th>
      <td>$2.82</td>
      <td>$4.20</td>
      <td>58</td>
      <td>$163.81</td>
    </tr>
    <tr>
      <th>30-33</th>
      <td>$3.09</td>
      <td>$4.22</td>
      <td>56</td>
      <td>$172.93</td>
    </tr>
    <tr>
      <th>34-37</th>
      <td>$2.85</td>
      <td>$4.27</td>
      <td>36</td>
      <td>$102.59</td>
    </tr>
    <tr>
      <th>38-40</th>
      <td>$3.08</td>
      <td>$5.07</td>
      <td>28</td>
      <td>$86.24</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>$2.88</td>
      <td>$2.88</td>
      <td>3</td>
      <td>$8.64</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#SN
#Purchase Count
#Average Purchase Price
#Total Purchase Value


spenders_df = pd.DataFrame(heroes_df)
top_spnd_grp = spenders_df.groupby(['SN'])
top_spnd_df = pd.DataFrame({"Purchase Count":top_spnd_grp["Price"].count(), "Average Purchase Price":top_spnd_grp["Price"].mean(),"Total Purchase Value":top_spnd_grp["Price"].sum()})
top_spnd_df = top_spnd_df.sort_values("Total Purchase Value", ascending=False)
top_spnd_df["Average Purchase Price"] = top_spnd_df["Average Purchase Price"].map("${:.2f}".format)
top_spnd_df["Total Purchase Value"] = top_spnd_df["Total Purchase Value"].map("${:,.2f}".format)
top_spnd_df.head()
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
      <th>Purchase Count</th>
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
      <th>Undirrala66</th>
      <td>$3.41</td>
      <td>5</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items

#Identify the 5 most popular items by purchase count, then list (in a table):
#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value


popit_df = pd.DataFrame(heroes_df)
popitems_grp = popit_df.groupby(['Item ID','Item Name'])
popitems_df = pd.DataFrame({"Purchase Count":popitems_grp["Price"].count(), "Item Price":popitems_grp["Price"].mean(),"Total Purchase Value":popitems_grp["Price"].sum()})
popitems_df = popitems_df.sort_values("Purchase Count", ascending=False)
popitems_df["Item Price"] = popitems_df["Item Price"].map("${:.2f}".format)
popitems_df["Total Purchase Value"] = popitems_df["Total Purchase Value"].map("${:,.2f}".format)

popitems_df.head()
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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items

#Identify the 5 most profitable items by total purchase value, then list (in a table):
#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

profit_df = pd.DataFrame(heroes_df)
profit_grp = profit_df.groupby(['Item ID','Item Name'])
profit_df = pd.DataFrame({"Purchase Count":profit_grp["Price"].count(), "Item Price":profit_grp["Price"].mean(),"Total Purchase Value":profit_grp["Price"].sum()})
profit_df = profit_df.sort_values("Total Purchase Value", ascending=False)
profit_df["Item Price"] = profit_df["Item Price"].map("${:.2f}".format)
profit_df["Total Purchase Value"] = profit_df["Total Purchase Value"].map("${:,.2f}".format)

profit_df.head()
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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <th>34</th>
      <th>Retribution Axe</th>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


